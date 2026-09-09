# Perceptron
## Perceptron의 기본 원리
> 여러 입력에 각각 *Weight*를 적용한 뒤 합산하고, *Activation Function* 을 통해 하나의 출력을 생성
> 입력을 두 Class로 구분하는 가장 기본적인 *Linear Binary Classifier*
- Input
    - 여러 Feature를 하나의 Vector로 입력
- Weight
    - 각 입력 Feature가 출력에 미치는 영향의 크기와 방향을 결정
- Bias
    - Decision Boundary의 위치를 조정
- Activation Function
    - 계산 결과를 최종 출력으로 변환
    - $z = \sum_{i=1}^{n} w_i x_i + b$
    - $\hat{y} = \phi(z)$

## 선형 결합과 분류
> 입력 Feature를 Weight와 함께 *선형 결합*하여 하나의 값을 계산
> 계산된 값이 Threshold를 넘는지에 따라 Class를 결정

- $z = \mathbf{w}^{T}\mathbf{x} + b$
	- 입력 Feature에 각각 Weight를 적용해 합산하고 Bias를 더해 
	  *분류 기준이 되는 선형 결합값*을 계산

대표적으로 Step Function을 사용
- $\phi(z)=\begin{cases}1, & z \geq 0 \\ 0, & z < 0\end{cases}$
	- 선형 결합값 $z$가 기준값 이상인지에 따라 *0 또는 1의 Class로 변환*

```text
x1 ── w1 ─┐
x2 ── w2 ─┼→ Weighted Sum + Bias → Activation → Output
x3 ── w3 ─┘
```

## Perceptron의 학습
> 예측값과 실제값의 차이를 이용해 *Weight와 Bias를 반복적으로 수정*
> 잘못 분류된 Sample을 기준으로 Decision Boundary를 이동
- Weight Update
	- $w_i \leftarrow w_i + \eta (y - \hat{y})x_i$
- Bias Update
	- $b \leftarrow b + \eta (y - \hat{y})$
		- $\eta$ : Learning Rate
		- $y$ : 실제 Label
		- $\hat{y}$ : 예측값

## 표현 능력과 한계
> 하나의 Perceptron은 하나의 *Linear Decision Boundary*만 학습 가능
> 따라서 Linear Separable한 문제는 처리할 수 있지만 복잡한 비선형 관계는 표현하기 어려움
- Linear Separable
    - AND
    - OR
- Non-Linearly Separable
    - XOR
        - 하나의 Perceptron으로 해결 불가능

→ 여러 Perceptron을 Layer 형태로 연결
→ [[Multi-Layer Perceptron]]

---

## Note
- 기본 구성
    - Input
    - Weight
    - Bias
    - Activation Function
- 기본 연산
    - Linear Combination
    - Step Function
- 학습
    - Perceptron Learning Rule
    - Learning Rate
- 분류 특성
    - Linear Decision Boundary
    - Linear Separability
- 한계
    - XOR Problem
    - [[Multi-Layer Perceptron]]


## 참고
- [딥 러닝을 이용한 자연어 처리 입문 - RAG, 에이전트, LLM 파인튜닝까지](https://wikidocs.net/24958)