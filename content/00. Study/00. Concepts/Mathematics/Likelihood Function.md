# Likelihood Function

## Definition

가능도함수(Likelihood Function)는 관측 데이터를 고정하고 파라미터가 그 데이터를 얼마나 잘 설명하는지 나타내는 함수다.

## Intuition

같은 데이터를 만들어낼 가능성이 높은 파라미터를 더 그럴듯한 것으로 평가한다.

## Mathematical Definition

$$
L(\theta\mid D)=p(D\mid\theta)
$$

## Example

동전 결과가 주어졌을 때 앞면 확률 $\theta$가 여러 결과를 얼마나 잘 설명하는지 비교할 수 있다.

## Related Concepts

- [[Conditional Probability]]
- [[Bayes' Theorem]]
- [[Prior Distribution]]
- [[Posterior Distribution]]

## Applications in AI

최대가능도추정과 베이지안 추론에서 모델 파라미터를 데이터에 맞추는 기준이 된다.
