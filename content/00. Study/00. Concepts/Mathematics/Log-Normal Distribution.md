# Log-Normal Distribution

## Definition

로그정규분포(Log-Normal Distribution)는 확률변수의 로그가 정규분포를 따르는 연속분포다.

## Intuition

곱셈적 요인들이 누적되어 항상 양수이고 오른쪽으로 긴 꼬리를 가진 데이터를 표현한다.

## Mathematical Definition

$$
X\sim\operatorname{LogNormal}(\mu,\sigma^2)\iff \log X\sim\mathcal{N}(\mu,\sigma^2)
$$

## Example

소득, 반응 시간, 파일 크기처럼 양수이며 큰 값이 드물게 나타나는 데이터를 모델링할 수 있다.

## Related Concepts

- [[Continuous Distribution]]
- [[Normal Distribution]]
- [[Probability Density Function]]
- [[Uncertainty]]

## Applications in AI

양수 제약이 있는 값과 긴 꼬리를 가진 데이터의 확률 모델링에 사용된다.
