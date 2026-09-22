# Binomial Distribution

## Definition

이항분포(Binomial Distribution)는 동일한 성공 확률을 가진 독립적인 베르누이 시행을 여러 번 했을 때 성공 횟수의 분포다.

## Intuition

정해진 횟수의 시도에서 성공이 몇 번 발생하는지 모델링한다.

## Mathematical Definition

$$
P(X=k)=\binom{n}{k}p^k(1-p)^{n-k}
$$

## Example

동전을 10번 던져 앞면이 나온 횟수를 이항분포로 표현할 수 있다.

## Related Concepts

- [[Discrete Distribution]]
- [[Discrete Random Variable]]
- [[Probability Mass Function]]
- [[Expected Value]]

## Applications in AI

이진 사건의 반복 횟수와 성공률을 추정하는 통계 모델에 사용된다.
