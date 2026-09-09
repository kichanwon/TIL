## 1번. 홀수 합과 짝수 합

` #반복문 , #조건문, #정수 연산`

### 문제
정수 `N`개가 주어진다.  
짝수의 합과 홀수의 합을 각각 출력하시오.

### 입력 형식
첫 번째 줄에 정수의 개수 `N`이 주어진다.  
두 번째 줄에 `N`개의 정수가 공백으로 구분되어 주어진다.

### 출력 형식
첫 번째 줄에 짝수의 합을 출력한다.  
두 번째 줄에 홀수의 합을 출력한다.

### 제한
```text
1 <= N <= 100
0 <= 정수 <= 100
```

>[!important] TC 1: 모두 짝수

입력
```text
4
2 4 6 8
```
출력
```text
20
0
```

>[!important] TC 2: 모두 홀수

입력
```text
3
1 3 5
```
출력
```text
0
9
```

>[!important] TC 3: 모든 값 0

입력
```text
3
0 0 0
```
출력
```text
0
0
```

#### TC 4: 기본 혼합
입력
```text
5
1 2 3 4 5
```
출력
```text
6
9
```

#### TC 5: 0 포함
입력
```text
4
0 1 2 3
```
출력
```text
2
4
```

#### TC 6: N=1 짝수
입력
```text
1
10
```
출력
```text
10
0
```

#### TC 7: N=1 홀수
입력
```text
1
7
```
출력
```text
0
7
```

#### TC 8: 큰 값 혼합
입력
```text
5
100 99 98 1 2
```
출력
```text
200
100
```

#### TC 9: 짝수와 홀수 동일 합
입력
```text
4
1 2 3 2
```
출력
```text
4
4
```

#### TC 10: 여러 값 혼합
입력
```text
6
11 22 33 44 55 66
```
출력
```text
132
99
```

---

## 2번. 두 번째로 큰 점수

` #배열 , #최댓값 , #중복 제거 , #조건문`

### 문제
학생 점수 `N`개가 주어진다.  
서로 다른 점수 중 두 번째로 큰 점수를 출력하시오.

서로 다른 점수가 2개 미만이면 `None`을 출력한다.

### 입력 형식
첫 번째 줄에 점수 개수 `N`이 주어진다.  
두 번째 줄에 `N`개의 점수가 공백으로 구분되어 주어진다.

### 출력 형식
두 번째로 큰 서로 다른 점수를 출력한다.  
없으면 `None`을 출력한다.

### 제한
```text
1 <= N <= 100
0 <= 점수 <= 100
```

### 테스트 케이스 10개

>[!important] TC 1: 중복 최댓값

입력
```text
5
100 100 90 80 70
```
출력
```text
90
```

>[!important] TC 2: 모두 같은 값

입력
```text
4
50 50 50 50
```
출력
```text
None
```

#### TC 3: 기본
입력
```text
5
10 20 30 40 50
```
출력
```text
40
```

#### TC 4: N=1
입력
```text
1
80
```
출력
```text
None
```

#### TC 5: 두 값만 존재
입력
```text
2
70 90
```
출력
```text
70
```

#### TC 6: 내림차순 입력
입력
```text
5
100 90 80 70 60
```
출력
```text
90
```

#### TC 7: 오름차순 입력
입력
```text
5
1 2 3 4 5
```
출력
```text
4
```

#### TC 8: 0 포함
입력
```text
4
0 0 1 1
```
출력
```text
0
```

#### TC 9: 최댓값이 중간에 등장
입력
```text
6
30 80 20 100 90 100
```
출력
```text
90
```

#### TC 10: 두 번째 값 갱신 확인
입력
```text
6
10 50 40 60 55 60
```
출력
```text
55
```

---

## 3번. 모음 개수 세기

` #문자열 , #반복문 , #문자 검사`

### 문제
알파벳 소문자로 이루어진 문자열 `S`가 주어진다.  
문자열에 포함된 모음의 개수를 출력하시오.

모음은 `a`, `e`, `i`, `o`, `u`이다.

### 입력 형식
첫 번째 줄에 문자열 `S`가 주어진다.

### 출력 형식
모음의 개수를 출력한다.

### 제한
```text
1 <= 문자열 길이 <= 100
문자열은 알파벳 소문자로만 이루어진다.
```

### 테스트 케이스 10개

>[!important] TC 1: 기본

입력
```text
apple
```
출력
```text
2
```

>[!important] TC 2: 모음 없음

입력
```text
rhythm
```
출력
```text
0
```

> [!important] TC 3: 모두 모음

입력
```text
aeiou
```
출력
```text
5
```

#### TC 4: 한 글자 모음
입력
```text
a
```
출력
```text
1
```

#### TC 5: 한 글자 자음
입력
```text
b
```
출력
```text
0
```

#### TC 6: 반복 모음
입력
```text
banana
```
출력
```text
3
```

#### TC 7: 긴 문자열
입력
```text
programming
```
출력
```text
3
```

#### TC 8: u 포함 확인
입력
```text
computer
```
출력
```text
3
```

#### TC 9: o 여러 개
입력
```text
boooooook
```
출력
```text
7
```

#### TC 10: 섞인 문자열
입력
```text
university
```
출력
```text
4
```

---

## 4번. 특정 확장자 파일 출력

` #문자열 , #2차원 문자 배열 , #문자열 비교 , #필터링`

### 문제
파일 이름 `N`개와 찾을 확장자 `E`가 주어진다.  
확장자가 `E`와 같은 파일 이름을 입력 순서대로 출력하시오.

파일 이름에는 점(`.`)이 정확히 한 번 포함된다.  
찾는 확장자는 점 없이 주어진다.

해당하는 파일이 없으면 `None`을 출력한다.

### 입력 형식
첫 번째 줄에 파일 수 `N`이 주어진다.  
두 번째 줄부터 `N`개의 줄에 파일 이름이 하나씩 주어진다.  
마지막 줄에 찾을 확장자 `E`가 주어진다.

### 출력 형식
조건을 만족하는 파일 이름을 입력 순서대로 출력한다.  
없으면 `None`을 출력한다.

### 제한
```text
1 <= N <= 100
파일 이름 길이 <= 30
확장자 길이 <= 10
```

### 테스트 케이스 10개

>[!important] TC 1: 기본 필터링

입력
```text
5
a.txt
b.py
c.txt
d.c
e.txt
txt
```
출력
```text
a.txt
c.txt
e.txt
```

>[!important] TC 2: 해당 파일 없음

입력
```text
3
main.py
test.c
readme.md
txt
```
출력
```text
None
```

>[!important] TC 3: 입력 순서 유지

입력
```text
5
z.txt
a.txt
m.py
b.txt
c.c
txt
```
출력
```text
z.txt
a.txt
b.txt
```

#### TC 4: 모든 파일 해당
입력
```text
3
a.py
b.py
c.py
py
```
출력
```text
a.py
b.py
c.py
```

#### TC 5: 하나만 해당
입력
```text
4
one.c
two.py
three.java
four.txt
java
```
출력
```text
three.java
```

#### TC 6: 숫자 포함 파일명
입력
```text
4
file1.txt
file2.py
file3.txt
file4.c
txt
```
출력
```text
file1.txt
file3.txt
```

#### TC 7: 확장자가 한 글자
입력
```text
3
a.c
b.h
c.c
c
```
출력
```text
a.c
c.c
```

#### TC 8: N=1이고 해당
입력
```text
1
main.py
py
```
출력
```text
main.py
```

#### TC 9: N=1이고 미해당
입력
```text
1
main.py
c
```
출력
```text
None
```

#### TC 10: 여러 확장자 혼합
입력
```text
6
a.md
b.md
c.txt
d.py
e.md
f.txt
md
```
출력
```text
a.md
b.md
e.md
```

---

## 5번. 이름과 점수 정렬

` #배열 , #2차원 문자 배열 , #문자열 비교 , #정렬 , #병렬 배열`

### 문제
학생 `N`명의 이름과 점수가 주어진다.  
점수가 높은 학생부터 이름을 출력하시오.

점수가 같으면 이름을 사전순으로 출력한다.

### 입력 형식
첫 번째 줄에 학생 수 `N`이 주어진다.  
두 번째 줄부터 `N`개의 줄에 `이름 점수`가 공백으로 구분되어 주어진다.

### 출력 형식
정렬된 학생 이름을 한 줄에 하나씩 출력한다.

### 제한
```text
1 <= N <= 100
이름은 알파벳 소문자로만 이루어진다.
이름 길이 <= 20
0 <= 점수 <= 100
```

### 테스트 케이스 10개

>[!important] TC 1: 기본 점수 내림차순

입력
```text
3
kim 80
lee 90
park 70
```
출력
```text
lee
kim
park
```

>[!important] TC 2: 동점 사전순

입력
```text
3
kim 80
lee 80
ahn 80
```
출력
```text
ahn
kim
lee
```

#### TC 3: 점수와 이름 기준 혼합
입력
```text
5
b 90
a 90
c 80
e 70
d 80
```
출력
```text
a
b
c
d
e
```

#### TC 4: N=1
입력
```text
1
kim 100
```
출력
```text
kim
```

#### TC 5: 이미 정렬된 입력
입력
```text
3
a 100
b 90
c 80
```
출력
```text
a
b
c
```

#### TC 6: 역순 입력
입력
```text
3
c 80
b 90
a 100
```
출력
```text
a
b
c
```

#### TC 7: 0점 포함
입력
```text
4
a 0
b 100
c 0
d 50
```
출력
```text
b
d
a
c
```

#### TC 8: 이름 사전순 확인
입력
```text
4
zoo 60
alpha 60
beta 60
apple 60
```
출력
```text
alpha
apple
beta
zoo
```

#### TC 9: 일부 동점
입력
```text
6
a 70
b 80
c 80
d 90
e 90
f 70
```
출력
```text
d
e
b
c
a
f
```

#### TC 10: 다양한 점수
입력
```text
5
tom 55
jane 100
bob 55
amy 100
zoe 80
```
출력
```text
amy
jane
zoe
bob
tom
```

---

## 6번. 로그인 실패 사용자 찾기

` #문자열 , #2차원 문자 배열 , #병렬 배열 , #누적 횟수 , #정렬`

### 문제
로그인 기록 `N`개가 주어진다.  
각 기록은 사용자 ID와 로그인 결과로 이루어진다.

로그인 결과는 `success` 또는 `fail`이다.  
사용자별 `fail` 횟수를 계산하시오.

`fail` 횟수가 3회 이상인 사용자를 다음 기준으로 출력하시오.

1. `fail` 횟수 내림차순
2. `fail` 횟수가 같으면 사용자 ID 사전순

해당 사용자가 없으면 `None`을 출력한다.

### 입력 형식
첫 번째 줄에 로그인 기록 수 `N`이 주어진다.  
두 번째 줄부터 `N`개의 줄에 `사용자ID 결과`가 공백으로 구분되어 주어진다.

### 출력 형식
조건을 만족하는 사용자 ID를 한 줄에 하나씩 출력한다.  
없으면 `None`을 출력한다.

### 제한
```text
1 <= N <= 1000
사용자 ID는 알파벳 소문자와 숫자로 이루어진다.
사용자 ID 길이 <= 20
```

### 테스트 케이스 10개

>[!important] TC 1: 기본 조건 만족

입력
```text
8
kim fail
lee success
kim fail
park fail
kim fail
park fail
park fail
lee fail
```
출력
```text
kim
park
```

>[!important] TC 2: 아무도 3회 이상 실패하지 않음

입력
```text
5
a fail
b fail
a success
b fail
c success
```
출력
```text
None
```

>[!important] TC 3: 동률 사전순

입력
```text
6
z fail
a fail
z fail
a fail
z fail
a fail
```
출력
```text
a
z
```

#### TC 4: fail 횟수 내림차순
입력
```text
9
a fail
b fail
a fail
b fail
a fail
b fail
a fail
c fail
c fail
```
출력
```text
a
b
```

#### TC 5: success는 fail에 영향 없음
입력
```text
7
a success
a success
a fail
a fail
a fail
b success
b fail
```
출력
```text
a
```

#### TC 6: 숫자 포함 ID
입력
```text
6
u1 fail
u2 fail
u1 fail
u2 fail
u1 fail
u2 success
```
출력
```text
u1
```

#### TC 7: 한 명만 존재
입력
```text
3
abc fail
abc fail
abc fail
```
출력
```text
abc
```

#### TC 8: 한 명만 있고 조건 미달
입력
```text
2
abc fail
abc success
```
출력
```text
None
```

#### TC 9: 여러 명 정렬 복합
입력
```text
12
c fail
b fail
a fail
c fail
b fail
a fail
c fail
b fail
a fail
c fail
a fail
d fail
```
출력
```text
a
c
b
```

#### TC 10: success만 있는 사용자 포함
입력
```text
7
a success
b success
c fail
c fail
c fail
d success
e fail
```
출력
```text
c
```
