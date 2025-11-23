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
- **Keywords:** \[Residual, ]

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
- [>] **채널 수(C)가 클수록 FLOPs가 $C^2$에 비례하여 증가**
  

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

### 방법론
- 모델 구조
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

- 네트워크 구성 요소
Batch Normalization
He Initialization
Global Average Pooling
Zero-padding identity shortcut
Fully convolutional inference

- 학습 구성
데이터 증강 방법
Optimizer와 하이퍼파라미터
Learning rate 스케줄
Dropout 미사용
Multi-scale testing

---

### 실험 결과 및 분석

Plain vs Residual 비교 (18/34-layer)
Degradation 문제 시각화(Figure 4)
Shortcut 옵션(A/B/C) 실험 결과
Bottleneck 깊이 증가(50/101/152-layer) 성능
SOTA 비교(ILSVRC 2015)

---

### 결론 및 시사점

Residual Learning의 영향
Shortcut의 본질적 의의
깊은 네트워크 설계에 대한 새로운 기준
후속 연구(Pre-activation ResNet 등)로 이어진 흐름

---

### 개인 코멘트
- 이해가 어려웠던 부분  
- 추가로 찾아볼 개념 (논문 내 용어·참고 문헌 등)
	- Pre-activation (later variant)

- 추가 참고할 논문
(1) Normalized Initialization 계열
Xavier Initialization
Glorot & Bengio — “Understanding the Difficulty of Training Deep Feedforward Neural Networks” (2010)

He Initialization (ReLU 최적화)
He et al. — “Delving Deep into Rectifiers: Surpassing Human-Level Performance on ImageNet Classification” (2015)

(2) Intermediate Normalization Layers 계열
Batch Normalization
Ioffe & Szegedy — “Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift” (2015)


---



# 메모

## ✔ **x = 블록의 입력(feature map)**
- 이전 레이어에서 넘어온 값
- “원본 정보”
## ✔ **F(x) = Residual 함수가 학습해야 할 것**
- **블록 내부 레이어(Conv-BN-ReLU-Conv...)가 만들어낸 변환**
- 즉, 학습 가능한 부분
- 초기화 때문에 처음에는 F(x) ≈ 0
## ✔ **H(x) = 블록의 전체 출력(목표 함수)**
- 이 블록이 **최종적으로 표현해야 하는 함수

|용어|의미|누가 정하는가|
|---|---|---|
|**x**|입력|이전 레이어가 출력한 feature|
|**F(x)**|학습해야 하는 “변화량”|네트워크(학습)|
|**H(x)**|블록이 최종적으로 표현해야 하는 목표 함수|데이터/학습 목적|


1. 딥러닝 초기에는 Vanishing/Exploding 때문에 깊게 쌓기 어려움.
   → Xavier/He 초기화, BatchNorm 덕분에 20~30층은 학습 가능.

2. 그런데 깊게 쌓으면 성능이 좋아져야 하는데,
   실제로는 deeper model이 shallower model보다 훈련 오차가 더 커지는
   “Degradation Problem”이 발생.

3. 깊은 신경망은 항등함수 H(x)=x를 직접 구현하기 매우 어려움.
   → 비선형 조합으로 x를 그대로 내보내는 것이 복잡함.
   → 초기화는 F(x)=0으로 가기 쉽게 해주지만, H(x)=x 자체는 어려움.

4. Residual을 도입.
   H(x)=F(x)+x 로 바꾸면, F(x)=0이 최적해가 되어
   깊은 네트워크에서도 최적화가 매우 쉬움.

5. Shortcut으로 gradient를 손실 없이 보냄.
6. Bottleneck Architecture로 연산량 감소.

