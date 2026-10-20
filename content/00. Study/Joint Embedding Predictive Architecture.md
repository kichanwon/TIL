#### 오리지날 모델
1. 최초의 JEPA에 대한 제안: [Yann Lecun on a Vision to Make AI Systems Learn and Reason like Animals and Humans](https://ai.meta.com/blog/yann-lecun-advances-in-ai-research/?utm_source=www.turingpost.com&utm_medium=referral&utm_campaign=topic-4-what-is-jepa)
2. I-JEPA: [Self-Supervised Learning from Images with a Joint-Embedding Predictive Architecture](https://arxiv.org/abs/2301.08243?utm_source=www.turingpost.com&utm_medium=referral&utm_campaign=topic-4-what-is-jepa)
3. MC-JEPA: [A Joint-Embedding Predictive Architecture for Self-Supervised Learning of Motion and Content Features](https://arxiv.org/abs/2307.12698?utm_source=www.turingpost.com&utm_medium=referral&utm_campaign=topic-4-what-is-jepa)
4. V-JEPA: [The Next Step toward Advanced Machine Intelligence](https://ai.meta.com/blog/v-jepa-yann-lecun-ai-model-video-joint-embedding-predictive-architecture/?utm_source=www.turingpost.com&utm_medium=referral&utm_campaign=topic-4-what-is-jepa)
5. Generalizing JEPA: [Learning and Leveraging World Models in Visual Representation Learning](https://arxiv.org/abs/2403.00504?utm_source=www.turingpost.com&utm_medium=referral&utm_campaign=topic-4-what-is-jepa)

#### 얀 르쿤 강연
- [Munich Presentation (2023년 9월 29일)](https://x.com/TheTuringPost/status/1709190072715886892?utm_source=www.turingpost.com&utm_medium=referral&utm_campaign=topic-4-what-is-jepa)
- [Harvard Presentation (2024년 3월 28일)](https://x.com/TheTuringPost/status/1781835268196307296?utm_source=www.turingpost.com&utm_medium=referral&utm_campaign=topic-4-what-is-jepa) 

#### JEPA에서 영감을 받은 모델

##### 오디오와 스피치 영역
- [A-JEPA](https://arxiv.org/abs/2311.15830v3?utm_source=www.turingpost.com&utm_medium=referral&utm_campaign=topic-4-what-is-jepa): 오디오 및 음성 분류 작업에서 문맥의 의미를 더 잘 이해하도록 하기 위해서 ‘마스크 모델링’ 원리를 사용
- [Investigating Design Choices in Joint-Embedding Predictive Architectures for General Audio Representation Learning](https://arxiv.org/abs/2405.08679?utm_source=www.turingpost.com&utm_medium=referral&utm_campaign=topic-4-what-is-jepa): 자기지도 방식의 오디오 표현 (Audio Representation) 학습에서 마스킹 전략과 샘플의 길이를 분석

##### 비전 (Visual)과 공간 (Spatial) 데이터 영역
- [S-JEA](https://arxiv.org/abs/2305.11701?utm_source=www.turingpost.com&utm_medium=referral&utm_campaign=topic-4-what-is-jepa): 스택형 조인트 임베딩 아키텍처에서 계층적 의미 표현을 통해 시각적 표현 학습을 향상
- [DMT-JEPA](https://arxiv.org/abs/2405.17995?utm_source=www.turingpost.com&utm_medium=referral&utm_campaign=topic-4-what-is-jepa): 분류, 객체 감지 및 세그먼테이션에 적용할 수 있는 Local Semantic Understanding(로컬 의미 이해)에 중점을 둔 이미지 모델링을 목표
- [JEP-KD](https://arxiv.org/abs/2403.18843?utm_source=www.turingpost.com&utm_medium=referral&utm_campaign=topic-4-what-is-jepa): 시각적 음성 인식 모델을 오디오 기능에 맞춰 조정하여 음성 인식 성능을 향상
- [Point-JEPA](https://arxiv.org/abs/2404.16432?utm_source=www.turingpost.com&utm_medium=referral&utm_campaign=topic-4-what-is-jepa): JEPA를 포인트 클라우드 데이터에 적용, 공간 데이터셋의 효율성과 표현 학습을 향상
- [Signal-JEPA](https://arxiv.org/abs/2403.11772?utm_source=www.turingpost.com&utm_medium=referral&utm_campaign=topic-4-what-is-jepa): EEG 신호 처리를 위해서 뇌파 분석 시의 Cross-Dataset Transfer 및 분류 성능을 개선

##### 그래프와 동적인 데이터를 다루는 영역
- [Graph-JEPA](https://arxiv.org/abs/2309.16014?utm_source=www.turingpost.com&utm_medium=referral&utm_campaign=topic-4-what-is-jepa): 서브그래프 표현을 위해 쌍곡선 좌표 예측을 사용하는 최초의 그래프용 JEPA
- [ST-JEMA](https://arxiv.org/abs/2403.06432?utm_source=www.turingpost.com&utm_medium=referral&utm_campaign=topic-4-what-is-jepa): 높은 수준의 의미론적 표현에 초점을 맞춰, fMRI 데이터에서 동적 기능적 연결성을 더 잘 학습하도록 함

##### 시계열 데이터 및 원격 센싱 영역
- [LaT-PFN](https://arxiv.org/abs/2405.10093?utm_source=www.turingpost.com&utm_medium=referral&utm_campaign=topic-4-what-is-jepa): 시계열 예측과 JEPA를 결합하여, 강력한 ICL (In-Context Learning)을 지원
- [Time-Series JEPA](https://arxiv.org/abs/2406.04853?utm_source=www.turingpost.com&utm_medium=referral&utm_campaign=topic-4-what-is-jepa): 센서 데이터의 시공간적 상관관계를 통해 제한된 용량의 네트워크에서 원격 제어를 최적화