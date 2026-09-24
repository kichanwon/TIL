# Linear and Nonlinear
## Definition
- Linear(선형): 변수들이 1차 형태로 결합된 관계
- 다항식 기준으로 차수가 2 이상이면 일반적으로 Nonlinear
- 선형대수에서는 엄밀히 $f(x)=Ax$ 형태를 의미

## Intuition
- 입력이 변하면 출력도 일정한 비율과 방향으로 변함
- 그래프 관점
    - 1차원 입력: 직선
    - 2차원 이상: 평면 또는 초평면
- 차원이 높아도 Linear일 수 있음
    - $x+y+z$: 3차원이지만 Linear
## Mathematical Definition
- 차원: 입력 변수의 개수
    - $x^2$: 1차원
    - $x+y$: 2차원
- 차수: 변수의 지수
    - $x^2$: 2차
    - $x+y$: 1차
- Linear transformation 조건
$$f(x+y)=f(x)+f(y)$$
$$f(cx)=cf(x)$$
- 일반형
$$f(x)=Ax$$

- $f(x)=Ax+b$는 $b\neq0$일 때 **_Affine_**
## Example
$y=2x+1$은 직선 관계이고, $y=x^2$는 입력에 따라 기울기가 달라지는 비선형 관계다.

## Related Concepts
- Dimension
    - 입력 변수의 개수
- Degree
    - 다항식의 차수
- Nonlinear
    - $x^2$, $xy$, $\sin x$ 등
- Affine
    - $Ax+b$
- [[Function]]
- [[Linear Transformation]]
- [[Matrix]]
- [[Neural Networks/Multi-Layer Perceptron]]

## Applications in AI
- Linear Regression
	- $y=w^Tx+b$
- Logistic Regression의 선형 결합 부분
	- $z=w^Tx+b$
- Neural Network의 Linear Layer
	- $z=Wx+b$
- 신경망에서는 Linear/Affine 변환 뒤에 ReLU, Sigmoid 같은 **_Nonlinear Activation_** 을 적용해 복잡한 결정 경계와 함수 관계를 학습함