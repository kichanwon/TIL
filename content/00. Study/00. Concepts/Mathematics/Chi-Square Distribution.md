# Chi-Square Distribution

## Definition

카이제곱분포(Chi-Square Distribution)는 서로 독립인 표준정규변수들의 제곱합이 따르는 분포다.

## Intuition

여러 표준화된 오차의 크기를 제곱해 합친 양을 모델링한다.

## Mathematical Definition

독립인 $Z_i\sim\mathcal{N}(0,1)$에 대해

$$
X=\sum_{i=1}^{k}Z_i^2\sim\chi_k^2
$$

## Example

범주형 변수의 관측 빈도와 기대 빈도를 비교하는 검정에 사용할 수 있다.

## Related Concepts

- [[Continuous Distribution]]
- [[Normal Distribution]]
- [[Variance]]
- [[Uncertainty]]

## Applications in AI

분산 추정, 적합도 검정, 특성 간 독립성 분석과 통계적 검정에 활용된다.
