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
- 동일한 모델구조를 통해 활성화 함수(ReLU, Sigmoid, Tanh)가 CNN의 학습 성능에 미치는 영향을 비교.
- 모든 실험에서 optimizer는 Adam, 학습률은 0.001, batch size는 64로 고정.  
- CIFAR-10 데이터셋을 40,000/10,000/10,000으로 분할하여 학습·검증·테스트에 사용.   

### **실험 상세**
![[/static/images/cifar-10_activation_step.png]]
![[cifar-10_activation_step.png]]
![[/static/images/cifar-10_activation_relative.png]]![[cifar-10_activation_relative.png]]
Train Loss 그래프:
- Tanh (파란색) → 가장 빠르게 안정적으로 감소.
- ReLU (빨간/노란색) → 완만하게 감소.
- Sigmoid (초록색) → 감소 정체.
![[/static/images/cifar-10_acc.png]]![[cifar-10_acc.png]]
Val Accuracy 그래프:
- Tanh > ReLU ≫ Sigmoid
- Tanh는 epoch이 늘어날수록 꾸준히 증가.
- Sigmoid는 거의 향상 없음.
#### **정리**
- **ReLU (빨간/노란색)**
    - 손실 감소가 완만하며, 학습 초반 변동이 존재함.
- **Sigmoid (초록색)**
    - 손실 감소가 거의 진행되지 않음.
    - 전체 학습 과정에서 정확도 향상 폭이 가장 낮음.
- **Tanh (파란색)**
    - 손실이 가장 빠르고 안정적으로 감소함.
    - ReLU보다 빠른 수렴을 보이며, 검증 정확도 또한 가장 높음.
- **종합 관찰**
    - Tanh > ReLU ≫ Sigmoid 순으로 성능 차이를 보임.
    - 동일한 조건(Adam, lr=0.001, epoch=10)에서도 활성화 함수 선택이 수렴 속도와 최종 정확도에 뚜렷한 영향을 미침.

---
### **결론**
- 활성화 함수에 따른 학습 성능 차이를 비교함.
- CIFAR-10 정규화(mean≈0.49, std≈0.25)로 입력 분포가 -2~+2 범위로 형성됨.
- Tanh는 양·음 영역 모두에서 활성화되어 ReLU보다 빠르고 안정적으로 수렴함.
- 학습률 0.001, 10에폭의 짧은 학습 조건에서 Tanh는 작은 gradient로도 충분히 학습됨.
- ReLU는 수렴 속도가 느려 동일 조건에서 성능이 낮게 나타남.
- Adam 옵티마이저의 gradient scaling 효과로 Tanh의 작은 gradient가 보완됨.
- Dropout(0.5)이 적용된 환경에서 ReLU의 희소 출력 특성으로 정보 손실이 더 큼.
- 전체적으로 본 실험 환경에서는 Tanh가 ReLU보다 낮은 손실과 높은 정확도를 보임.

### **고찰**
- **GPU 사용량 저조(너무 긴 학습 시간)**
	- 간단한 모델임에도 학습에 너무 긴 시간이 걸림
- **정확도 저조**

---
## **실험 2: 활성화 함수별 학습 성능 비교 개선된 모델(_ReLU_ / _Tanh)**

### **실험 환경**
- **모델:** ImprovedCNN
- **변수:** 활성화 함수 \[ReLU, Tanh]
- **고정:** optimizer = Adam, lr = 0.001
- **데이터셋:** CIFAR-10 (3×32×32 RGB)
_데이터 분할 및 전처리_
- Train / Validation / Test = 40,000 : 10,000 : 10,000
- 검증 데이터 비율: 0.2 (20%)
- 정규화: 픽셀값 \[0, 1] 스케일링
#### **모델 구조**
- 입력 : (3, 32, 32) CIFAR-10 컬러 이미지
- Feature Extractor 블록
    - Conv2d(3→32, 3×3) → BatchNorm → ActivationFn\[ReLU/Tanh]
    - Conv2d(32→64, 3×3) → BatchNorm → ActivationFn\[ReLU/Tanh] → MaxPool(2×2)
    - Conv2d(64→128, 3×3) → BatchNorm → ActivationFn\[ReLU/Tanh]
    - Conv2d(128→128, 3×3) → BatchNorm → ActivationFn\[ReLU/Tanh] → MaxPool(2×2)
    - Conv2d(128→256, 3×3) → BatchNorm → ActivationFn\[ReLU/Tanh]
    - Conv2d(256→256, 3×3) → BatchNorm → ActivationFn\[ReLU/Tanh] → MaxPool(2×2)
- Classifier 블록
    - Flatten → Linear(256×4×4→256) → ActivationFn[ReLU/Tanh] → Dropout(0.5) → Linear(256→10)
- 출력 : (B, 10) – \`CrossEntropyLoss\` 로 softmax 처리
### **실험 내용**
- 실험 1(SimpleCNN) 결과에서 Tanh가 더 안정적으로 수렴했으나 모델 용량이 작아 최종 정확도 제한이 있었음.
- ImprovedCNN으로 네트워크 깊이와 채널 수를 증가시켜 활성화 함수 효과를 다시 비교함.
### **실험 상세**



### **결론**

### **고찰**

---
## **CIFAR-10 실험 한계**
