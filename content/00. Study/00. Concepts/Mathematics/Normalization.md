# Normalization

## Definition

정규화(Normalization)는 데이터를 정해진 범위나 기준에 맞도록 변환하는 과정이다.

## Intuition

서로 다른 단위와 크기의 데이터를 모델이 안정적으로 비교하고 처리할 수 있게 만든다.

## Mathematical Definition

최솟값-최댓값 정규화의 예는 다음과 같다.

$$
x' = \frac{x-x_{\min}}{x_{\max}-x_{\min}}
$$

## Example

픽셀값을 0부터 255 범위에서 0부터 1 범위로 바꿀 수 있다.

## Related Concepts

- [[Scaling]]
- [[Standardization]]
- [[Mean]]
- [[Standard Deviation]]
- [[Neural Networks/Convolutional Neural Network]]

## Applications in AI

입력 특성의 범위를 제한하고 모델 학습의 수치적 안정성을 높인다.
