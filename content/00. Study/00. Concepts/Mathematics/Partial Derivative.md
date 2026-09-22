# Partial Derivative

## Definition

편미분(Partial Derivative)은 여러 입력을 가진 함수에서 한 변수에 대해서만 미분하는 것이다.

## Intuition

다른 입력들은 고정하고 특정 입력 하나가 출력에 미치는 영향만 측정한다.

## Mathematical Definition

$$
\frac{\partial f}{\partial x_i}
$$

는 다른 변수들을 고정했을 때 $x_i$ 방향의 변화율이다.

## Example

$$
f(x,y)=x^2+3xy \quad\Rightarrow\quad \frac{\partial f}{\partial x}=2x+3y
$$

## Related Concepts

- [[Derivative]]
- [[Gradient]]
- [[Loss Function]]
- [[Gradient Descent]]

## Applications in AI

손실 함수가 수많은 파라미터를 입력으로 가질 때 각 파라미터를 어떻게 조정할지 계산한다.
