---
tags:
draft: false
---
## 목적
- CIFAR-10 데이터셋을 사용하여 CNN의 학습 설정에 따른 성능 변화를 분석
- AlexNet의 주요 기법(ReLU, Dropout, Overlapping Pooling, LRN 등)이 CIFAR-10에서 주는 효과를 검증하고자 함

---
### **공통 실험 환경**
- optimizer = Adam(lr=0.001)
- batch_size = 64
- epoch = 10
- 데이터셋: CIFAR-10 (3×32×32 RGB)

## **실험 1: 활성화 함수별 학습 성능 비교 (_ReLU_ / _Sigmoid_ / _Tanh)**

### **실험 환경**
- **모델:** SimpleCNN
- **변수:** 활성화 함수 \[ReLU, Sigmoid, Tanh]
- **고정:** optimizer = Adam, lr = 0.001, batch_size = 64, epoch = 10
- **데이터셋:** CIFAR-10 (3×32×32 RGB)
_데이터 분할 및 전처리_
- Train / Validation / Test = 40,000 : 10,000 : 10,000
- 검증 데이터 비율: 0.2 (20%)
- 정규화: 픽셀값 \[0, 1] 스케일링
#### **모델 구조**
- 입력: (3, 32, 32) CIFAR-10 컬러 이미지
- Conv 블록 1: Conv2d(3→32, 3×3) → BatchNorm → ActivationFn\[ReLU, Sigmoid, Tanh] → MaxPool(2×2)
- Conv 블록 2: Conv2d(32→64, 3×3) → BatchNorm → ActivationFn\[ReLU, Sigmoid, Tanh] → MaxPool(2×2)
- FC 블록: Flatten → Linear(4096→128) → ActivationFn\[ReLU, Sigmoid, Tanh] → Dropout(0.5) →Linear(128→10)
- 출력:(B, 10), softmax는 \`CrossEntropyLoss\`에서 처리
### **실험 내용**
![[/static/images/cifar-10_activation_step.png]]
![[cifar-10_activation_step.png]]
![[/static/images/cifar-10_activation_relative.png]]![[cifar-10_activation_relative.png]]
![[/static/images/cifar-10_acc.png]]![[cifar-10_acc.png]]

### **실험 상세**

### **결론**

### **고찰**

---

## **CIFAR-10 실험 한계**
