# Sum and Product of Random Variables

## Definition

확률변수의 합과 곱은 여러 불확실한 양을 결합해 새로운 확률변수를 만드는 연산이다.

## Intuition

여러 랜덤한 요인이 더해지거나 서로 영향을 주는 상황의 결과를 모델링한다.

## Mathematical Definition

독립인 연속확률변수의 합은 컨볼루션으로 분포를 계산할 수 있다.

$$
f_{X+Y}(z)=\int f_X(x)f_Y(z-x)\,dx
$$

## Example

여러 측정 오차의 합이나 투자 수익률의 곱을 새로운 확률변수로 다룰 수 있다.

## Related Concepts

- [[Random Variable]]
- [[Joint Distribution]]
- [[Marginal Probability]]
- [[Expected Value]]
- [[Variance]]

## Applications in AI

여러 불확실한 신호를 결합하고, 확률 모델의 변환과 합성 분포를 계산하는 데 사용된다.
