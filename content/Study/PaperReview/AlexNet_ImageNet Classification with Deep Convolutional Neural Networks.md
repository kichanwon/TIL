---
tags:
  - paper_review
draft: false
title: "ImageNet Classification with Deep Convolutional\rNeural Networks"
---
**DOI:** [10.48550/arXiv.1404.5997](https://doi.org/10.48550/arXiv.1404.5997)  

### 기본 정보
- 논문 제목: ImageNet Classification with Deep Convolutional Neural Networks
- 저자: Alex Krizhevsky, Ilya Sutskever, Geoffrey E. Hinton
- 소속: University of Toronto
- 학회: NIPS (NeurIPS 2012)
- 키워드
	\[ImageNet, GPU, ReLU, LRN, Overlapping Pooling, Data Augmentation, Dropout]

---

### 연구 배경
- 기존 이미지 분류는 **사람이 설계한 특징(handcrafted features)** 에 의존(예: SIFT, HOG) → 복잡한 패턴 학습 한계
- 딥러닝은 가능성이 있었지만 
  **학습 불안정성(gradient explosion/vanishing), 적절한 초기화 기법 부재, 컴퓨팅 자원 부족** 등으로 인해 
  실제 대규모 학습에 적용하기 어려웠음
- **대규모 데이터(ImageNet) + GPU 병렬 연산 + 개선된 [[CNN]] 구조**로 이를 해결하고자 함
	- 특히 **ImageNet**은 이전 데이터셋(MNIST, CIFAR-10 등)에 비해
	  규모가 100배 이상 크고, 라벨 품질이 상대적으로 높으며, 
	  다양한 물체 클래스(1,000종)를 포함해 
	  모델의 범용적 시각 표현 학습 능력을 평가하기에 적합한 벤치마크였음.
---

### 주요 아이디어
![[alexnet_architecture.webp]]
- 전체 **약 6천만(60M) 개의 파라미터**, **65만(650K)개의 뉴런**을 포함하여, 당시 기준으로 매우 대규모 네트워크 구조
- 각 층은 점진적으로 **저수준 → 고수준 특징**을 학습
- 주요 기법:
    - **ReLU Nonlinearity (Rectifies Linear Unit)** 
		- 기존 활성화 함수인 `tanh`나 `sigmoid`는 입력이 커질수록 **gradient가 0에 수렴(gradient vanishing)** 하는 문제 존재.  
		- ReLU는 **음수 입력을 0**으로, **양수는 그대로 통과**시키는 단순한 비선형 함수로, 
		  계산이 빠르고 gradient 소실이 거의 발생하지 않아 학습 속도와 안정성을 크게 향상.
		- 특히 GPU 병렬 학습 환경에서 연산 효율이 매우 높아, **대규모 CNN 학습을 실질적으로 가능하게 한 핵심 요인**이 되었음.
    - **LRN (Local Response Normalization)**
		- 생물학적 신경 모델에서 착안한 개념으로, 
		  한 뉴런의 활성도가 인접 뉴런의 활성도에 의해 억제되는 **‘local competition’** 현상을 모방.
		- 이는 특정 feature map이 과도하게 활성화되는 것을 방지하고,
		  **ReLU의 비선형 활성으로 인한 스파스(sparse)한 표현을 균형 있게 유지**하기 위한 정규화 기법.  
		- 당시에는 약간의 정확도 향상을 보였으나, 이후 연구에서는 **Batch Normalization**이 등장하면서 대체됨.
    - **Overlapping Pooling**
		- Pooling 윈도우가 겹치도록 설계하여 정보 손실을 줄이고, 모델의 일반화 성능 향상을 유도.  
    - **Dropout**
		- Fully Connected Layer에서 랜덤하게 뉴런을 비활성화하여 **co-adaptation(공적응)** 을 방지하고 과적합을 줄임.
    - **Data Augmentation**
		- 랜덤 크롭, 좌우 반전, 색상 왜곡(PCA 기반) 등으로 **데이터 다양성 확보 및 일반화 성능 향상.**  
    - **Training on Multiple GPUs**
		- 두 개의 GPU에 네트워크를 분할하여 병렬 학습 → 당시 하드웨어 한계를 넘어 대규모 모델 학습을 실현.

---
### 방법론
#### 데이터셋 / 실험 환경
**데이터셋:** ImageNet ILSVRC 2012
- 약 **120만 장**의 학습 이미지, **1000개 클래스**
- **전처리:**
    - 이미지를 **256×256**으로 리사이즈 후 **224×224 랜덤 크롭**
    - **좌우 반전, 색상 왜곡(PCA 기반)** 등 **데이터 증강**
    - RGB 채널 평균값 제거로 정규화 수행
- **하드웨어:**
    - **GPU 2개 (NVIDIA GTX 580)** 병렬 학습
    - 각 GPU가 네트워크의 절반을 담당하며, 특정 층에서(feature map 간) 통신하도록 설계 → 병목 최소화 및 메모리 분산 효율 향상
#### 모델 구조 및 설정
- **총 8개 층:**
    - **5개 Convolutional Layer + 3개 Fully Connected Layer**
- **구성:**
	(CONV → ReLU → LRN → MaxPool)×2 → (CONV → ReLU)×3 → MaxPool → (FC → ReLU → Dropout)×2 → FC → Softmax
- **정규화/비선형 기법:**
    - **ReLU** 활성화 함수
    - **LRN(Local Response Normalization)**
    - **Dropout(p=0.5)** – 과적합 방지
#### 학습 설정 (Hyperparameters)
- **Optimizer:** Stochastic Gradient Descent (SGD)  
- **Batch size:** 128
- **Momentum:** 0.9  
- **Weight Decay:** 0.0005  
- **Initial LR:** 0.01 → 개선 없을 시 10배 감소
- **Epochs:** 약 90회 (5~6일, GPU 2개 기준)
- **Weight Init:** 평균 0, 표준편차 0.01의 정규분포 / Bias는 0 또는 1

---

### 실험 결과 및 분석
- **데이터:** ImageNet ILSVRC 2012 (120만 장, 1000 클래스)
- **학습:** SGD, LR=0.01, Momentum=0.9, Dropout=0.5, GPU 2개 병렬

|모델|Top-1 Error|Top-5 Error|비고|
|---|---|---|---|
|AlexNet (1 CNN)|40.7%|18.2%|단일 모델|
|AlexNet (7 CNNs)|36.7%|15.3%|앙상블 (우승)|
|2위 (SIFT+FV)|-|26.2%|전통적 방법|
- 기존 대비 **Top-5 Error 10%p 이상 향상**, 딥러닝 우수성 입증
- **성능 향상 요인:**
	- GPU + ReLU + Dropout 
- **의의:** 
	- 딥러닝이 전통적 머신러닝을 능가함을 실증 
	- CNN의 자동 특징 학습 가능성 제시 
- **한계:** 
	- 연산 자원·메모리 의존 높음

#### 성능 향상 요인 분석
AlexNet의 성능 개선은 단일 요인이라기보다 여러 혁신이 결합된 결과임
- **ReLU:** `tanh` 대비 학습 속도 약 **6배 향상**, gradient vanishing 완화로 대규모 학습 가능.  
- **Dropout:** 과적합 억제 및 일반화 성능 약 **+2%p 향상**.  
- **GPU 병렬:** 메모리 한계를 극복해 대규모 파라미터(≈60M) 학습 실현, 학습 시간 **1/5 단축**.  
> ReLU → 학습 안정성 ↑ · Dropout → 일반화 ↑ · GPU → 모델 확장 ↑  
#### LRN과 Overlapping Pooling의 재평가
- **LRN (Local Response Normalization):**  
- **LRN:** Top-1 약 **+1.4% 향상** 보고되었으나, 후속 연구에서 효과 미미 → 이후 **BatchNorm**으로 대체.  
- **Overlapping Pooling:** Feature 손실 감소로 **소폭 성능 향상(+0.4%)**, 이후 모델(VGG·ResNet 등)에서도 기본 채택.
#### 한계
  - 여전히 연산 자원 의존도가 높고, 학습에 **수일 단위의 GPU 시간**이 필요.  
  - 데이터 증강과 Dropout의 효과가 결합되어, **순수 구조적 개선의 기여도를 분리 평가하기 어려움.**
#### 요약
> AlexNet은 단일 기술적 혁신이 아닌,  
> **ReLU, Dropout, GPU 병렬화, 데이터 증강**의 복합적 조합으로 성능을 폭발적으로 향상시켰음.  
> 다만 일부 기법(LRN 등)은 이후 비효율적임이 밝혀져  
> 딥러닝 구조 발전의 ‘중간 단계적 시도’로 평가됨.

---

### 결론 및 시사점
- “**대규모 데이터 + GPU + Deep CNN**” 조합의 효율성 입증
- 이후 딥러닝 기반 비전 모델(AlexNet → VGG → ResNet) 발전의 초석 마련
- 학습 시 **ReLU, Dropout, 데이터 증강, GPU 활용**의 중요성 확인
**후속 방향:**
- BatchNorm, ResNet으로 학습 안정성 강화
- 3D·영상·멀티모달 확장 연구로 발전

---

### 개인 코멘트
- 이해가 어려웠던 부분
- 다시 찾아볼 개념 (논문 내 용어·참고 문헌 등)
- 추가 참고할 논문

**ReLU:** 일부 뉴런이 영구적으로 비활성화되는 **dying ReLU 문제**가 존재

