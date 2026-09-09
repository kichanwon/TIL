
강의 링크: [bit.ly/kku_c_programming](https://bit.ly/kku_c_programming)
# 0. 개발환경 설정
## 0-1. 자료 정리
수업을 시작하기 전에 내려받은 자료를 한곳에 모아둡니다. 
바탕화면이나 다운로드 폴더처럼 찾기 쉬운 곳에 `c_programming`이라는 폴더를 만들고, 수업 자료를 모두 그 안에 넣어요.
```plaintext
c_programming
├── games
└── 강의 PDF 파일
```

> [!important] 압축은 `c_programming` 폴더 안에서 풀기
> `games.zip`을 아무 곳에서나 풀지 말고, 방금 만든 `c_programming` 폴더 안에서 풀어요.  
> 압축을 제대로 풀면 `c_programming` 폴더 안에 `games` 폴더가 새로 생깁니다.

압축을 푼 뒤에는 아래처럼 보이면 됩니다.

```plaintext
c_programming
├── games
└── 강의 PDF 파일
```

> [!warning] 헷갈리기 쉬운 부분
> VS Code에서 압축을 풀라고 할 때 위치를 묻는 화면이 나오면 `c_programming` 폴더를 선택합니다.  
> `games.zip`을  다른 폴더 안에 풀거나, 다운로드 폴더 바깥의 다른 위치에 풀면 수업 중에 폴더를 찾기 어려울 수 있어요.

## 0-2. VS Code에서 Qwen code 설치하기

### VS Code 안에서 터미널 열기
VS Code 안에서 터미널을 열어요. Windows 실습에서는 VS Code 아래쪽에 열리는 `cmd` 터미널을 기준으로 진행합니다.
```text
Terminal → New Terminal
```
또는 VS Code 화면 아래쪽의 터미널 버튼을 눌러도 돼요.
VS Code 터미널 오른쪽 위의 아래쪽 화살표를 누릅니다.
#### Window
```Windows
터미널 오른쪽 위 화살표
→ Select Default Profile
→ Command Prompt
```
#### Mac
```Mac
bash / zsh 상관없이 가능
```

이번 수업에서는 터미널에 VS Code 안에서 `Qwen code` 을 설치해 사용합니다. 이렇게 하면 파일 목록, Qwen code 대화창, 게임 실행 터미널을 한 화면에서 볼 수 있어요.

```plaintext
1. pwd 명령어를 통해 현재 위치가 /c_programming으로 되어 있는지 확인하기
   - 만약 아니라면, cd/c_programming 으로 폴더 이동
     
2. ls 명령어를 통해 아래 항목들이 보이는지 확인하기
		games
		강의자료.pdf
```

## macOS에서 Qwen CLI 설치

```bash
bash -c "$(curl -fsSL https://qwen-code-assets.oss-cn-hangzhou.aliyuncs.com/installation/install-qwen.sh)" -s --source qwenchat
```
## Windows cmd에서 Qwen code 설치
VS Code에서 `Terminal → New Terminal`을 열고, 터미널이 cmd인지 확인한 뒤 아래 명령어를 입력합니다.

```cmd
curl -fsSL -o %TEMP%\install-qwen.bat https://qwen-code-assets.oss-cn-hangzhou.aliyuncs.com/installation/install-qwen.bat && %TEMP%\install-qwen.bat --source qwenchat
```

## 버전 확인

설치가 끝나면 버전을 확인합니다.

```cmd
qwen --version
```
정상적으로 버전이 나오면 설치 완료입니다.
Qwen code를 실행합니다.

> [!warning] Windows에서 `qwen` 실행이 안 되는 경우
> VS Code를 완전히 종료한 뒤 다시 실행하고, New Terminal을 새로 열어 다시 확인합니다.

## 0-3. Qwen code에서 API Key 입력하기

Qwen CLI를 처음 실행하면 provider, endpoint, key, model 정보등을 입력하라는 화면이 나옵니다.  

이번 수업에서는 아래 값을 사용합니다.
```text
Connect a Provider: Custom Provider

Custom Provider: OpenAI-compatible

API Endpoint: http://222.116.135.70/v1 (개인 pc 사용)
              http://10.108.90.20/v1 (학교 컴퓨터 사용)

API Key: 117d353dd5ed9f329672c1a58ef5ce58627a5af7141829b9a5145cdfea9effbc

Model IDs: qwen3.6-27b

Advanced Config:
	Enable thinking: off
	Enable modality: off
	Context window: auto
```

> [!note] 동작 확인
> `안녕` 이라고 간단히 입력하여 잘 동작하는 지 확인해보아요.

> [!warning] CLI가 익숙하지 않은 경우
> **WebUI**:
> 	[http://222.116.135.70:3002](http://222.116.135.70:3002/)
> 	[http://10.108.90.20:3002](http://10.108.90.20:3002)



## 0-4. 문제가 생겼을 때 먼저 확인할 것  

> [!check] 빠른 점검표
> - `c_programming` 폴더 안에 `games.zip`, `games`, 강의 PDF가 함께 있나요?
> - VS Code에서 `games`만 따로 연 것이 아니라 `c_programming` 폴더를 열었나요?
> - VS Code 안의 cmd 터미널에서 명령어를 입력하고 있나요?
> - VS Code에서 Qwen code을 설치했나요?
> - Qwen code 화면에서 설정을 잘 입력했나요?
> - Qwen code 대화창과 VS Code 터미널이 같은 `c_programming` 폴더에서 실행되나요?
> - 오류 메시지를 일부만 말하지 않고 그대로 Qwen code에게 보여줬나요?

# 1. 오늘의 큰 흐름

> [!abstract] 핵심 흐름
> `c_programming` 폴더 만들기 → 수업 자료를 `c_programming` 안에 모으기 → `c_programming` 안에서 `games.zip` 압축 풀기 → VS Code에서 `c_programming` 폴더 열기 → VS Code 터미널 열기 → Qwen code 열기 →  실행하기 → 개선점 말하기 → Qwen code와 함께 게임 발전시키기

수업 중에는 아래 순서대로 진행해요.

---

# 2. VS Code에서 `C_Programming` 폴더 열기

VS Code를 실행하고 아래 메뉴를 선택해요.

```text
File → Open Folder...
```

여기서 프로젝트 하나만 바로 여는 것이 아니라, 먼저 `C_Programming` 폴더 전체를 열어요.

폴더를 제대로 열었다면 왼쪽 파일 목록에 `games`, 강의 PDF가 함께 보일 거예요.

---

# 3. 프로젝트 구조 & 실행

## 3-1. 폴더 구조

> [!important] 목적
> `student-template`은 C 언어의 구조체를 사용하여 플레이어, 몬스터, 맵을 표현하는 기초 텍스트 RPG예요.

```text
games/
├── Makefile
├── include/
│   ├── player.h
│   ├── monster.h
│   ├── map.h
│   ├── battle.h
│   └── game.h
└── src/
    ├── main.c
    ├── player.c
    ├── monster.c
    ├── map.c
    ├── battle.c
    └── game.c
```
`include` 폴더에는 구조체와 함수 선언이 들어 있어요.

```c
// include/game.h
void runGame(void);
```
위 코드는 "`runGame`이라는 함수가 있다"는 약속을 다른 파일에 알려주는 역할을 해요.

`src` 폴더에는 실제 함수 구현이 들어 있어요.

```c
// src/main.c
#include "game.h"

int main(void) {
    runGame();

    return 0;
}
```
`main.c`는 프로그램의 시작점이에요. 여기서는 복잡한 일을 직접 하지 않고, `runGame()`을 호출해서 게임 전체 실행을 `game.c`에 맡겨요.

## 3-2. 코드 실행 흐름

게임의 큰 흐름은 `src/game.c`의 `runGame()` 함수에서 시작돼요. 초기화 부분만 보면 다음과 같아요.

```c
void runGame(void) {
    Player player = createPlayer();
    Room rooms[ROOM_COUNT];

    srand((unsigned int)time(NULL));

    createMap(rooms);
    printTitle();
    handleRoomEvent(rooms, &player);
}
```

간단히 보면 다음 순서예요.
```text
플레이어 생성
방 배열 생성
랜덤 기능 준비
맵 생성
제목 출력
현재 방 이벤트 처리
```

`createPlayer()`는 `src/player.c`에 구현되어 있어요.

```c
Player createPlayer(void) {
    Player player = {
        30,
        30,
        5,
        0,
        1,
        0
    };

    return player;
}
```

이 코드는 플레이어의 초기 체력, 최대 체력, 공격력, 현재 방 번호, 포션 개수, 클리어 여부를 한 번에 정해요. 이런 값들을 하나로 묶기 위해 `Player` 구조체를 사용해요.

`createMap()`은 `src/map.c`에 구현되어 있어요. 방 배열의 일부만 보면 다음과 같아요.

```c
Room defaultRooms[ROOM_COUNT] = {
    {0, "마을 입구", ROOM_START, "모험을 시작하는 안전한 방입니다.", {1, 2, -1, -1}, 2, 0},
    {1, "숲길", ROOM_NORMAL, "조용한 숲길입니다.", {0, 3, 4, -1}, 3, 0},
    // 나머지 방도 같은 방식으로 이어집니다.
};
```

방 하나에는 방 번호, 방 이름, 방 종류, 설명, 이동 가능한 방 번호, 이동 가능한 방 개수, 아이템 여부가 들어 있어요. 그래서 맵도 `Room` 구조체 배열로 표현할 수 있어요.

실제 게임 반복은 메뉴 입력으로 진행돼요.

```c
if (choice == ACTION_MOVE) {
    chooseNextRoom(rooms, &player);
} else if (choice == ACTION_STATUS) {
    printPlayer(&player);
} else if (choice == ACTION_INVENTORY) {
    printInventory(&player);
} else if (choice == ACTION_POTION) {
    usePotion(&player);
}
```
사용자가 숫자를 입력하면 이동하기, 상태 보기, 인벤토리 보기, 포션 사용 중 하나가 실행돼요. 이때 플레이어 정보를 바꾸어야 하므로 `Player *player`처럼 주소를 전달해요.

## 3-3. 빌드 방법
> [!tip] Make 사용법
> 실행 파일을 만들 때는 `make`를 사용할 거예요.

```bash
make
```

`Makefile`에는 어떤 `.c` 파일들을 함께 컴파일할지 적혀 있어요.
```makefile
SRC = \
	src/main.c \
	src/player.c \
	src/monster.c \
	src/map.c \
	src/battle.c \
	src/game.c
```

`make` 명령을 실행하면 위 파일들이 함께 컴파일되고 `game` 실행 파일이 만들어져요.

```bash
./game
```
`./game` 명령어로 텍스트 RPG를 실행할 수 있어요.

---
# 4. 구조체
구조체는 여러 개의 값을 하나로 묶어서 새로운 자료형처럼 사용할 수 있게 해주는 C 언어 문법입니다.

예를 들어 플레이어를 표현하려면 체력, 최대 체력, 공격력, 현재 위치, 포션 개수 같은 값이 필요합니다.

```c
int hp;
int maxHp;
int attack;
int currentRoom;
int potion;
```

이 값들을 따로따로 관리하면 함수에 넘겨야 할 값이 많아지고, 어떤 값들이 한 플레이어에 속하는지 코드만 보고 바로 알기 어렵습니다.

구조체를 사용하면 관련 있는 값들을 하나의 묶음으로 만들 수 있습니다.

```c
typedef struct {
    int hp;
    int maxHp;
    int attack;
    int currentRoom;
    int potion;
    int hasWon;
} Player;
```

여기서 `Player`는 플레이어 정보를 담는 새로운 자료형 이름입니다.
`Player player;`처럼 변수를 만들면, `player` 하나 안에 체력, 공격력, 현재 방 번호 같은 정보가 함께 들어갑니다.

구조체 안의 값은 `.`을 사용해서 접근합니다.

```c
Player player = createPlayer();

printf("체력: %d\n", player.hp);
player.currentRoom = 3;
```

함수에서 구조체 값을 직접 바꾸고 싶을 때는 주소를 넘겨서 포인터로 받습니다.
이때는 `.` 대신 `->`를 사용합니다.

```c
void usePotion(Player *player) {
    player->hp += 10;
    player->potion--;
}
```

`player->hp`는 `(*player).hp`와 같은 뜻입니다.
포인터를 사용하면 함수 안에서 바꾼 값이 원래 `Player` 변수에도 반영됩니다.

이 프로젝트에서는 구조체가 게임의 핵심 데이터를 표현합니다.

- `Player`: 플레이어의 체력, 공격력, 현재 위치, 포션 개수
- `Monster`: 몬스터의 이름, 체력, 공격력
- `Room`: 방 번호, 방 종류, 이동 가능한 다음 방 정보

> [!important] 
> 즉, 구조체는 단순히 문법을 외우기 위한 내용이 아니라, 관련 있는 데이터를 하나의 의미 있는 단위로 표현하는 방법입니다.
> 학생, 상품, 좌표, 플레이어처럼 여러 정보를 함께 가져야 하는 대상을 만들 때 구조체를 사용합니다.

---

# 5. Qwen code 열기

## 5-1. Qwen code 대화 시작하기

VS Code에서 Qwen code를 설치하고 API Key 입력까지 끝났다면, 이제 터미널에서 `qwen`명령어를 입력하여  대화창을 열어요.


> [!important] 
> Qwen code 대화와 게임 실행은 각각 다른 VS Code의 터미널에서 진행합니다.  

---


# 6. 게임을  개선하기

지금 들어 있는 게임들은 일부러 아주 간단하게 만들어져 있어요. 그래서 실행해보면 기능이 적고 게임에 부족한 점이 보일 수 있어요.

> [!summary] 이번 단계의 목표
> 새 게임을 처음부터 만드는 것이 아니라, **이미 있는 게임을 Qwen을 활용하여 더 재밌게 개발하는 것**이에요.


## 6-1. Qwen code를 대하는 방식

Qwen code는 정답을 대신 제출해주는 도구라기보다는, 옆에서 같이 코드를 읽고 실험을 도와주는 조교에 가까워요.

> [!example] 좋은 사용 방식
> - 먼저 파일 구조를 설명해달라고 요청해요.
> - 실행 방법을 물어본 뒤 직접 실행해봐요.
> - 오류가 나면 오류 메시지를 그대로 보여주고 원인을 물어봐요.
> - 코드를 고치기 전에는 “아직 수정하지 마”라고 말해 분석만 시켜요.
> - 수정이 필요할 때는 “어느 파일을 어떻게 바꿀지 먼저 설명해줘”라고 요청해요.

> [!warning] 처음부터 완성 요청 금지
> 처음부터 “게임을 완성해줘”라고 요청하면 내가 무엇을 배우고 있는지 놓치기 쉬워요.  
> 작은 질문으로 나누어 요청하는 것이 수업에 더 잘 맞아요.


## 6-2. `@`로 구조체 구현 파일 언급하기

프로젝트를 말할 때는 이름을 직접 입력해도 되고, `@`를 이용해서 폴더나 파일을 언급해도 돼요.

> [!tip] `@` 사용법
> `@`를 입력하면 아래쪽에 관련 파일이나 폴더 후보가 뜰 수 있어요.  
> 원하는 프로젝트 폴더가 보이면 방향키로 고르거나 `Tab`을 눌러 선택하면 됩니다.

후보가 안 나와도 괜찮아요. 프로젝트 이름을 텍스트로 끝까지 입력하고, Qwen code에게 찾아달라고 하면 됩니다.

## 6-3. 프롬프트에는 context를 같이 주기

Qwen code에게 요청할 때는 짧게 한 문장만 말하는 것보다, 내가 어떤 상황에서 무엇을 원하는지 같이 설명해주는 것이 좋아요. 이런 배경 설명을 **context**라고 생각하면 됩니다.

> [!important] context가 많을수록 더 잘 알아들어요
> Qwen code는 여러분이 보고 있는 화면, 방금 실행한 게임, 마음에 들었던 이미지, 참고하고 싶은 URL을 자동으로 모두 아는 것이 아니에요.  
> 그래서 원하는 결과가 있다면 그 정보를 같이 알려주는 것이 중요해요.

좋은 프롬프트에는 보통 이런 내용이 들어가면 좋아요.

```plaintext
지금 어떤 게임을 수정하고 있는지
어떤 파일이나 이미지를 참고하면 되는지
무엇이 마음에 들고 무엇이 아쉬운지
원하는 결과가 어떤 모습인지
참고할 인터넷 URL이나 예시가 있는지
아직 수정하지 말고 설명만 원하는지, 바로 구현해도 되는지
```

> [!tip] URL도 context가 될 수 있어요
> 인터넷 주소, 이미지 파일, 오류 메시지, 실행 결과, 마음에 드는 게임 영상 설명도 모두 context가 될 수 있어요.  
> Qwen code에게 “이걸 참고해서”라고 말하면 훨씬 구체적으로 도와줄 수 있습니다.

---

## 7. 개선 주제

학생들에게는 완성된 코드를 주지 않고, `student-template`에서 직접 기능을 하나씩 추가해보게 합니다.
아래 내용은 정답 코드가 아니라, Qwen code에게 입력할 수 있는 프롬프트 예시입니다.

> [!tip] Qwen code에게 요청할 때
> “이 코드를 그대로 붙여줘”보다 “`@파일명`에서 어떤 함수나 구조체를 어떤 기능을 하도록 바꿔줘”처럼 요청하는 것이 좋습니다.

```plaintext
student-template의 현재 구조를 유지하면서 아래 기능 중 하나를 추가하고 싶어.
먼저 어떤 파일의 어떤 함수나 구조체가 바뀌는지 설명한 뒤, 필요한 코드만 수정해줘.
```


## 7-1. 지도 보기 기능

지도 기능은 현재 방과 각 방의 연결 정보를 출력하는 기능입니다.
처음에는 그래픽 지도보다, 방 번호와 이동 가능한 방을 표처럼 보여주는 방식이 구현하기 쉽습니다.

```plaintext
@player.h 에서 메뉴 선택 상수에 ACTION_MAP을 추가하고, 기존 메뉴 번호와 겹치지 않도록 정리해줘.
@map.h 에 printSimpleMap 함수 선언을 추가해줘.
@map.c 에 printSimpleMap 함수를 만들어서 현재 방은 *로 표시하고, 각 방의 이동 가능한 방 번호를 출력하도록 바꿔줘.
@game.c 의 printMenu 함수와 runGame 함수에서 플레이어가 지도 보기를 선택하면 printSimpleMap이 실행되도록 바꿔줘.
뒤로가기 기능도 같이 추가되어 있다면 ACTION_BACK과 ACTION_MAP의 번호가 겹치지 않도록 메뉴 번호를 함께 정리해줘.
수정하기 전에 어떤 파일의 어떤 함수와 상수가 바뀌는지 먼저 설명해줘.
```

> [!note] 요청할 때 넣으면 좋은 context
> 새 지도 보기 메뉴를 번호로 넣을 지, `m`과 같은 단축어로 입력받을 지도 함께 말해주면 좋습니다.

## 7-2. 방문한 방 표시하기

한 번 방문한 방에서는 같은 이벤트가 반복되지 않게 하려면 `Room` 구조체에 방문 여부를 저장합니다.

```plaintext
@include/map.h 에서 Room 구조체에 방문 여부를 저장하는 visited 필드를 추가해줘.
@src/map.c 에서 방 목록을 초기화하는 부분이 있다면 모든 방의 visited가 처음에는 0으로 시작하도록 맞춰줘.
@src/map.c 의 handleRoomEvent 함수가 이미 방문한 방이면 같은 이벤트를 반복하지 않고 안내 메시지만 출력하도록 바꿔줘.
@src/map.c 의 handleRoomEvent 함수에서 방 이벤트 처리가 끝나면 visited 값을 1로 바꾸도록 수정해줘.
가능하면 지도 보기 기능과 연결해서, 방문한 방에는 "방문함" 표시가 나오도록 printSimpleMap 함수도 같이 바꿔줘.
수정하기 전에 Room 구조체와 handleRoomEvent 함수가 어떻게 연결되는지 설명해줘.
```

이 기능을 지도 보기와 연결하면, 방문한 방에는 `방문함` 같은 표시를 붙일 수도 있습니다.

## 7-3. 전투 선택지 만들기

현재 전투가 자동으로 진행된다면, 전투 중 선택지를 넣어 조건문과 반복문을 더 분명하게 연습할 수 있습니다.

```plaintext
@src/battle.c 의 battle 함수에서 플레이어가 매 턴 공격, 포션 사용, 도망가기 중 하나를 선택할 수 있도록 바꿔줘.
@src/battle.c 의 battle 함수에서 잘못된 입력이나 숫자가 아닌 입력이 들어오면 다시 선택하게 해줘.
@src/battle.c 의 battle 함수에서 공격을 선택했을 때만 기존 플레이어 공격 코드가 실행되도록 흐름을 정리해줘.
@src/battle.c 의 battle 함수에서 포션 사용을 선택하면 기존 usePotion 함수를 호출하도록 연결해줘.
@src/battle.c 의 battle 함수에서 도망가기를 선택하면 전투를 종료하고 도망 결과를 호출한 쪽에서 알 수 있도록 반환값을 정리해줘.
수정하기 전에 battle 함수의 현재 반복 구조와 몬스터 반격이 어느 시점에 실행되는지 먼저 설명해줘.
```

> [!note] 요청할 때 넣으면 좋은 context
> 포션을 사용한 턴에도 몬스터가 공격할지, 포션을 사용하면 그 턴은 안전하게 넘길지 규칙을 정해서 같이 알려주면 좋습니다.

## 7-4. 골드와 보상 시스템

몬스터를 처치했을 때 골드를 얻도록 만들려면 플레이어와 몬스터가 각각 골드 정보를 가져야 합니다.

```plaintext
@include/player.h 의 Player 구조체에 플레이어가 가진 골드를 저장하는 gold 필드를 추가해줘.
@src/player.c 의 createPlayer 함수에서 gold가 0으로 시작하도록 초기화해줘.
@include/monster.h 의 Monster 구조체에 몬스터 처치 보상 골드를 저장하는 rewardGold 필드를 추가해줘.
@src/monster.c 에서 몬스터를 생성하거나 초기화하는 함수들이 rewardGold 값을 함께 설정하도록 수정해줘.
@src/battle.c 의 battle 함수에서 몬스터를 처치하면 player의 gold가 monster의 rewardGold만큼 증가하도록 바꿔줘.
@src/player.c 의 printPlayer 함수와 printInventory 함수에서 현재 gold를 확인할 수 있도록 출력 내용을 추가해줘.
이미 previousRoom 같은 다른 필드가 추가되어 있다면 지우지 말고, 기존 필드를 유지한 채 gold만 추가해줘.
수정하기 전에 Player 구조체, Monster 구조체, battle 함수, 출력 함수가 어떤 역할로 연결되는지 먼저 설명해줘.
```

## 7-5. 상자 보상 다양화

상자 방에서 항상 같은 보상만 나오면 게임이 단조로울 수 있습니다. 랜덤 보상을 넣으면 같은 상자 방이라도 결과가 달라집니다.

```plaintext
@src/battle.c 에 handleChestRoom 함수가 있다면, handleChestRoom 함수가 포션만 주는 대신 여러 보상 중 하나를 랜덤으로 주도록 바꿔줘.
만약 handleChestRoom 함수가 다른 파일에 있다면, 실제로 정의된 파일을 먼저 알려주고 그 파일의 handleChestRoom 함수를 수정해줘.
handleChestRoom 함수에서 보상 종류는 포션 획득, 공격력 증가, 동전 발견처럼 서로 다른 결과가 나오도록 해줘.
골드 시스템이 이미 구현되어 있다면 동전 발견 보상은 player의 gold가 증가하도록 연결해줘.
상자를 한 번 열고 나면 같은 상자에서 보상을 다시 받을 수 없도록 기존 hasItem 처리 흐름을 유지해줘.
수정하기 전에 handleChestRoom 함수가 어느 파일에 있고, Room 구조체의 hasItem 필드와 어떻게 연결되는지 먼저 설명해줘.
```

## 7-6. 방 종류 안내 출력하기

게임을 처음 실행했을 때 방 기호의 의미를 알려주면 학생들이 맵을 이해하기 쉽습니다.

```plaintext
@src/game.c 에 printRoomGuide 함수를 추가해서 S, ., M, C, ?, B 기호가 각각 어떤 방인지 출력하도록 바꿔줘.
@src/game.c 의 runGame 함수에서 printTitle 다음에 printRoomGuide가 실행되도록 수정해줘.
printRoomGuide 함수는 게임 시작 안내에만 사용되도록 만들고, 매 턴 반복 출력되지 않게 해줘.
수정하기 전에 game.c 안에서 printTitle 함수와 runGame 함수가 어떤 순서로 실행되는지 먼저 설명해줘.
```

## 7-7. 추천 구현 방식

처음부터 모든 기능을 한 번에 추가하지 말고, 한 기능씩 고른 뒤 아래 순서로 진행합니다.

```plaintext
1. 어떤 구조체에 정보가 더 필요한지 찾기
2. 어떤 함수가 그 정보를 읽거나 바꾸는지 찾기
3. 함수 선언을 헤더 파일에 추가해야 하는지 확인하기
4. 코드 수정 후 make로 빌드하기
5. ./game으로 직접 실행해서 동작 확인하기
```

기능을 추가할 때마다 구조체가 왜 필요한지, 포인터를 왜 사용하는지 같이 확인하는 것이 이 수업의 핵심입니다.
각자 원하는 기능을 구현해보세요! (e.g. 턴제 전투, 전투 선택지, 상자 보상 다양화, 뒤로 이동하기)
