---
title: Language Models
description: 언어 모델의 기본 구조와 학습 원리
---
[강의_재생목록](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV)

![[Pasted image 20260824030637.png]]
Remember it's all about **efficiency**:
- Resources: data + hardware (compute, memory, communication bandwidth)
- How do you train the best model given a fixed set of resources?

Today, we are compute-constrained, so design decisions will reflect squeezing the most out of given hardware.
- Systems: clearly about efficiency
- Tokenization: working with raw bytes is elegant, but compute-inefficient with today's model architectures
- Model architecture: many changes motivated by reducing memory or FLOPs (e.g., sharing KV caches, sliding window attention)
- Data filtering: avoid wasting precious compute updating on bad / irrelevant data
- Scaling laws: use less compute on smaller models to do hyperparameter tuning


## Tokenization
>a tokenizer converts between raw inputs (bytes) and sequences of integers (tokens)

![](https://cs336.stanford.edu/lectures/images/tokenized-example.png)
Popular tokenizer: **Byte-Pair Encoding** (BPE)  
- [[Sennrich+ 2015]](https://arxiv.org/abs/1508.07909)
Intuition: break input into frequently-occuring chunks

Efficiency lens
- Reduce context length (1000 bytes → ~250 tokens)
- Adaptive computation (more modeling capacity on interesting parts of input)
The dream: tokenizer-free model architectures, which operate directly on bytes  
- [[Xue+ 2021]](https://arxiv.org/abs/2105.13626)
- [[Yu+ 2023]](https://arxiv.org/pdf/2305.07185.pdf)
- [[Pagnoni+ 2024]](https://arxiv.org/abs/2412.09871)
- [[Deiseroth+ 2024]](https://arxiv.org/abs/2406.19223)
- [[Hwang+ 2025]](https://arxiv.org/abs/2507.07955)
	These are promising, but have not yet been scaled up to the frontier.

## Model architecture
Starting point: original Transformer  
- [[Vaswani+ 2017]](https://arxiv.org/pdf/1706.03762.pdf)
![](https://cs336.stanford.edu/lectures/images/transformer-architecture.png)
  

Refinements:
- Activation functions: ReLU, SwiGLU 
	- [[Shazeer 2020]](https://arxiv.org/pdf/2002.05202.pdf)

- Positional encodings: sinusoidal, RoPE 
	- [[Su+ 2021]](https://arxiv.org/pdf/2104.09864.pdf)

- Normalization: LayerNorm, RMSNorm, QK norm, pre-norm versus post-norm 
	- [[Ba+ 2016]](https://arxiv.org/pdf/1607.06450.pdf)
	- [[Zhang+ 2019]](https://arxiv.org/abs/1910.07467)
	- [[Dehghani+ 2023]](https://arxiv.org/abs/2302.05442)
	- [[Xiong+ 2020]](https://arxiv.org/pdf/2002.04745.pdf)

- Attention: full, sparse/local attention, group-query attention (GQA), multi-head latent attention (MLA) 
	- [[Child+ 2019]](https://arxiv.org/pdf/1904.10509.pdf)
	- [[Ainslie+ 2023]](https://arxiv.org/pdf/2305.13245.pdf)
	- [[DeepSeek-AI+ 2024]](https://arxiv.org/abs/2405.04434)

- Recurrence/state-space models/linear attention: Mamba, Gated DeltaNet 
	- [[Katharopoulos+ 2020]](https://arxiv.org/abs/2006.16236)
	- [[Dao+ 2024]](https://arxiv.org/abs/2405.21060)
	- [[Yang+ 2024]](https://arxiv.org/abs/2412.06464)
	- [[Lahoti+ 2026]](https://arxiv.org/abs/2603.15569)

- MLP: dense, mixture of experts 
	- [[Shazeer+ 2017]](https://arxiv.org/pdf/1701.06538.pdf)
	- [[Fedus+ 2021]](https://arxiv.org/abs/2101.03961)

- Shape (hidden dimension, depth, number of heads, number of experts)