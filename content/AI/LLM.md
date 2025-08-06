# Large Language Models Explained

Large language models (LLMs) are deep learning algorithms that can recognize, summarize, translate, predict, and generate content using very large datasets.

Large language models (LLMs)은 매우 큰 데이터 세트를 사용하여 콘텐츠를 인식, 요약, 번역, 예측하고 생성할 수 있는 딥러닝 알고리즘입니다.

---
## What are Large Language Models?

Large language models 은 주로 [[Transformer Networks]]라고 불리는 딥러닝 아키텍처의 한 종류입니다.
 [[Transformer Networks]]은 이 문장의 단어와 같이 순차적 데이터의 관계를 추적하여 맥락과 의미를 학습하는 신경망입니다.

Transformer는 여러 개의 transformer 블록, 즉 레이어로 구성됩니다. 예를 들어, transformer에는  self-attention layers, feed-forward layers, normalization layers 이 있으며, 이 모든 계층이 함께 작동하여 입력을 해석하고 추론 시 출력 스트림을 예측합니다. 레이어를 쌓아 더 깊은 transformer와 강력한 언어 모델을 만들 수 있습니다. 

Transformer는 Google에서 2017년 논문 "Attention Is All You Need"에서 처음 소개되었습니다.[“Attention Is All You Need.”](https://arxiv.org/abs/1706.03762)  

![Explore about how transformer models work](https://www.nvidia.com/content/nvidiaGDC/us/en_US/glossary/large-language-models/_jcr_content/root/responsivegrid/nv_container_1795650/nv_container/nv_image.coreimg.svg/1685574368357/llm-how-it-works-chart.svg "Explore about how transformer models work")

Figure 1. How transformer models work.



**transformer를 대규모 언어 모델에 특히 적합하게 만드는 두 가지 주요 혁신이 있습니다. 
positional encoding (위치 인코딩)과 self-attention입니다.**

Positional encoding은 주어진 시퀀스 내에서 입력이 발생하는 순서를 포함합니다. 기본적으로, 문장 내의 단어를 순차적으로 신경망에 입력하는 대신, 위치 인코딩 덕분에 단어를 비순차적으로 입력할 수 있습니다.

Self-attention은 입력 데이터를 처리하는 동안 각 부분에 가중치를 할당합니다. 이 가중치는 나머지 입력에 비해 해당 입력의 중요성을 나타냅니다. 즉, 모델은 더 이상 모든 입력에 동일한 주의를 기울일 필요가 없으며, 실제로 중요한 입력 부분에 집중할 수 있습니다. 신경망이 입력의 어떤 부분에 주의를 기울여야 하는 지에 대한 이러한 표현은 모델이 엄청난 양의 데이터를 걸러내고 분석하면서 시간이 지남에 따라 학습됩니다.

이 두 가지 기술을 함께 사용하면 서로 다른 요소가 거리에 크게 구애받지 않고 비순차적으로 서로에게 영향을 미치고 관계를 맺는 미묘한 방식과 맥락을 분석할 수 있습니다.

데이터를 비순차적으로 처리하는 기능은 복잡한 문제를 여러 개의 작은 동시 계산으로 분해할 수 있게 해줍니다. 당연히 GPU는 이러한 유형의 문제를 병렬로 해결하는 데 적합하므로 레이블이 지정되지 않은 대규모 데이터 세트와 거대한  [[Transformer Networks]] 를 대규모로 처리할 수 있습니다.

## Why are Large Language Models Important?

지금까지 AI 모델은 인식(perception)과 이해(understanding)에 중점을 두었습니다. 

하지만 수천 억 개의 매개변수가 포함된 인터넷 규모의 데이터세트로 학습된 대규모 언어 모델을 통해 이제 AI 모델이 인간과 유사한 콘텐츠를 생성할 수 있는 능력을 갖추게 되었습니다.

모델은 신뢰할 수 있는 방식으로 읽고, 쓰고, 코딩하고, 그림을 그리고, 만들 수 있으며, 인간의 창의력을 증강하고 산업 전반의 생산성을 향상시켜 세계에서 가장 어려운 문제를 해결할 수 있습니다. 

이러한 LLM의 응용 분야는 수많은 사용 사례에 걸쳐 있습니다. 예를 들어, AI 시스템은 단백질 서열의 언어를 학습하여 과학자들이 획기적이고 생명을 구할 수 있는 백신을 개발하는 데 도움이 되는 실행 가능한 화합물을 제공할 수 있습니다. 

인간이 가장 잘하는 창의력, 의사소통, 창작 또는 소프트웨어 프로그래머는 자연어 설명을 기반으로 코드를 생성하기 위해 LLM을 활용하여 생산성을 높일 수 있습니다. 

## What are Large Language Model examples?

전체 컴퓨팅 스택의 발전으로 점점 더 정교한 LLM을 개발할 수 있게 되었습니다. 2020년 6월, OpenAI는 짧은 프롬프트로 텍스트와 코드를 생성하는 1,750억 개의 파라미터로 구성된 모델인 GPT-3를 출시했습니다. 2021년에는 엔비디아와 마이크로소프트가 5,300억 개의 파라미터로 구성된 세계 최대 규모의 독해 및 자연어 추론 모델 중 하나인 메가트론 튜링 자연어 생성 530B(Megatron-Turing Natural Language Generation 530B)를 개발했습니다. 

LLM의 규모가 커짐에 따라 그 역량도 커졌습니다. 텍스트 기반 콘텐츠에 대한 LLM 사용 사례는 크게 다음과 같이 나눌 수 있습니다: 

1. Generation (e.g., 스토리 작성, 마케팅 콘텐츠 제작)
    
2. Summarization (e.g., 법률적 의역, 회의록 요약)
    
3. Translation (e.g., 언어 간, 텍스트-코드 간)
    
4. Classification (e.g., 독성 분류, 감정 분석)
    
5. Chatbot (e.g., 오픈 도메인 Q+A, 가상 비서)



## How Do Large Language Models Work?

대규모 언어 모델은 [[Unsupervised Learning]](비지도 학습)을 사용하여 학습됩니다. 비지도 학습을 통해 모델은 레이블이 지정되지 않은 데이터 세트를 사용하여 이전에 알려지지 않은 데이터 패턴을 찾을 수 있습니다. 또한 AI 모델을 구축할 때 가장 큰 어려움 중 하나인 광범위한 데이터 라벨링이 필요하지 않습니다.

LLM이 거치는 광범위한 학습 프로세스 덕분에 특정 작업에 대해 모델을 학습시킬 필요가 없으며 대신 여러 사용 사례에 사용할 수 있습니다. 이러한 유형의 모델을 foundation models (기초 모델)이라고 합니다. 

기초 모델이 많은 교육이나 훈련 없이도 다양한 목적에 맞는 텍스트를 생성할 수 있는 기능을 zero-shot learning 이라고 합니다. 이 기능의 다양한 변형에는 one-shot 또는 few-shot 학습이 포함되며, 기초 모델에 작업을 수행하는 방법을 보여주는 하나 또는 몇 개의 예시가 제공되어 특정 사용 사례를 이해하고 더 잘 수행할 수 있도록 합니다.

특정 사용 사례에 맞게 이러한 대규모 언어 모델을 배포하려면 여러 가지 기술을 사용하여 모델을 사용자 지정하여 정확도를 높일 수 있습니다. 일부 기술에는  [prompt tuning](https://developer.nvidia.com/blog/adapting-p-tuning-to-solve-non-english-downstream-tasks/), fine-tuning, and [adapters](https://docs.nvidia.com/deeplearning/nemo/user-guide/docs/en/v1.10.0/core/adapters/intro.html)가 있습니다. 

![The structure of encoder-decoder language models](https://www.nvidia.com/content/nvidiaGDC/us/en_US/glossary/large-language-models/_jcr_content/root/responsivegrid/nv_container_8711364/nv_container/nv_image.coreimg.svg/1685574368854/llm-encoder-decoder-chart.svg "The structure of encoder-decoder language models")

Figure 2. Image shows the structure of encoder-decoder language models.


다양한 유형의 사용 사례에 적합한 여러 대규모 언어 모델이 있습니다:

- **Encoder only**: 이러한 모델은 일반적으로 분류 및 감정 분석과 같이 언어를 이해할 수 있는 작업에 적합합니다. 인코더 전용 모델의 예로는 BERT(Bidirectional Encoder Representations from Transformers)가 있습니다.

- **Decoder only**: 이 모델은 언어와 콘텐츠를 생성하는 데 매우 능숙합니다. 일부 사용 사례에는 스토리 작성 및 블로그 생성이 포함됩니다. 디코더 전용 아키텍처의 예로는 GPT-3(Generative Pretrained Transformer 3)가 있습니다.

- **Encoder-decoder**: 이 모델은 트랜스포머 아키텍처의 인코더와 디코더 구성 요소를 결합하여 콘텐츠를 이해하고 생성합니다. 이 아키텍처가 빛을 발하는 몇 가지 사용 사례에는 번역과 요약이 포함됩니다. 인코더-디코더 아키텍처의 예로는 T5(Text-to-Text Transformer)가 있습니다.


## What are the Challenges of Large Language Models?

대규모 언어 모델을 개발하고 유지 관리하는 데 필요한 막대한 자본 투자, 대규모 데이터 세트, 기술 전문성, 대규모 컴퓨팅 인프라는 대부분의 기업에게 진입 장벽으로 작용해 왔습니다.

![Compute required for training transformer models](https://www.nvidia.com/content/nvidiaGDC/us/en_US/glossary/large-language-models/_jcr_content/root/responsivegrid/nv_container_1788249/nv_container/nv_image_210846034.coreimg.svg/1685574369134/llm-requirements-chart.svg "Compute required for training transformer models")

Figure 3. Compute required for training transformer models.

1. **Compute-, cost-, and time-intensive workload**: LLM을 유지 및 개발하려면 상당한 자본 투자, 기술 전문 지식, 대규모 컴퓨팅 인프라가 필요합니다. LLM을 훈련하려면 수천 대의 GPU와 몇 주에서 몇 달의 전용 훈련 시간이 필요합니다. 
2. **Scale of data required**: 앞서 언급했듯이 대규모 모델을 학습하려면 상당한 양의 데이터가 필요합니다. 많은 기업이 대규모 언어 모델을 학습 시킬 수 있을 만큼의 대규모 데이터 세트에 액세스하는 데 어려움을 겪고 있습니다. 이 문제는 금융 또는 건강 데이터와 같이 비공개 데이터가 필요한 사용 사례의 경우 더욱 복잡해집니다. 실제로 모델 학습에 필요한 데이터가 존재하지 않을 수도 있습니다.
3. **Technical expertise**:  대규모 언어 모델을 학습하고 배포하는 것은 그 규모 때문에 매우 어렵고  deep learning workflows, transformers, 분산 소프트웨어 및 하드웨어에 대한 높은 이해와 수천 개의 GPU를 동시에 관리할 수 있는 역량이 필요합니다.

https://www.nvidia.com/en-us/glossary/large-language-models/