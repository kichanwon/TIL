---
title: Memo
description: 아직 분류하지 않은 생각, 아이디어와 할 일
draft: true
---

# Memo

## Project ideas

- [ ] Search engine 만들기
- [ ] 일기예보 만들기
- [ ] 목소리 기반 에이전트 만들기


1. [VLM / Spatial Intelligence] 멀티모달 LLM의 3D 공간 상식 및 물리적 모순 검증 (Probing & Benchmark)
	- 기본 아이디어: GPT-4o, Claude 3.5 Sonnet, LLaVA 같은 최신 비전-언어 모델(VLM)이 2D 이미지 속 '그림자 방향', '거울 반사', '물체의 무게 중심' 같은 기본적인 3D 물리 법칙과 공간 지능을 제대로 이해하는지를 검증하는 지엽적인 데이터셋(예: 200~300개 이미지)을 직접 구축하고 제로샷(Zero-shot) 성능을 리포트합니다. 모델들이 처참하게 틀리는 양상(Failure Mode)을 분석합니다.
	- 확장: 단순 평가를 넘어, 모델에게 아주 간단한 '공간적 프롬프팅(Spatial Prompting, 예: 바운딩 박스 가이드 제공)'이나 'Test-Time Training (TTT)' 을 먹였을 때 이 물리적 모순을 스스로 교정할 수 있는지 방법론(Methodology)을 추가합니다.
2. [Generative Model / Watermarking] 최신 이미지 생성 모델의 워터마크 취약점 공격 분석 (Adversarial Attack)
	- 기본 아이디어: 최근 Meta(Watermark Anything)나 Google(SynthID) 등이 생성 AI 이미지에 심는 보이지 않는 워터마크 기술을 발표했습니다. 기존의 노이즈 필터링, 크롭(Crop) 외에 최신 초해상도(Super-Resolution) 모델이나 이미지-투-이미지 변환 모델을 거치게 했을 때 워터마크가 얼마나 쉽게 깨지는지(Visual Paraphrasing)를 정량적으로 비교 분석(Empirical Study)합니다.
	- 확장: 워터마크를 깨는 공격 방식을 넘어, 이러한 생성적 공격(Generative Attack)에도 살아남을 수 있는 강건한(Robust) 워터마크 임베딩 손실 함수(Loss Function)를 제안하여 디펜스 전략까지 논문에 포함시킵니다.
3. [Data / Efficiency] 가우시안 스플래팅(3DGS)의 불필요한 가우시안 제거 전략 (Pruning Strategy)
	- 기본 아이디어: 3D Gaussian Splatting은 렌더링이 빠르지만 용량이 큽니다. 렌더링 품질(PSNR)을 거의 떨어뜨리지 않으면서, 카메라 시점 궤적에서 보이지 않거나 기여도가 극히 낮은 가우시안들을 빠르게 솎아내는(Pruning) 단순한 휴리스틱 알고리즘을 구현하고 NeRF 단공개 데이터셋(Mip-NeRF 360 등)에서 성능을 검증합니다.
	-  확장: 단순 휴리스틱을 넘어, 가우시안의 그래디언트(Gradient) 변화량을 추적해 학습 과정 중에 실시간으로 가지치기를 수행하는 동적 푸르닝(Dynamic Pruning) 모듈을 설계하고 메모리 효율성 그래프를 추가합니다.