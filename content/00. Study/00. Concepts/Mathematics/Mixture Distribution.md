# Mixture Distribution

## Definition

혼합분포(Mixture Distribution)는 여러 확률분포를 가중합해 만든 분포다.

## Intuition

하나의 데이터가 서로 다른 여러 집단에서 발생할 수 있다는 가정을 분포 하나로 표현한다.

## Mathematical Definition

$$
p(x)=\sum_{k=1}^{K}\pi_kp_k(x),\qquad \pi_k\ge0,\quad\sum_k\pi_k=1
$$

## Example

서로 다른 평균을 가진 여러 정규분포를 섞어 군집화 데이터의 분포를 표현할 수 있다.

## Related Concepts

- [[Probability Distribution]]
- [[Joint Distribution]]
- [[Random Variable]]
- [[Uncertainty]]

## Applications in AI

혼합 가우시안 모델, 군집화, 잠재변수 모델에서 여러 데이터 생성 원인을 표현한다.
