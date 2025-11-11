---
draft: true
---

# 문제 3: 두 수의 최소공배수

## 문제 설명

두 수 **a**와 **b**가 주어졌을 때, 두 수의 **최소공배수(LCM, Least Common Multiple)**를 구하는 프로그램을 작성하시오.

최소공배수란 두 수를 모두 나눌 수 있는 **가장 작은 양의 정수**를 의미한다. 예를 들어, 15와 21의 최소공배수는 105이다.

## 입력 형식 (`input.txt`)

- 첫째 줄에 테스트 케이스의 개수 `n` (1 ≤ n ≤ 1000)이 주어진다.
- 다음 `n`개의 줄에는 두 자연수 `a`와 `b`가 공백으로 구분되어 주어진다. (1 ≤ a, b ≤ 1,000,000)

## 출력 형식 (`output.txt`)

- 각 테스트 케이스마다 한 줄에 하나씩 두 수의 최소공배수를 출력한다.

## 개념 설명

- 두 수의 최소공배수는 다음 식으로 계산할 수 있다:
    
    ```
    LCM(a, b) = (a × b) / GCD(a, b)
    ```
    
- `GCD(a, b)`는 두 수의 **최대공약수(Greatest Common Divisor)**이며, 두 수를 모두 나눌 수 있는 가장 큰 수를 의미한다.
- 최대공약수는 **유클리드 호제법**으로 쉽게 구할 수 있다. 예를 들어, a ≥ b일 때 다음 과정을 반복한다:  
    `GCD(a, b) = GCD(b, a mod b)`  
    나머지가 0이 되면, 그때의 b가 바로 최대공약수이다.

## 채점 유의사항

- **출제된 출력 예시를 그대로 복사하여 제출하면 0점 처리됩니다.**
- 프로그램은 반드시 `input.txt`의 입력을 읽어 `output.txt`로 결과를 출력해야 합니다.

## 입력 예시 1 (`input.txt`)

3
15 21
33 22
9 10

## 출력 예시 1 (`output.txt`)

105
66
90

## 입력 예시 2 (`input.txt`)

4
1 1
12 18
1000000 1
48 180

## 출력 예시 2 (`output.txt`)

1
36
1000000
720

## 입력 예시 3 (`input.txt`)

5
7 13
8 20
21 6
14 28
999999 999999

## 출력 예시 3 (`output.txt`)

91
40
42
28
999999


---

# 문제 4: 쪽지 해독 — 뒤집힌 암호

## 문제 설명

길에서 발견한 쪽지에는 암호가 적혀 있었고, 당신은 그 암호가 **뒤집혀 있으면** 해독된다는 것을 발견했다. 각 줄에 적힌 암호(문자열)를 뒤집어서 원래 문장을 복원하는 프로그램을 작성하라.

## 입력 형식 (`input.txt`)

- 입력은 여러 줄로 주어진다. 각 줄에는 하나의 암호 문자열이 주어진다.
- 문자열의 길이는 최대 500 문자이다.
- 마지막 줄에는 `END`가 주어진다. (`END`는 해독 대상이 아니며 출력하지 않는다.)

## 출력 형식 (`output.txt`)

- 각 암호 문자열을 뒤집어 해독한 결과를 한 줄에 하나씩 출력한다.
- `END`는 입력의 종료 표식이므로 출력하지 않는다.

## 채점 유의사항

- 각 줄은 공백과 특수문자를 포함할 수 있으며, 전체 문자열을 문자 단위로 뒤집어야 한다.
- 입력에는 빈 줄이 있을 수 있으며, 빈 줄이 들어오면 빈 줄을 뒤집은 결과(역시 빈 줄)를 출력하면 된다.
- 입력의 총 줄 수는 제한이 크지 않으므로 표준 입력/출력 방법으로 처리하면 된다.
- **출제된 출력 예시를 그대로 복사하여 제출하면 0점 처리됩니다.**
- 프로그램은 반드시 `input.txt`의 입력을 읽어 `output.txt`로 결과를 출력해야 합니다.

## 입력 예시 1 (`input.txt`)

!edoc doog a tahW
noitacitsufbo
erafraw enirambus detcirtsernu yraurbeF fo tsrif eht no nigeb ot dnetni eW
lla sees rodroM fo drol eht ,ssertrof sih nihtiw delaecnoC
END

## 출력 예시 1 (`output.txt`)

What a good code!
obfustication
We intend to begin on the first of February unrestricted submarine warfare
Concealed within his fortress, the lord of Mordor sees all

## 입력 예시 2 (`input.txt`)

dlroW ,olleH
gnirts desrever a si sihT

4321 0987
END

## 출력 예시 2 (`output.txt`)

Hello, World
This is a reversed string

7890 1234

## 입력 예시 3 (`input.txt`)

!!!gnizama si sihT
)'uoy era woH'(
IA si eman ym
END

## 출력 예시 3 (`output.txt`)

This is amazing!!!
('How are you')
my name is AI



