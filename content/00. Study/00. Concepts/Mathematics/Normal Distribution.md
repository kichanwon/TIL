# Normal Distribution

## Definition

정규분포(Normal Distribution)는 평균을 중심으로 좌우 대칭인 종 모양의 연속분포다.

## Intuition

평균 주변의 값이 많고 평균에서 멀어질수록 값이 드물어지는 현상을 모델링한다.

## Mathematical Definition

$$
f(x)=\frac{1}{\sigma\sqrt{2\pi}}\exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)
$$

## Example

측정 오차나 여러 작은 독립 요인의 합은 정규분포로 근사되는 경우가 많다.

## Related Concepts

- [[Continuous Distribution]]
- [[Probability Density Function]]
- [[Mean]]
- [[Variance]]
- [[Standard Deviation]]

## Applications in AI

오차 모델, 가우시안 혼합 모델, 잠재변수 모델, 가중치 초기화와 통계적 추론에 사용된다.
