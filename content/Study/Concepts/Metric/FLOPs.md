---
draft:
title: "FLOPS: Floating Point Operations"
---
Floating Point Operations

딥러닝에서 **연산량(계산 비용)을 측정하는 지표**
- 곱셈
- 덧셈
같은 **부동소수점 계산 횟수**를 센 값이다.

예:
- 3×3 Conv 연산 한 번 = 여러 FLOP의 집합
- 모델 전체 연산량은 모든 Conv/Fully Connected 등의 FLOP을 더한 값

따라서:
**FLOPs = 모델이 한 번 forward-pass 할 때 필요한 총 계산량**
→ 계산 비용 비교, 속도 비교에 매우 자주 쓰임  
