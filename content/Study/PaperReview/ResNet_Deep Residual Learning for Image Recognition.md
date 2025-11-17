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
- 기존 CNN의 한계
- 깊은 네트워크의 성능 저하(Degradation Problem)
- 초기화·정규화로 해결된 문제와 남은 문제
- Residual Learning이 필요한 이유

>새로 추가한 레이어들을 y = x(항등함수)로 두고, 기존 얕은 네트워크의 가중치를 그대로 복사하면, 더 깊은 네트워크도 얕은 네트워크와 똑같은 해를 낼 수 있다.
>따라서 더 깊게 만들었는데 훈련 오차가 더 커질 이유가 없다.
>하지만 실제로는 깊어질수록 성능(훈련 오차)이 오히려 나빠지고, 그 해에도 도달하지 못하며 학습도 더 어렵고 오래 걸린다.

- 깊은 CNN은 더 높은 성능을 낼 것처럼 보이지만 실제로는 반대로 **깊을수록 훈련 오차가 증가하는 Degradation Problem**이 발생
- Xavier/He 초기화, BatchNorm 등으로 **Vanishing/Exploding**은 어느 정도 해결되었음
    → 그러나 **깊어질수록 성능이 떨어지는 문제(degradation)**는 여전히 해결되지 않음
- 깊은 네트워크가 **항등함수 H(x)=x**를 직접 구현하는 것이 매우 어려움
- 따라서 “층을 쌓으면 자동으로 좋아진다”는 가설이 깨지고, **Residual Learning이 필요하게 됨**

> “얕은 네트워크의 최적해를 깊은 네트워크도 구현할 수 있음이 보장되는데, 실제로는 깊을수록 훈련 오차가 커진다.”  
> → 이 모순이 ResNet의 출발점.

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
    $H(x)-x=F(x)$ / $H(x)=F(x)+x$
- 목표 함수가 $H(x)=x$라면,
    $F(x)=H(x)−x=0$

**의미**
- 레이어는 **항등함수 전체**를 만드는 대신
    단순히 **출력을 0으로 만드는 것만 학습**하면 됨.

- 현대 초기화는 모든 weight를 **0 근처의 작은 값**으로 둠.
	Xavier/He 초기화는 대부분의 가중치가 이미 **0 근처**
	따라서 Residual block의 초깃값은 자연스럽게
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

#### **Bottleneck Architecture**


---

### 방법론
- 모델 구조
Residual block 구조(기본)
Bottleneck block 구조
Downsampling 전략(stride 2 conv)
Network depth (34, 50, 101, 152-layer)

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


## 1) 딥러닝 초기화에서 기본적으로 **파라미터는 0 주변의 작은 random 값**으로 설정됨
- Xavier / Kaiming initialization  
    → 대부분의 weight가 **이미 0 근처**임
- Residual block에서 F(x)는 _학습 초기부터 매우 작은 값_  
    즉 **항등함수에 매우 가까운 상태**에서 시작됨
➡️ **애초에 최적점(=0)이랑 멀지 않으니 도달이 쉬움**
## 2) "비선형 조합으로 항등함수 만들기"보다 훨씬 단순한 목표
기존 네트워크라면:
$Conv + BN + ReLU + Conv + ... → H(x)=x\text{Conv + BN + ReLU + Conv + ... → }$
$H(x) = xConv + BN + ReLU + Conv + ... → H(x)=x$

이걸 만족시키려면…
- Conv 필터가 매우 특정한 형태가 되어야 함
- BN scaling/shift가 절묘하게 조정되어야 함
- ReLU가 입력을 찌그러뜨리지 않도록 값의 분포도 유지해야 함

즉 **매우 복잡한 조합이 필요**.
반면 Residual에서는:
$F(x)=0$
이를 위해 필요한 조건:
- 모든 weight가 **0 근처**
- bias도 **0 근처**
이게 끝.  
즉, **복잡한 조합 → 단순히 모두 작은 값**으로 바뀜.
➡️ **조건이 단순하므로 최적화가 훨씬 쉬움**
## 3) “작은 weight로 만드는 F(x)”는 연산적으로 안정적
작은 weight는 다음 장점이 있음:
- 활성값 폭발 없음
- ReLU에서 dead neuron 문제 감소
- BN이 안정적으로 작동
- gradient도 안정적으로 흐름
즉, **학습이 흔들리지 않고 자연스럽게 0을 향함**
## 4) Gradient가 shortcut을 통해 그대로 전달되므로 "학습이 망가지지 않음"
Residual block에서는 gradient가 다음처럼 흐름:
$∂H∂x=1+∂F∂x\frac{\partial H}{\partial x} = 1 + \frac{\partial F}{\partial x}∂x∂H​=1+∂x∂F​$

초기에는 $∂F∂x\frac{\partial F}{\partial x}∂x∂F$​가 매우 작음 → 0 근처  
그러므로 gradient는 거의 **1**.
즉:
- F(x)가 0 근처여도 gradient는 잘 흐름
- 학습이 끊기지 않음
- 안정적으로 최적점 근처에서 fine-tuning 가능
➡️ **0 근처 파라미터를 유지하는 것이 gradient 측면에서도 자연스럽고 안정적**
# 🧠 핵심 요약
> **기존 네트워크:**  
> 항등함수 만들려면 “지극히 복잡한 weight 조합”을 찾아야 함  
> → 매우 어려움

> **Residual 네트워크:**  
> 항등함수 만들려면 “F(x)를 0으로 만들기 = weight를 작은 값으로 두기”  
> → 초기값 자체가 이미 0 근처  
> → 학습도 0 근처에서 안정적으로 유지  
> → 매우 쉽고 빠르게 최적점 도달

즉,
### **“파라미터를 0 근처로 둔다” = “최적해가 weight=0 쪽에 있기 때문에 학습하기 매우 쉬운 형태”**