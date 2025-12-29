---
draft: false
title: "ResNet: Deep Residual Learning for Image Recognition"
tags:
  - paper_review
---
# 📄 Deep Residual Learning for Image Recognition
**Authors:** Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun 
**Affiliation:** Microsoft Research, Xi’an Jiaotong University 
**Conference:**  (1449673200000) 
**DOI:** [10.48550/arXiv.1512.03385](https://doi.org/10.48550/arXiv.1512.03385) 
- **Keywords:** \[Residual, IdentityShorcut, Degradation, ] 

---

### 연구 배경
![[/static/images/ResNet_fig_1.png]]
>_"Is learning better networks as easy as stacking more layers?"_
>(네트워크를 깊게 쌓기만 하면 성능이 좋아질까?)
- Xavier/He 초기화, BatchNorm 등으로 **Vanishing/Exploding**은 어느 정도 해결
	- 그러나 **깊어질수록 성능이 떨어지는 문제(degradation)** 는 여전히 해결되지 않음
- 얕은 네트워크의 최적해를 깊은 네트워크도 구현할 수 있음이 보장되는데,
	- 실제로는 **깊을수록 훈련 오차가 증가하는 Degradation Problem**
→ 이 모순이 ResNet의 출발점.

---

### 주요 아이디어
 >**Residual Learning**으로 최적화 난이도 해결
 >**Identity Shortcut**으로 gradient 흐름 문제 해결
 >**Bottleneck Architerture**로 [[FLOPs]] 감소로 깊이 확장 > 성능 향상
#### **Residual learning의 이해**
> 새로운 레이어가 수행해야 할 목표 가정: **항등함수 $H(x)=x$**

**기존 CNN**
- 문제 상황
	- 네트워크는 $H(x) = x$ **항등함수(identity function)** 를 직접 구현해야 함.
	- 네트워크 구성: Conv → BN → ReLU → Conv … 같은 **비선형 스택**
	- 항등함수를 이런 비선형 조합으로 정확히 구현하는 것은 **매우 어려운 최적화 문제**.
- 왜 어려운가?
	- 파라미터가 조금만 변해도 입력 $x$ 가 쉽게 **왜곡됨**
	- 깊은 레이어일수록 항등함수 유지가 **거의 불가능**
	- 역전파가 항등함수 해에 수렴하기 **매우 어려움**
	  → 깊은 비선형 네트워크에게 "그냥 아무것도 하지 마라"는 요구는 **극도로 난이도가 높음**.

**ResNet**
- ResNet은
	- $H(x)=F(x)+x$ / $H(x)-x=F(x)$ 
- 목표 함수가 $H(x)=x$라면,
	- $F(x)=H(x)−x=0$

**의미**
- 레이어는 **항등함수 전체**를 만드는 대신 단순히 **출력을 0으로 만드는 것만 학습**하면 됨.

- [[Initialization&Normalization|현대 초기화는 모든 weight를 0 근처의 작은 값 으로 둠.]]
	- Xavier/He 초기화는 대부분의 가중치가 이미 **0 근처**
	- 따라서 Residual block의 초깃값은 자연스럽게
$$F(x)\approx 0$$
→ 즉, 이미 **항등함수에 매우 가까운 상태로 시작**

- 파라미터를 $0$ 근처로 두는 것이 훨씬 쉬움
→ 최적화 난도 대폭 감소.


**기존 방식**
- 요구: “비선형 레이어 스택으로 복잡한 항등함수 $H(x)=x$를 구현하라.”
- 결과: **극도로 어려운 최적화 문제**

**Residual 방식**
- 요구: “잔차 $F(x)$ 를 $0$으로 만들어라.”
- 결과: 파라미터를 $0$ 근처에 위치시키면 됨 → **매우 쉬움**

**요약**
- **비선형 레이어 스택에게 항등함수 $H(x)=x$ 를 만들게 하는 것은 어렵다.**
- **잔차 $F(x)$ 를 $0$으로 만들게 하는 것은 압도적으로 쉽다.**  
→ 그래서 ResNet은 매우 깊어도 학습이 잘됨.

#### **Identity Shortcut**
- Shortcut connection은 입력 $x$를 **그대로 다음 블록으로 전달**하는 경로.
- 파라미터가 없는 **항등 맵핑(identity mapping)** 이므로 연산량 증가 없음.
- Residual block의 출력은
    $H(x)=F(x)+x$
    으로 계산됨.

왜 필요한가?
1. **항등함수를 “구조적으로” 쉽게 만들어서 최적화가 쉬워짐**
	- 기존 CNN은 비선형 조합으로 $H(x)=x$를 만들어야 하므로 어려움
	- identity shortcut은 $x$를 그대로 전달함으로써 
	- **항등함수를 네트워크 구조 차원에서 보장**
	- → Residual block이 “$x$ 그대로 전달 + 작은 변화 $F(x)$”만 학습하면 됨
2. **Gradient 흐름을 직접 전달하여 깊어져도 학습이 망가지지 않음**
	- 역전파 시 gradient가 shortcut을 통해 아래처럼 흐름 _(BN, ReLU의 도함수 X)_
	$$\frac{\partial H}{\partial x} = 1 + \frac{\partial F}{\partial x}​$$
	- 초기화 시점에서는 $\frac{\partial F}{\partial x} \approx 0$ 이므로  
	    gradient $\approx 1$
	    → gradient가 소실되지 않고, 깊은 네트워크도 안정적으로 학습됨
	- 이후 학습이 진행되면서 $F(x)$는 필요한 residual 값을 학습

**Shortcut 옵션 (Dimension mismatch 처리)**
- **Option A — Zero-padding identity shortcut**
    - 차원이 늘어날 때 부족한 채널을 0으로 채우기
	    - (0으로 채운 곳은 residual learning X)
	- 파라미터가 없는 **완전한 identity mapping**
	- 가장 가볍고 단순한 방식
- **Option B — Projection shortcut (1×1 convolution)**
	- 입력/출력 채널이 다를 때 **1×1 Conv로 projection**을 수행하여 shape matching
	- projection layer가 추가되므로 **연산량이 소폭 증가**
- **Option C — Full projection shortcut**
	- 모든 shortcut을 1×1 Conv projection으로 대체하는 방식
	- projection layer 수가 많아져 FLOPs·메모리 모두 크게 증가

세 옵션 모두 plain network보다 훨씬 좋은 성능을 보이며,  
- A는 가장 가볍고 기본
- B는 약간 더 정확,
- C는 미세하게 더 정확하지만 비효율
ResNet은 A/B 기반의 “Residual + Identity Shortcut” 구조

**요약**
- ResNet의 핵심은 “Residual + Identity Shortcut” 조합
	- Residual 개념과 Shortcut으로 구현
- Projection은 차원 맞추기가 필요한 경우에만 보조적으로 사용

#### **Bottleneck Architecture**
 **핵심 아이디어**
> **3×3 Conv는 연산량이 가장 크므로,  
> 채널을 줄여서 연산한 뒤 다시 복원**

- Conv의 연산량(FLOPs)은
$$H \times W \times C_{in} \times C_{out} \times 3 \times 3$$
-> **채널 수(C)가 클수록 FLOPs가 $C^2$에 비례하여 증가**
  

**구조**
1) **1×1 Conv (차원 축소)**
	- $e.g.$ 256 → 64
	- 연산량 거의 없음
2) **3×3 Conv (특징 추출)**
	- 축소된 채널(64ch)에서 연산 수행
3) **1×1 Conv (차원 복원)**
	- $e.g.$ 64 → 256
	- 원래 채널 수로 되돌림

---

### **방법론**
- **모델 구조**
![[/static/images/resnet50_arhitecture.webp]]
1. **Stem (입력 처리 구간)**
	그림의 ZERO PAD → CONV → BN → ReLU → MAX POOL
	이 부분은 **Stage로 보기 이전의 초기 처리 단계**
	- 7×7 Conv (stride=2) — 특징 추출
	- BatchNorm, ReLU — 정규화 + 비선형 변환
	- MaxPool — 공간 크기 절반으로 축소
	이후 Stage에서 bottleneck block이 처리하기 좋은 크기로 **초기 Feature Extractor**
	**출력: 56×56, 64ch**
2. **Stage 2 (conv2_x)**: Residual + Bottleneck
	- **CONV BLOCK (파란색) = Bottleneck block + Projection shortcut**
		- Stage가 바뀌는 순간(해상도/채널 증가) → dimension mismatch 발생
		- shortcut이 x를 그대로 전달할 수 없음
		- 따라서 projection shortcut(1×1 conv)이 필요한 block이 **CONV BLOCK**
	- **ID BLOCK ×2 (빨간색) = Bottleneck block + Identity shortcut**
		- 입력/출력 dimension이 같기 때문에 정확히 **Identity shortcut만 사용** → ID BLOCK
3. **Stage 3,4,5**
	- Stage 3: CONV BLOCK + ID BLOCK ×3
	- Stage 4: CONV BLOCK + ID BLOCK ×5
	- Stage 5: CONV BLOCK + ID BLOCK ×2
4. **Head (출력 처리 구간)**
	- **AVG POOL [[GAP|(Global Average Pooling)]]**
	- **[[Flatten]]** (생략 가능)
	- **FC (Fully Connected Layer)**
	- **[[Softmax]]**
- **output - Final prediction**


**Stage** 
"해상도(spatial size)·채널(channel) 유지되는 Block 묶음"
- 첫 block: **CONV BLOCK (projection shortcut)**  
    → Stage 전환으로 인해 dimension mismatch 해결
- 나머지 block들: **ID BLOCK (identity shortcut)**  
    → block 내부에서는 채널/해상도 변하지 않음  
    → shortcut이 그대로 x를 전달해도 차원 OK
**Block 내부 구조**
- CONV BLOCK / ID BLOCK 내부는 Bottleneck 구조
	1×1 Conv (축소) → 3×3 Conv → 1×1 Conv (복원)
- "입력과 출력 채널은 항상 동일"

요약
1) Stage는 여러 Residual Block들의 묶음
2) 첫 block은 projection shortcut이 필요하여 CONV BLOCK
3) 나머지는 identity shortcut으로 ID BLOCK
4) 모든 block 내부는 Bottleneck 구조
5) Stage가 바뀔 때 dimension mismatch가 생김

- **모듈 구성**
**A. Stem 모듈**

| 구성 요소                                                                              | 설명                                                                                                                                                  |
| ---------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Zero Padding**                                                                   | 입력 텐서의 공간 크기를 유지하기 위해 주변을 0으로 채운다. 이로써 합성곱 필터가 경계 영역에서도 동일한 연산 범위를 갖는다. ResNet에서는 stem의 7×7 conv와 residual block 내부의 3×3 conv에서 출력 해상도 유지를 위해 사용된다. |
| **7×7 Conv**                                                                       | 입력 이미지의 넓은 수용영역을 한 번에 처리하여 저해상도 특징을 빠르게 추출한다. stride=2를 적용해 공간 크기를 절반으로 줄이며, 초기 특징지도를 구성한다.                                                         |
| **[[DRAFTED-Batch Normalization\|BatchNorm]]**                                     | 합성곱 연산 뒤에 배치되어 internal covariate shift를 완화하고 학습 안정성을 확보한다.                                                                                         |
| **[[DRAFTED-Rectified Linear Units Improve Restricted Boltzmann Machines\|ReLU]]** | 비선형성을 부여하며, 음수 영역을 0으로 절단하여 gradient 흐름을 유지한다.                                                                                                      |
| **[[Max Pooling\|MaxPool]]**                                                       | 큰 stride의 풀링을 통해 해상도를 빠르게 축소하고, 이후 residual block이 처리하기 적합한 크기의 특징지도를 생성한다.                                                                         |

**B. Residual Block 모듈**

| 구성 요소                              | 설명                                                                                                                                                         |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Convolution + BatchNorm + ReLU** | 각 residual branch는 conv → [[DRAFTED-Batch Normalization\|BN]]   → [[DRAFTED-Rectified Linear Units Improve Restricted Boltzmann Machines\|ReLU]] 순으로 구성된다. |
| **Basic Block / Bottleneck Block** |                                                                                                                                                            |
| _Basic Block_                      | 3×3 conv × 2 구조로 shallow 네트워크(ResNet-18/34)에 사용.                                                                                                           |
| _Bottleneck Block_                 | 1×1 → 3×3 → 1×1 conv 구조로 deep 네트워크(ResNet-50/101/152)에 사용.                                                                                                 |
| **Shortcut 종류**                    |                                                                                                                                                            |
| _Identity Shortcut_                | 입력과 출력의 채널 수가 동일할 때 직접 더함                                                                                                                                  |
| _Projection Shortcut_              | 차원이 다를 경우 1×1 conv를 이용해 선형 변환                                                                                                                              |
| _Zero-Padding Shortcut_            | 출력 채널이 많을 때, 부족한 부분을 0으로 채워 일치시킴.                                                                                                                          |
| **Element-wise Addition**          | 변환된 잔차 $F(x)$와 입력 $x$를 element-wise로 더하여 최종 출력을 형성.                                                                                                        |
C. Head 모듈
- [[GAP|Global Average Pooling]]  
- Fully Connected Layer  
- [[Softmax]]

D. 학습 안정 모듈
- [[He initialization]]  
- BatchNorm의 scaling / shifting  
- Fully convolutional inference


- **학습 구성**
~~데이터 증강 방법
Optimizer와 하이퍼파라미터
Learning rate 스케줄
Dropout 미사용
Multi-scale testing~~

---

### 실험 결과 및 분석
~~Plain vs Residual 비교 (18/34-layer)
Degradation 문제 시각화(Figure 4)
Shortcut 옵션(A/B/C) 실험 결과
Bottleneck 깊이 증가(50/101/152-layer) 성능
SOTA 비교(ILSVRC 2015)~~


---

### 결론 및 시사점
~~Residual Learning의 영향
Shortcut의 본질적 의의
깊은 네트워크 설계에 대한 새로운 기준
후속 연구(Pre-activation ResNet 등)로 이어진 흐름~~

---

### 개인 코멘트
- 이해가 어려웠던 부분  
- 추가로 찾아볼 개념 (논문 내 용어·참고 문헌 등)
	- ResNet v2: Pre-activation (later variant)

- 추가 참고할 논문

(1) Normalized Initialization 계열
- Xavier Initialization
Glorot & Bengio — “Understanding the Difficulty of Training Deep Feedforward Neural Networks” (2010)
- He Initialization (ReLU 최적화)
He et al. — “Delving Deep into Rectifiers: Surpassing Human-Level Performance on ImageNet Classification” (2015)

(2) Intermediate Normalization Layers 계열
Batch Normalization
Ioffe & Szegedy — “Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift” (2015)
