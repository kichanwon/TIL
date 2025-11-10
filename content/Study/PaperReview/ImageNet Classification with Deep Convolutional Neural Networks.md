---
tags:
  - paper_review
draft: false
title: "ImageNet Classification with Deep Convolutional\rNeural Networks"
---
- [ImageNet Classification with Deep Convolutional Neural Networks](https://proceedings.neurips.cc/paper_files/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf)

- [참고](https://jjuon.tistory.com/22)

### 기본 정보
- 논문 제목: ImageNet Classification with Deep Convolutional Neural Networks
- 저자: Alex Krizhevsky, Ilya Sutskever, Geoffrey E. Hinton
- 소속: University of Toronto
- 학회: NIPS (NeurIPS 2012)
- 키워드
	CNN, Deep Learning, ImageNet, GPU, ReLU, Data Augmentation, Dropout

---

### 연구 배경
- 기존 이미지 분류는 **사람이 설계한 특징(handcrafted features)** 에 의존(예: SIFT, HOG) → 복잡한 패턴 학습 한계
- 딥러닝은 가능성이 있었지만 **학습 속도·과적합 문제**로 대규모 데이터 적용이 어려움
- **대규모 데이터(ImageNet) + GPU 병렬 연산 + 개선된 CNN 구조**로 이를 해결하고자 함
---

### 주요 아이디어
![[Pasted image 20251110145241.png]]
- 전체 **약 6천만(60M) 개의 파라미터**, **65만(650K)개의 뉴런**을 포함하여, 당시 기준으로 매우 대규모 네트워크 구조
- 각 층은 점진적으로 **저수준 → 고수준 특징**을 학습
- 주요 기법:
    - **ReLU Nonlinearity:** 빠른 학습, Gradient Vanishing 완화
    - **Training on Multiple GPUs:** GPU 병렬 학습으로 대규모 학습 가능
    - **Local Response Normalization:** 정규화로 일반화 향상
    - **Overlapping Pooling:** 정보 손실 완화
    - **Dropout:** 과적합 방지
    - **Data Augmentation:** 랜덤 크롭, 좌우 반전, 색상 왜곡

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
#### 비교 및 평가
- **Baseline:**
    - 전통적 SIFT+Fisher Vector+SVM, LeNet 계열 shallow CNN
- **평가 지표:** Top-1 / Top-5 Error (%)
- **결과:**
    - **AlexNet:** Top-5 Error **15.3%**
    - **2위 모델:** 26.2% → **약 10%p 향상**

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

- **성능 향상 요인:** GPU + ReLU + Dropout
- **의의:**
    - 딥러닝이 전통적 머신러닝을 능가함을 실증
    - CNN의 자동 특징 학습 가능성 제시
- **한계:**
    - 연산 자원·메모리 의존 높음

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
**LRN:** 당시에는 Top-1 약 1.4%, Top-5 약 1.2% 향상을 보고했으나, 이후 연구에서 **Batch Normalization**으로 대체되며 **효과가 제한적**임이 밝혀짐.
