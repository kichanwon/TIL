# Language Models from Scratch
Problem: researchers are becoming **disconnected** f rom the underlying technology.

## Building small language models might not be representative of large language models

![[Pasted image 20260824023926.png]]

> "작은 모델에서 관찰한 특성이 큰 모델에서도 동일하다고 가정할 수 없다."

작은 모델(760M) FLOPs 비율 
- MHA 35%, FFN 44%
큰 모델(175B) FLOPs 비율
- MHA 17%, FFN 80%

*모델의 Scale 변화*에 따른 *모델의 계산 특성 변화*가 일어날 수 있다.
즉 *FLOPS 구성 변화*에 따른 *계산/메모리/하드에어 특성 변화*가 있을 수 있고,
이는 **병목 및 최적화 포인트의 변화 가능성**이다.

![[Pasted image 20260824024739.png]]
> Scale 증가에 따른 모델의 task 성능/행동 변화


## It's all about efficiency  
> How do you train the best model given a fixed set of resources?  
### Resources    
- Data  
- Compute  
- Memory  
- Communication bandwidth  
현재는 **compute-constrained**이므로, 제한된 Hardware에서 최대한의 성능을 얻는 방향으로 설계한다.  
![[Pasted image 20260824030637.png]]
### Efficiency drives design decisions  
- **Data processing** → 나쁜/불필요한 데이터에 Compute 낭비하지 않기  
- **Tokenization** → sequence length를 줄여 Compute 효율 향상  
- **Model architecture** → Memory/FLOPs 감소  
- **Training** → 제한된 Compute를 효율적으로 사용  
- **Scaling laws** → 작은 모델로 Hyperparameter 탐색  
- **Alignment** → 원하는 목적에 맞게 모델을 조정하여 필요한 Base Model 규모 감소  


---  
  
## Course Overview
systems()
scaling_laws()
data()
alignment()

  
    text("Credible open-weight models (weights + paper):")
    text("- Meta's Llama models "), link(llama_2023), link(llama_2_2023), link(llama_3_2024)
    text('- Mistral\'s models '), link(mistral_7b_2023), link(mixtral_2024)
    text("- DeepSeek\'s models "), link(deepseek_67b_2024), link(deepseek_v2_2024), link(deepseek_v3_2024), link(deepseek_r1_2025), link(deepseek_v3_2_2025)
    text("- Alibaba\'s Qwen models "), link(qwen_2_5_2024), link(qwen_3_2025)
    text("- Moonshot's Kimi models "), link(kimi_1_5_2025), link(kimi_k2_5_2026)
    text("- Z.ai's GLM models "), link(glm_4_5_2025), link(glm_5_2026)
    text("- Minimax\'s models "), link(minimax_m2_5_2026)
    text("- Xiaomi's MIMO models "), link(xiaomi_mimo_v2_2026)
    text("These models are approaching closed models (GPT, Claude, Gemini, etc.).")

    text("Open-source models (weights + paper + code + data):")
    text("- AI2's Olmo models "), link(olmo_7b_2024), link(olmo_2_2025), link(olmo_3_2025)
    text("- NVIDIA's Nemotron models "), link(nemotron_15b_2024), link(nemotron_3_2025)
    text("- Marin's models (open development) "), link(marin_8b_2025), link(marin_32b_2025)

    text("Openness is important for trust and innovation "), link("https://arxiv.org/abs/2403.07918")
    text("Ideas from open models enable us to teach CS336.")

    text("What is a language model?")
    text("- 2018 (BERT): something you fine-tune")
    text("- 2020 (GPT-3): something you prompt")
    text("- 2022 (ChatGPT): something you talk to "), link(title="example conversation", url="https://huggingface.co/datasets/HuggingFaceTB/smoltalk/viewer/all/train?row=72&conversation-viewer=72")
    text("- 2026 (agents): something that acts autonomously "), link(title="example trace", url="https://huggingface.co/datasets/nebius/SWE-rebench-openhands-trajectories/viewer/default/train?conversation-viewer=1")

    text("The fundamentals are the same (attention, kernels, optimization).")
    text("The specs are different (longer context, inference efficiency matters even more).")

