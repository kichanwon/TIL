---
draft: true
---

## ⚙️ ② Batch Size 증가의 의미, 동작, 효과

|항목|내용|
|---|---|
|**개요**|학습 시 한 번에 처리하는 샘플 수를 늘려 **계산 효율** 향상 및 **gradient 분산 안정화** 유도|
|**필요성**|GPU 병렬 처리 최적화, I/O 오버헤드 감소, 학습 속도 향상 목적|
|**동작 원리**|`N`개 샘플의 평균 gradient를 구해 가중치 갱신 → batch size 증가 시 gradient의 **분산(variance)** 감소 → 안정된 방향으로 수렴|
|**기대효과**|학습 안정성↑, GPU 효율↑, 학습 시간 단축|
|**단점**|메모리 사용량 증가, local minima 탐색 능력 저하, 일반화 성능 하락 가능|
|**보완 방법**|batch 증가 시 `LR ∝ batch_size` (Linear Scaling Rule) 적용, 작은 warm-up 단계 추가|

**장단점 요약**

|구분|장점|단점|
|---|---|---|
|**Batch Size ↑**|안정적 gradient, 빠른 수렴, GPU 효율 증가|일반화 능력↓, 메모리 한계, fine-tuning 어려움|
|**Batch Size ↓**|일반화↑, 메모리 효율|수렴 불안정, 학습 속도 느림|

[Batch/Epoch/Iteration](https://bruders.tistory.com/79)
[Batch & LearningRate](https://davidlds.tistory.com/33)
[Large Batch vs Less LearningRate](https://honeyjamtech.tistory.com/43)



---
---


```python
ACCUMULATION_STEPS = 1 (누적 없음)

BATCH_SIZE | GPU 사용 | 정확도
128        | 441.9    | 0.6340  ✅ 최고!
512        | 661.9    | 0.5960  ❌ 하락
```

### 이유 1: Generalization Gap (일반화 격차)

**큰 배치의 문제:**
```
작은 배치 (128):
샘플 다양성: ⬜⬛⬜⬛⬜⬛⬜⬛  (다양함)
→ Gradient 노이즈 많음
→ Loss landscape의 다양한 경로 탐색
→ 일반화 성능 ↑

큰 배치 (512):
샘플 다양성: ⬜⬜⬜⬜⬜⬜⬜⬜  (비슷함)
→ Gradient 노이즈 적음
→ Loss landscape의 특정 경로만 탐색
→ Sharp minimum에 빠짐
→ 일반화 성능 ↓
```

### 이유 2: Sharp vs Flat Minima
```
Loss Landscape:

작은 배치 (Noisy Gradient):
     ╱╲  sharp
    ╱  ╲       → 여기 빠지면 과적합
───────────

큰 배치 (Smooth Gradient):
  ╱────╲  flat
 ╱      ╲     → 여기 가면 일반화 좋음
───────────

문제: 큰 배치는 sharp minimum에 빠지기 쉬움!
```

### 이유 3: 업데이트 횟수 차이

python

```python
# 전체 데이터 18,000개 가정

BATCH_SIZE = 128:
에포크당 업데이트 횟수 = 18,000 / 128 = 140회
→ 모델이 140번 학습 기회

BATCH_SIZE = 512:
에포크당 업데이트 횟수 = 18,000 / 512 = 35회
→ 모델이 35번 학습 기회

→ 4배 적은 업데이트! 학습 부족 가능
```

### 시각화

```
Epoch 1 동안의 모델 업데이트:

BATCH=128 (140 steps):
W₀ → W₁ → W₂ → ... → W₁₄₀  (세밀하게 조정)

BATCH=512 (35 steps):
W₀ ━━━━━━━━━━━━━━━━━━━━━━→ W₃₅  (성큼성큼 이동)

→ 작은 배치가 더 세밀하게 최적점 탐색!
```

### 해결 방법

**1. 학습률 스케일링:**

python

```python
# Linear Scaling Rule
BATCH_SIZE = 512
LEARNING_RATE = 0.001 * (512 / 128)  # 4배 증가
```

**2. Warmup:**

python

```python
# 초기에 학습률을 점진적으로 증가
scheduler = torch.optim.lr_scheduler.LinearLR(
    optimizer,
    start_factor=0.1,
    total_iters=5  # 5 에포크 동안 warmup
)
```

**3. 에포크 증가:**

python

```python
# 큰 배치는 더 많은 에포크 필요
BATCH_SIZE = 512
NUM_EPOCHS = 10  # 3 → 10으로 증가
```



