# Weibull Distribution

## Definition

베이불분포(Weibull Distribution)는 시간에 따라 변할 수 있는 고장률과 생존 시간을 모델링하는 연속분포다.

## Intuition

제품이나 시스템이 오래될수록 고장 가능성이 커지거나 작아지는 현상을 표현할 수 있다.

## Mathematical Definition

$$
f(x)=\frac{k}{\lambda}\left(\frac{x}{\lambda}\right)^{k-1}e^{-(x/\lambda)^k},\qquad x\ge0
$$

## Example

기계 부품의 수명과 고장까지 걸리는 시간을 모델링할 수 있다.

## Related Concepts

- [[Continuous Distribution]]
- [[Probability Density Function]]
- [[Exponential Distribution]]
- [[Uncertainty]]

## Applications in AI

신뢰성 분석, 예측 유지보수, 생존 분석과 시간-사건 모델에 사용된다.
