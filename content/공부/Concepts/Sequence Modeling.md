## Sequence를 다루는 이유
1. *기존 신경망([[01. Feed-Forward Neural Network|FFN]])* 은 *입력을 독립적으로 처리*하며 **이전 입력에 대한 상태를 직접 유지하지 않음**
2. 하지만 실제 데이터에는 **이전 상태와 순서 정보가 현재 의미에 영향을 주는 경우**가 많음
3. 따라서 *과거 정보를 기억*하면서 *현재 입력을 처리*할 수 있는 구조가 필요해짐

## Sequence Modeling의 기본 원리
### 순차 정보의 상태 표현
> 기존 FFN은 각 입력을 독립적으로 처리하며 *이전 입력에 대한 상태를 유지하지 않음*
> Sequence에서는 현재 입력뿐 아니라 *이전 시점의 정보까지 함께 고려할 필요*가 있음
- [RNN](<Recurrent Neural Network.md>)
	- 현재 입력과 이전 상태를 함께 사용해 현재 상태를 계산
	- 이전 시점의 정보를 Hidden State에 저장하여 다음 시점으로 전달
		- $h_t = f(x_t, h_{t-1})$

### 장기 의존성 문제와 기억 구조
> Sequence가 길어질수록 *초반 정보와 Gradient가 여러 Time Step을 거쳐 전달되기 어려워짐*
> 이로 인해 먼 과거의 정보를 현재 시점까지 학습하기 어려운 문제가 발생
> [[Vanishing Gradient]] → [[Long Term Dependency]]
- [[Long Short Term Memory|LSTM]]
	- Cell State와 Gate 구조를 사용해 필요한 정보를 오래 유지
    - 장기 의존성 문제와 Vanishing Gradient를 완화
- [[Gated Recurrent Unit|GRU]]
	- LSTM을 단순화하여 Hidden State와 두 개의 Gate로 정보 유지
	- 파라미터가 적고 계산이 비교적 단순함

### 비정렬 Sequence 변환
> 기존 Many-to-Many 구조(Synchronized)는 입력과 출력이 각 Time Step에서 대응되는 경우에 적합
> 하지만 번역처럼 *입력과 출력의 길이·순서가 다르거나 직접 대응하지 않는 경우*
> 	별도의 Sequence 변환 구조가 필요

- Many-to-Many(Unsynchronized)
	- [Seq2Seq](Seq2Seq.md)
		- 입력 Sequence를 출력 Sequence로 변환하는 구조
		- [Encoder–Decoder](Encoder–Decoder)
			- Encoder가 입력 Sequence를 표현으로 변환
			- Decoder가 해당 표현을 기반으로 출력 Sequence를 생성
### 선택적 정보 참조
> 초기 Encoder–Decoder는 
> 	입력 Sequence 전체를 *하나의 Context Vector로 압축*하여 *Decoder에 전달*
> Sequence가 길어질수록 *하나의 고정된 표현만으로 모든 입력 정보를 유지하기 어려운 문제*가 발생
- [Attention](Attention.md)
	- Decoder가 출력 시점마다 Encoder의 여러 상태를 직접 참조
	- 필요한 정보에 더 높은 가중치를 부여하여 선택적으로 활용

---
## Note
- 시퀀스의 계산 방식
    - [[Recurrent Neural Network]]
    - [[Long Short Term Memory]]
    - [[Gated Recurrent Unit]]
- 모델의 입출력 형태
    - One-to-One
    - One-to-Many
    - Many-to-One
    - Many-to-Many
        - Synchronized
        - Unsynchronized
- 전체 모델 아키텍처
    - [[Seq2Seq]]
	    - [[Encoder-Decoder]]
- 정보 참조 메커니즘
    - [[Attention]]
    - [[Self-Attention]]
    - [[Cross-Attention]]
