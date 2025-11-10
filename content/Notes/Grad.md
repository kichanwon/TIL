---
draft: true
---

## 🧠 ③ `grad` 속성의 의미·필요성·동작 원리

|항목|내용|
|---|---|
|**개요**|`tensor.grad`는 해당 텐서가 **loss에 대해 가지는 기울기(∂L/∂x)** 를 저장하는 속성|
|**필요성**|역전파(backpropagation) 과정에서 각 파라미터별 gradient 누적 → Optimizer가 이를 사용해 가중치 업데이트 수행|
|**동작 원리**||

1. **순전파:** 연산 그래프(DAG)에 연산 및 종속 관계 기록 (`.grad_fn`)
    
2. **역전파:** `.backward()` 호출 시, Chain Rule에 따라 기울기 계산
    
3. **저장:** leaf tensor(학습 가능한 파라미터)에 `.grad` 값 저장
    
4. **업데이트:** Optimizer가 `param -= lr * param.grad` 수행
    
5. **초기화:** 다음 step 전 `optimizer.zero_grad()`로 gradient 초기화 |  
    | **결과** | 자동미분 시스템이 각 연산의 gradient를 정확히 추적하여 학습 가능 |  
    | **유의점** | `.grad`는 **leaf tensor만 보유**, 중간 텐서는 `.grad_fn`만 존재 |
    

**요약 표**

|구분|의미|역할|주의사항|
|---|---|---|---|
|`.grad`|loss에 대한 기울기 저장|Optimizer가 가중치 업데이트에 사용|backward 후 초기화 필요|
|`.grad_fn`|연산 이력(함수 노드)|DAG 추적용|leaf tensor에는 없음|
