# Posterior Distribution

## Definition

사후분포(Posterior Distribution)는 데이터를 관측한 뒤 갱신된 파라미터나 가설의 분포다.

## Intuition

사전 믿음과 데이터의 증거를 결합한 최종적인 믿음이다.

## Mathematical Definition

$$
p(\theta\mid D)=\frac{p(D\mid\theta)p(\theta)}{p(D)}
$$

## Example

동전 던지기 결과를 관측한 뒤 앞면 확률에 대한 불확실성을 사후분포로 표현할 수 있다.

## Related Concepts

- [[Bayes' Theorem]]
- [[Prior Distribution]]
- [[Likelihood Function]]
- [[Uncertainty]]

## Applications in AI

모델 파라미터의 가능한 값과 그 불확실성을 함께 추론할 수 있게 한다.
