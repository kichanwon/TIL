# Convolutional Neural Network
## CNN의 기본 원리
> [[Multi-Layer Perceptron]]의 Fully Connected 구조는 
> 	이미지의 *공간적 관계를 직접 활용하기 어렵고 Parameter 수가 빠르게 증가*
> CNN은 *Convolution 연산*을 통해 이미지의 지역적 특징을 추출하고, 
> 	동일한 Filter를 전체 영역에서 공유하여 효율적으로 공간 정보를 학습
- [[Convolution]]
    - 작은 Filter를 입력의 여러 위치에 적용하여 지역적 특징을 추출
- [[Feature Map]]
    - Filter가 입력을 순회하며 생성한 특징 표현
- [[Weight Sharing]]
    - 동일한 Filter의 Weight를 모든 위치에서 공유
- [[Local Connectivity]]
    - 전체 입력이 아니라 주변의 작은 영역만 연결

```text
Input Image
     ↓
Convolution
     ↓
Activation
     ↓
Feature Map
     ↓
Pooling
     ↓
Convolution
     ↓
...
     ↓
Classification
```

## Convolution 연산
> 작은 크기의 *Kernel(Filter)* 을 입력 위에서 이동시키며 해당 영역과의 연산을 수행
> 이미지 전체를 한 번에 연결하지 않고 *지역적인 패턴*을 반복적으로 탐색

입력 $X$와 Kernel $K$에 대해 기본적인 2차원 Convolution은 다음과 같이 표현
$$Y(i,j) = \sum_m \sum_n X(i+m,j+n)K(m,n)$$
- $X$
    - Input
- $K$
    - Kernel 또는 Filter
- $Y$
    - 생성된 Feature Map
- $(i,j)$
    - Filter가 적용되는 위치

```text
Input

┌─────────────┐
│ ■ ■ ■       │
│ ■ ■ ■       │
│ ■ ■ ■       │
│             │
│             │
└─────────────┘
   ↑
 3×3 Kernel
```

## Feature 추출
> 초기 Layer에서는 Edge나 Texture와 같은 *단순한 특징*을 추출
> 
> Layer가 깊어질수록 이전 Feature를 조합하여 *더 복잡하고 추상적인 특징*을 학습

```text
Input Image
    ↓
Edge / Line
    ↓
Texture / Pattern
    ↓
Object Part
    ↓
Object
```

- Low-Level Feature
    - Edge
    - Line
    - Texture
- High-Level Feature
    - Shape
    - Object Part
    - Object Representation

## Weight Sharing과 Parameter 효율성
> MLP는 각 입력과 Neuron 사이에 서로 다른 Weight가 필요
> CNN은 하나의 Filter를 여러 위치에서 반복 사용하여 *Parameter 수를 크게 줄임*

MLP의 Fully Connected 연산
$\mathbf{y} = W\mathbf{x}+\mathbf{b}$

CNN에서는 작은 Kernel의 Weight를 전체 공간에 공유
```text
같은 Kernel

[ K ] → Image 좌측
[ K ] → Image 중앙
[ K ] → Image 우측

모든 위치에서 동일한 Weight 사용
```
→ 이미지 크기가 커져도 모든 Pixel마다 독립적인 Weight를 만들 필요가 없음

## Stride와 Padding
> Kernel이 입력 위를 이동하는 간격과 입력 가장자리의 처리 방법에 따라 Feature Map의 크기가 결정
- [[Stride]]
    - Kernel이 한 번에 이동하는 간격
- [[Padding]]
    - 입력 가장자리에 값을 추가하여 출력 크기를 조절

출력 크기
$$O = \left\lfloor \frac{N+2P-K}{S} \right\rfloor +1$$

- $N$: Input Size
- $K$: Kernel Size
- $P$: Padding
- $S$: Stride
- $O$: Output Size

## Pooling
> Feature Map의 공간 크기를 축소하여 계산량을 줄이고 중요한 특징을 유지
> 작은 위치 변화에 대한 민감도를 줄이는 역할도 수행
- [[Max Pooling]]
    - 영역 내 가장 큰 값을 선택
- [[Average Pooling]]
    - 영역 내 평균값을 사용

```text
Feature Map
    ↓
2×2 Max Pooling
    ↓
Smaller Feature Map
```

## CNN의 전체 구조
> Convolution Layer에서 특징을 추출하고, 깊은 Feature를 기반으로 최종 예측을 수행

전통적인 CNN 구조
```text
Input
  ↓
Convolution
  ↓
Activation
  ↓
Pooling
  ↓
Convolution
  ↓
Activation
  ↓
Pooling
  ↓
Flatten
  ↓
Fully Connected
  ↓
Output
```
- Feature Extraction
    - Convolution
    - Activation
    - Pooling
- Prediction
    - Fully Connected Layer
    - Output Layer

## CNN의 학습
> Filter의 값도 사람이 직접 정하는 것이 아니라 *Backpropagation을 통해 학습*
> Loss를 최소화하도록 Convolution Kernel과 이후 Layer의 Weight를 함께 업데이트
- [[Forward Propagation]]
- [[Loss Function]]
- [[Backpropagation]]
- [[Gradient Descent]]
```text
Input
  ↓
Feature Extraction
  ↓
Prediction
  ↓
Loss
  ↓
Backpropagation
  ↓
Kernel / Weight Update
```

## CNN의 특징과 한계
> CNN은 이미지의 *공간적 구조와 지역적 패턴*을 효과적으로 활용
> 하지만 멀리 떨어진 영역 간 관계를 직접 모델링하려면 여러 Layer를 거쳐야 함
- 장점
    - Local Feature 추출
    - Weight Sharing
    - Parameter 효율성
    - 공간적 구조 활용
- 한계
    - 장거리 관계를 직접 참조하기 어려움
    - 깊은 구조에서 넓은 [[Receptive Field]]가 필요
    - Sequence의 시간적 상태를 직접 유지하지 않음
        - [[Recurrent Neural Network]]
    - 전역적 관계를 직접 참조하는 구조
        - [[Attention]]
        - [[Vision Transformer]]

---

## Note
- 기본 연산
    - [[Convolution]]
    - [[Kernel]]
    - [[Feature Map]]
    - [[Activation Function]]
- 연결 구조
    - [[Local Connectivity]]
    - [[Weight Sharing]]
    - [[Receptive Field]]
- Feature Map 제어
    - [[Stride]]
    - [[Padding]]
    - [[Pooling]]
- 학습
    - [[Forward Propagation]]
    - [[Loss Function]]
    - [[Backpropagation]]
    - [[Gradient Descent]]
- 대표 구조
    - [[LeNet]]
    - [[AlexNet]]
    - [[VGG]]
    - [[ResNet]]
- 확장
    - [[Vision Transformer]]