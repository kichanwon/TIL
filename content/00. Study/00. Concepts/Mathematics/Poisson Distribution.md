# Poisson Distribution

## Definition

푸아송분포(Poisson Distribution)는 일정한 시간이나 공간 구간에서 사건이 발생하는 횟수를 모델링하는 이산분포다.

## Intuition

평균적으로 일정한 빈도로 발생하는 사건이 특정 구간에서 몇 번 일어나는지 표현한다.

## Mathematical Definition

$$
P(X=k)=\frac{\lambda^ke^{-\lambda}}{k!}
$$

## Example

한 시간 동안 서버에 도착하는 요청 수를 푸아송분포로 모델링할 수 있다.

## Related Concepts

- [[Discrete Distribution]]
- [[Discrete Random Variable]]
- [[Probability Mass Function]]
- [[Expected Value]]

## Applications in AI

이벤트 횟수, 도착률, 결함 수와 같은 카운트 데이터를 분석하는 데 사용된다.
