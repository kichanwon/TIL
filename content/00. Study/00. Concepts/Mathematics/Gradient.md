# Gradient

## Definition

그래디언트(Gradient)는 다변수 함수의 각 입력 방향 편미분을 벡터로 모은 것이다.

## Intuition

함수값이 가장 빠르게 증가하는 방향과 그 증가율을 함께 나타낸다.

## Mathematical Definition

$$
\nabla f(\mathbf{x})=
\begin{bmatrix}
\frac{\partial f}{\partial x_1}\\
\vdots\\
\frac{\partial f}{\partial x_n}
\end{bmatrix}
$$

## Example

손실 함수의 그래디언트가 양수인 파라미터는 손실을 줄이기 위해 반대 방향으로 이동시킬 수 있다.

## Related Concepts

- [[Partial Derivative]]
- [[Vector]]
- [[Optimization]]
- [[Gradient Descent]]

## Applications in AI

신경망 학습은 손실 함수의 그래디언트를 계산하고 파라미터를 갱신하는 과정이다.
