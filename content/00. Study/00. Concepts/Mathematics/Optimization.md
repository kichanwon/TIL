# Optimization

## Definition

최적화(Optimization)는 목적 함수의 값을 가장 작게 하거나 크게 하는 입력을 찾는 과정이다.

## Intuition

가능한 선택지 중에서 원하는 기준을 가장 잘 만족하는 지점을 찾는 문제다.

## Mathematical Definition

$$
\mathbf{x}^*=\arg\min_{\mathbf{x}}J(\mathbf{x})
$$

## Example

모델의 예측 오차를 최소화하는 가중치를 찾는 것이 머신러닝 최적화 문제다.

## Related Concepts

- [[Objective Function]]
- [[Loss Function]]
- [[Gradient]]
- [[Gradient Descent]]

## Applications in AI

학습 데이터에 대한 손실을 최소화하는 파라미터를 찾는 것이 대부분의 지도학습 알고리즘의 핵심이다.
