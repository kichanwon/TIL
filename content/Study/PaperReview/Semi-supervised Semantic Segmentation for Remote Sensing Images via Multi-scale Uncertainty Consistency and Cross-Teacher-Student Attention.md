---
tags:
  - paper_review
  - paper_semina
draft: false
title: Semi-supervised Semantic Segmentation for Remote Sensing Images via Multi-scale Uncertainty Consistency and Cross-Teacher-Student Attention
---
# 📄 Semi-supervised Semantic Segmentation for Remote Sensing Images via Multi-scale Uncertainty Consistency and Cross-Teacher-Student Attention
| **Title** | Semi-supervised Semantic Segmentation for Remote Sensing Images via Multi-scale Uncertainty Consistency and Cross-Teacher-Student Attention |
|------|------|
| **Authors** |  |
| **Affiliation** |  |
| **Conference** | IEEE Transactions on Geoscience and Remote Sensing (1735657200000) |
| **DOI** | [10.1109/TGRS.2025.3585489](https://doi.org/10.1109/TGRS.2025.3585489) |
| **Keywords** |  |


---
### 연구 배경
- **Semantic Segmentation for Remote Sensing Images**
	    - **재난 대응**, **토지 이용 분석**, **환경 모니터링** 등 다양한 응용 분야에서 **핵심적 역할**
	- **지구 관측 위성의 증가**
	    - **대규모 원격 탐사 영상**이 지속적으로 수집됨
	    - **딥러닝 기반 분석 적용 가능성** 확대
	- **원격 탐사 영상 데이터 특성**
	    - **픽셀 단위 라벨링**이 필수적임
	    - **클래스 수가 많고 라벨링 규칙이 복잡**함
	    - **대규모 데이터에 대한 [[Supervised vs Semi-Supervised vs Unsupervised Learning|완전 지도 학습]] 적용이 어려움**  
	     
	- 이에 따라 **준지도 학습**이 효과적인 대안으로 주목받음  
	 
- **자연 이미지 분야의 기존 Semi-supervised Semantic Segmentation 방법과 한계**  
    - **Pseudo-label 기반 Self-training**  
        - pros:   
          모델 예측을 pseudo-label로 활용하여 unlabeled data를 학습에 사용
        - cons:   
          학습 초기의 **잘못된 pseudo-label**로 인해 오류가 누적됨
    - **Feature perturbation 기반 일관성 학습** 
        - pros:   
          입력 또는 특징 공간에서의 교란(perturbation)에 대한 예측 일관성 강제
        - cons:   
          교란 설계와 라벨 분포에 민감함
    - **Teacher–Student 일관성 프레임워크** 
        - pros:   
          Teacher 모델의 예측을 Student 모델의 학습 신호로 사용  
          **최종 출력(prediction) 수준의 일관성**을 중심으로 설계됨  
        - cons:   
          **출력 수준의 일관성만 고려**하여 내부 표현이나 스케일 차이를 충분히 반영하지 못함
     
- **Remote Sensing 이미지와 자연 이미지 간 도메인 특성 차이**
    - **서로 다른 크기의 객체들이 동시에 존재**
        - 예: 대형 건물, 소형 자동차, 길게 이어진 도로
    - **높은 클래스 간 시각적 유사성**
        - 예: 도로 vs 주차장, 건물 옥상 vs 콘크리트 지면
	- 결과적으로 
		- **픽셀 경계 모호·소형 객체 누락·클래스 혼동** 발생
	- 기존 **Semi-supervised Semantic Segmentation 방법들**은   
	  **자연 이미지 도메인 가정**에 기반하여 설계되었으며  
	- **원격 탐사 영상의 다중 스케일 특성과 높은 클래스 유사성**을 충분히 고려하지 못함
	- **핵심 한계 요인**: 자연 이미지와 원격 탐사 영상 간의 **도메인 차이**가   
	  성능을 제한하는 주요 요인으로 작용함

---
### 주요 아이디어 
- **Multi-scale Uncertainty Consistency (MSUC) 모듈**
	- 기존 준지도 학습의 한계
	    - Teacher–Student 간 **출력 일관성**만을 정규화
	    - **중간 계층의 풍부한 표현 정보** 활용 부족
	- 제안 방식
	    - 네트워크의 **서로 다른 계층(feature maps)** 간 일관성을 **불확실성(uncertainty)** 기반으로 제약
	    - 라벨이 없는 데이터에서 **다중 스케일 표현 학습 능력** 강화
	- 기대 효과
	    - **스케일별 표현 불안정성 완화**
	    - **풍부하고 안정적인 다중 스케일 특징 학습** 가능  
	 
- **Cross-Teacher-Student Attention Mechanism**
	- Remote Sensing 이미지의 문제
	    - 원격탐사 영상의 **높은 클래스 간 시각적 유사성**으로 인한 구분 어려움
	- 제안 방식
	    - **Student 인코더 출력 → Query**, **Teacher 인코더 출력 → Key·Value**로 사용
	    - **교차 네트워크 Attention**을 통해 Teacher의 보완적 특징을 Student에 전달
	- 구조적 특징
	    - 기존의 **출력 수준 단방향 지도 방식**을 **특징 수준 상호 학습(feature-level mutual learning)** 으로 확장
	- 기대 효과
	    - **Teacher–Student 간 상호 보완적 표현 학습 강화**
	    - **시각적으로 유사한 클래스 간 판별력 향상**

---
### 방법론 (어떻게 구현했는가?)
- 데이터셋 / 실험 환경
- 모델 구조, 학습 설정, 하이퍼파라미터
- 비교 대상 (Baseline)
Section III-A. Main Optimization Objectives
>describes the main optimization objectives, 

Section III-B. Multi-Scale Uncertainty Consistency Module
- Uncertainty Estimation
- Consistency Loss Functions for Multiscale Uncertainty
>introduces the principles of the multi-scale uncertainty consistency module

Section III-C. Cross-Teacher-Student Attention
>presents the basic principles of the crossteacher-student attention module.

The overall structure of our approach is shown in Fig. 2
![[static/images/MUCA_Overall.png]]

---

### 실험 결과 및 분석
- 주요 성능 지표 요약 (표나 그래프로)
- 비교 결과 요약 (향상된 부분 / 한계점)
- 저자의 해석과 내 생각 비교

---

### 결론 및 시사점
- 논문이 제시한 핵심 결론
- 내 학습·프로젝트에 적용 가능한 부분
- 남은 의문점 또는 후속 연구 아이디어

---

### 개인 코멘트
- 이해가 어려웠던 부분
- 다시 찾아볼 개념 (논문 내 용어·참고 문헌 등)
- 추가 참고할 논문



# temp note
첫째, 우리는 최초로 다중 스케일 불확실성 일관성 모듈(MSUC)을 제안한다. 이 모듈은 네트워크 모델의 서로 다른 계층에 있는 특징 맵 간의 일관성을 제약함으로써, 라벨이 없는 데이터에 대한 반감독 학습 알고리즘의 다중 스케일 학습 능력을 향상시킨다. 기존 준지도 학습 알고리즘은 마지막 계층에서만 교사-학생 간 일관성 정규화를 수행하여, 중간 계층에서 대량의 라벨링되지 않은 데이터로부터 학습된 풍부한 정보를 무시합니다. 반면, 제안된 준지도 학습 방법은 MSUC 모듈을 통해 라벨링되지 않은 RS 이미지로부터 효율적이고 풍부한 다중 스케일 정보를 학습할 수 있습니다.

둘째, 높은 클래스 간 유사성 문제를 해결하기 위한 핵심은 모델의 특징 추출 능력을 향상시키는 것이다. 따라서 우리는 교사 네트워크가 학생 네트워크가 인코더의 출력을 재구성하도록 돕는 교차 교사-학생 어텐션 모듈(CTSA)을 제안한다. 구체적으로, 학생의 인코더 결과를 쿼리로, 교사의 결과를 키와 값으로 사용한다. 이러한 교차 네트워크 어텐션의 장점은 학생과 교사 모델 모두로부터 심층 특징을 학습할 수 있다는 점입니다. 이는 RS 이미지에서 구분이 어려운 범주를 분할하는 데 도움이 됩니다. 예를 들어, 그림 1에서 볼 수 있듯이, 우리의 반감독 학습 모델(그림 1(h))은 RS 이미지에서 SOTA Unimatch 방법(그림 1(g))보다 더 정확한 분할 결과를 달성할 수 있습니다.

2) 풍부한 다중 스케일 정보를 학습하기 위해 MUCA는 네트워크의 서로 다른 계층에서 특징 맵 간의 일관성을 제약하는 다중 스케일 불확실성 일관성 정규화를 도입합니다.

3) 높은 클래스 간 유사성을 구별하기 위해 MUCA는 새로운 교차 교사-학생 어텐션을 활용하여 학생 네트워크가 교사에 의해 식별 가능한 인코딩을 재구성하도록 안내합니다.
