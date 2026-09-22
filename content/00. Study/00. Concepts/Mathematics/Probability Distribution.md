# Probability Distribution

## Definition

확률분포(Probability Distribution)는 확률변수가 가질 수 있는 값과 그 확률을 나타내는 규칙이다.

## Intuition

데이터가 어떤 값 주변에 얼마나 자주 나타나는지 설명하는 지도다.

## Mathematical Definition

이산형은 확률질량함수, 연속형은 확률밀도함수로 표현할 수 있다.

$$
\sum_xP(X=x)=1 \quad\text{or}\quad \int_{-\infty}^{\infty}f_X(x)dx=1
$$

## Example

동전 앞면 개수는 이항분포, 측정 오차는 정규분포로 모델링할 수 있다.

## Related Concepts

- [[Random Variable]]
- [[Discrete Distribution]]
- [[Continuous Distribution]]
- [[Joint Distribution]]
- [[Neural Networks/Multi-Layer Perceptron]]

## Applications in AI

생성 모델, 베이지안 추론, 분류기의 확률 출력에서 데이터와 예측의 불확실성을 표현한다.
