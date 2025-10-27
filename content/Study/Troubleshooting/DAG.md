---
draft: false
---
## DAG 구성 요소
- **Leaf Tensor**: 사용자에 의해 직접 생성된 텐서 (보통 학습 파라미터)
- **Non-leaf Tensor**: 연산을 통해 새로 만들어진 텐서 (연산 기록을 가짐)
- **Root**: 역전파의 최종 출력 지점
## 그래프 특성
- **동적(dynamic) 그래프**:
    - `.backward()` 호출 후 그래프는 즉시 해제됨
    - 다음 연산 시점에서 새로운 그래프 생성
    - → 반복마다 shape, size, 연산 변경 가능



![[DAG.png]]