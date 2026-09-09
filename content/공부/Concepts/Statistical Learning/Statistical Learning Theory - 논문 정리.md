## 1. An Overview of Statistical Learning Theory

### Vladimir Vapnik, 1999

### 핵심 문제

**알 수 없는 실제 데이터 분포에서, 제한된 학습 데이터만 가지고 어떻게 실제 위험을 낮추는 함수를 학습할 것인가**

Vapnik은 학습을 미지의 확률분포에서 발생한 i.i.d. 표본을 이용하여 가장 좋은 함수를 선택하는 문제로 정의한다. 궁극적인 목표는 학습 데이터의 오차가 아니라 **expected risk를 최소화하는 것**이다.

### 논문의 흐름

단순히 Training Error를 최소화하는 **ERM(Empirical Risk Minimization)**에서 출발한다.

Remp(f)→min⁡R_{emp}(f)\rightarrow \min

그러나 ERM이 실제 위험까지 최소화하려면 경험적 위험이 실제 위험으로 적절하게 수렴해야 한다. Vapnik은 이를 **generalization 문제**로 보고, consistency와 convergence rate를 학습 이론의 핵심 문제로 제시한다.

여기서 모델의 표현 능력, 즉 **capacity**를 측정하기 위해 VC Dimension을 도입한다. VC Dimension이 유한하다는 것은 분포에 독립적인 ERM consistency와 연결된다.

그 다음 작은 데이터에서는 단순히 empirical risk만 줄이면 안 되므로

Empirical Risk+Model Capacity\text{Empirical Risk} + \text{Model Capacity}

를 함께 고려하는 **Structural Risk Minimization(SRM)**을 제시한다. 즉, **학습 오차와 모델 복잡도의 trade-off**이다.

마지막으로 이 이론을 실제 알고리즘으로 구현하는 대표적인 사례가 **SVM**이다. Maximum margin을 통해 capacity를 제어하면서 empirical risk도 낮추는 구조이다.

### 한 줄 요약

> **좋은 학습이란 Training Error를 최소화하는 것이 아니라, 데이터 양에 비해 적절한 모델 capacity를 선택하여 generalization error를 통제하는 것이다.**

---

# 2. Statistical Modeling: The Two Cultures

### Leo Breiman, 2001

이 논문은 세 논문 중 가장 **비판적이고 철학적인 성격**이 강하다.

### 핵심 문제

Breiman은 데이터 분석에 두 개의 문화가 있다고 주장한다.

### Data Modeling Culture

먼저 데이터가 특정한 확률모형에 의해 만들어졌다고 가정한다.

예:

Y=f(X,θ,ϵ)Y=f(X,\theta,\epsilon)

Linear Regression, Logistic Regression, Cox Model 등이 이에 해당한다.

모델의 타당성은 주로 goodness-of-fit, residual analysis 등을 이용해 판단한다.

### Algorithmic Modeling Culture

반대로 실제 자연의 데이터 생성 과정을 **unknown black box**로 취급한다.

우리가 알고 싶은 것은 내부 구조를 정확하게 가정하는 것이 아니라

f(X)→Yf(X)\rightarrow Y

를 잘 예측하는 알고리즘을 찾는 것이다.

Decision Tree, Neural Network 등이 대표적이며 모델 평가는 **predictive accuracy**를 중심으로 이루어진다.

---

### Breiman의 비판

당시 통계학은 거의 전적으로 Data Modeling Culture에 치우쳐 있다고 지적한다.

문제는 모델이 실제 자연을 잘못 표현한다면

> 모델에서 얻어진 결론이 자연에 대한 결론이라고 할 수 없다

는 점이다.

Breiman은 복잡한 실제 시스템에 단순한 parametric model을 강요하면 잘못된 과학적 결론으로 이어질 수 있다고 비판한다.

따라서 그는 모델을

**“설명이 얼마나 예쁜가”**

보다

**“새로운 데이터에서 얼마나 잘 예측하는가”**

로 검증해야 한다고 주장한다.

실제로 test set이나 cross-validation을 통한 predictive accuracy를 중요한 평가 기준으로 제시한다.

---

### 중요한 세 가지 논점

Breiman은 당시 머신러닝 연구로부터 통계학이 배워야 할 중요한 관점으로 다음을 제시한다.

- **Rashomon** — 좋은 성능을 내는 모델은 하나가 아닐 수 있음
- **Occam** — 단순한 모델이 반드시 정확한 것은 아님
- **Bellman** — 고차원성이 반드시 저주로만 작용하는 것은 아님

따라서 최종 주장은 **통계모델을 폐기하자**가 아니다.

문제와 데이터에 따라

Data ModelorAlgorithmic Model\text{Data Model} \quad\text{or}\quad \text{Algorithmic Model}

을 선택해야 한다는 것이다. Breiman은 마지막에 명시적으로 data model 자체에 반대하는 것이 아니라 **문제와 데이터가 방법을 결정해야 한다**고 정리한다.

### 한 줄 요약

> **통계학은 현실을 설명하기 위한 가정된 모델에만 집착하지 말고, 실제 예측 성능을 기준으로 알고리즘적 모델도 적극적으로 사용해야 한다.**

---

# 3. A Few Useful Things to Know About Machine Learning

### Pedro Domingos, 2012

앞 두 논문보다 훨씬 **실무 지향적**이다.

저자가 명시적으로 말하듯 이 논문은 ML 교과서에 잘 나오지 않는 연구자와 실무자의 **12가지 folk knowledge**를 정리한다.

### 가장 중요한 출발점

> **“It’s Generalization that Counts.”**

Machine Learning의 목적은 Training Set에서 잘하는 것이 아니라 **Training Set을 넘어 일반화하는 것**이다.

Training Data를 외우는 것은 쉽지만, 처음 보는 데이터에서 잘 동작하지 못한다면 학습에 성공한 것이 아니다. 따라서 Train/Test 분리와 Cross-validation이 중요하다.

---

### 핵심 주장들

#### 1. Learning = Representation + Evaluation + Optimization

ML 알고리즘은 사실 크게 세 요소의 조합이다.

Learning=Representation+Evaluation+OptimizationLearning = Representation + Evaluation + Optimization

- Representation: 어떤 함수를 표현할 것인가
- Evaluation: 무엇을 좋은 모델이라고 볼 것인가
- Optimization: 그 모델을 어떻게 찾을 것인가

---

#### 2. Data Alone Is Not Enough

데이터가 아무리 많아도 **가정 없이 일반화할 수 없다.**

모든 learner에는 smoothness, locality, limited complexity 같은 일종의 **inductive bias**가 필요하다.

이는 No Free Lunch 관점과 연결된다.

---

#### 3. Overfitting은 ML의 핵심 문제

Training Accuracy는 매우 높은데 Test Accuracy가 떨어지는 상황이 대표적인 overfitting이다.

이를 bias와 variance의 trade-off로 이해할 수 있으며, 강력한 모델이 항상 좋은 모델은 아니다. Regularization이나 cross-validation도 완벽한 해결책은 아니다.

---

#### 4. 고차원에서는 직관이 깨짐

Feature가 증가하면 고정된 데이터가 전체 공간에서 차지하는 비율이 급격하게 감소한다.

즉 **Curse of Dimensionality** 때문에 일반화가 어려워진다.

---

#### 5. 이론적 보장을 과신하지 말 것

이론적인 generalization bound나 asymptotic consistency는 중요하지만 실제 finite-data 상황에서 지나치게 느슨할 수 있다.

Domingos는 이론의 주요 역할을 **실제 모델 선택 기준이라기보다 학습을 이해하고 알고리즘 설계를 이끄는 도구**로 본다.

이 지점은 Vapnik 논문을 발제하는 입장에서 상당히 중요한 비판점이다.

---

#### 6. Feature Engineering이 중요함

실제 ML 프로젝트에서 성능을 결정하는 핵심 요소 중 하나는 알고리즘보다 **feature**이다.

데이터 수집, 정제, 전처리, feature 설계가 실제 작업의 상당 부분을 차지한다.

---

#### 7. 많은 데이터가 좋은 알고리즘보다 강력한 경우가 많음

논문의 유명한 표현이다.

> “A dumb algorithm with lots and lots of data beats a clever one with modest amounts of it.”

즉 알고리즘을 조금 개선하는 것보다 데이터 확보가 더 큰 성능 향상을 가져오는 경우가 많다는 주장이다.

---

#### 8. 단순한 모델이 반드시 정확한 것은 아님

Domingos는 Occam's Razor를 그대로 ML의 generalization 원리로 받아들이는 것을 경계한다.

모델의 parameter 수와 overfitting 사이에도 필연적인 관계가 없으며, 단순성 자체가 높은 predictive accuracy를 보장하지 않는다.

---

#### 9. Representable ≠ Learnable

어떤 함수가 특정 모델로 **표현 가능하다고 해서 실제로 학습 가능한 것은 아니다.**

Finite data, 계산 시간, memory, optimization 문제 때문에 실제 학습 가능한 함수의 범위는 훨씬 좁다.

### 한 줄 요약

> **ML에서 중요한 것은 복잡한 알고리즘 자체가 아니라 일반화이며, 이를 위해 데이터·feature·inductive bias·모델 복잡도·검증 방법을 함께 고려해야 한다.**

---

# 세 논문의 관계

세 편을 순서대로 놓으면 상당히 자연스럽다.

### Vapnik — 이론

어떻게 일반화를 보장할 것인가?\boxed{\text{어떻게 일반화를 보장할 것인가?}}

ERM → VC Dimension → SRM → SVM

### Breiman — 방법론적 비판

좋은 모델을 무엇으로 판단할 것인가?\boxed{\text{좋은 모델을 무엇으로 판단할 것인가?}}

모델 가정보다 실제 Prediction을 중요하게 보자.

### Domingos — 실전적 관점

실제로 일반화가 잘 되는 ML을 어떻게 만들 것인가?\boxed{\text{실제로 일반화가 잘 되는 ML을 어떻게 만들 것인가?}}

데이터, feature, bias-variance, validation, ensemble, model choice를 함께 보자.

결국 세 논문을 관통하는 공통 키워드는 **Generalization**이다. 차이는 Vapnik은 이를 **수학적으로 설명하고**, Breiman은 **통계학의 모델링 관점에서 재검토하며**, Domingos는 **실제 머신러닝 수행 과정에서 어떻게 다뤄야 하는지 정리한다**는 점이다.