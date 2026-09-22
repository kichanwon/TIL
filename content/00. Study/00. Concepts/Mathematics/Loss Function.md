# Loss Function

## Definition

손실함수(Loss Function)는 모델의 예측과 정답이 얼마나 다른지를 수치화하는 함수다.

## Intuition

예측이 나쁠수록 큰 값을 내고, 예측이 좋을수록 작은 값을 내는 오차 측정기다.

## Mathematical Definition

평균제곱오차는 다음과 같이 표현된다.

$$
L(\theta)=\frac{1}{n}\sum_{i=1}^{n}(y_i-\hat{y}_i)^2
$$

## Example

회귀 문제에서는 평균제곱오차를, 분류 문제에서는 교차 엔트로피를 사용할 수 있다.

## Related Concepts

- [[Objective Function]]
- [[Gradient]]
- [[Gradient Descent]]
- [[Neural Networks/Multi-Layer Perceptron]]

## Applications in AI

손실함수의 그래디언트를 이용해 신경망의 가중치를 업데이트하고 모델을 학습시킨다.
