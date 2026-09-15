Train/Validation/Test 분할로 확인하는 것은 엄밀히 말해

> "현재 확보한 데이터가 미래 실제 환경과 같은 분포에서 생성된다는 가정 아래, 모델이 보지 않은 샘플에도 잘 동작할 가능성"

을 추정하는 것이다.

## 1. 우리가 실제로 평가하는 것은 무엇인가

우리가 원하는 것은 실제 환경에서의 기대 위험이다.

Rreal(f)=E(x,y)∼Preal[L(f(x),y)]R_{\text{real}}(f) = \mathbb{E}_{(x,y)\sim P_{\text{real}}} [L(f(x),y)]

그런데 PrealP_{\text{real}} 전체를 알 수 없으므로 실제 계산이 불가능하다.

대신 가지고 있는 Test set으로

R^test(f)=1n∑i=1nL(f(xi),yi)\hat R_{\text{test}}(f) = \frac{1}{n} \sum_{i=1}^{n} L(f(x_i),y_i)

를 계산한다.

문제는 이 둘이 같으려면 사실상

Ptest(X,Y)≈Preal(X,Y)P_{\text{test}}(X,Y) \approx P_{\text{real}}(X,Y)

이라는 가정이 필요하다는 것이다.

즉,

```
Test 성능이 좋음
       ↓
Test와 실제 환경의 분포가 비슷하다는 가정
       ↓
실제 환경에서도 잘 동작할 것이라고 추론
```

이지,

```
Test 성능이 좋음
       ↓
실제 환경 성능이 보장됨
```

이 아니다.

---

# 2. Train / Validation / Test가 해결하는 문제

세 데이터를 나누는 가장 중요한 이유는 **동일한 데이터에 대한 암기와 성능 평가를 분리하기 위해서**다.

### Train

DtrainD_{train}

모델의 parameter를 결정한다.

θ∗=arg⁡min⁡θR^train(θ)\theta^* = \arg\min_\theta \hat R_{train}(\theta)

---

### Validation

DvalD_{val}

hyperparameter, architecture, epoch 등을 결정한다.

예:

λ,learning rate,number of layers\lambda,\quad \text{learning rate},\quad \text{number of layers}

Validation을 반복해서 보면 사실상 validation 데이터에도 적응하게 된다.

---

### Test

마지막에 한 번 사용해서

R^test\hat R_{test}

를 계산한다.

따라서 Test set의 목적은

> **학습 과정에서 보지 않은 데이터에 대한 일반화 성능 추정**

이다.

하지만 여기에는 중요한 전제가 있다.

Dtrain,Dval,DtestPD_{train}, D_{val}, D_{test} \overset{i.i.d.}{\sim} P

그리고 미래 데이터도

Dreal∼PD_{real}\sim P

라고 가정하는 것이다.

---

# 3. 결국 핵심은 i.i.d. 가정

전통적인 Statistical Learning Theory에서 매우 중요한 가정이다.

(xi,yi)P(X,Y)(x_i,y_i) \overset{i.i.d.}{\sim} P(X,Y)

즉,

1. 데이터가 독립적으로 추출되고
2. 동일한 분포에서 추출된다고 본다.

이 경우 Test set은 실제 분포 PP에서 뽑은 작은 표본이므로

R^test(f)≈R(f)\hat R_{test}(f) \approx R(f)

라고 볼 수 있다.

샘플 수가 많아질수록 Law of Large Numbers에 의해

R^test(f)→R(f)\hat R_{test}(f) \rightarrow R(f)

로 수렴한다.

그래서 Test set을 사용하는 것이다.

---

# 4. 그런데 Real-world에서는 이 가정이 자주 깨진다

여기서 실제 ML의 중요한 문제가 나온다.

예를 들어 새를 탐지하는 모델을 만든다고 해보자.

훈련 데이터가

```
지역      충주
계절      여름
시간      낮
카메라    Sony Camera A
날씨      맑음
조류      까치, 참새 중심
```

이라면 Train/Test를 랜덤하게 나누더라도

```
Train
충주 / 여름 / 낮 / Camera A

Test
충주 / 여름 / 낮 / Camera A
```

가 된다.

Test accuracy가 95%라고 하자.

그런데 실제 배포 환경이

```
제주
겨울
새벽
Camera B
비 또는 안개
새로운 조류
```

라면

Ptest(X,Y)≠Preal(X,Y)P_{test}(X,Y) \neq P_{real}(X,Y)

이다.

이 경우 Test 95%라는 숫자는 실제 환경에 대한 보장이 거의 되지 않는다.

---

# 5. 이것을 Distribution Shift라고 한다

실제 deployment에서 중요한 문제다.

대표적으로 세 종류로 생각할 수 있다.

### Covariate Shift

입력 분포가 변한다.

Ptrain(X)≠Preal(X)P_{train}(X) \neq P_{real}(X)

예:

```
맑은 날 → 비 오는 날
낮 → 밤
Camera A → Camera B
```

---

### Label Shift

클래스 빈도가 변한다.

Ptrain(Y)≠Preal(Y)P_{train}(Y) \neq P_{real}(Y)

예:

훈련 데이터

```
참새 60%
까치 30%
까마귀 10%
```

실제 환경

```
참새 10%
까치 20%
까마귀 70%
```

---

### Concept Shift

더 심각한 경우다.

Ptrain(Y∣X)≠Preal(Y∣X)P_{train}(Y|X) \neq P_{real}(Y|X)

즉 같은 입력 패턴의 의미 자체가 변한다.

---

# 6. 그래서 단순 Random Split만으로는 부족하다

예를 들어 데이터가

```
2024년
2025년
2026년
```

에 걸쳐 있다면 랜덤하게

```
80% train
10% validation
10% test
```

하는 것보다

```
Train       2024
Validation  2025
Test        2026
```

이 실제 deployment를 더 잘 모사할 수 있다.

즉 **Test set을 어떻게 만드는가가 모델 평가에서 매우 중요하다.**

실제 환경이 무엇인지 먼저 정의해야 한다.

PtargetP_{target}

그리고 Test set이 가능한 한

Dtest∼PtargetD_{test} \sim P_{target}

이 되도록 설계해야 한다.

---

# 7. "일반화 성능"에는 사실 두 가지 의미가 섞여 있다

보통 논문에서 일반화라고 하면

### In-distribution Generalization

Ptrain=PtestP_{train} = P_{test}

라고 가정한다.

즉 같은 분포에서 새로운 샘플을 잘 맞히는가.

대부분의 일반적인 Train/Test 실험이 이것이다.

---

반면 실제 우리가 원하는 것은 종종

### Out-of-distribution Generalization

Ptrain≠PrealP_{train} \neq P_{real}

에서도 잘 동작하는가이다.

이것은 훨씬 어려운 문제다.

```
훈련하지 않은 지역
훈련하지 않은 계절
새로운 센서
새로운 조명
새로운 배경
새로운 객체
```

에서 성능을 유지할 수 있는지를 보는 것이다.

---

# 8. 따라서 실제 환경 일반화를 확인하려면 Test를 여러 층으로 만들어야 한다

예를 들어 UAV 생태영상이라면 다음과 같이 평가하는 것이 훨씬 강하다.

```
Training set
충주 / Site A / 여름 / Camera A

        ↓

ID Test
충주 / Site A / 여름 / Camera A

        ↓

Temporal Test
충주 / Site A / 가을

        ↓

Spatial Test
충주 / Site B

        ↓

Sensor Test
Camera B

        ↓

External Test
다른 지역 / 다른 연구팀 데이터

        ↓

Field Deployment
실제 운영 데이터
```

여기서 아래로 갈수록 Real-world 일반화에 대한 증거가 강해진다.

---

# 9. "보장" 대신 "증거를 축적한다"고 보는 것이 정확하다

ML에서는 다음과 같은 구조다.

Training performance<Validation evidence<Test evidence<External validation<Real deployment evidence\text{Training performance} < \text{Validation evidence} < \text{Test evidence} < \text{External validation} < \text{Real deployment evidence}

어떤 하나의 Test accuracy가 일반화를 증명하는 것이 아니다.

여러 조건에서 반복적으로

Rtest(1),Rtest(2),Rtest(3),…R_{test}^{(1)}, R_{test}^{(2)}, R_{test}^{(3)}, \dots

를 측정하면서 모델의 일반화 가능성에 대한 **증거를 축적**하는 것이다.

---

# 10. Statistical Learning Theory에서는 이를 어떻게 다루는가

이론적으로는 다음 관계를 연구한다.

R(f)≤R^train(f)+Generalization GapR(f) \le \hat R_{train}(f) + \text{Generalization Gap}

즉

R(f)⏟True Risk−R^train(f)⏟Empirical Risk\underbrace{R(f)}_{\text{True Risk}} - \underbrace{\hat R_{train}(f)}_{\text{Empirical Risk}}

가 얼마나 커질 수 있는가를 분석한다.

대표적으로 모델의 hypothesis class가 복잡할수록 일반화 bound가 나빠질 수 있다.

VC theory에서는 대략

R(f)≲R^(f)+O(d+log⁡(1/δ)n)R(f) \lesssim \hat R(f) + O \left( \sqrt{ \frac{ d+\log(1/\delta) }{n} } \right)

같은 형태로 나타난다.

- nn: 데이터 수
- dd: hypothesis complexity, 예를 들어 VC dimension
- δ\delta: 확률적 신뢰 수준

즉 데이터가 많고 모델 class가 적절히 제한되어 있으면 empirical performance와 true performance의 차이가 작아질 가능성이 높다는 것이다.

다만 현대 Deep Neural Network에서는 parameter 수가 매우 크기 때문에 **고전적인 VC bound가 실제 현상을 설명하기에는 지나치게 느슨한 경우가 많다.**

그래서 현대 DL 일반화 연구에서는

- implicit regularization
- SGD bias
- margin
- flat minima
- PAC-Bayes
- algorithmic stability
- norm-based bounds

등이 계속 연구되고 있다.

---

# 11. 가장 중요한 구분

다음 세 문장은 전혀 다르다.

### ① Training performance

> 모델이 학습 데이터를 얼마나 잘 설명하는가

---

### ② Test performance

> 동일한 데이터 생성 분포에서 나온 새로운 샘플을 얼마나 잘 처리하는가

---

### ③ Real-world robustness

> 데이터 분포가 변하는 실제 환경에서도 성능이 유지되는가

보통 우리가 하는

Train/Validation/TestTrain / Validation / Test

는 주로 **②를 측정하기 위한 방법**이다.

③을 완전히 보장하는 방법은 아니다.

---

# 12. 그래서 실전에서는 평가를 이렇게 생각하는 것이 가장 정확하다

Generalization=Model 문제+Dataset 문제+Evaluation design 문제\boxed{ \text{Generalization} = \text{Model 문제} + \text{Dataset 문제} + \text{Evaluation design 문제} }

특히 많은 경우 모델 자체보다 **Test set을 어떻게 구성했는가**가 더 중요하다.

결국

Test performance≠Real-world performance guarantee\boxed{ \text{Test performance} \neq \text{Real-world performance guarantee} }

이고,

Test performance=Real-world performance의 추정치\boxed{ \text{Test performance} = \text{Real-world performance의 추정치} }

일 뿐이다. 그 추정이 타당하려면 가장 중요한 조건은

Ptest≈Pdeployment\boxed{ P_{test}\approx P_{deployment} }

이다.

이 관점을 잡으면 왜 최근 ML 연구에서 단순히 `random train/test split`뿐 아니라 **external validation, domain generalization, OOD evaluation, temporal split, geographic split, deployment monitoring**을 중요하게 보는지도 자연스럽게 연결된다.