| 구분             | 종류           | 역할                     |
| -------------- | ------------ | ---------------------- |
| `make`         | 빌드 자동화 프로그램  | Makefile을 읽고 명령어 실행    |
| `Makefile`     | 빌드 설정 텍스트 파일 | 실행할 명령어와 의존성 규칙 정의     |
| `gcc`, `clang` | C 컴파일러       | C 소스코드를 컴파일하여 실행 파일 생성 |
| `run.c`        | C 소스 파일      | 개발자가 작성한 C 프로그램        |
| `run`          | 실행 파일        | 컴파일된 프로그램              |

---
```makefile
CC = gcc

.PHONY: run
run: run.c
	$(CC) -O3 -o run run.c -lm
	$(CC) -O3 -o runq runq.c -lm

rundebug: run.c
	$(CC) -g -o run run.c -lm
	$(CC) -g -o runq runq.c -lm

.PHONY: runfast
runfast: run.c
	$(CC) -Ofast -o run run.c -lm
	$(CC) -Ofast -o runq runq.c -lm

.PHONY: runomp
runomp: run.c
	$(CC) -Ofast -fopenmp -march=native run.c  -lm  -o run
	$(CC) -Ofast -fopenmp -march=native runq.c  -lm  -o runq

.PHONY: win64
win64:
	x86_64-w64-mingw32-gcc -Ofast -D_WIN32 -o run.exe -I. run.c win.c
	x86_64-w64-mingw32-gcc -Ofast -D_WIN32 -o runq.exe -I. runq.c win.c

.PHONY: rungnu
rungnu:
	$(CC) -Ofast -std=gnu11 -o run run.c -lm
	$(CC) -Ofast -std=gnu11 -o runq runq.c -lm

.PHONY: runompgnu
runompgnu:
	$(CC) -Ofast -fopenmp -std=gnu11 run.c  -lm  -o run
	$(CC) -Ofast -fopenmp -std=gnu11 runq.c  -lm  -o runq

.PHONY: test
test:
	pytest

.PHONY: testc
testc:
	pytest -k runc

VERBOSITY ?= 0
.PHONY: testcc
testcc:
	$(CC) -DVERBOSITY=$(VERBOSITY) -O3 -o testc test.c -lm
	./testc

.PHONY: clean
clean:
	rm -f run
	rm -f runq
```

## 1. 컴파일러 설정
```makefile
CC = gcc
```
- `CC`: C 컴파일러를 지정하는 변수
- `gcc`: GNU C 컴파일러

## 2. 기본 빌드
```makefile
.PHONY: run
run: run.c
	$(CC) -O3 -o run run.c -lm
	$(CC) -O3 -o runq runq.c -lm
```
- `.PHONY`: 파일 존재 여부와 관계없이 항상 실행하는 작업으로 선언
- `run: run.c`: Target(`run`)과 Dependency(`run.c`) 정의
- `$(CC)`: 지정된 컴파일러 사용
- `-O3`: 높은 수준의 컴파일 최적화
- `-o`: 생성할 실행 파일 이름 지정
- `-lm`: 수학 라이브러리 링크
_실행:_ `make run` → `run`, `runq` 실행 파일 생성

## 3. 주요 빌드 옵션

|명령어|주요 옵션|목적|
|---|---|---|
|`make run`|`-O3`|기본 최적화 빌드|
|`make rundebug`|`-g`|디버깅 정보 포함|
|`make runfast`|`-Ofast`|공격적인 성능 최적화|
|`make runomp`|`-fopenmp`|OpenMP 병렬 처리|
|`make clean`|`rm -f`|생성된 실행 파일 삭제|
### 핵심 컴파일 옵션
- `-O3`: 높은 수준의 성능 최적화 
- `-Ofast`: 일부 부동소수점 연산 규칙을 완화하는 추가 최적화
- `-g`: 디버깅 정보 생성
- `-fopenmp`: OpenMP 병렬 처리 활성화
- `-march=native`: 현재 CPU에 맞춘 명령어 최적화

## 4. 실행 과정
```text
make run
   ↓
Makefile의 run 작업 실행
   ↓
gcc로 run.c, runq.c 컴파일
   ↓
run, runq 실행 파일 생성
   ↓
./run stories15M.bin
   ↓
Transformer 추론 실행
```

