- 차원(Dimension)
    - 입력 변수의 개수
    - $x^2$: 1차원
    - $x+y$: 2차원
- 차수(Degree)
    - 변수의 지수 기준
    - $x^2$: 2차
    - $x+y$: 1차
- Linear(선형)
    - 변수들이 1차로만 결합
    - 예: $2x$, $x+y$, $2x+3y-z$
- Nonlinear(비선형)
    - 제곱, 변수 간 곱, 비선형 함수 등이 포함
    - 예: $x^2$, $xy$, $sin x$
- 중요
    - 차원이 높다고 Nonlinear인 것은 아님
    - $x+y+z$: 3차원이지만 Linear
    - $x^2$: 1차원이지만 Nonlinear
- Affine
    - $Ax+b$
    - $b=0$ 이면 Linear
    - $Linear⊂Affine$
# Linear and Nonlinear

## Definition
- Linear(선형): 변수들이 1차 형태로 결합된 관계
- 다항식 기준으로 차수가 2 이상이면 일반적으로 Nonlinear
- 선형대수에서는 엄밀히 f(x)=Axf(x)=Ax 형태를 의미

## Intuition
- 입력이 변하면 출력도 일정한 비율과 방향으로 변함
- 그래프 관점
    - 1차원 입력: 직선
    - 2차원 이상: 평면 또는 초평면
- 차원이 높아도 Linear일 수 있음
    - x+y+zx+y+z: 3차원이지만 Linear
## Mathematical Definition
- 차원: 입력 변수의 개수
    - $x^2$: 1차원
    - $x+y$: 2차원
- 차수: 변수의 지수
    - $x^2$: 2차
    - $x+y$: 1차
- Linear transformation 조건

f(x+y)=f(x)+f(y)f(x+y)=f(x)+f(y) f(cx)=cf(x)f(cx)=cf(x)

- 일반형

f(x)=Axf(x)=Ax

- f(x)=Ax+bf(x)=Ax+b는 b≠0b\neq0일 때 **Affine**
## Example

$y=2x+1$은 직선 관계이고, $y=x^2$는 입력에 따라 기울기가 달라지는 비선형 관계다.

## Related Concepts

- [[Function]]
- [[Linear Transformation]]
- [[Matrix]]
- [[Neural Networks/Multi-Layer Perceptron]]

## Applications in AI

비선형 활성화 함수는 신경망이 단순한 선형 조합을 넘어 복잡한 결정 경계와 함수 관계를 학습하도록 한다.
