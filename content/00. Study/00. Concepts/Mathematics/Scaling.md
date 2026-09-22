# Scaling

## Definition

스케일링(Scaling)은 특성값의 수치 범위를 조정해 서로 비교 가능한 크기로 만드는 전처리다.

## Intuition

단위가 서로 다른 특성들이 특정 특성만 과도하게 지배하지 않도록 크기를 맞춘다.

## Mathematical Definition

대표적으로 최솟값과 최댓값을 이용해 구간을 변환한다.

$$
x' = \frac{x-x_{\min}}{x_{\max}-x_{\min}}
$$

## Example

소득과 나이처럼 단위와 범위가 다른 특성을 비슷한 수치 범위로 변환할 수 있다.

## Related Concepts

- [[Normalization]]
- [[Standardization]]
- [[Mean]]
- [[Variance]]

## Applications in AI

거리 기반 모델과 경사 기반 최적화가 특정 특성의 크기에 치우치지 않도록 한다.
