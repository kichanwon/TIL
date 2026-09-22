# Uncertainty

## Definition

불확실성(Uncertainty)은 데이터나 모델의 결과가 하나로 확정되지 않은 정도다.

## Intuition

모델이 무엇을 예측했는지만이 아니라 그 예측을 얼마나 믿을 수 있는지도 나타낸다.

## Mathematical Definition

불확실성은 분산, 엔트로피, 신뢰구간 등 여러 방식으로 측정할 수 있다.

$$
H(X)=-\sum_xp(x)\log p(x)
$$

## Example

두 클래스의 확률이 각각 0.5인 예측은 한 클래스 확률이 0.99인 예측보다 불확실하다.

## Related Concepts

- [[Probability Distribution]]
- [[Variance]]
- [[Standard Deviation]]
- [[Prior Distribution]]
- [[Posterior Distribution]]

## Applications in AI

신뢰도 추정, 이상치 탐지, 안전한 의사결정, 베이지안 딥러닝에 활용된다.
