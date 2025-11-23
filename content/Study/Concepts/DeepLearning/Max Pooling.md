---
title: "Max Pooling: Spatial Downsampling via Local Maximum Selection"
---
**MaxPool은 “Tensor의 공간에서 최대값을 추출하는 연산”**  
2×2 영역에서 가장 큰 값만 선택해 **해상도를 줄이는 연산**

**MaxPool은 공간 정보를 줄이며 중요한 활성만 남긴다**  
예: 7×7×C → MaxPool(2×2) → 3×3×C
- 영역 내 최대값만 남김
- 값 선택이므로 **정보 손실 발생**
- 대신 **특징 존재 여부를 강조**
- 작은 위치 변화에 둔감해짐

**장점**
- 계산량 감소
- 노이즈 억제
- 특징 존재 여부 중심의 표현 강화