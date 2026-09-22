# Derivative

## Definition

미분(Derivative)은 함수의 입력이 변할 때 출력이 변하는 순간 변화율이다.

## Intuition

곡선 위 한 점에서의 접선 기울기로 생각할 수 있다. 기울기가 크면 입력의 작은 변화가 출력에 큰 변화를 만든다.

## Mathematical Definition

$$
f'(x)=\lim_{h\to0}\frac{f(x+h)-f(x)}{h}
$$

## Example

$$
f(x)=x^2 \quad\Rightarrow\quad f'(x)=2x
$$

## Related Concepts

- [[Function]]
- [[Calculus]]
- [[Partial Derivative]]
- [[Gradient]]
- [[Gradient Descent]]

## Applications in AI

모델 파라미터를 조금 바꿨을 때 손실이 얼마나 변하는지 계산해 학습 방향을 정한다.
