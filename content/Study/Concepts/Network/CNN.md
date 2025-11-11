---
draft:
---
### CNN 개요
- **Convolutional Neural Network (CNN)**  
    시각 데이터(이미지, 영상 등)를 처리하는 데 특화된 신경망 구조.  
    지역적인 패턴을 학습하고, 계층적으로 복잡한 특징을 구성함.

---
### CNN의 주요 특징
1. **지역 연결 (Local Connectivity)**
    - 각 뉴런은 입력의 일부(국소 영역)만 관찰함.
    - 이미지 내의 지역적 패턴(엣지, 질감 등)을 효율적으로 학습.
2. **가중치 공유 (Weight Sharing)**
    - 동일한 필터(커널)를 입력 전체에 적용.
    - 파라미터 수 감소 → 계산 효율↑, 일반화 성능↑
3. **계층적 특징 학습 (Hierarchical Feature Learning)**
    - 하위층: 단순한 특징(선, 색상)
    - 상위층: 복합적인 특징(물체, 얼굴 등)
4. **풀링 (Pooling)**
    - 공간 크기를 줄여 위치 불변성 강화.
    - 주요 정보만 남기고 잡음 제거.
5. **위치 불변성 (Translation Invariance)**
	- 합성곱층: Translation Equivariance (입력 이동 시 특징맵도 동일하게 이동)
	- 풀링층: Translation Invariance (입력 위치가 약간 변해도 동일한 특징 추출)
	- 같은 물체가 이미지 내 다른 위치에 있어도 인식 가능
6. **자동 특징 추출 (Automatic Feature Extraction)**
    - 사람이 직접 특징을 설계할 필요 없음.
    - 학습을 통해 유용한 특징을 스스로 학습.
7. **효율적인 파라미터 구조**
    - 완전연결 신경망 대비 파라미터 수 적음.
    - 대규모 이미지 입력에도 효율적.

---

### CNN의 기본 구조
- **입력층 (Input Layer)** → 이미지 데이터 (예: 224×224×3)
- **합성곱층 (Convolution Layer)** → 특징맵 추출
- **풀링층 (Pooling Layer)** → 공간 축소 및 불변성 확보
- **완전연결층 (Fully Connected Layer)** → 분류기 역할
- **출력층 (Output Layer)** → Softmax 등으로 결과 도출

---

### 대표 CNN 아키텍처
- **LeNet-5 (1998)** – 최초의 CNN, 숫자 인식에 사용
- **[[AlexNet_ImageNet Classification with Deep Convolutional Neural Networks|AlexNet (2012)]]** – ReLU, Dropout 도입, GPU 학습 보편화
- **VGG (2014)** – 단순한 구조(3×3 필터 반복)로 깊은 네트워크
- **GoogLeNet/Inception (2014)** – 병렬 필터 구조
- **[[ResNet_Deep Residual Learning for Image Recognition|ResNet (2015)]]** – 잔차 연결(Residual Connection)로 초심층 네트워크 가능

---

### CNN의 주요 응용
- 이미지 분류 (Image Classification)
- 객체 탐지 (Object Detection)
- 이미지 분할 (Segmentation)
- 얼굴 인식, 자율주행, 의료 영상 분석 등