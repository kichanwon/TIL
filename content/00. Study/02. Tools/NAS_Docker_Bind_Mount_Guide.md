# NAS 개인 홈 디렉터리를 Docker 컨테이너에 바인딩하는 절차

## 1. 문서 목적

이 문서는 연구실 GPU 서버의 Docker 컨테이너에서 Synology NAS의 사용자별 개인 홈 디렉터리를 사용할 수 있도록 구성하는 절차를 정리한 인수인계 문서다.

구성 목표는 다음과 같다.

- NAS의 사용자별 `home` 공유를 Docker 호스트에 CIFS로 마운트한다.
- 호스트에 마운트된 NAS 경로를 Docker 컨테이너 내부의 하위 디렉터리에 bind mount 한다.
- 컨테이너의 기존 홈 디렉터리 전체를 덮지 않는다.
- 사용자별 UID/GID를 맞춰 일반 사용자 권한으로 읽기·쓰기 가능하게 한다.
- 컨테이너 재생성 후에도 NAS 데이터와 SSH 공개키가 유지되도록 한다.
- NAS 계정 비밀번호는 호스트의 root 전용 credential 파일에만 저장한다.

---

## 2. 전체 구조

최종 구조는 다음과 같다.

```text
Synology NAS
//10.108.90.22/home
        |
        | CIFS/SMB mount
        v
Docker 호스트
/srv/docker-storage/nas/gpu03/home
        |
        | Docker bind mount
        v
gpu-03 컨테이너
/home/gpu03user/nas-home
```

중요한 점은 다음과 같다.

- 이 구성은 파일 동기화가 아니다.
- 주기적으로 업로드하거나 다운로드하지 않는다.
- 컨테이너에서 `/home/gpu03user/nas-home`에 파일을 쓰면 즉시 NAS에 기록된다.
- NAS에서 파일을 만들거나 수정하면 컨테이너에서도 동일한 경로에서 바로 보인다.
- 로컬 SSD와 비교하면 작은 파일이 많은 작업은 느릴 수 있다.
- Git 저장소, Python 가상환경, `node_modules`, 빌드 캐시 등은 로컬 디스크 사용을 권장한다.
- 데이터셋, 체크포인트, 결과물, 백업, 공유 파일은 NAS에 두는 것이 적합하다.

---

## 3. 현재 서버 구성 정보

### 3.1 NAS

```text
NAS IP: 10.108.90.22
SMB 포트: 445
NAS 웹 관리 포트: 5000
```

SMB 접속 시 다음 형식은 잘못된 형식이다.

```text
//http://10.108.90.22:5000
```

SMB 접속에는 `http://`와 웹 관리 포트 `5000`을 사용하지 않는다.

정상 형식:

```text
//10.108.90.22/home
```

### 3.2 NAS 공유 목록

다음 명령으로 확인한다.

```bash
sudo smbclient -L //10.108.90.22 \
  -A /root/.nas-gpu03-credentials \
  -m SMB3
```

확인된 주요 공유:

```text
AILAB-share
home
```

사용자 개인 디렉터리는 `home` 공유를 사용한다.

`home`은 로그인한 NAS 사용자 본인의 개인 홈 디렉터리다.

### 3.3 Docker 서비스

`gpu-03` 서비스의 주요 설정:

```yaml
environment:
  USER_NAME: gpu03user
  USER_UID: "1203"
  USER_GID: "1203"

ports:
  - "10003:22"
```

의미:

```text
호스트 10.107.60.152:10003
    -> gpu-03 컨테이너:22

컨테이너 사용자:
gpu03user
UID 1203
GID 1203
```

---

## 4. 사전 요구사항

아래 작업은 Docker 컨테이너 내부가 아니라 Docker 호스트에서 수행한다.

현재 위치 확인:

```bash
test -f /.dockerenv \
  && echo "Docker 컨테이너 내부" \
  || echo "Docker 호스트"
```

`Docker 컨테이너 내부`가 나오면 호스트 셸로 이동한다.

필요 패키지 설치:

```bash
sudo apt update
sudo apt install -y cifs-utils smbclient
```

SMB 포트 확인:

```bash
nc -vz -w 3 10.108.90.22 445
```

정상 예:

```text
Connection to 10.108.90.22 445 port [tcp/microsoft-ds] succeeded!
```

---

## 5. NAS credential 파일 생성

사용자별 credential 파일은 root 전용으로 관리한다.

경로:

```text
/root/.nas-gpu00-credentials
/root/.nas-gpu01-credentials
/root/.nas-gpu02-credentials
/root/.nas-gpu03-credentials
```

파일 생성:

```bash
sudo install -o root -g root -m 0600 /dev/null \
  /root/.nas-gpu03-credentials
```

내용 입력:

```bash
sudo tee /root/.nas-gpu03-credentials >/dev/null <<'EOF'
username=NAS_USERNAME
password=NAS_PASSWORD
EOF
```

권한 정리:

```bash
sudo chown root:root /root/.nas-gpu03-credentials
sudo chmod 600 /root/.nas-gpu03-credentials
```

값을 노출하지 않고 항목 존재 여부 확인:

```bash
sudo sed -E 's/=.*/=<configured>/' \
  /root/.nas-gpu03-credentials
```

정상 출력:

```text
username=<configured>
password=<configured>
```

주의:

- credential 파일을 Git 저장소에 넣지 않는다.
- Compose 파일에 비밀번호를 직접 기록하지 않는다.
- 일반 사용자가 읽을 수 있도록 권한을 낮추지 않는다.
- 파일 권한은 반드시 `600`, 소유자는 `root:root`로 유지한다.

---

## 6. 호스트 마운트 디렉터리 생성

권장 구조:

```text
/srv/docker-storage/
└── nas/
    ├── gpu00/
    │   └── home/
    ├── gpu01/
    │   └── home/
    ├── gpu02/
    │   └── home/
    └── gpu03/
        └── home/
```

`gpu03` 생성:

```bash
sudo install -d -o root -g root -m 0755 \
  /srv/docker-storage/nas/gpu03/home
```

전체 사용자 디렉터리 생성:

```bash
sudo bash <<'EOF'
set -euo pipefail

for id in 00 01 02 03; do
  install -d -o root -g root -m 0755 \
    "/srv/docker-storage/nas/gpu${id}/home"

  cred="/root/.nas-gpu${id}-credentials"

  if [ ! -e "$cred" ]; then
    install -o root -g root -m 0600 /dev/null "$cred"
  else
    chown root:root "$cred"
    chmod 0600 "$cred"
  fi
done
EOF
```

---

## 7. NAS 개인 홈 접근 테스트

마운트 전에 NAS 계정이 자신의 `home` 공유에 접근 가능한지 확인한다.

```bash
sudo smbclient //10.108.90.22/home \
  -A /root/.nas-gpu03-credentials \
  -m SMB3 \
  -c 'ls'
```

정상이라면 파일 목록이 출력된다.

주요 오류:

```text
NT_STATUS_LOGON_FAILURE
```

- 사용자명 또는 비밀번호 오류

```text
NT_STATUS_ACCESS_DENIED
```

- 로그인 성공
- 해당 NAS 공유 권한 없음

```text
NT_STATUS_NOT_FOUND
```

- NAS 주소 또는 공유 이름 오류
- `http://`, `:5000` 등을 잘못 포함했는지 확인

```text
Connection refused
```

- NAS SMB 서비스 미실행
- 445 포트 차단

---

## 8. NAS를 Docker 호스트에 마운트

`gpu-03` 컨테이너 사용자는 UID/GID가 `1203:1203`이다.

따라서 CIFS 마운트 옵션도 `uid=1203,gid=1203`으로 맞춘다.

기존 마운트가 있으면 해제:

```bash
sudo umount /srv/docker-storage/nas/gpu03/home 2>/dev/null || true
```

마운트:

```bash
sudo mount -t cifs \
  //10.108.90.22/home \
  /srv/docker-storage/nas/gpu03/home \
  -o "credentials=/root/.nas-gpu03-credentials,vers=3.0,iocharset=utf8,noserverino,uid=1203,gid=1203,file_mode=0664,dir_mode=0775"
```

옵션 설명:

```text
credentials=...
  NAS 사용자명과 비밀번호를 별도 파일에서 읽는다.

vers=3.0
  SMB 3.0을 사용한다.

iocharset=utf8
  한글 파일명 처리를 위해 UTF-8을 사용한다.

noserverino
  일부 NAS 환경에서 inode 관련 문제를 줄인다.

uid=1203
  마운트된 파일을 로컬에서 UID 1203 소유로 보이게 한다.

gid=1203
  마운트된 파일을 로컬에서 GID 1203 소유로 보이게 한다.

file_mode=0664
  파일 기본 권한을 rw-rw-r-- 형태로 보이게 한다.

dir_mode=0775
  디렉터리 기본 권한을 rwxrwxr-x 형태로 보이게 한다.
```

---

## 9. 호스트 마운트 검증

마운트 상태 확인:

```bash
findmnt -T /srv/docker-storage/nas/gpu03/home
```

정상 출력에는 다음 정보가 포함된다.

```text
SOURCE: //10.108.90.22/home
FSTYPE: cifs
OPTIONS: uid=1203,gid=1203,...
```

숫자 UID/GID 확인:

```bash
ls -ldn /srv/docker-storage/nas/gpu03/home
```

정상적으로 `1203 1203`이 보여야 한다.

읽기·쓰기·삭제 테스트:

```bash
set -e

MOUNT_POINT="/srv/docker-storage/nas/gpu03/home"
TEST_DIR="$MOUNT_POINT/.gpu03-nas-test"
TEST_FILE="$TEST_DIR/test.txt"

mountpoint -q "$MOUNT_POINT"

mkdir "$TEST_DIR"
printf 'NAS test: %s\n' "$(date -Iseconds)" > "$TEST_FILE"

cat "$TEST_FILE"
ls -ln "$TEST_FILE"

mv "$TEST_FILE" "$TEST_DIR/renamed.txt"
rm "$TEST_DIR/renamed.txt"
rmdir "$TEST_DIR"

echo "GPU03 NAS host read/write/delete test: OK"
```

Synology File Station에서 `home`을 새로고침하면 테스트 파일 생성 여부를 확인할 수 있다.

---

## 10. Docker Compose에 NAS bind mount 추가

기존 Compose에서 사용자 홈 전체 마운트는 유지한다.

기존 구성:

```yaml
volumes:
  - type: bind
    source: /srv/gpu-containers/gpu-03/home
    target: /home/gpu03user

  - type: bind
    source: /srv/gpu-containers/gpu-03/ssh-host-keys
    target: /etc/ssh/hostkeys
```

NAS용 세 번째 bind mount를 추가한다.

```yaml
volumes:
  - type: bind
    source: /srv/gpu-containers/gpu-03/home
    target: /home/gpu03user

  - type: bind
    source: /srv/gpu-containers/gpu-03/ssh-host-keys
    target: /etc/ssh/hostkeys

  - type: bind
    source: /srv/docker-storage/nas/gpu03/home
    target: /home/gpu03user/nas-home
```

`gpu-03` 전체 예시:

```yaml
services:
  gpu-03:
    image: gpu-ssh:base
    container_name: gpu-03
    hostname: gpu-03
    restart: unless-stopped
    init: true

    cpuset: "${SOLO_CPUSET}"
    cpu_shares: 1024
    shm_size: "32g"

    environment:
      USER_NAME: gpu03user
      USER_UID: "1203"
      USER_GID: "1203"

    ports:
      - "10003:22"

    volumes:
      - type: bind
        source: /srv/gpu-containers/gpu-03/home
        target: /home/gpu03user

      - type: bind
        source: /srv/gpu-containers/gpu-03/ssh-host-keys
        target: /etc/ssh/hostkeys

      - type: bind
        source: /srv/docker-storage/nas/gpu03/home
        target: /home/gpu03user/nas-home

    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              device_ids: ["3"]
              capabilities: [gpu]
```

절대 다음처럼 연결하지 않는다.

```yaml
source: /srv/docker-storage/nas/gpu03/home
target: /home/gpu03user
```

이렇게 하면 NAS가 컨테이너 홈 전체를 덮는다.

가려지는 주요 파일과 폴더:

```text
/home/gpu03user/.ssh
/home/gpu03user/.conda
/home/gpu03user/.vscode-server
/home/gpu03user/week1_project
/home/gpu03user/.bashrc
```

항상 NAS는 홈의 하위 디렉터리에 연결한다.

```text
/home/gpu03user/nas-home
```

---

## 11. Compose 설정 적용

Compose 파일 위치:

```text
~/gpu-stack/compose.yaml
```

설정 검증:

```bash
cd ~/gpu-stack
sudo docker compose config
```

`gpu-03` 컨테이너 재생성:

```bash
sudo docker compose up -d --force-recreate gpu-03
```

상태 확인:

```bash
sudo docker compose ps gpu-03
sudo docker logs --tail 100 gpu-03
sudo docker port gpu-03 22
```

정상 포트:

```text
0.0.0.0:10003
[::]:10003
```

Docker mount 확인:

```bash
sudo docker inspect gpu-03 \
  --format '{{range .Mounts}}{{println .Source "->" .Destination}}{{end}}'
```

정상 출력에 다음 항목이 있어야 한다.

```text
/srv/docker-storage/nas/gpu03/home -> /home/gpu03user/nas-home
```

---

## 12. 컨테이너 내부 검증

컨테이너에서 NAS 경로 확인:

```bash
sudo docker exec -u 1203:1203 gpu-03 sh -lc '
id
ls -la /home/gpu03user/nas-home
findmnt -T /home/gpu03user/nas-home || true
'
```

읽기·쓰기·삭제 테스트:

```bash
sudo docker exec -u 1203:1203 gpu-03 sh -lc '
set -e

TEST_FILE=/home/gpu03user/nas-home/.docker-nas-test

printf "Docker NAS test\n" > "$TEST_FILE"
cat "$TEST_FILE"
ls -ln "$TEST_FILE"
rm "$TEST_FILE"

echo "GPU-03 Docker NAS test: OK"
'
```

정상 완료 문구:

```text
GPU-03 Docker NAS test: OK
```

컨테이너 내부에서는 다음 경로가 NAS 개인 홈이다.

```text
/home/gpu03user/nas-home
```

기존 로컬 홈은 계속 다음 경로다.

```text
/home/gpu03user
```

---

## 13. 기존 프로젝트를 NAS로 복사

기존 프로젝트는 자동으로 NAS로 이동하지 않는다.

예:

```text
/home/gpu03user/week1_project
```

이 경로는 컨테이너 로컬 홈이다.

NAS로 1회 복사:

```bash
sudo docker exec -u 1203:1203 gpu-03 sh -lc '
rsync -a --info=progress2 \
  /home/gpu03user/week1_project/ \
  /home/gpu03user/nas-home/week1_project/
'
```

이후 두 경로는 서로 자동 동기화되지 않는다.

로컬 원본:

```text
/home/gpu03user/week1_project
```

NAS 복사본:

```text
/home/gpu03user/nas-home/week1_project
```

실시간으로 NAS를 작업 경로로 쓰려면 NAS 경로에서 직접 작업한다.

```bash
cd /home/gpu03user/nas-home
```

성능을 고려한 권장 방식:

```text
로컬 SSD:
  소스 코드
  Git 저장소
  conda/venv
  빌드 결과
  캐시
  작은 파일이 많은 작업

NAS:
  데이터셋
  모델 체크포인트
  결과물
  백업
  공유 파일
  대용량 순차 읽기/쓰기 파일
```

---

## 14. 재부팅 후 자동 마운트

수동 `mount` 명령은 호스트 재부팅 후 유지되지 않는다.

자동 마운트를 위해 `/etc/fstab`에 등록한다.

백업:

```bash
sudo cp -a /etc/fstab "/etc/fstab.backup.$(date +%Y%m%d-%H%M%S)"
```

추가할 항목:

```fstab
//10.108.90.22/home /srv/docker-storage/nas/gpu03/home cifs credentials=/root/.nas-gpu03-credentials,vers=3.0,iocharset=utf8,noserverino,uid=1203,gid=1203,file_mode=0664,dir_mode=0775,_netdev,nofail,x-systemd.automount 0 0
```

옵션 설명:

```text
_netdev
  네트워크 파일시스템임을 표시한다.

nofail
  NAS 연결 실패 때문에 호스트 부팅 전체가 실패하지 않도록 한다.

x-systemd.automount
  실제 접근 시 자동 마운트한다.
  부팅 시 NAS 응답 지연의 영향을 줄인다.
```

적용 전 문법 및 마운트 테스트:

```bash
sudo umount /srv/docker-storage/nas/gpu03/home 2>/dev/null || true
sudo mount -a
findmnt -T /srv/docker-storage/nas/gpu03/home
```

Docker가 NAS 마운트보다 먼저 시작하면 빈 로컬 디렉터리가 컨테이너에 연결될 수 있다.

운영 시에는 Docker 컨테이너 시작 전에 NAS 마운트가 정상인지 확인한다.

```bash
mountpoint -q /srv/docker-storage/nas/gpu03/home
```

---

## 15. 사용자별 확장

현재 Compose UID/GID:

```text
gpu-00: UID 1200, GID 1200
gpu-01: UID 1201, GID 1201
gpu-02: UID 1202, GID 1202
gpu-03: UID 1203, GID 1203
```

사용자별 구성표:

| 서비스 | NAS credential | 호스트 마운트 | 컨테이너 대상 | UID:GID |
|---|---|---|---|---|
| gpu-00 | `/root/.nas-gpu00-credentials` | `/srv/docker-storage/nas/gpu00/home` | `/home/gpu00user/nas-home` | `1200:1200` |
| gpu-01 | `/root/.nas-gpu01-credentials` | `/srv/docker-storage/nas/gpu01/home` | `/home/gpu01user/nas-home` | `1201:1201` |
| gpu-02 | `/root/.nas-gpu02-credentials` | `/srv/docker-storage/nas/gpu02/home` | `/home/gpu02user/nas-home` | `1202:1202` |
| gpu-03 | `/root/.nas-gpu03-credentials` | `/srv/docker-storage/nas/gpu03/home` | `/home/gpu03user/nas-home` | `1203:1203` |

`gpu-00` 마운트 예:

```bash
sudo mount -t cifs \
  //10.108.90.22/home \
  /srv/docker-storage/nas/gpu00/home \
  -o "credentials=/root/.nas-gpu00-credentials,vers=3.0,iocharset=utf8,noserverino,uid=1200,gid=1200,file_mode=0664,dir_mode=0775"
```

`gpu-01` 마운트 예:

```bash
sudo mount -t cifs \
  //10.108.90.22/home \
  /srv/docker-storage/nas/gpu01/home \
  -o "credentials=/root/.nas-gpu01-credentials,vers=3.0,iocharset=utf8,noserverino,uid=1201,gid=1201,file_mode=0664,dir_mode=0775"
```

`gpu-02` 마운트 예:

```bash
sudo mount -t cifs \
  //10.108.90.22/home \
  /srv/docker-storage/nas/gpu02/home \
  -o "credentials=/root/.nas-gpu02-credentials,vers=3.0,iocharset=utf8,noserverino,uid=1202,gid=1202,file_mode=0664,dir_mode=0775"
```

`gpu-03` 마운트 예:

```bash
sudo mount -t cifs \
  //10.108.90.22/home \
  /srv/docker-storage/nas/gpu03/home \
  -o "credentials=/root/.nas-gpu03-credentials,vers=3.0,iocharset=utf8,noserverino,uid=1203,gid=1203,file_mode=0664,dir_mode=0775"
```

---

## 16. 컨테이너 재생성 후 SSH 접속 주의사항

Compose 변경 후 다음 명령을 실행하면 컨테이너가 재생성된다.

```bash
sudo docker compose up -d --force-recreate gpu-03
```

컨테이너 내부 `/etc/shadow`에 수동으로 설정한 비밀번호는 재생성 시 초기화될 수 있다.

증상:

```text
Failed password for gpu03user
Permission denied, please try again.
```

컨테이너 상태와 포트가 정상이어도 비밀번호만 실패할 수 있다.

상태 확인:

```bash
sudo docker compose ps gpu-03
sudo docker logs --tail 100 gpu-03
sudo docker port gpu-03 22
```

비밀번호 재설정:

```bash
sudo docker exec -it gpu-03 passwd gpu03user
```

계정 상태:

```bash
sudo docker exec gpu-03 passwd -S gpu03user
```

두 번째 필드:

```text
P   비밀번호 설정됨
L   계정 잠김
NP  비밀번호 없음
```

SSH 접속 포트:

```text
호스트 10003 -> 컨테이너 22
```

Mac의 SSH 설정:

```sshconfig
Host gpu-03-internal
    HostName 10.107.60.152
    User gpu03user
    Port 10003
```

확인:

```bash
ssh -G gpu-03-internal |
  grep -E '^(hostname|user|port) '
```

정상:

```text
hostname 10.107.60.152
user gpu03user
port 10003
```

---

## 17. SSH 공개키 인증 권장

컨테이너 재생성 후에도 접속 정보를 유지하려면 비밀번호보다 공개키 인증을 권장한다.

호스트의 사용자 홈 경로는 bind mount로 유지된다.

```text
/srv/gpu-containers/gpu-03/home
    -> /home/gpu03user
```

Mac 공개키 확인:

```bash
cat ~/.ssh/id_ed25519.pub
```

호스트에 등록:

```bash
PUBLIC_KEY='ssh-ed25519 AAAA...Mac_Public_Key...'

sudo install -d \
  -o 1203 -g 1203 -m 0700 \
  /srv/gpu-containers/gpu-03/home/.ssh

printf '%s\n' "$PUBLIC_KEY" |
sudo tee -a \
  /srv/gpu-containers/gpu-03/home/.ssh/authorized_keys \
  >/dev/null

sudo chown 1203:1203 \
  /srv/gpu-containers/gpu-03/home/.ssh/authorized_keys

sudo chmod 0600 \
  /srv/gpu-containers/gpu-03/home/.ssh/authorized_keys
```

확인:

```bash
sudo docker exec gpu-03 sh -lc '
stat -c "%U:%G %a %n" \
  /home/gpu03user/.ssh \
  /home/gpu03user/.ssh/authorized_keys
'
```

정상:

```text
gpu03user:gpu03user 700 /home/gpu03user/.ssh
gpu03user:gpu03user 600 /home/gpu03user/.ssh/authorized_keys
```

---

## 18. 문제 해결

### 18.1 `Couldn't chdir ... No such file or directory`

원인:

- 호스트 마운트 대상 디렉터리가 없음

해결:

```bash
sudo install -d -o root -g root -m 0755 \
  /srv/docker-storage/nas/gpu03/home
```

### 18.2 `-A: command not found`

원인:

- 줄바꿈용 `\` 뒤에 공백이 있음
- 줄 연결이 끊겨 `-A`가 별도 명령으로 실행됨

해결:

한 줄로 실행한다.

```bash
sudo smbclient -L //10.108.90.22 -A /root/.nas-gpu03-credentials -m SMB3
```

### 18.3 `Connection to http: failed`

원인:

```text
//http://10.108.90.22:5000
```

처럼 잘못 입력함

해결:

```text
//10.108.90.22
```

사용

### 18.4 NAS File Station에 파일이 보이지 않음

확인할 점:

- 현재 보고 있는 경로가 컨테이너 로컬 홈인지 NAS 경로인지 구분
- `/home/gpu03user`는 로컬 홈
- `/home/gpu03user/nas-home`만 NAS
- 기존 프로젝트는 자동으로 NAS로 이동하지 않음

### 18.5 컨테이너 내부에서 쓰기 권한 없음

확인:

```bash
sudo docker exec gpu-03 id gpu03user
findmnt -T /srv/docker-storage/nas/gpu03/home
ls -ldn /srv/docker-storage/nas/gpu03/home
```

`gpu03user`의 UID/GID와 마운트 옵션의 `uid/gid`가 같아야 한다.

현재 기준:

```text
1203:1203
```

### 18.6 컨테이너 재시작 후 NAS가 비어 보임

가능한 원인:

- 호스트 NAS 마운트가 해제됨
- 빈 로컬 디렉터리가 컨테이너에 bind mount 됨

확인:

```bash
mountpoint -q /srv/docker-storage/nas/gpu03/home
findmnt -T /srv/docker-storage/nas/gpu03/home
```

마운트가 없으면 Docker 컨테이너를 내린 후 NAS를 다시 마운트하고 컨테이너를 시작한다.

```bash
cd ~/gpu-stack

sudo docker compose stop gpu-03

sudo mount -a

mountpoint -q /srv/docker-storage/nas/gpu03/home

sudo docker compose up -d gpu-03
```

### 18.7 SSH는 도달하지만 비밀번호가 거부됨

로그:

```text
Failed password for gpu03user
```

해결:

```bash
sudo docker exec -it gpu-03 passwd gpu03user
```

포트 설정 확인:

```bash
ssh -G gpu-03-internal |
  grep -E '^(hostname|user|port) '
```

`port 10003`이어야 한다.

---

## 19. 운영 점검 명령

호스트 NAS 마운트:

```bash
findmnt -T /srv/docker-storage/nas/gpu03/home
```

호스트 읽기/쓰기:

```bash
touch /srv/docker-storage/nas/gpu03/home/.healthcheck
rm /srv/docker-storage/nas/gpu03/home/.healthcheck
```

Docker mount:

```bash
sudo docker inspect gpu-03 \
  --format '{{range .Mounts}}{{println .Source "->" .Destination}}{{end}}'
```

컨테이너 읽기/쓰기:

```bash
sudo docker exec -u 1203:1203 gpu-03 sh -lc '
touch /home/gpu03user/nas-home/.healthcheck
rm /home/gpu03user/nas-home/.healthcheck
'
```

컨테이너 SSH:

```bash
sudo docker compose ps gpu-03
sudo docker logs --tail 50 gpu-03
sudo docker port gpu-03 22
```

---

## 20. 최종 체크리스트

```text
[ ] NAS 445 포트 연결 가능
[ ] cifs-utils 설치
[ ] smbclient 설치
[ ] /root/.nas-gpu03-credentials 생성
[ ] credential 파일 권한 600
[ ] /srv/docker-storage/nas/gpu03/home 생성
[ ] //10.108.90.22/home 마운트
[ ] uid=1203,gid=1203 적용
[ ] 호스트에서 읽기/쓰기/삭제 테스트 성공
[ ] compose.yaml에 NAS bind mount 추가
[ ] target이 /home/gpu03user/nas-home인지 확인
[ ] docker compose config 성공
[ ] gpu-03 재생성
[ ] docker inspect에서 bind mount 확인
[ ] 컨테이너 내부 읽기/쓰기/삭제 테스트 성공
[ ] /etc/fstab 자동 마운트 등록
[ ] 공개키 인증 등록
[ ] SSH Port 10003 설정 확인
```

---

## 21. 핵심 원칙 요약

```text
1. NAS 마운트는 Docker 호스트에서 수행한다.
2. 컨테이너 내부에서 NAS를 직접 mount하지 않는다.
3. NAS credential은 root 전용 파일에 보관한다.
4. 컨테이너 UID/GID와 CIFS uid/gid를 일치시킨다.
5. NAS는 사용자 홈 전체가 아니라 nas-home 하위에 연결한다.
6. 이 구성은 동기화가 아니라 직접 원격 파일시스템 사용이다.
7. 컨테이너 재생성 후 비밀번호가 초기화될 수 있으므로 공개키 인증을 사용한다.
8. 호스트 재부팅 후에는 NAS 자동 마운트 상태를 먼저 확인한다.
9. 작은 파일이 많은 개발 작업은 로컬 SSD를 사용한다.
10. 데이터셋, 결과물, 체크포인트, 백업은 NAS를 사용한다.
```
