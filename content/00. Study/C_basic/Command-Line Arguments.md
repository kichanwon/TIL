# Command-Line Arguments in C

## 1. Overview
- **Command-Line Arguments**: 프로그램 실행 시 터미널에서 전달하는 입력값
	- **`argc`**: 전달된 인자의 개수 (프로그램 이름 포함)
	- **`argv`**: 전달된 인자들을 문자열로 저장하는 배열
- **목적**: 소스코드를 수정하지 않고 실행 시 프로그램의 동작을 설정

## 2. argc & argv
```c
int main(int argc, char *argv[]) {
    return 0;
}
```

| 구문           | 의미                          |
| ------------ | --------------------------- |
| `argc`       | Argument Count, 인자 개수       |
| `argv`       | Argument Vector, 문자열 포인터 배열 |
| `argv[0]`    | 실행 프로그램 이름 또는 경로            |
| `argv[i]`    | i번째 인자 문자열                  |
| `argv[i][j]` | i번째 문자열의 j번째 문자             |

### 입력 예시
```bash
./run stories15M.bin -n 128 -t 0.8
```

| Index     | Value              |
| --------- | ------------------ |
| `argv[0]` | `"./run"`          |
| `argv[1]` | `"stories15M.bin"` |
| `argv[2]` | `"-n"`             |
| `argv[3]` | `"128"`            |
| `argv[4]` | `"-t"`             |
| `argv[5]` | `"0.8"`            |
- `argc = 6`
- 모든 `argv` 원소는 문자열이며, 숫자도 문자열로 전달됨
- `argv[argc]`는 `NULL`

## 3. 문자열 배열과 포인터
```c
char *argv[];
```
- `char`: 문자 자료형
- `char *`: 문자의 메모리 주소를 저장하는 포인터
- `char *argv[]`: 문자열의 시작 주소들을 저장하는 배열
- `char *argv[]`와 `char **argv`는 `main()`의 매개변수 선언에서 동일하게 사용 가능

예시:
```c
argv[2]       // "-n"
argv[2][0]    // '-'
argv[2][1]    // 'n'
```
- `argv[2]`: 세 번째 인자 문자열
- `argv[2][0]`: 해당 문자열의 첫 번째 문자
- `argv[2][1]`: 해당 문자열의 두 번째 문자

## 4. 인자 순회
```c
for (int i = 2; i < argc; i += 2) {
    printf("%s: %s\n", argv[i], argv[i + 1]);
}
```
- `i = 2`: 세 번째 인자부터 순회
- `i < argc`: 인자 배열 범위 검사
- `i += 2`: 옵션 이름과 값을 한 쌍씩 처리
- `argv[i]`: 옵션 이름
- `argv[i + 1]`: 옵션 값

출력:
```text
-n: 128
-t: 0.8
```
위 코드는 옵션과 값이 항상 쌍으로 존재한다고 가정한 예시이다. 실제 구현에서는 `i + 1 < argc` 여부를 먼저 검사해야 한다.

## 5. 문자열 처리 함수
명령행 인자는 문자열이므로, 숫자 변환이나 문자열 비교가 필요하다.

| 함수         | 역할             | 예시                   |
| ---------- | -------------- | -------------------- |
| `strlen()` | 문자열 길이 반환      | `strlen("-n")` → `2` |
| `strcmp()` | 문자열 비교         | 같으면 `0`              |
| `atoi()`   | 문자열 → `int`    | `"128"` → `128`      |
| `atof()`   | 문자열 → `double` | `"0.8"` → `0.8`      |

### 문자열 비교
```c
if (strcmp(argv[2], "-n") == 0) {
    printf("Step option\n");
}
```
- `strcmp()`가 `0`을 반환하면 두 문자열이 동일함
- 문자열의 내용 비교에는 ` == ` 대신 `strcmp()` 사용

### 숫자 변환
```c
int steps = atoi(argv[3]);
float temperature = atof(argv[5]);
```
- `atoi()`: 문자열을 정수로 변환
- `atof()`: 문자열을 실수로 변환
- `atof()`의 반환형은 `double`이며, `float`에 대입하면 변환됨
- 엄격한 입력 검증이 필요하면 `strtol()`, `strtof()` 사용

## 6. 입력값 검사
```c
if (argc < 2) {
    printf("Missing arguments\n");
    return 1;
}
```
- `argc < 2`: 프로그램 이름 외에 인자가 전달되지 않은 경우
- `return 1`: 오류 상태로 프로그램 종료

옵션 형식 검사:
```c
if (argv[i][0] != '-') {
    return 1;
}

if (strlen(argv[i]) != 2) {
    return 1;
}
```
- `argv[i][0] != '-'`: 옵션이 `-`로 시작하는지 검사
- `strlen(argv[i]) != 2`: 옵션이 `-n`처럼 두 글자인지 검사

## 7. 핵심 정리
```text
Terminal Input
      ↓
main(argc, argv)
      ↓
Argument Parsing
      ↓
String Comparison / Type Conversion
      ↓
Variables
      ↓
Program Execution
```
- `argc`: 입력 개수 관리
- `argv`: 입력 문자열 접근
- `strlen()`, `strcmp()`: 문자열 검사 및 비교
- `atoi()`, `atof()`: 문자열을 숫자로 변환
- `for`: 여러 명령행 옵션을 순차적으로 처리