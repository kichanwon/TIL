
A transformer model is a neural network that learns context and thus meaning by tracking relationships in sequential data like the words in this sentence.

transformer model은 이 문장의 단어와 같은 순차적 데이터의 관계를 추적하여 맥락과 의미를 학습하는 신경망입니다.

---

## **So, What’s a Transformer Model?**

트랜스포머 모델은 이 문장의 단어와 같이 연속된 데이터의 관계를 추적하여 문맥과 의미를 학습하는 신경망입니다.

트랜스포머 모델은  attention 또는 self-attention이라고 하는 진화하는 수학적 기법을 적용하여 일련의 멀리 떨어진 데이터 요소들이 서로 영향을 주고받는 미묘한 방식을 감지합니다.

## **Two big innovations**

1. **Positional encoding:** 
    
2. **Self-attention:** 

## **No Labels, More Performance**

Transformer가 등장하기 전에는 사용자들이 대량의 레이블이 지정된 데이터 세트로 신경망을 훈련해야 했는데, 이를 만드는 데는 비용과 시간이 많이 걸렸습니다. 
Transformer는 요소 간의 패턴을 수학적으로 찾아내어 그런 필요성을 없애고, 웹과 기업 데이터베이스에 있는 수조 개의 이미지와 Petabyte 규모의 텍스트 데이터를 제공합니다.

또한, Transformer가 사용하는 수학은 병렬 처리에 적합하므로 이러한 모델을 빠르게 실행할 수 있습니다.

## **How Transformers Pay Attention**

대부분의 신경망과 마찬가지로 Transformer 모델은 기본적으로 데이터를 처리하는 대규모 인코더/디코더 블록입니다.

이러한 블록에 작지만 전략적인 추가 요소를 추가하면(아래 다이어그램 참조)
transformer가 유일무이하게 강력해집니다.

[![Example of a transformer model and self-attention](https://blogs.nvidia.com/wp-content/uploads/2022/03/Transformer-model-example-aidan-gomez-672x400.jpg)](https://blogs.nvidia.com/wp-content/uploads/2022/03/Transformer-model-example-aidan-gomez.jpg)

Transformer는 positional encoder (위치 인코더)를 사용하여 네트워크로 들어오고 나가는 데이터 요소에 태그를 지정합니다. 어텐션 유닛은 이러한 태그를 따라가며 각 요소가 다른 요소와 어떻게 관련되어 있는지에 대한 일종의 대수적 맵을 계산합니다.

Attention queries (어텐션 쿼리)는 일반적으로 멀티헤드 어텐션이라고 하는 방정식 행렬을 계산하여 병렬로 실행됩니다.

이러한 도구를 사용하면 컴퓨터는 사람과 동일한 패턴을 볼 수 있습니다.

## **Self-Attention Finds Meaning**

예를 들어, 다음 문장에서:

_She poured water from the pitcher to the cup until it was full._ 

우리는 "it"이 cup을 가리키는 것을 알고 있지만 다음 문장에서는

_She poured water from the pitcher to the cup until it was empty._

우리는 "그것"이 pitcher를 가리키는 것을 알고 있습니다.

>“Meaning is a result of relationships between things, and self-attention is a general way of learning relationships,” 
>"의미는 사물 간의 관계에서 비롯되고, self-attention은 관계를 배우는 일반적인 방법입니다."



[IBM](https://www.ibm.com/think/topics/transformer-model)
## How do transformer models work?

트랜스포머 모델은 self-attention 메커니즘과 feedforward 신경망을 포함하는 일련의 계층을 통해 토큰 시퀀스나 기타 구조화된 데이터 등 입력 데이터를 처리하는 방식으로 작동합니다. 트랜스포머 모델의 작동 방식에 대한 핵심 아이디어는 몇 가지 주요 단계로 나눌 수 있습니다.

영어 문장을 프랑스어로 변환해야 한다고 가정해 봅시다. 다음은 트랜스포머 모델을 사용하여 이 작업을 수행하기 위해 수행해야 하는 단계입니다.

1. **Input embeddings:** The input sentence is first transformed into numerical representations called embeddings. These capture the semantic meaning of the tokens in the input sequence. For sequences of words, these embeddings can be learned during training or obtained from pre-trained word embeddings.
    
2. **Positional encoding:** Positional encoding is typically introduced as a set of additional values or vectors that are added to the token embeddings before feeding them into the transformer model. These positional encodings have specific patterns that encode the position information.
    
3. **Multi-head attention:** Self-attention operates in multiple "attention heads" to capture different types of relationships between tokens. Softmax functions, a type of activation function, are used to calculate attention weights in the self-attention mechanism.
    
4. **Layer normalization and residual connections:** The model uses layer normalization and residual connections to stabilize and speed up training.
    
5. **Feedforward neural networks:** The output of the self-attention layer is passed through feedforward layers. These networks apply non-linear transformations to the token representations, allowing the model to capture complex patterns and relationships in the data.
    
6. **Stacked layers:** Transformers typically consist of multiple layers stacked on top of each other. Each layer processes the output of the previous layer, gradually refining the representations. Stacking multiple layers enables the model to capture hierarchical and abstract features in the data.
    
7. **Output layer:** In sequence-to-sequence tasks like neural machine translation, a separate decoder module can be added on top of the encoder to generate the output sequence.
    
8. **Training:** Transformer models are trained using supervised learning, where they learn to minimize a loss function that quantifies the difference between the model's predictions and the ground truth for the given task. Training typically involves optimization techniques like Adam or stochastic gradient descent (SGD).
    
9. **Inference:** After training, the model can be used for inference on new data. During inference, the input sequence is passed through the pre-trained model, and the model generates predictions or representations for the given task.


## MoE (Mixture of Experts)

모델 파라미터 수가 많을수록, 모델 성능이 올라간다는 것은 이미 여러 연구를 통해 Scaling-Law가 입증되었습니다. 그래서 모델의 성능을 파악할 때 파라미터 수는 가장 중요한 요소 중 하나입니다. 
하지만 모두가 OpenAI, Google, Meta 같은 빅테크 기업들처럼 컴퓨팅 자원을 자유롭게 사용할 수는 없기에, 현실적으로 제한된 컴퓨팅 자원으로 모델 학습/서빙을 해야합니다.

모델 사이즈가 커질수록 학습/서빙 비용은 늘어날 수 밖에 없는데, MoE는 학습/서빙 비용은 유지하면서도 모델 사이즈를 키울 수 있는 방법입니다.

[MOE](https://sooftware.io/moe/)
