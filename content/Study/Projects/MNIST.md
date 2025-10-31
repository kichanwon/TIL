---
tags:
draft: false
---
## 목적
- MNIST 데이터셋을 사용하여 CNN의 학습 설정(batch size, 모델 구조, learning rate)에 따른 성능 변화를 분석
- AlexNet의 주요 기법(ReLU, Dropout, Overlapping Pooling 등)을 적용하여 학습 효율과 과적합 억제 효과를 검증
---
## **실험 1: BATCH_SIZE 비교**
### **실험 환경**
- 모델: SimpleCNN
- 변수: batch size \[64, 128, 1024]  
- 고정: optimizer=Adam, lr=0.001, epoch=5
- 데이터셋: MNIST (28×28 grayscale)
	**데이터 분할 및 전처리**
	- Train / Validation / Test = 48,000 : 12,000 : 10,000  
	- 검증 데이터 비율: 0.2 (20%) 
#### **모델 구조**
- 입력: (1, 28, 28) MNIST 흑백 이미지
- Conv 블록 1: Conv2d(1→32, 3×3) → BatchNorm → ReLU → MaxPool(2×2)
- Conv 블록 2: Conv2d(32→64, 3×3) → BatchNorm → ReLU → MaxPool(2×2)
- FC 블록: Flatten → Linear(3136→128) → ReLU → Dropout(0.5) → Linear(128→10)
- 출력: (B, 10), softmax는 \`CrossEntropyLoss\`에서 처리
#### **변수 요약**
| 변수    | 유형        | 주요 파라미터         | 출력 크기           |
| ----- | --------- | --------------- | --------------- |
| conv1 | Conv2d    | 1→32, k=3, p=1  | (B, 32, 28, 28) |
| pool1 | MaxPool2d | k=2, s=2        | (B, 32, 14, 14) |
| conv2 | Conv2d    | 32→64, k=3, p=1 | (B, 64, 14, 14) |
| pool2 | MaxPool2d | k=2, s=2        | (B, 64, 7, 7)   |
| fc1   | Linear    | 3136→128        | (B, 128)        |
| fc2   | Linear    | 128→10          | (B, 10)         |
### **실험 내용**
- batch size 변화에 따른 학습 안정성과 일반화 성능 비교  
- step 수 변화가 수렴 속도 및 손실 곡선에 미치는 영향 관찰

### **결과 요약**
- 작은 batch
	- gradient noise가 커 다양한 방향으로 탐색하며 빠르게 수렴
	- 학습은 다소 불안정하지만 일반화 성능이 우수하게 나타남  
- 큰 batch
	- gradient noise가 작아 학습이 안정적으로 진행됨
	- 학습 안정성은 높으나 일반화 성능은 낮게 나타남
- 학습 안정성과 일반화 성능 사이에는 명확한 상충 관계(Trade-Off)가 존재

### **실험 상세**

| Batch Size | Train Accuracy | Validation Accuracy | Train Loss | Validation Loss | Relative Time |
| ---------- | -------------- | ------------------- | ---------- | --------------- | ------------- |
| 64 (주황)    | 0.944          | **0.9895**          | 0.179      | **0.0375**      | 1.157 min     |
| 128 (회색)   | 0.934          | 0.9843              | 0.212      | 0.0523          | 1.104 min     |
| 1024 (녹색)  | 0.936          | 0.9832              | 0.217      | 0.0574          | 1.251 min     |
![Attachment/LB_StableLearning](/Attachment/LB_StableLearning.png)
![Attachment/SB_Generalization](/Attachment/SB_Generalization.png)
- **Small Batch (주황)**
    - 손실 곡선의 진폭이 크고 진동이 많으며, 다양한 방향으로 탐색하면서 빠르게 손실이 감소함
    - gradient noise가 커서 local minima를 탈출하기 쉬움
    - 여러 방향 탐색을 통해 flat minima에 수렴하며, 일반화 성능이 높게 나타남
- **Large Batch (녹색)**
    - 손실 곡선이 부드럽고 진동이 거의 없어 학습이 안정적으로 진행됨
    - 초기 수렴 속도는 느리지만 꾸준히 감소하는 형태를 보임
    - gradient noise가 작아 수렴 방향이 일정하나, 탐색 범위가 제한적임
    - sharp minima로 수렴하는 경향을 보여 일반화 성능은 상대적으로 낮음
    - step 수가 줄었음에도 학습 시간이 증가함
	**학습 시간 증가 원인**
	- 큰 Batch는 한 번의 step에서 더 많은 데이터를 처리하므로 메모리 접근 및 I/O 비용이 증가함
	- GPU의 병렬 처리 효율이 포화되어 커널 실행이 순차화될 경우 계산 지연이 발생함
	- 데이터 전송량이 많아 CPU–GPU 간 통신 병목(transfer bottleneck) 이 발생함
	- 결과적으로 step 수는 줄더라도 step당 연산 비용이 커져 전체 학습 시간이 증가할 수 있음

### **결론**
- **작은 batch**
    - step 수가 많아 weight가 자주 업데이트되며, 세밀한 학습이 이루어짐
    - gradient의 불규칙성이 존재해 모델이 더 넓은 영역(flat minima)을 탐색 가능
    - loss 곡선의 진동이 크고 요동이 심해 학습 안정성이 낮음
    - GPU 사용 효율이 떨어져 전체 학습 속도가 느리게 진행
    - 안정적인 학습을 위해 비교적 작은 learning rate를 사용하는 것이 적절
- **큰 batch**
    - gradient가 안정적으로 계산되지만 업데이트 횟수가 적어 변화가 느리게 진행
    - gradient noise가 작아 방향이 일정하게 유지
    - 손실이 빠르고 안정적으로 감소하나 sharp minima로 수렴하는 경향이 있어 일반화 성능이 낮아질 가능성 有
    - loss 곡선이 부드럽고 진동이 거의 없어 학습 안정성이 높음
    - GPU 자원을 효율적으로 활용해 학습 속도가 빠름
    - 안정적인 학습이 가능하므로 비교적 큰 learning rate를 설정 가능

---
## **실험 2: ReLU vs Sigmoid**
### **실험 환경**
- 모델: SimpleCNN_Sigmoid (ReLU → Sigmoid 적용)
- 변수: 활성화 함수 \[ReLU, Sigmoid]
- 고정: optimizer=Adam, lr=0.001, epoch=5, batch size=64
- 데이터셋: MNIST (28×28 grayscale)  
    **데이터 분할 및 전처리**
    - Train / Validation / Test = 48,000 : 12,000 : 10,000
    - 검증 데이터 비율: 0.2 (20%)
#### **모델 구조**
- 입력: (1, 28, 28) MNIST 흑백 이미지
- Conv 블록 1: Conv2d(1→32, 3×3) → BatchNorm → **Sigmoid** → MaxPool(2×2)
- Conv 블록 2: Conv2d(32→64, 3×3) → BatchNorm → **Sigmoid** → MaxPool(2×2)
- FC 블록: Flatten → Linear(3136→128) → **Sigmoid** → Dropout(0.5) → Linear(128→10)
- 출력: (B, 10), softmax는 \`CrossEntropyLoss\`에서 처리
#### **변수 요약**

|변수|유형|주요 파라미터|출력 크기|
|---|---|---|---|
|conv1|Conv2d|1→32, k=3, p=1|(B, 32, 28, 28)|
|pool1|MaxPool2d|k=2, s=2|(B, 32, 14, 14)|
|conv2|Conv2d|32→64, k=3, p=1|(B, 64, 14, 14)|
|pool2|MaxPool2d|k=2, s=2|(B, 64, 7, 7)|
|fc1|Linear|3136→128|(B, 128)|
|fc2|Linear|128→10|(B, 10)|
### **실험 내용**

### **결과 요약**

### **실험 상세**

![Attachment/ReLu-Sigmoid_Generalization](/Attachment/ReLu-Sigmoid_Generalization.png)
![Attachment/ReLu-Sigmoid_lrstable](/Attachment/ReLu-Sigmoid_lrstable.png)


### **결론**

---
## **실험 3: Dropout**
### **실험 환경**
- 모델: SimpleCNN_Dropout
- 변수: Dropout p \[0.0, 0.3, 0.5]
- 고정: optimizer=Adam, lr=0.001, epoch=5, batch size=64
- 데이터셋: MNIST (28×28 grayscale)
	**데이터 분할 및 전처리**
	- Train / Validation / Test = 48,000 : 12,000 : 10,000  
	- 검증 데이터 비율: 0.2 (20%) 
#### **모델 구조**
- 입력: (1, 28, 28) MNIST 흑백 이미지
- Conv 블록 1: Conv2d(1→32, 3×3) → BatchNorm → ReLU → MaxPool(2×2)
- Conv 블록 2: Conv2d(32→64, 3×3) → BatchNorm → ReLU → MaxPool(2×2)
- FC 블록: Flatten → Linear(3136→128) → ReLU → **Dropout(p)** → Linear(128→10)
- 출력: (B, 10), softmax는 \`CrossEntropyLoss\`에서 처리
#### **변수 요약**
| 변수    | 유형        | 주요 파라미터         | 출력 크기           |
| ----- | --------- | --------------- | --------------- |
| conv1 | Conv2d    | 1→32, k=3, p=1  | (B, 32, 28, 28) |
| pool1 | MaxPool2d | k=2, s=2        | (B, 32, 14, 14) |
| conv2 | Conv2d    | 32→64, k=3, p=1 | (B, 64, 14, 14) |
| pool2 | MaxPool2d | k=2, s=2        | (B, 64, 7, 7)   |
| fc1   | Linear    | 3136→128        | (B, 128)        |
| fc2   | Linear    | 128→10          | (B, 10)         |
### **실험 내용**

### **결과 요약**

### **실험 상세**

![[Dropout (2).png]][[Dropout (3).png]]![[Dropout (4).png]]![[Dropout (1).png]]
### **결론**

---
## **실험 4: Overlapping Pooling**
### **실험 환경**
- 모델: SimpleCNN_Overlapping
- 변수: Pooling 구성 \[(k=2, s=2), (k=3, s=2)]
- 고정: optimizer=Adam, lr=0.001, epoch=5, batch size=64
- 데이터셋: MNIST (28×28 grayscale)
	**데이터 분할 및 전처리**
	- Train / Validation / Test = 48,000 : 12,000 : 10,000  
	- 검증 데이터 비율: 0.2 (20%) 
#### **모델 구조**
- 입력: (1, 28, 28) MNIST 흑백 이미지
- Conv 블록 1: Conv2d(1→32, 3×3) → BatchNorm → ReLU → **MaxPool(3×3, stride 2)**
- Conv 블록 2: Conv2d(32→64, 3×3) → BatchNorm → ReLU → **MaxPool(3×3, stride 2)**
- FC 블록: Flatten → **Linear(2304→128)** → ReLU → Dropout(0.5) → Linear(128→10)
- 출력: (B, 10), softmax는 \`CrossEntropyLoss\`에서 처리
#### **변수 요약**
| 변수    | 유형        | 주요 파라미터         | 출력 크기           |
| ----- | --------- | --------------- | --------------- |
| conv1 | Conv2d    | 1→32, k=3, p=1  | (B, 32, 28, 28) |
| pool1 | MaxPool2d | k=3, s=2        | (B, 32, 13, 13) |
| conv2 | Conv2d    | 32→64, k=3, p=1 | (B, 64, 13, 13) |
| pool2 | MaxPool2d | k=3, s=2        | (B, 64, 6, 6)   |
| fc1   | Linear    | 2304→128        | (B, 128)        |
| fc2   | Linear    | 128→10          | (B, 10)         |
### **실험 내용**

### **결과 요약**

### **실험 상세**
![[OverlappingPooling (1).png]]![[OverlappingPooling (2).png]]![[OverlappingPooling (3).png]]![[OverlappingPooling (4).png]]

### **결론**

---
## **실험 5: Data Augmentation**
### **실험 환경**
- 모델: SimpleCNN
- 변수: Augmentation  
- 고정: optimizer=Adam, lr=0.001, epoch=5, batch size=64
- 데이터셋: MNIST (28×28 grayscale)
	**데이터 분할 및 전처리**
	- Train / Validation / Test = 48,000 : 12,000 : 10,000  
	- 검증 데이터 비율: 0.2 (20%) 
	- **증강 적용:** RandomRotation(±15°), RandomAffine(translate 0.1, scale 0.9–1.1)
#### **모델 구조**
- 입력: (1, 28, 28) MNIST 흑백 이미지
- Conv 블록 1: Conv2d(1→32, 3×3) → BatchNorm → ReLU → MaxPool(2×2)
- Conv 블록 2: Conv2d(32→64, 3×3) → BatchNorm → ReLU → MaxPool(2×2)
- FC 블록: Flatten → Linear(3136→128) → ReLU → Dropout(0.5) → Linear(128→10)
- 출력: (B, 10), softmax는 \`CrossEntropyLoss\`에서 처리
#### **변수 요약**
| 변수    | 유형        | 주요 파라미터         | 출력 크기           |
| ----- | --------- | --------------- | --------------- |
| conv1 | Conv2d    | 1→32, k=3, p=1  | (B, 32, 28, 28) |
| pool1 | MaxPool2d | k=2, s=2        | (B, 32, 14, 14) |
| conv2 | Conv2d    | 32→64, k=3, p=1 | (B, 64, 14, 14) |
| pool2 | MaxPool2d | k=2, s=2        | (B, 64, 7, 7)   |
| fc1   | Linear    | 3136→128        | (B, 128)        |
| fc2   | Linear    | 128→10          | (B, 10)         |
### **실험 내용**

### **결과 요약**

### **실험 상세**

### **결론**

---
## **실험 6: BatchNorm vs LRN**
### **실험 환경**
- 모델: SimpleCNN_LRN
- 변수: 정규화 기법 \[BatchNorm, LRN]
- 고정: optimizer=Adam, lr=0.001, epoch=5, batch size=64
- 데이터셋: MNIST (28×28 grayscale)
	**데이터 분할 및 전처리**
	- Train / Validation / Test = 48,000 : 12,000 : 10,000  
	- 검증 데이터 비율: 0.2 (20%) 
#### **모델 구조**
- 입력: (1, 28, 28) MNIST 흑백 이미지
- Conv 블록 1: Conv2d(1→32, 3×3) → **LRN(5, α=1e-4, β=0.75, k=2)** → ReLU → MaxPool(2×2)
- Conv 블록 2: Conv2d(32→64, 3×3) → **LRN(5, α=1e-4, β=0.75, k=2)** → ReLU → MaxPool(2×2)
- FC 블록: Flatten → Linear(3136→128) → ReLU → Dropout(0.5) → Linear(128→10)
- 출력: (B, 10), softmax는 \`CrossEntropyLoss\`에서 처리
#### **변수 요약**
| 변수    | 유형     | 주요 파라미터                     | 출력 크기           |
| ----- | ------ | --------------------------- | --------------- |
| conv1 | Conv2d | 1→32, k=3, p=1              | (B, 32, 28, 28) |
| lrn1  | LRN    | size=5, α=1e-4, β=0.75, k=2 | (B, 32, 28, 28) |
| conv2 | Conv2d | 32→64, k=3, p=1             | (B, 64, 14, 14) |
| lrn2  | LRN    | size=5, α=1e-4, β=0.75, k=2 | (B, 64, 14, 14) |
| fc1   | Linear | 3136→128                    | (B, 128)        |
| fc2   | Linear | 128→10                      | (B, 10)         |
### **실험 내용**

### **결과 요약**

### **실험 상세**

### **결론**
