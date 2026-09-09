# Recurrent Neural Network
## RNN의 기본 원리
> [[01. Feed-Forward Neural Network|Feed-Forward Neural Network]]은 각 입력을 
> 	독립적으로 처리하며 *이전 입력에 대한 상태를 직접 유지하지 않음*
> RNN은 현재 입력과 이전 시점의 Hidden State를 함께 사용하여 *순차적인 정보와 시간적 의존성*을 처리

- Input
    - 현재 시점의 입력
- Hidden State
    - 이전 시점의 정보를 저장하여 다음 시점으로 전달
- Output
    - 현재 Hidden State를 기반으로 출력 생성
```text
x1        x2        x3        x4
↓         ↓         ↓         ↓
RNN ───→ RNN ────→ RNN ────→ RNN
h1        h2        h3        h4
```

## Hidden State의 계산
> 현재 입력과 이전 Hidden State를 함께 사용해 *현재 Hidden State*를 계산
> 동일한 RNN Cell과 Weight를 각 Time Step에서 반복적으로 사용

$$\mathbf{h}_t = \phi \left( W_x\mathbf{x}_t + W_h\mathbf{h}_{t-1} + \mathbf{b}_h \right)$$
- 현재 입력과 이전 상태에 각각 Weight를 적용한 뒤 Activation Function을 통해 *현재 상태를 계산*
- $\mathbf{x}_t$
    - 현재 시점의 입력
- $\mathbf{h}_{t-1}$
    - 이전 시점의 Hidden State
- $\mathbf{h}_t$
    - 현재 시점의 Hidden State
- $W_x$
    - Input에 적용되는 Weight
- $W_h$
    - 이전 Hidden State에 적용되는 Recurrent Weight
- $\phi$
    - [[Activation Function]]

## Output의 계산
> 현재 Hidden State에 Output Weight를 적용하여 *현재 시점의 출력*을 생성

$$\mathbf{y}_t = \psi \left( W_y\mathbf{h}_t + \mathbf{b}_y \right)$$
- 현재 Hidden State를 기반으로 각 Time Step의 Output을 계산
- $W_y$
    - Output에 적용되는 Weight
- $\psi$
    - 문제에 따라 사용하는 Output Activation Function

## Time Step과 Weight Sharing
> RNN을 시간축으로 펼치면 여러 Cell이 연결된 형태로 보이지만 
> 	*모든 Time Step에서 동일한 Weight를 공유*
> Sequence 길이에 관계없이 동일한 연산 구조를 반복 적용

```text
t = 1        t = 2        t = 3

 x1           x2           x3
  ↓            ↓            ↓
[RNN] ─────→ [RNN] ─────→ [RNN]
  W            W            W
  ↓            ↓            ↓
  h1           h2           h3
```
- [[Weight Sharing]]
    - 동일한 $W_x$, $W_h$, $W_y$를 모든 Time Step에서 사용
- [[Hidden State]]
    - 이전 시점의 정보를 다음 시점으로 전달

## RNN의 입출력 형태
> RNN은 문제에 따라 입력 Sequence와 출력의 형태를 다양하게 구성 가능
- One-to-One
- One-to-Many
- Many-to-One
- Many-to-Many
    - Synchronized
    - Unsynchronized

Many-to-One
```text
x1 → x2 → x3 → RNN → Class
```
Many-to-Many
```text
x1 → h1 → y1
x2 → h2 → y2
x3 → h3 → y3
```

## RNN의 학습
> Forward 과정에서는 Time Step을 따라 Hidden State를 순차적으로 계산
> 학습 과정에서는 Loss의 Gradient를 시간축의 이전 시점까지 전달하는 
> 	[[Backpropagation Through Time]]을 사용

```text
Forward

h1 → h2 → h3 → h4
```

```text
Backward

h1 ← h2 ← h3 ← h4 ← Loss
```

- [[Forward Propagation]]
- [[Backpropagation Through Time]]
- [[Gradient Descent]]

## 장기 의존성 문제
> Sequence가 길어질수록 Gradient가 많은 Time Step을 거쳐 전달되어야 함
> 반복적인 연산 과정에서 Gradient가 매우 작아지거나 커질 수 있어 
> 	*[[Long Term Dependency|LongTermDependency]] 문제*가 발생
- [[Vanishing Gradient]]
    - Gradient가 반복적으로 작아져 이전 Time Step까지 충분히 전달되지 않음
- [[Exploding Gradient]]
    - Gradient가 반복적으로 커져 학습이 불안정해짐
- [[Long Term Dependency]]
    - 먼 과거의 정보와 현재 출력 사이의 관계를 학습하기 어려움

```text
과거                           현재
h1 → h2 → h3 → ... → h100 → Output
↑
Gradient가 먼 과거의 Time Step까지 역전파되어야 함
```
→ 장기적인 정보를 안정적으로 유지할 수 있는 구조 필요
- [[Long Short Term Memory]]
- [[Gated Recurrent Unit]]

## RNN의 특징과 한계
> Hidden State를 통해 Sequence의 순서와 이전 정보를 처리할 수 있음
> 하지만 Time Step을 순차적으로 계산해야 하므로 긴 Sequence에서는 학습과 병렬화에 한계가 존재

- 장점
    - 순서 정보 처리
    - 시간적 의존성 모델링
    - 가변 길이 Sequence 처리
    - Weight Sharing
- 한계
    - [[Vanishing Gradient]]
    - [[Exploding Gradient]]
    - [[Long Term Dependency]]
    - Time Step 간 순차 계산
        - 병렬화가 어려움

→ 기억 구조 개선
- [[Long Short Term Memory]]
- [[Gated Recurrent Unit]]

→ 필요한 입력 정보를 직접 참조
- [[Attention]]

---

## Note

- 기본 구성
    - Input
    - [[Hidden State]]
    - Output
- 기본 연산
    - Recurrent Operation
    - [[Weight Sharing]]
    - [[Activation Function]]
- 입출력 형태
    - One-to-One
    - One-to-Many
    - Many-to-One
    - Many-to-Many
- 학습
    - [[Forward Propagation]]
    - [[Backpropagation Through Time]]
    - [[Gradient Descent]]
- 문제점
    - [[Vanishing Gradient]]
    - [[Exploding Gradient]]
    - [[Long Term Dependency]]
- 확장
    - [[Long Short Term Memory]]
    - [[Gated Recurrent Unit]]
    - [[Attention]]