# Multi-Layer Perceptron
## MLP의 기본 원리
> 하나의 *[[Perceptron]]* 이 표현할 수 있는 *선형 관계의 한계*를 보완하기 위해 여러 Layer를 연결
> *Fully Connected Layer*와 *Activation Function*을 반복하여 복잡한 비선형 관계를 학습
- Input Layer
    - 입력 Feature를 전달
- Hidden Layer
    - Linear Transformation과 Activation을 반복
- Output Layer
    - 최종 예측값을 생성
```text
Input Layer
     ↓
Hidden Layer
     ↓
Activation
     ↓
Hidden Layer
     ↓
Activation
     ↓
Output Layer
```

## Layer의 연산
> 각 Layer는 이전 Layer의 출력을 입력으로 받아 *Weight와 Bias를 적용*
> Activation Function을 통해 비선형성을 추가

- $\mathbf{z}^{(l)} = W^{(l)}\mathbf{a}^{(l-1)}+\mathbf{b}^{(l)}$
	- 이전 Layer의 출력에 현재 Layer의 Weight와 Bias를 적용해 *선형 결합값*을 계산
- $\mathbf{a}^{(l)}=\phi\left(\mathbf{z}^{(l)}\right)$
	- 선형 결합값에 Activation Function을 적용해 *현재 Layer의 출력*을 계산

	- $\mathbf{a}^{(l-1)}$
	    - 이전 Layer의 출력
	- $W^{(l)}$
	    - 현재 Layer의 Weight
	- $\mathbf{b}^{(l)}$
	    - Bias
	- $\phi$
	    - Activation Function

## Fully Connected 구조
> 한 Layer의 각 Neuron이 *이전 Layer의 모든 Neuron이 다음 Layer의 _모든 Neuron_과 연결*
> 각 연결마다 독립적인 Weight를 학습

```text
x1 ─┬─→ h1
    ├─→ h2
    └─→ h3

x2 ─┬─→ h1
    ├─→ h2
    └─→ h3

x3 ─┬─→ h1
    ├─→ h2
    └─→ h3
```
- [[Fully Connected Layer]]
- [[Weight]]
- [[Bias]]

## 비선형 표현
> Linear Layer만 여러 개 연결하면 전체 연산은 결국 하나의 Linear Transformation과 동일
> Layer 사이에 *[[Activation Function]]* 을 추가하여 비선형 관계를 표현
- [[Sigmoid]]
- [[Tanh]]
- [[ReLU]]

```text
Linear
  ↓
Activation
  ↓
Linear
  ↓
Activation
```

→ 여러 Layer를 이용해 [[Perceptron]]으로 해결하기 어려운 XOR과 같은 비선형 문제를 학습 가능

## MLP의 학습
> Input에서 Output 방향으로 값을 계산하는 *Forward Propagation* 수행
> Loss를 계산한 뒤 *Backpropagation*을 통해 각 Layer의 Weight와 Bias를 업데이트

```text
Input
  ↓
Forward Propagation
  ↓
Prediction
  ↓
Loss
  ↓
Backpropagation
  ↓
Weight Update
```

- [[Forward Propagation]]
- [[Loss Function]]
- [[Backpropagation]]
- [[Gradient Descent]]

## MLP의 확장과 한계
> Hidden Layer를 여러 층으로 깊게 구성한 MLP는 *[[Deep Neural Network]]의 한 형태*로 볼 수 있음
> 하지만 데이터의 *공간적 구조나 시간적 순서를 명시적으로 활용하는 구조는 없음*
- 공간적 구조를 가진 입력
	- Fully Connected 구조에서는 Parameter 수가 빠르게 증가
	- 공간적 관계를 명시적으로 활용하기 어려움
	- [[Convolutional Neural Network]]

- Sequence 입력
	- 이전 입력에 대한 State를 직접 유지하지 않음
	- [[Recurrent Neural Network]]

---

## Note
- 구성
    - Input Layer
    - Hidden Layer
    - Output Layer
- 기본 연산
    - Linear Transformation
    - [[Fully Connected Layer]]
    - [[Activation Function]]
- 학습
    - [[Forward Propagation]]
    - [[Backpropagation]]
    - [[Gradient Descent]]
- 확장
    - [[Deep Neural Network]]
    - [[Convolutional Neural Network]]
- Sequence 데이터 처리
    - [[Recurrent Neural Network]]
