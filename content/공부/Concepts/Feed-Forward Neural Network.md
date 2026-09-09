# Feed-Forward Neural Network

## FFN의 기본 원리
> 정보가 *Input에서 Output 방향으로만 전달*되며 **이전 계산 결과를 다시 참조하지 않음**
> 각 Sample을 *독립적으로 처리*하며 **Sample 간 상태를 유지하지 않음**
- [[Perceptron]]
    - 하나의 선형 결합과 Activation으로 출력을 계산
- [[Multi-Layer Perceptron]]
    - 여러 Fully Connected Layer와 Activation을 순차적으로 연결
    - 가장 전형적인 FFN 구조
```text
Input
  ↓
Fully Connected Layer
  ↓
Activation
  ↓
Fully Connected Layer
  ↓
Activation
  ↓
Output
```
- Linear Transformation 
	- 입력에 Weight와 Bias를 적용
	- $\mathbf{z} = W\mathbf{x} + \mathbf{b}$
- Activation Function
	- 비선형성을 추가하여 복잡한 관계를 학습
	- $\mathbf{h} = \phi(\mathbf{z})$

## FFN의 확장
> 여러 Layer를 쌓아 더 복잡한 특징과 비선형 관계를 학습
> 목적과 데이터 특성에 따라 다양한 Feed-Forward 구조로 확장
- Deep [[Multi-Layer Perceptron|MLP]]
	- MLP에 여러 Hidden Layer를 쌓은 구조  
	- 일반적으로 [[Deep Neural Network|DNN]]의 한 형태
- [[Convolutional Neural Network|CNN]]
    - 구조적으로 Feed-Forward 계열에 포함 가능
    - 이미지의 공간적 특징을 처리하기 위한 특화 구조로 주로 별도 분류

## FFN의 입력 처리
> 하나의 입력을 *고정된 Vector 형태*로 받아 Output까지 순방향으로 계산
> 각 Sample 사이의 *순서나 이전 상태를 명시적으로 유지하지 않음*

e.g. 이미지 분류
```text
Image Pixels
     ↓
    MLP
     ↓
 Cat / Dog
```

## 순차 데이터 처리의 한계
> 문장·음성·시계열처럼 *현재 입력의 의미가 이전 입력과 순서에 의존하는 데이터*를 직접 처리하기 어려움
> 이전 시점의 정보를 유지할 수 있는 별도의 상태 표현이 필요
- Sequence Modeling
    - [[Recurrent Neural Network]]

---

## Note
- Feed-Forward Neural Network
    - [[Perceptron]]
    - [[Multi-Layer Perceptron]]
    - [[Deep Neural Network]]
    - [[Convolutional Neural Network]]
- 대표적인 FFN 구조
    - [[Multi-Layer Perceptron]]
- Sequence 데이터 처리
    - [[Recurrent Neural Network]]