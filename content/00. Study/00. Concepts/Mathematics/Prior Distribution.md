# Prior Distribution

## Definition

사전분포(Prior Distribution)는 데이터를 관측하기 전에 파라미터나 가설에 대해 가지고 있는 믿음을 나타내는 분포다.

## Intuition

새로운 데이터를 보기 전의 출발점이다.

## Mathematical Definition

베이즈 추론에서 사후분포는 다음 비례식을 따른다.

$$
p(\theta\mid D)\propto p(D\mid\theta)p(\theta)
$$

## Example

동전의 앞면 확률에 대해 가능한 값들을 미리 균등하게 믿는 사전분포를 둘 수 있다.

## Related Concepts

- [[Bayes' Theorem]]
- [[Likelihood Function]]
- [[Posterior Distribution]]
- [[Uncertainty]]

## Applications in AI

데이터가 적을 때 사전 지식을 반영하고, 모델 파라미터의 불확실성을 표현한다.
