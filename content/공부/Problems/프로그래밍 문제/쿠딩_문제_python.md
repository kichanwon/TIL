## 1번. 합격자 통계

` #반복문 , #조건문, #리스트, #합계, #평균`

### 문제
학생 `N`명의 점수가 주어진다.  
점수가 60점 이상이면 합격, 60점 미만이면 불합격이다.

전체 평균을 소수 첫째 자리까지 출력하고, 합격자 수와 불합격자 수를 출력하시오.

### 입력 형식
첫 번째 줄에 학생 수 `N`이 주어진다.  
두 번째 줄에 `N`개의 정수 점수가 공백으로 구분되어 주어진다.

### 출력 형식
첫 번째 줄에 평균을 소수 첫째 자리까지 출력한다.  
두 번째 줄에 합격자 수를 출력한다.  
세 번째 줄에 불합격자 수를 출력한다.

### 제한
```text
1 <= N <= 100
0 <= 점수 <= 100
```

### 테스트 케이스 10개

>[!important] TC 1: 기본 합격/불합격 혼합

입력
```text
5
80 70 50 40 60
```
출력
```text
60.0
3
2
```

>[!important] TC 2: 경계값 60 포함

입력
```text
4
59 60 61 0
```
출력
```text
45.0
2
2
```

>[!important] TC 3: 다양한 점수 혼합

입력
```text
6
45 60 75 30 90 59
```
출력
```text
59.8
3
3
```

#### TC 4: 모두 합격
입력
```text
4
100 90 80 70
```
출력
```text
85.0
4
0
```

#### TC 5: 모두 불합격
입력
```text
3
10 20 30
```
출력
```text
20.0
0
3
```

#### TC 6: 학생 1명 합격
입력
```text
1
60
```
출력
```text
60.0
1
0
```

#### TC 7: 학생 1명 불합격
입력
```text
1
59
```
출력
```text
59.0
0
1
```

#### TC 8: 평균 반올림 확인
입력
```text
3
80 81 82
```
출력
```text
81.0
3
0
```

#### TC 9: 평균 소수 첫째 자리 확인
입력
```text
3
50 51 52
```
출력
```text
51.0
0
3
```

#### TC 10: 0점과 100점 포함
입력
```text
4
0 100 0 100
```
출력
```text
50.0
2
2
```

---

## 2번. 파일 확장자 찾기

` #문자열 , #조건문, #리스트, #필터링`

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
확장자가 `E`와 같은 파일 이름을 입력 순서대로 한 줄에 하나씩 출력한다.  
없으면 `None`을 출력한다.

### 제한
```text
1 <= N <= 100
파일 이름 길이 <= 30
확장자 길이 <= 10
파일 이름은 알파벳 소문자, 숫자, 점(.)으로 이루어진다.
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

>[!important] TC 3: 입력 순서 유지 확인

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

## 3번. 가장 많이 주문된 메뉴

` #딕셔너리 , #빈도수, #문자열, #정렬 기준`

### 문제
주문 기록 `N`개가 주어진다.  
각 주문 기록은 메뉴 이름 하나로 이루어진다.

가장 많이 주문된 메뉴 이름과 주문 횟수를 출력하시오.  
가장 많이 주문된 메뉴가 여러 개이면 메뉴 이름이 사전순으로 가장 앞서는 것을 출력한다.

### 입력 형식
첫 번째 줄에 주문 수 `N`이 주어진다.  
두 번째 줄부터 `N`개의 줄에 메뉴 이름이 하나씩 주어진다.

### 출력 형식
메뉴 이름과 주문 횟수를 공백으로 구분하여 출력한다.

### 제한
```text
1 <= N <= 1000
메뉴 이름은 알파벳 소문자로만 이루어진다.
메뉴 이름 길이 <= 20
```

### 테스트 케이스 10개

>[!important] TC 1: 기본 최빈값

입력
```text
5
ramen
pizza
ramen
burger
ramen
```
출력
```text
ramen 3
```

>[!important] TC 2: 동률 사전순

입력
```text
4
pizza
ramen
pizza
ramen
```
출력
```text
pizza 2
```

#### TC 3: 모든 메뉴 1회
입력
```text
4
z
b
a
c
```
출력
```text
a 1
```

#### TC 4: 하나의 메뉴만 존재
입력
```text
3
kimchi
kimchi
kimchi
```
출력
```text
kimchi 3
```

#### TC 5: N=1
입력
```text
1
coffee
```
출력
```text
coffee 1
```

#### TC 6: 후반에 최빈값 등장
입력
```text
6
a
b
c
c
c
b
```
출력
```text
c 3
```

#### TC 7: 사전순 비교 확인
입력
```text
6
melon
apple
melon
apple
banana
banana
```
출력
```text
apple 2
```

#### TC 8: 긴 메뉴명
입력
```text
5
americano
latte
americano
latte
americano
```
출력
```text
americano 3
```

#### TC 9: 세 메뉴 중 하나 우세
입력
```text
7
a
b
a
c
b
a
c
```
출력
```text
a 3
```

#### TC 10: 동률이나 입력 순서와 무관
입력
```text
4
zoo
alpha
zoo
alpha
```
출력
```text
alpha 2
```

---

## 4번. 상품별 매출 합계

` #딕셔너리 , #누적 합계, #정렬, #문자열 처리`

### 문제
판매 기록 `N`개가 주어진다.  
각 기록은 상품명, 판매 개수, 개당 가격으로 이루어진다.

상품별 총매출을 계산하여 총매출이 높은 상품부터 출력하시오.  
총매출이 같으면 상품명을 사전순으로 출력한다.

### 입력 형식
첫 번째 줄에 판매 기록 수 `N`이 주어진다.  
두 번째 줄부터 `N`개의 줄에 `상품명 개수 가격`이 공백으로 구분되어 주어진다.

### 출력 형식
상품명과 총매출을 한 줄에 하나씩 출력한다.

### 제한
```text
1 <= N <= 1000
상품명은 알파벳 소문자로만 이루어진다.
1 <= 개수 <= 100
1 <= 가격 <= 10000
```

### 테스트 케이스 10개

>[!important] TC 1: 기본 매출 합산

입력
```text
4
apple 2 1000
banana 1 2000
apple 3 1000
melon 1 5000
```
출력
```text
apple 5000
melon 5000
banana 2000
```

>[!important] TC 2: 누적 후 순위 역전

입력
```text
5
a 1 100
b 1 500
a 5 100
c 1 200
a 1 100
```
출력
```text
a 700
b 500
c 200
```

#### TC 3: 매출 동률 사전순
입력
```text
3
b 1 100
a 2 50
c 1 80
```
출력
```text
a 100
b 100
c 80
```

#### TC 4: 하나의 상품만 반복
입력
```text
3
pen 1 100
pen 2 100
pen 3 100
```
출력
```text
pen 600
```

#### TC 5: N=1
입력
```text
1
book 2 5000
```
출력
```text
book 10000
```

#### TC 6: 큰 가격 계산
입력
```text
2
laptop 2 10000
mouse 10 1000
```
출력
```text
laptop 20000
mouse 10000
```

#### TC 7: 입력 순서와 정렬 순서 다름
입력
```text
4
c 1 10
a 1 30
b 1 20
d 1 40
```
출력
```text
d 40
a 30
b 20
c 10
```

#### TC 8: 모든 상품 동률
입력
```text
3
z 1 100
y 2 50
x 4 25
```
출력
```text
x 100
y 100
z 100
```

#### TC 9: 상품명 사전순 확인
입력
```text
4
aa 1 100
ab 1 100
ac 1 90
aa 1 50
```
출력
```text
aa 150
ab 100
ac 90
```

#### TC 10: 여러 상품 혼합
입력
```text
6
apple 1 300
banana 2 100
apple 2 300
carrot 5 100
banana 1 100
date 1 1000
```
출력
```text
date 1000
apple 900
carrot 500
banana 300
```

---

## 5번. 로그인 실패 사용자 정렬

` #딕셔너리 , #로그 처리, #필터링, #다중 기준 정렬`

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
1 <= N <= 10000
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

---

## 6번. 패키지별 최신 버전 찾기

` #문자열 파싱 , #튜플 비교, #딕셔너리, #정렬`

### 문제
패키지 버전 기록 `N`개가 주어진다.  
각 기록은 패키지명과 버전 번호로 이루어진다.

버전 번호 형식은 다음과 같다.

```text
주버전.부버전.패치버전
```

각 부분은 정수로 비교한다.  
패키지별로 가장 최신 버전 하나를 출력하시오.

출력은 패키지명 사전순으로 한다.

### 입력 형식
첫 번째 줄에 기록 수 `N`이 주어진다.  
두 번째 줄부터 `N`개의 줄에 `패키지명 버전번호`가 공백으로 구분되어 주어진다.

### 출력 형식
패키지명과 최신 버전 번호를 공백으로 구분하여 한 줄에 하나씩 출력한다.

### 제한
```text
1 <= N <= 1000
패키지명은 알파벳 소문자로만 이루어진다.
0 <= 주버전, 부버전, 패치버전 <= 99
```

### 테스트 케이스 10개

>[!important] TC 1: 기본 최신 버전 선택

입력
```text
4
app 1.0.0
app 1.2.0
lib 0.9.1
lib 1.0.0
```
출력
```text
app 1.2.0
lib 1.0.0
```

>[!important] TC 2: 문자열 정렬 오류 방지

입력
```text
2
app 1.10.0
app 1.2.0
```
출력
```text
app 1.10.0
```

>[!important] TC 3: 여러 패키지 사전순 출력

입력
```text
3
z 1.0.0
a 2.0.0
m 1.5.0
```
출력
```text
a 2.0.0
m 1.5.0
z 1.0.0
```

#### TC 4: 패치버전 비교
입력
```text
3
a 1.0.1
a 1.0.9
a 1.0.3
```
출력
```text
a 1.0.9
```

#### TC 5: 주버전 우선
입력
```text
3
a 2.0.0
a 1.99.99
a 1.50.50
```
출력
```text
a 2.0.0
```

#### TC 6: 같은 버전 중복
입력
```text
3
app 1.0.0
app 1.0.0
app 0.9.9
```
출력
```text
app 1.0.0
```

#### TC 7: 부버전 우선
입력
```text
3
x 1.3.0
x 1.4.0
x 1.2.99
```
출력
```text
x 1.4.0
```

#### TC 8: 0 버전 포함
입력
```text
2
core 0.0.0
core 0.0.1
```
출력
```text
core 0.0.1
```

#### TC 9: 여러 패키지 갱신
입력
```text
6
a 1.0.0
b 1.0.0
a 1.1.0
b 0.9.9
c 2.0.0
c 1.9.9
```
출력
```text
a 1.1.0
b 1.0.0
c 2.0.0
```

#### TC 10: 큰 숫자 비교
입력
```text
4
pkg 9.99.99
pkg 10.0.0
tool 1.9.10
tool 1.10.0
```
출력
```text
pkg 10.0.0
tool 1.10.0
```
