---
aliases:
  - "LLM Optimizer: MuonClip vs. Adam"
---
## 1. 연구 배경
- 대규모 언어 모델(LLM) 학습에서는 Adam 및 AdamW가 대표적인 최적화 알고리즘(Optimizer)으로 사용됨.
- LLM의 파라미터 규모가 수천억~조 단위로 증가하면서 기존 Optimizer의 한계가 부각됨.
    - 파라미터별 Optimizer State 저장에 따른 높은 메모리 사용량
    - 비효율적인 파라미터 업데이트로 인한 학습 비용 증가
    - 대규모 학습에서의 안정성 확보 필요
    
- Moonshot AI는 Kimi K2 모델 학습에 새로운 Optimizer인 MuonClip을 적용함.
    - 약 1조 개 파라미터
    - 15.5조 토큰 학습
    - 보고된 학습 과정에서 Loss Spike 없이 안정성 확보
    
- 핵심 목적: AdamW의 안정성을 유지하면서 학습 효율성과 파라미터 업데이트 효율을 개선하는 것.

## 2. Optimizer의 발전 과정
### 2.1 SGD (Stochastic Gradient Descent)
- 전체 데이터 대신 미니배치(Mini-batch)를 이용하여 Gradient 계산
- Gradient의 반대 방향으로 파라미터 업데이트
- 한계
    - 학습 과정에서 진동 발생 가능
    - Gradient 방향이 불안정할 경우 수렴 속도 저하

### 2.2 Momentum
- 과거 Gradient의 이동 평균을 이용하여 업데이트 방향 결정
- Gradient의 진동을 완화하고 일관된 방향의 업데이트를 강화
- 기존 SGD보다 안정적인 수렴 유도

### 2.3 AdaGrad 및 RMSProp
- AdaGrad
    - 파라미터별 Gradient의 제곱 누적값을 이용하여 학습률 조정
    - 학습이 진행될수록 유효 학습률이 지나치게 감소할 수 있음

- RMSProp
    - Gradient 제곱의 지수 이동 평균 사용
    - AdaGrad의 과도한 학습률 감소 문제 완화

### 2.4 Adam
- Momentum과 RMSProp의 장점을 결합
- Gradient의 1차 및 2차 모멘트를 이용하여 파라미터별 업데이트 크기 조절
- 딥러닝에서 널리 사용되는 Optimizer로 발전

## 3. Adam의 핵심 원리
### 3.1 1차 모멘트 (First Moment)
- Gradient의 지수 이동 평균 계산
- 과거 Gradient 방향을 반영하여 업데이트 방향 안정화

$[ m_t=\beta_1m_{t-1}+(1-\beta_1)g_t ]$
- \($m_t$\): 1차 모멘트
- \($g_t$\): 현재 Gradient
- \($\beta_1$\): 과거 Gradient 반영 비율, 일반적으로 0.9

### 3.2 2차 모멘트 (Second Moment)
- Gradient 제곱의 지수 이동 평균 계산
- 파라미터별 업데이트 크기를 조절하는 데 활용

$[ v_t=\beta_2v_{t-1}+(1-\beta_2)g_t^2 ]$
- \($v_t$\): 2차 원시 모멘트 추정값
- \($\beta_2$\): 일반적으로 0.999
- Gradient 제곱값이 큰 파라미터의 업데이트 크기를 상대적으로 축소

### 3.3 Bias Correction
- 초기 모멘트 값이 0으로 설정되어 발생하는 편향 보정
$$[ \hat m_t=\frac{m_t}{1-\beta_1^t} ]$$

$$[ \hat v_t=\frac{v_t}{1-\beta_2^t} ]$$

### 3.4 파라미터 업데이트
$$[ \theta_t=\theta_{t-1}- \eta\frac{\hat m_t}{\sqrt{\hat v_t}+\epsilon} ]$$
- \($\theta$\): 모델 파라미터 
- \($\eta$\): Learning Rate
- \($\epsilon$\): 수치적 안정성을 위한 작은 상수
- Gradient의 평균적 방향과 크기를 반영하여 업데이트

## 4. AdamW: Adam의 개선
### 기존 Adam의 한계
- L2 정규화를 Gradient에 직접 포함하는 방식에서는 Weight Decay와 적응형 학습률이 서로 영향을 미침.
- 정규화 항이 1차 및 2차 모멘트 계산에 포함되어 Weight Decay의 효과를 독립적으로 조절하기 어려움.

### AdamW의 핵심 개선
- Decoupled Weight Decay 적용
- Gradient 기반 업데이트와 Weight Decay를 분리
$$[ \theta_t=(1-\eta\lambda)\theta_{t-1} -\eta\frac{\hat m_t}{\sqrt{\hat v_t}+\epsilon} ]$$
- \($\lambda$\): Weight Decay 계수
- 학습률과 Weight Decay를 독립적으로 조절 가능
- Transformer 및 LLM 학습에서 널리 사용되는 표준 Optimizer로 정착

## 5. Muon: 새로운 최적화 방식
### 5.1 등장 배경
- AdamW는 파라미터별 1차 및 2차 모멘트를 저장해야 하므로 메모리 부담이 큼.
- 행렬 업데이트가 특정 지배적인 방향에 집중되는 현상을 개선할 필요가 있음.
- Muon은 행렬의 기하학적 특성을 활용하는 새로운 업데이트 방식을 제안

### 5.2 핵심 아이디어: Orthogonalization
- Muon: MomentUm Orthogonalized by Newton-Schulz
- Gradient로부터 계산한 Momentum 행렬에 직교화(Orthogonalization) 적용
- 행렬의 특이값(Singular Values)을 균등하게 조정하는 방향으로 변환
- 특정 지배적인 방향에 대한 업데이트 집중을 완화
- 보다 다양한 방향으로 파라미터 업데이트가 이루어지도록 유도

### 5.3 Newton-Schulz Iteration
- 직교화된 행렬을 효율적으로 근사하기 위한 반복 알고리즘
- SVD(Singular Value Decomposition)를 직접 수행하는 대신 행렬 곱셈 기반 반복 연산 사용
- GPU의 행렬 연산 성능을 활용하여 계산 효율 확보

### 5.4 적용 방식
- Muon은 주로 2차원 가중치 행렬에 적용
    - Linear Layer의 Weight
    - Transformer 내부 행렬 가중치
- Bias, LayerNorm, Embedding 등에는 AdamW 사용
- 따라서 실질적으로 Muon + AdamW의 Hybrid Optimizer 구조

## 6. Muon의 한계: 학습 불안정성
- 대규모 Transformer 학습에서 Attention Logit 폭발 문제가 발생할 수 있음.
- Muon의 업데이트로 Query 및 Key 관련 가중치가 증가하면서 Attention Score가 과도하게 커질 수 있음.
$$[ \text{Attention}(Q,K,V)= \operatorname{softmax} \left(\frac{QK^T}{\sqrt{d_k}}\right)V ]$$

- Attention Logit이 과도하게 증가하면 다음 문제 발생 가능
- Softmax Saturation
- 특정 토큰으로 Attention 집중
- Gradient 소실 및 학습 불안정
- Loss Spike 또는 학습 발산(Divergence)

## 7. MuonClip: Muon의 안정성 개선
### 7.1 핵심 구조
$$[ \boxed{ \text{MuonClip}= \text{Muon}+ \text{Weight Decay}+ \text{Update RMS Scaling}+ \text{QK-Clip} } ]$$
- Muon의 효율적인 행렬 업데이트 유지
- Weight Decay 및 Update RMS Scaling으로 업데이트 제어
- QK-Clip을 이용하여 Attention Score 폭발 방지

### 7.2 QK-Clip 작동 과정
1. Monitoring
    - 각 Attention Head의 최대 Attention Logit 관측
2. Threshold Checking
    - 설정한 임계값 초과 여부 확인
    - Kimi K2에서는 임계값 100 사용
3. Conditional Intervention
    - 임계값을 초과한 Head에 대해서만 개입
4. Weight Rescaling
    - Query 및 Key 관련 Projection Weight 크기 조정
    - Attention Score 폭발 원인 억제

### 7.3 QK-Clip의 특징
- Attention Logit 자체를 단순히 제한하는 방식과 차별화
- 문제가 발생한 Attention Head만 선택적으로 제어
- 정상적인 Attention Head에 대한 불필요한 개입 최소화
- 대규모 학습에서 안정성 확보를 위한 피드백 제어 메커니즘 제공

## 8. AdamW vs. Muon vs. MuonClip

| 비교 항목   | AdamW       | Muon         | MuonClip       |
| ------- | ----------- | ------------ | -------------- |
| 핵심 원리   | 적응형 모멘트 추정  | Momentum 직교화 | Muon + QK-Clip |
| 업데이트    | 원소별 조정      | 행렬 구조 활용     | 행렬 구조 + 안정성 제어 |
| 상태 메모리  | 상대적으로 높음    | 낮출 수 있음      | 낮출 수 있음        |
| 토큰 효율   | 기준          | 개선 보고        | 개선 보고          |
| 대규모 안정성 | 높음          | 불안정 가능       | 안정성 개선         |
| 구현 복잡도  | 낮음          | 중간           | 높음             |
| 주요 적용   | 일반 딥러닝, LLM | 행렬 기반 학습     | 대규모 LLM 학습     |

## 9. 결론 및 시사점
- AdamW
	- 안정성과 범용성이 높은 Optimizer
	- 일반적인 딥러닝 학습 및 Fine-tuning에 적합
	- 구현과 하이퍼파라미터 조절이 상대적으로 간단

- Muon
	- 직교화를 통한 행렬 업데이트 효율 개선
	- 토큰 효율성 및 학습 시간 개선 가능
	- 대규모 Transformer 학습에서 안정성 문제가 발생할 수 있음

- MuonClip
	- Muon의 장점을 유지하면서 QK-Clip으로 학습 안정성 보완
	- Kimi K2 대규모 사전학습에서 효과를 입증한 사례
	- 기존 Optimizer 대비 구현 및 운영 복잡도가 높음

핵심 요약: 
AdamW는 각 파라미터의 Gradient 크기를 적응적으로 조절하는 방식인 반면, 
Muon은 행렬의 기하학적 구조를 활용하여 업데이트 방향을 조정함. 
MuonClip은 여기에 Attention Logit 제어 메커니즘을 결합하여 대규모 LLM 학습의 효율성과 안정성을 함께 개선하고자 하는 Optimizer임.