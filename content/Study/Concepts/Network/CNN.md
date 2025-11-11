---
draft:
---
# Convolutional Neural Network (CNN)

## 1. CNN 개요

**Convolutional Neural Network (CNN)**  
시각 데이터(이미지, 영상 등)를 처리하는 데 특화된 인공 신경망 구조.  
지역적인 패턴을 학습하고, 계층적으로 복잡한 특징을 구성하여 인식 성능을 향상시킴.

---

## 2. CNN의 주요 특징

1. **지역 연결 (Local Connectivity)**
    - 각 뉴런은 입력의 일부(국소 영역, receptive field)만 관찰함.
    - 이미지 내의 지역적 패턴(엣지, 질감 등)을 효율적으로 학습.
2. **가중치 공유 (Weight Sharing)**
    - 동일한 필터(커널)를 입력 전체에 적용하여 특징맵(feature map)을 생성.
    - 파라미터 수 감소 → 계산 효율 증가 및 일반화 성능 향상.
3. **계층적 특징 학습 (Hierarchical Feature Learning)**
    - 하위층: 단순한 특징(선, 색상 등)
    - 상위층: 복합적인 특징(물체, 얼굴 등)
    - 층이 깊어질수록 더 추상적이고 의미 있는 표현을 학습.
4. **풀링 (Pooling)**
    - 공간 크기를 줄여 위치 불변성 강화.
    - 주요 정보만 남기고 잡음 제거.
    - **종류:**
        - _Max Pooling_: 최대값 선택 → 강한 특징 보존
        - _Average Pooling_: 평균값 선택 → 부드러운 특징 유지
    - 너무 과도한 풀링은 정보 손실을 초래할 수 있음.
5. **위치 불변성 (Translation Invariance)**
    - **합성곱층:** _Translation Equivariance_ → 입력이 이동하면 출력도 같은 방향으로 이동
    - **풀링층:** _Translation Invariance_ → 입력의 위치가 약간 변해도 출력이 거의 동일하게 유지
    - 이를 통해 CNN은 같은 물체가 이미지 내 다른 위치에 있더라도 안정적으로 인식 가능.
6. **자동 특징 추출 (Automatic Feature Extraction)**
    - 사람이 직접 특징을 설계할 필요 없이, 학습을 통해 유용한 특징을 자동으로 학습.
7. **효율적인 파라미터 구조**
    - 지역 연결 + 가중치 공유로 파라미터 수 대폭 감소.
    - 예시:
        - 200×200 이미지를 1000개 뉴런에 완전연결 → 40M 파라미터
        - 5×5 필터 10개 사용 → 250 파라미터 (약 16만 배 감소)
    - 덕분에 깊은 네트워크 구성과 대규모 데이터 학습이 가능.

---

## 3. CNN의 기본 구조
- **입력층 (Input Layer)** → 이미지 데이터 (예: 224×224×3)
- **합성곱층 (Convolution Layer)** → 필터와 합성곱 연산을 통해 특징맵 추출
- **활성화 함수 (Activation Function)** → 비선형성 도입 (주로 ReLU)
- **풀링층 (Pooling Layer)** → 공간 축소 및 불변성 확보
- **\[합성곱 → 활성화 → 풀링] 블록 반복**
- **완전연결층 (Fully Connected Layer)** → 분류기 역할
- **출력층 (Output Layer)** → Softmax 등으로 최종 결과 도출

---

## 4. 활성화 함수 (Activation Functions)
- **ReLU (Rectified Linear Unit)**
    - f(x)=max⁡(0,x)f(x) = \max(0, x)f(x)=max(0,x)
    - Vanishing Gradient 문제 완화
    - 계산이 빠르고 학습 효율적
    - 단점: _Dying ReLU 문제_ (일부 뉴런이 0으로 고정될 수 있음)
- **Sigmoid**
    - 출력 범위: (0, 1)
    - 주로 이진 분류의 출력층에서 사용
    - 단점: Vanishing Gradient 문제 존재
- **Tanh**
    - 출력 범위: (-1, 1), zero-centered
    - Sigmoid보다 개선되었으나 여전히 gradient 소실 가능

---

## 5. 수용장 (Receptive Field)
- 특정 층의 한 뉴런이 “볼 수 있는” 입력 이미지 영역.
- 층이 깊어질수록 수용장이 커짐.
    - 하위층: 작은 수용장 → 엣지, 색상 등 지역적 패턴 감지
    - 상위층: 큰 수용장 → 복잡한 전체 객체 인식
- 설계 시, 탐지하려는 객체의 크기에 맞는 수용장 확보가 중요.

---

## 6. 과적합 방지 기법 (Regularization Techniques)
- **Dropout**
    - 훈련 중 일부 뉴런을 무작위로 비활성화 → 과적합 방지, 일반화 성능 향상
    - 주로 FC층에 사용
- **Batch Normalization**
    - 각 배치의 평균과 분산을 기준으로 정규화
    - 학습 안정화, 수렴 속도 향상
    - Internal Covariate Shift 완화, 높은 학습률 사용 가능
- **Data Augmentation**
    - 훈련 데이터를 인위적으로 확장
    - 예: 회전, 반전, 크롭, 색상 변화
    - 적은 데이터로도 강건한 모델 학습 가능
- **L2 Regularization (Weight Decay)**
    - 가중치 크기를 제한하여 과적합 방지

---

## 7. CNN 발전의 역사적 배경
- **ImageNet Large Scale Visual Recognition Challenge (ILSVRC)**
    - 2010년부터 시작된 대규모 이미지 분류 경진대회
    - 1,000개 클래스, 약 120만 장의 훈련 이미지
    - CNN 발전의 주요 동력
    - **2012년 AlexNet** → 압도적 승리로 딥러닝 시대 개막
    - **2015년 ResNet** → 인간 성능 초과 (Top-5 error 3.57%)
    - **2017년 종료** → 벤치마크로서 역할 완료

---

## 8. 대표 CNN 아키텍처
- **LeNet-5 (1998)**
    - 최초의 실용적 CNN, 손글씨 숫자 인식(MNIST)
    - 2개 합성곱층 + 풀링층 + FC층 구조
- **AlexNet (2012)**
    - ILSVRC 2012 우승 (Top-5 error 15.3%)
    - 8층 구조 (5 Conv + 3 FC)
    - ReLU 활성화 함수 대중화
    - Dropout, Data Augmentation 도입
    - GPU 병렬 학습 활용
- **VGG (2014)**
    - 3×3 작은 필터를 반복 사용하는 단순하고 깊은 구조
    - VGG-16, VGG-19 (16층, 19층)
    - 깊이가 성능 향상에 중요함을 입증
- **GoogLeNet / Inception (2014)**
    - Inception 모듈: 여러 크기의 필터(1×1, 3×3, 5×5)를 병렬로 사용
    - 1×1 convolution으로 차원 축소
    - 22층 구조지만 파라미터 수는 AlexNet보다 적음
- **ResNet (2015)**
    - Residual (Skip) Connection 도입
    - Vanishing Gradient 문제 해결
    - 152층 이상의 초심층 네트워크 학습 가능
    - ILSVRC 2015 우승 (Top-5 error 3.57%)

---

## 9. CNN의 주요 응용
- 이미지 분류 (Image Classification)
- 객체 탐지 (Object Detection: e.g., R-CNN, YOLO, SSD)
- 이미지 분할 (Segmentation: e.g., U-Net, Mask R-CNN)
- 얼굴 인식 (Face Recognition)
- 자율주행 (차선 인식, 객체 감지)
- 의료 영상 분석 (종양 검출, 병변 분류 등)

---

## 요약
CNN은 지역적 특징 학습과 계층적 표현 학습을 통해 이미지 인식 분야에서 탁월한 성능을 보이는 신경망 구조.  
합성곱, 풀링, 활성화, 정규화 등의 요소를 적절히 조합함으로써 효율적이고 일반화된 시각 인식 모델을 구축 가능.