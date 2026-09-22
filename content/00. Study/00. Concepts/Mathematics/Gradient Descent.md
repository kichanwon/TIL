# Gradient Descent

## Definition

경사하강법(Gradient Descent)은 함수값이 감소하는 방향으로 반복 이동해 최솟값을 찾는 최적화 알고리즘이다.

## Intuition

현재 위치에서 가장 가파르게 내려가는 방향으로 조금씩 이동하는 과정이다.

## Mathematical Definition

$$
\theta_{t+1}=\theta_t-\eta\nabla_\theta L(\theta_t)
$$

여기서 $\eta$는 학습률이다.

## Example

학습률이 너무 크면 최솟값을 지나치고, 너무 작으면 학습이 느려질 수 있다.

## Related Concepts

- [[Gradient]]
- [[Optimization]]
- [[Loss Function]]
- [[Neural Networks/Multi-Layer Perceptron]]

## Applications in AI

역전파로 계산한 그래디언트를 사용해 신경망의 파라미터를 업데이트한다.
