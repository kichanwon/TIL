# Linear and Nonlinear

## Definition
선형(Linear) 관계는 덧셈과 스칼라 곱의 구조를 보존하는 관계
비선형(Nonlinear) 관계는 이러한 구조만으로 표현되지 않는 관계

## Intuition

선형 모델은 입력의 효과를 일정한 비율로 조합한다. 비선형 모델은 곡선, 경계, 상호작용처럼 더 복잡한 패턴을 표현할 수 있다.

## Mathematical Definition

선형 변환 $T$는 다음을 만족한다.

$$
T(a\mathbf{x}+b\mathbf{y})=aT(\mathbf{x})+bT(\mathbf{y})
$$

## Example

$y=2x+1$은 직선 관계이고, $y=x^2$는 입력에 따라 기울기가 달라지는 비선형 관계다.

## Related Concepts

- [[Function]]
- [[Linear Transformation]]
- [[Matrix]]
- [[Neural Networks/Multi-Layer Perceptron]]

## Applications in AI

비선형 활성화 함수는 신경망이 단순한 선형 조합을 넘어 복잡한 결정 경계와 함수 관계를 학습하도록 한다.
