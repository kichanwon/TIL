---
title:
tags:
draft:
---
## Supervised Learning

**Definition**  
모든 학습 데이터에 정답 라벨이 존재하며, 입력과 출력의 관계를 직접 학습하는 방식이다.

**Key Characteristics**
- 목적 함수가 명확하여 학습이 안정적임
- 충분한 라벨이 있을 경우 가장 높은 성능을 달성 가능

**Limitations**
- 라벨링 비용과 시간이 매우 큼
- 라벨 수가 적으면 과적합 발생

**Typical Use Cases**
- 이미지 분류
- 객체 탐지
- 의미 분할
- 회귀 문제

---
## Unsupervised Learning

**Definition**  
라벨이 전혀 없는 데이터만을 사용하여 데이터의 구조나 분포를 학습하는 방식이다.

**Key Characteristics**
- 라벨링 비용이 없음
- 데이터의 잠재 구조를 파악하는 데 유리함

**Limitations**
- 명확한 정답 기준이 없음
- 결과의 의미 해석과 성능 평가가 어려움

**Typical Use Cases**
- 군집화
- 차원 축소
- 이상치 탐지
- 표현 학습 및 사전 학습

---
## Semi-Supervised Learning

**Definition**  
소량의 라벨 데이터와 대량의 무라벨 데이터를 함께 사용하여 학습하는 방식이다.

**Key Characteristics**
- 라벨 비용 대비 높은 성능
- 무라벨 데이터를 활용하여 일반화 성능 향상
- 일관성 정규화, pseudo-labeling 기법을 주로 사용

**Limitations**
- 학습 구조가 복잡함
- 잘못된 pseudo-label이 누적될 위험 존재    

**Typical Use Cases**
- 원격 탐사 영상 분석    
- 의료 영상 분할    
- 자율주행 데이터 학습    

---
## Comparison

| Paradigm        | Labeled Data | Label Cost | Performance Potential |
| --------------- | ------------ | ---------- | --------------------- |
| Supervised      | 전체 필요        | 높음         | 높음                    |
| Semi-Supervised | 일부 필요        | 중간         | 중~높음                  |
| Unsupervised    | 불필요          | 없음         | 낮음~중간                 |

---
## Key Insight

- **Supervised Learning**은 라벨이 충분할 때 최적의 선택    
- **Unsupervised Learning**은 데이터 구조 탐색 목적에 적합    
- **Semi-Supervised Learning**은 현실적인 데이터 환경에서 가장 효율적인 학습 방식