---
draft:
title: Top-N Acc/Error
---
# Top-N Accuracy / Top-N Error 정리

## Top-N Accuracy
정의:
모델이 예측한 상위 N개의 클래스 안에 정답이 포함되어 있으면 정답으로 계산하는 정확도.

수식  
$$\text{Top-N Accuracy} = \frac{\text{정답이 Top-N 안에 포함된 샘플 수}}{\text{전체 샘플 수}}$$

설명  
- Top-1 accuracy: 가장 높은 확률로 예측한 1개가 정답인지 확인  
- Top-5 accuracy: 예측한 상위 5개 후보 안에 정답이 포함되면 정답 처리  

필요 이유  
클래스 수가 매우 많을 때(예: ImageNet 1000-class) 단일 후보만 보는 것은 너무 엄격하므로, 상위 후보군에 정답이 있는지를 보는 것이 모델의 표현력을 더 잘 반영한다.

---

## Top-N Error

정의  
Top-N accuracy의 반대 개념.  
상위 N개 예측에 정답이 없을 때 오류로 계산한다.

수식  
$$\text{Top-N Error} = 1 - \text{Top-N Accuracy}$$

설명  
- Top-1 error: 모델의 1등 예측이 틀린 비율  
- Top-5 error: 상위 5개 예측 안에 정답이 없었던 비율  

---

## 예시

| 샘플 | 모델 예측 상위 5개 | 정답 | 판단 |
|------|----------------------|--------|--------|
| A | cat, dog, fox, tiger, rabbit | dog | Top-1: 틀림 / Top-5: 맞음 |
| B | airplane, car, ship, bus, bike | bird | Top-1: 틀림 / Top-5: 틀림 |
| C | frog, lizard, snake, turtle, fish | frog | Top-1: 맞음 / Top-5: 맞음 |

계산  
- Top-1 accuracy = 1/3  
- Top-1 error = 2/3  
- Top-5 accuracy = 2/3  
- Top-5 error = 1/3  

---

## 요약

- Top-N accuracy  
  모델이 예측한 상위 N개 안에 정답이 있으면 맞음

- Top-N error  
  상위 N개 안에 정답이 없으면 틀림  
  즉 $1 - \text{Top-N Accuracy}$

- ImageNet 평가에서는 관례적으로 Top-1 error와 Top-5 error를 사용함.
