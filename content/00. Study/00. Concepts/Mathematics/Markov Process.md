# Markov Process

## Definition

마르코프 과정(Markov Process)은 현재 상태가 주어지면 미래가 과거의 전체 이력과 독립적인 확률 과정이다.

## Intuition

미래를 예측할 때 지금 상태만 알면 충분하다는 기억 없음(memoryless) 가정을 둔다.

## Mathematical Definition

마르코프 성질은 다음과 같다.

$$
P(X_{t+1}\mid X_t,X_{t-1},\ldots)=P(X_{t+1}\mid X_t)
$$

## Example

현재 날씨만으로 내일 날씨를 예측하는 간단한 상태 전이 모델을 만들 수 있다.

## Related Concepts

- [[Probability]]
- [[Conditional Probability]]
- [[Joint Distribution]]
- [[Uncertainty]]

## Applications in AI

상태 전이 모델, 은닉 마르코프 모델, 강화학습과 순차 데이터 모델링의 기초가 된다.
