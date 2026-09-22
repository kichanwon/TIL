# Marginal Probability

## Definition

주변확률(Marginal Probability)은 결합분포에서 다른 변수를 합하거나 적분해 얻는 한 변수의 확률이다.

## Intuition

여러 변수의 관계를 알고 있을 때 관심 있는 변수 하나만 남기는 과정이다.

## Mathematical Definition

이산변수의 경우 다음과 같이 계산한다.

$$
P(X=x)=\sum_yP(X=x,Y=y)
$$

## Example

성별과 구매 여부의 결합표에서 성별에 관계없이 구매할 확률을 구할 수 있다.

## Related Concepts

- [[Joint Distribution]]
- [[Conditional Probability]]
- [[Probability Distribution]]
- [[Bayes' Theorem]]

## Applications in AI

잠재변수를 합쳐 관측변수의 확률을 계산하거나, 결합모델에서 관심 변수의 분포를 얻는다.
