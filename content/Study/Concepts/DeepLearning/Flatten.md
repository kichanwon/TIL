---
title: "Flatten Operation: Shape Transformation Without Information Loss"
---
Flatten은 “Tensor → 벡터”로 만드는 연산

# **Flatten은 단순히 모양만 바꾼다 (정보 보존 100%)**
7×7×2048 → Flatten → 100352차원 벡터
Flatten은 데이터의 **값을 그대로 유지**면서 배치를 1D 벡터로 바꾸는 것.
**정보를 전혀 손실시키지 않는다.**