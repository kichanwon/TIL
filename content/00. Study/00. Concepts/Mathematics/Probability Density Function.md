# Probability Density Function

## Definition

확률밀도함수(Probability Density Function, PDF)는 연속확률변수의 확률 밀도를 나타내는 함수다.

## Intuition

PDF의 높이는 특정 지점 주변에 값이 얼마나 몰려 있는지를 나타내며, 실제 확률은 구간의 넓이로 계산한다.

## Mathematical Definition

$$
P(a\le X\le b)=\int_a^b f_X(x)\,dx
$$

## Example

정규분포의 PDF를 적분하면 특정 구간에 관측값이 들어갈 확률을 얻는다.

## Related Concepts

- [[Continuous Random Variable]]
- [[Continuous Distribution]]
- [[Integral]]
- [[Normal Distribution]]

## Applications in AI

연속 데이터의 가능도를 계산하고 생성 모델과 베이지안 모델의 분포를 정의한다.
