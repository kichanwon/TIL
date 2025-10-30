---
draft: false
---
배치 사이즈는 옵티마이저와 연관이 깊다.

- [[content/Study/PaperReview/ON LARGE-BATCH TRAINING FOR DEEP LEARNING]]
- [참고1](https://davidlds.tistory.com/33)
- [참고2](https://bruders.tistory.com/79)
- [참고3](https://honeyjamtech.tistory.com/43)

---

### 조절 가능한 변수 요약

| 항목                   | 영향 대상            | 값 증가 시 효과                 | 값 감소 시 효과                |
| -------------------- | ---------------- | ------------------------- | ------------------------ |
| BATCH_SIZE           | GPU 효율, 학습 안정성   | GPU 효율 증가, 수렴 안정화, 일반화 저하 | 메모리 절약, 일반화 향상, 학습 속도 저하 |
| ACCUMULATION_STEPS   | 메모리 사용량, 업데이트 주기 | 메모리 절약, 큰 배치 효과 구현        | 업데이트 횟수 증가, 학습 노이즈 증가    |
| NUM_EPOCHS           | 학습량              | 학습 완성도 증가, 과적합 위험 증가      | 과소학습 위험 증가               |
| LEARNING_RATE        | 수렴 속도, 안정성       | 빠른 수렴, 발산 위험 증가           | 안정적이나 느린 학습              |
| MOMENTUM             | 수렴 안정성           | 수렴 가속, overshoot 위험 증가    | 느린 수렴, 진동 위험 증가          |
| TEST_SIZE / VAL_SIZE | 데이터 평가 신뢰성       | 평가 신뢰도 증가, 학습데이터 감소       | 평가 불안정, 학습 효율 증가         |
| IMAGE_SIZE           | 입력 정보량, 계산량      | 정확도 상승, 연산량 증가            | 속도 향상, 정보 손실 가능          |

---

1. Batch Size  
    큰 값일수록 GPU 효율이 높고 gradient 분산이 감소하여 안정적이지만, 일반화 성능이 떨어질 수 있다.  
    작은 값은 gradient 노이즈가 커지지만 다양한 경로를 탐색해 일반화에는 유리하다.
    
2. Gradient Accumulation  
    실제 메모리 한계를 넘지 않고 큰 배치 효과를 얻기 위한 방법이다.  
    accumulation step이 커질수록 메모리는 절약되지만 업데이트 주기가 길어져 학습이 느려질 수 있다.
    
3. Epoch 수  
    반복 학습 횟수로, 많을수록 정확도는 오르지만 과적합 위험이 커진다.  
    너무 적으면 충분히 학습되지 못한다.
    
4. Learning Rate  
    가중치 갱신 크기를 결정한다.  
    너무 크면 발산, 너무 작으면 느린 수렴이 발생한다.  
    큰 배치를 사용할 때는 learning rate를 비례해 늘려주는 것이 일반적이다.
    
5. Momentum  
    이전 gradient 방향을 일정 비율로 반영해 진동을 줄이고 수렴을 빠르게 만든다.  
    값이 너무 크면 overshoot이 발생하고, 너무 작으면 학습이 느려진다.
    
6. 데이터 분할 비율  
    검증 및 테스트 비율이 높을수록 평가 신뢰도는 올라가지만 학습 데이터가 줄어든다.  
    일반적으로 train:val:test 비율은 0.7:0.2:0.1이 적당하다.
    
7. Image Size  
    큰 입력 크기는 더 많은 특징을 학습할 수 있지만 연산량이 증가한다.  
    작은 크기는 속도는 빠르지만 세밀한 정보가 손실될 수 있다.
    

---

- batch
    - 학습에 사용하는 데이터 묶음 단위
    - batch_size로 지정
    - 한 batch를 모델에 넣어 forward, backward 수행
- step
    - 한 batch 학습이 끝나고 optimizer가 weight를 업데이트하는 단위
    - 1 batch 학습 후 1 step 증가
    - 전체 step 수 = (train 데이터 개수 / batch_size) × epoch 수
- optimizer
    - gradient를 이용해 모델의 weight를 실제로 수정하는 알고리즘
    - 각 step마다 loss를 줄이는 방향으로 weight 갱신
    - 예시: SGD, Adam, RMSprop

---

### 구체적인 흐름 (PyTorch 기준)

~~~python
for epoch in range(num_epochs):
	for batch_idx, (inputs, labels) in enumerate(train_loader):
		outputs = model(inputs)
		loss = criterion(outputs, labels)
		optimizer.zero_grad()
		loss.backward()
		optimizer.step()
~~~
1. batch 데이터를 불러옴
2. forward pass로 예측 계산
3. loss 계산
4. gradient 초기화
5. backward로 gradient 계산
6. optimizer가 gradient를 이용해 weight를 업데이트
7. 한 batch 학습이 끝나면 step 1 증가

---

### 예시
MNIST 데이터 60,000장  
batch_size = 64  
epoch = 5
- 한 epoch당 batch 수 = 60,000 ÷ 64 ≈ 938
- 총 step 수 = 938 × 5 = 4,690
- 4,690번 optimizer가 weight 업데이트 수행

---

### 심화 학습 (gradient accumulation)
GPU 메모리가 부족하거나 batch 크기를 크게 만들고 싶을 때 사용
~~~python
for i, (inputs, labels) in enumerate(loader):
    loss = model(inputs, labels) / accum_steps
    loss.backward()             
    if (i+1) % accum_steps == 0:
        optimizer.step()        
        optimizer.zero_grad()
~~~
- 여러 batch에서 gradient를 누적한 뒤 optimizer를 한 번만 호출
- n개의 batch → 1 step으로 처리
- 효과적으로 batch_size를 n배 늘린 효과

---

### 요약 핵심 포인트
- batch: 데이터 묶음 단위
- step: batch 1회 학습 후 weight 업데이트 단위
- optimizer: gradient를 이용해 weight 수정
- 1 batch = 1 step = 1 optimizer update
- gradient accumulation 시 여러 batch = 1 step
- step 수가 많을수록 모델은 더 자주 weight를 갱신하며 학습
- TensorBoard의 x축은 보통 step 수를 의미함

---
# batch
1. Batch Size 정의
    - 한 번의 학습 단계에서 모델이 동시에 처리하는 데이터 개수
    - 예: batch size = 64 → 한 번 weight를 업데이트할 때 64개의 이미지(데이터)를 보고 평균적으로 방향을 결정함
2. Batch Size와 Step 관계
    - steps per epoch = 전체 데이터 수 ÷ batch size
    - batch가 작을수록 step 수가 많아지고, 클수록 step 수가 줄어듦
3. Batch Size에 따른 특징
    - 작은 batch
        - step 수 많음 → 자주 weight 업데이트
        -  큼 → 방향이 자주 변함
        - 다양한 방향 탐색 → flat minima로 수렴 가능 → 일반화 성능 좋음
        - loss 곡선 요동 큼 → 학습 안정성 낮음
        - GPU 효율 낮음 → 학습 속도 느림
        - learning rate 작게 설정해야 함
    - 큰 batch
        - step 수 적음 → weight 업데이트 드묾
        - gradient noise 작음 → 방향 일정
        - 빠르고 안정적 수렴 → sharp minima로 수렴 가능 → 일반화 성능 낮음
        - loss 곡선 부드러움 → 학습 안정성 높음
        - GPU 효율 높음 → 학습 속도 빠름
        - learning rate 크게 설정 가능
4. Gradient Noise 영향
    - 작은 batch는 데이터 일부만 보고 gradient 계산 → gradient에 노이즈 포함
    - gradient noise는 탐색성을 높여 flat minima로 이동하게 함
    - 큰 batch는 gradient 평균이 정확 → 수렴은 빠르지만 탐색성 낮음
5. 학습 안정성 의미
    - loss, gradient, weight 변화가 요동치지 않고 꾸준히 감소하는 성질
    - 안정성이 높을수록
        - 손실 감소가 예측 가능함
        - gradient 폭주나 소멸이 줄어듦
        - 과적합이나 불안정 수렴 방지
        - 하이퍼파라미터 튜닝이 쉬움
        - 학습 효율이 높아짐
6. Step 수의 영향
    - step이 많을수록 weight 업데이트가 자주 일어나 세밀한 학습이 가능
    - step이 적을수록 업데이트가 적어 안정적이지만 탐색성이 낮음
    - step 수는 학습 속도, 안정성, 일반화 사이의 균형을 결정함
7. 핵심 요약
    - 작은 batch: step 많음, gradient noise 큼, 일반화 성능 좋음, 학습 느림
    - 큰 batch: step 적음, gradient noise 작음, 학습 안정성 높음, 일반화 약함
    - batch size는 일반화와 안정성 사이의 균형을 결정하는 요소