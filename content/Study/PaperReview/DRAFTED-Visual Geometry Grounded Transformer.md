---
tags:
  - paper_review
draft: true
title: "VGGT: Visual Geometry Grounded Transformer"
---
# 📄 VGGT: Visual Geometry Grounded Transformer
| **Title**       | VGGT: Visual Geometry Grounded Transformer                                                     |
| --------------- | ---------------------------------------------------------------------------------------------- |
| **Authors**     | Jianyuan Wang, Minghao Chen, Nikita Karaev, Andrea Vedaldi, Christian Rupprecht, David Novotny |
| **Affiliation** | Visual Geometry Group (University of Oxford), Meta AI                                          |
| **Conference**  | CVPR 2025                                                                                      |
| **DOI**         | [arXiv:2503.11651](https://arxiv.org/abs/2503.11651?utm_source=chatgpt.com)                    |
| **Keywords**    | 3D Reconstruction, Transformer, Multi-View Geometry, Depth Estimation, Camera Pose             |

---
>VGGT, a feed-forward neural network that directly infers all key 3D attributes of a scene, including camera parameters, point maps, depth maps, and 3D point tracks, from one, a few, or hundreds of its views.
>
>최적화 과정 없이 한번의 dustksdmfh dlalwldptj 3D 속성을 동시 추론하는 Feed-Forward형 트핸스포머 모델
### 연구 배경
- 이 논문은 여러 이미지로부터 장면의 3D 속성(카메라 파라미터, 깊이, 포인트 맵, 포인트 트랙 등)을 feed-forward neural network만으로 직접 추정하는 문제를 다룸
- 기존 3D 복원은 시각 기하학 기반 최적화(예: Bundle Adjustment)에 의존해왔으며, 학습 기반 방법은 주로 보조 역할(특징 매칭, 단안 깊이 추정 등)을 담당
- 최근에는 DUSt3R, MASt3R, VGGSfM 등에서 기하학과 학습의 통합이 진행되었으나, 여전히 후처리 최적화(post-optimization) 필요, 처리 가능한 이미지 수가 제한적

---

### 주요 아이디어

단일 트랜스포머 모델로 전체 3D 속성(내·외부 카메라 파라미터, 깊이, 포인트 맵, 트랙)을 초 단위로 추론할 수 있는 새로운 구조 제안.

후처리 없이도 기존 최적화 기반 SOTA 모델보다 우수한 결과를 달성.

후처리(BA)를 추가할 경우 전 영역에서 최고 성능을 기록, 특정 3D 작업에 특화된 모델보다도 품질을 향상시킴.

1. 3D 속성 통합 추론
	- 단일 피드포워드 신경망 구조 채택
	- 한번의 연산으로 카메라 파라미터, 깊이, 포인트 맵, 트래킹 정보를 동시 추론
2. 속도 향상
	- Visual Geometry Optimization 과정을 제거
	- 반복적인 연산 없이 1초 미만의 속도로 3D 재구성 완료 가능
3. 확장성
	- 비디오 내 물체 추적이나 새로운 시점 합성(Novel View Synthesis)과 같은 복잡한 다운스트림 작업의 Feature Backbone으로 활용 가능
4. 입력의 유연성
	- 피드포워드 신경망의 특성을 사용한 트랜스포머 구조
	- 입력 데이터의 양에 구애받지 않고 처리 가능


---

### 방법론

Bundle Adjustment 최적화 제거
- 기존의 3D 재구성은 Structure-from-Motion (SfM) 이론에 기반하여, 초기 해를 구한 뒤 BundleAdjustment (BA) 최적화 과정이 필수적
- VGGT는 BA 없이 순수 신경망 연산만으로 3D 구조를 해석

데이터 처리 속도 개선
- MVS방식을 사용한 Dust3R과 같은 기존 모델은 두장씩(Pairwise)만 처리 가능
- VGGT는 기하학적 후처리 과정이 없기에 다수의 이미지를 동시 처리 가능

범용 백복으로서의 설계
- 복잡한 3D 전용 아키텍처를 설계하는 대신, Inductive  Bias가 적은 표준 트랜스포머 구조를 채택
- VGGT가 대규모 데이터로부터 3D 기하학을 스스로 학습
- 다양한 3D 작업 기반모델(Foundation Model)로 사용 가능

-

Structure from Motion (SfM)
- 정적 이미지 세트 사용
- 카메라 파라미터 추정
- Sparse point clouds 재구성
- 이미지 매칭 / 삼각 측량 / 번들 조정 등 여러 단계로 구성

Multi-view Stereo (MVS)
- 겹치는 여러 이미지 입력
- 복수의 2D 이미지와 해당 이미지들의 카메라 파라미터를 사용해 고밀도 3D 모델 재구성
- 카메라 파라미터는 SfM으로 추정


$$
f\Big((I_i)_{i=1}^N\Big) = (g_i, D_i, P_i, T_i)_{i=1}^N
$$
- 입출력 함수
	- 입력: $N$장의 이미지 시퀀스  
	- 출력: 각 이미지 $I$에 대한 4가지 3D 속성  
	  - g: Camera Parameters  
	  - D: Depth Maps  
	  - P: Point Maps  
	  - T: Tracking Features

Feature Backbone
- 복잡한 기하학적 제약 대신 데이터 기반 학습
- GPT, CLIP와 유사한 범용 백본 구조 사용
- 입력 토큰화
	- DINO ViT: 각 이미지를 Patch 단위 토큰으로 변환
	- Pretrained Visual Features 활용
- Standard Self-Attention을 두 단계로 분리하여 교차 수행: Alternating Attention
	1. Frame-wise Attention: 이미지 내부 토큰 간 정보 교환 (Local)
	2. Global Attention: 모든 프레임 전체 토큰 간 정보 교환 (Global)
- 위 과정을 통해 계산 비용이 높은 Cross-Attention을 제거하였으며, 대용량 이미지셋 입력 시에도 연산 효율 유지

- Input Augmentation
- 이미지 토큰에 Camera Token & Register Tokens 추가
- First Frame Handling : 첫 프레임에 고유한 학습 토큰 부여 >> World Reference(원점) 식별
	1. Camera Head
	2. Dense Prediction Head

---

### 실험 결과 및 분석

두가지 예측 방식 (Point Map,  Depth+Cam)
Depth+Cam이 정확도가 더 높음 > 3D 좌표 재구성이라는 복잡한 문제를 깊이와 카메라 파라미터라는 하위 문제로 쪼개서 푸는 것이 더 효과적임을 증명

3D 재구성을 넘어 Two-View matching에 있어서도 높은 성능을 보여줌

---

### 결론 및 시사점

Cross-Attention이나 Global Self-Attention 만의 효과가 아닌  Alternating-Attention
카메라 파라미터, 깊이, 트래킹을 따로 배우는 것 보다 동시에 학습하는 것이 더 잘됨
학습 과정에서 상호작용이 있었음을 유추 가능

안 보이는 각도에서의 view도 생성 가능, 동적 트래킹에도 잘 적용 가능

- 대규모 이미지 입력에서 주요한 3D 속성을 동시 추론
- 복잡한 최적화 과정이 없는 Feed forward 방식 채택
- 다양한 3D 작업(MVS, Tracking, Posing...)에서 SOTA 달성
- 기존 모델 대비 높은 속도 (8s > 0.2s)
- 기존의 기하학적 후처리 과정 제거

---

### 개인 코멘트
- 이해가 어려웠던 부분
- 다시 찾아볼 개념 (논문 내 용어·참고 문헌 등)
- 추가 참고할 논문


---

### 1. VGGT는 어떤 방식으로 명시적 기하학 제약 없이도 3D 일관성을 유지하는가
- Attention 메커니즘이 기하학적 일관성을 내재적으로 학습함
- 각 이미지의 토큰이 self-attention을 통해 다른 시점의 대응 토큰과 상호작용하면서 에피폴라 제약과 유사한 관계를 암묵적으로 학습
- 삼각측량이나 Bundle Adjustment를 수행하지 않음
- Loss function이 깊이·포즈·트래킹 오차를 동시에 최소화하도록 구성되어 세 속성 간 일관성 유지
- 핵심 개념: 기하학을 계산하지 않고 학습함 (learning geometry, not solving it)

### 2. Alternating Attention의 구조적 이점은 계산 효율 외에 학습 안정성 측면에서도 설명 가능한가
- Frame-wise Attention(로컬): 동일 이미지 내 공간 구조 학습
- Global Attention(글로벌): 다른 시점 간 대응 관계 학습
- 두 단계를 번갈아 수행하여 Cross-Attention의 계산량 억제
- 학습 초기에 feature alignment가 불안정할 때 global attention 붕괴 방지
- 로컬 단계에서 기초 구조를 안정화하고 글로벌 단계에서 다중 시점 일관성 확보
- Frame-wise 단계에서 학습된 local geometry가 global attention의 anchor로 작용해 misalignment 완화

### 3. 왜 Depth+Pose 조합이 직접 Point Map 예측보다 일반화 성능이 좋은가
- Point Map 직접 예측은 3D 좌표를 바로 회귀해야 하므로 학습 난이도가 높고 오차 누적 발생
- Depth+Pose 조합은 Point Map을 두 개의 하위 기하학적 요소로 분리해 학습
- 관계식:
$$X_{3D} = R^{-1}(z \cdot K^{-1}x - t)$$
- 깊이 z, 카메라 파라미터(K, R, t)를 예측해 간접적으로 Point Map 계산
- 각 구성 요소의 gradient 경로가 분리되어 안정적 학습 가능
- 노이즈가 한 요소에 국한되어 누적 오차 감소
- 구조적 에러 분산 효과(structural error decoupling)로 일반화 성능 향상

### 4. VGGT의 접근이 ‘기하학적 계산의 종말’을 의미하는가, 아니면 새로운 통합 형태의 기하학인가
- 종말이 아니라 “학습된 기하학(learned geometry)”으로의 전환
- 전통적 SfM/MVS는 명시적 제약식(에피폴라, BA)을 풀었지만, VGGT는 동일 제약을 파라미터 공간에서 내재적으로 학습
- 명시적 계산 없이도 동일한 구조적 제약이 emergent하게 형성됨
- 기하학을 제거한 것이 아니라, 표현공간에 내장한 모델
- Geometry-free가 아니라 geometry-implicit 접근
- 학습 기반 기하학의 실질적 구현체로 평가 가능

### 5. VGGT는 3D Vision의 ‘AlexNet moment’로 볼 수 있는가 — 즉, 패러다임을 바꿀 만큼 충분히 일반적인가
- 패러다임 측면에서는 그렇다
    - AlexNet이 2D 비전에서 handcrafted feature를 학습 기반 표현으로 대체했듯, VGGT는 3D 비전에서 기하학적 계산을 학습 기반 추론으로 대체
- 기술적 측면에서는 다르다
    - AlexNet은 단일 태스크(분류) 중심, VGGT는 다중 태스크(깊이·포즈·트랙) 통합 학습
- 영향력 측면에서는 전환점에 해당하나, 아직 완전한 표준 backbone은 아님
    - 대규모 self-supervised 학습과 다양한 장면 일반화 필요
- 요약: 개념적으로는 AlexNet급, 실용적으로는 GPT-1 수준의 출발점