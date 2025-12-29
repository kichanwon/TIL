---
tags:
  - paper_review
draft: true
title: "ON LARGE-BATCH TRAINING FOR DEEP LEARNING : GENERALIZATION GAP AND SHARP MINIMA (2017)"
---
- [ON LARGE-BATCH TRAINING FOR DEEP LEARNING : GENERALIZATION GAP AND SHARP MINIMA (2017)](https://arxiv.org/pdf/1609.04836)
- [참고](https://velog.io/@becky-kwon/%EB%85%BC%EB%AC%B8-%EB%A6%AC%EB%B7%B0-ON-LARGE-BATCH-TRAINING-FOR-DEEP-LEARNING-GENERALIZATION-GAP-AND-SHARP-MINIMA-2017)
### 기본 정보
- 논문 제목
	ON LARGE-BATCH TRAINING FOR DEEP LEARNING : GENERALIZATION GAP AND SHARP MINIMA
- 저자
	Nitish Shirish Keskar, Dheevatsa Mudigere, Jorge Nocedal, Mikhail Smelyanskiy, Ping Tak Peter Tang
- 발표연도
	Published as a conference paper at ICLR 2017
- 주요 키워드
	Large Batch, Generalization Gap, Sharp Minima, Deep Learning Optimization

---
ABSTRACT
1 INTRODUCTION
1.1 NOTATION
2 DRAWBACKS OF LARGE-BATCH METHODS
2.1 OUR MAIN OBSERVATION
2.2 NUMERICAL EXPERIMENTS
2.2.1 PARAMETRIC PLOTS
2.2.2 SHARPNESS OF MINIMA
3 SUCCESS OF SMALL-BATCH METHODS
4 DISCUSSION AND CONCLUSION

### 연구 배경
- 대규모 데이터와 병렬 학습이 가능해지며 배치 크기(batch size) 를 크게 설정하는 경향이 증가.
- 그러나 실험적으로 큰 배치 크기일수록 테스트 정확도가 떨어지는 현상이 관찰됨.
- 기존 연구는 이 현상의 원인과 학습 동역학에 대한 명확한 설명 부족.
- 본 논문은 “큰 배치 학습이 왜 일반화 성능을 저하시킬까?” 에 대한 이론적·실험적 분석 수행.

---

### 주요 아이디어
- 핵심 개념 또는 모델 구조 요약
- 새로운 알고리즘/이론의 포인트
- Figure·Equation 중심으로 핵심 직관 정리

---

### 방법론
- 데이터셋 / 실험 환경
- 모델 구조, 학습 설정, 하이퍼파라미터
- 비교 대상 (Baseline)

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