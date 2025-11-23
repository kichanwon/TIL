---
tags:
  - paper_review
draft: false
title:
---
- [ON LARGE-BATCH TRAINING FOR DEEP LEARNING : GENERALIZATION GAP AND SHARP MINIMA (2017)](https://arxiv.org/pdf/1609.04836)

### 기본 정보
- 논문 제목
	ON LARGE-BATCH TRAINING FOR DEEP LEARNING : GENERALIZATION GAP AND SHARP MINIMA
- 저자
	Nitish Shirish Keskar, Dheevatsa Mudigere, Jorge Nocedal, Mikhail Smelyanskiy, Ping Tak Peter Tang
- 발표연도
	Published as a conference paper at ICLR 2017
- 주요 키워드
	Large Batch, Generalization Gap, Sharp Minima, Deep Learning Optimization

---
### ABSTRACT
The stochastic gradient descent (SGD) method and its variants are algorithms of choice for many Deep Learning tasks. These methods operate in a small-batch regime wherein a fraction of the training data, say 32–512 data points, is sampled to compute an approximation to the gradient. It has been observed in practice that when using a larger batch there is a degradation in the quality of the model, as measured by its ability to generalize. We investigate the cause for this generalization drop in the large-batch regime and present numerical evidence that supports the view that large-batch methods tend to converge to sharp minimizers of the training and testing functions—and as is well known, sharp minima lead to poorer generalization. In contrast, small-batch methods consistently converge to flat minimizers, and our experiments support a commonly held view that this is due to the inherent noise in the gradient estimation. We discuss several strategies to attempt to help large-batch methods eliminate this generalization gap.
확률적 경사 하강법(SGD)과 그 변형들은 많은 딥 러닝 작업에서 선호되는 알고리즘이다.
이 방법들은 소규모 배치 환경에서 작동하며, 훈련 데이터의 일부(예: 32~512개 데이터 포인트)를
추출하여 경사의 근사값을 계산한다. 실제 적용에서 관찰된 바에 따르면,
더 큰 배치 크기를 사용할 경우 모델의 일반화 능력으로 측정된
모델 품질이 저하되는 현상이 발생합니다. 본 연구는 대규모 배치 환경에서 발생하는 이러한 일반화 능력 저하의 원인을 규명하고,
대규모 배치 방법이 훈련 및 테스트 함수의 날카로운 최소값으로 수렴하는 경향이 있다는 관점을 뒷받침하는
수치적 증거를 제시합니다. 잘 알려진 바와 같이, 날카로운 최소값은 더 낮은 일반화 능력을 초래합니다. 반면 소배치 방법은 일관되게 평탄한 최소값으로 수렴하며, 우리의 실험은 이 현상이 기울기 추정의 내재적 잡음에 기인한다는 통설을 뒷받침합니다. 본 논문에서는 대규모 배치 방법이 이러한 일반화 격차를 해소할 수 있도록 돕기 위한 여러 전략을 논의합니다.

---
### 1 INTRODUCTION
Deep Learning has emerged as one of the cornerstones of large-scale machine learning. Deep Learning models are used for achieving state-of-the-art results on a wide variety of tasks including computer vision, natural language processing and reinforcement learning; see (Bengio et al., 2016) and the references therein. The problem of training these networks is one of non-convex optimization. Mathematically, this can be represented as:
딥 러닝은 대규모 기계 학습의 핵심 기술 중 하나로 부상했습니다. 딥 러닝 모델은 컴퓨터 비전, 자연어 처리, 강화 학습을 포함한 다양한 분야에서 최첨단 성과를 달성하는 데 활용됩니다; (Bengio et al., 2016) 및
해당 논문의 참고 문헌을 참조하십시오. 이러한 네트워크를 훈련하는 문제는 비볼록 최적화 문제에 해당합니다.
수학적으로 이는 다음과 같이 표현될 수 있습니다:

$$
\min_{x \in \mathbb{R}^n} f(x) := \frac{1}{M} \sum_{i=1}^{M} f_i(x),
$$
(1)
where fi is a loss function for data point i ∈ {1, 2, · · · , M} which captures the deviation of the model prediction from the data, and x is the vector of weights being optimized. The process of optimizing this function is also called training of the network. Stochastic Gradient Descent (SGD) (Bottou, 1998; Sutskever et al., 2013) and its variants are often used for training deep networks.
여기서 fi
는 데이터 포인트 i ∈ {1, 2, · · · , M}에 대한 손실 함수로, 모델 예측과 데이터 간의 편차를 포착하며, x는 최적화되는 가중치 벡터이다. 이 함수를 최적화하는 과정은 네트워크 훈련이라고도 한다. 확률적 경사 하강법(SGD)
(Bottou, 1998; Sutskever et al., 2013) 및 그 변형들은 딥 네트워크 훈련에 자주 사용된다.

These methods minimize the objective function f by iteratively taking steps of the form:
이러한 방법들은 다음과 같은 형태의 단계를 반복적으로 수행함으로써 목적 함수 f를 최소화합니다:
$$
x_{k+1} = x_k - \alpha_k 
\left(
\frac{1}{|B_k|} \sum_{i \in B_k} \nabla f_i(x_k)
\right)
$$
where Bk ⊂ {1, 2, · · · , M} is the batch sampled from the data set and αk is the step size at iteration k. These methods can be interpreted as gradient descent using noisy gradients, which and are often referred to as mini-batch gradients with batch size |Bk|. SGD and its variants are employed in a small-batch regime, where |Bk|  M and typically |Bk| ∈ {32, 64, · · · , 512}. These configurations have been successfully used in practice for a large number of applications; see e.g. (Simonyan & Zisserman, 2014; Graves et al., 2013; Mnih et al., 2013). Many theoretical properties of these methods are known. These include guarantees of: (a) convergence to minimizers of strongly-convex functions and to stationary points for non-convex functions (Bottou et al., 2016), (b) saddle-point avoidance (Ge et al., 2015; Lee et al., 2016), and (c) robustness to input data (Hardt et al., 2015).
여기서 Bk ⊂ {1, 2, · · · , M}은 데이터 세트에서 샘플링된 배치이며, αk는 반복 단계에서의 단계 크기이다.
k에서 사용되는 단계 크기이다. 이러한 방법들은 잡음이 있는 기울기를 사용하는 경사 하강법으로 해석될 수 있으며, 이는 종종 배치 크기 |Bk|를 가진 미니배치 기울기로 불린다. SGD와 그 변형들은 |Bk|가 작게 설정된 소규모 배치 환경에서 사용되며, 일반적으로 |Bk| ∈ {32, 64, · · · , 512}이다. 이러한 구성은 다수의 응용 분야에서 성공적으로 활용되어 왔으며, 예를 들어 (Simonyan & Zisserman, 2014; Graves et al., 2013; Mnih et al., 2013)을 참조하십시오. 이 방법들의 많은 이론적 특성이 알려져 있습니다. 여기에는 다음에 대한 보장이 포함됩니다: (a) 강함수형 함수의 최소값에 대한 수렴성
함수의 최소값 및 비볼록 함수의 정점(Bottou et al., 2016), (b) 안장점 회피(Ge et al., 2015; Lee et al., 2016), (c) 입력 데이터에 대한 강건성(Hardt et al., 2015) 등이 보장됩니다.

Stochastic gradient methods have, however, a major drawback: owing to the sequential nature of the iteration and small batch sizes, there is limited avenue for parallelization. While some efforts have been made to parallelize SGD for Deep Learning (Dean et al., 2012; Das et al., 2016; Zhang et al., 2015), the speed-ups and scalability obtained are often limited by the small batch sizes. One natural avenue for improving parallelism is to increase the batch size |Bk|. This increases the amount of computation per iteration, which can be effectively distributed. However, practitioners have observed that this leads to a loss in generalization performance; see e.g. (LeCun et al., 2012). In other words, the performance of the model on testing data sets is often worse when trained with largebatch methods as compared to small-batch methods. In our experiments, we have found the drop in generalization (also called generalization gap) to be as high as 5% even for smaller networks.
그러나 확률적 경사법에는 주요 단점이 존재한다: 반복 과정의 순차적 특성 및 작은 배치 크기 때문에 병렬화 가능성이 제한된다. 딥러닝을 위한 SGD 병렬화에 대한 일부 연구(Dean et al., 2012; Das et al., 2016; Zhang et al.,
2015), 얻어진 속도 향상과 확장성은 종종 작은 배치 크기에 의해 제한된다. 병렬성을 개선하기 위한 자연스러운 한 가지 방법은 배치 크기 |Bk|를 늘리는 것이다. 이는 반복당 계산량을 증가시켜 효과적으로 분산될 수 있다. 그러나 실무자들은 이것이 일반화 성능의 저하로 이어진다는 점을 관찰해왔다; 예를 들어 (LeCun et al., 2012)를 참조하라. 즉,
대규모 배치 방법으로 훈련된 모델은 소규모 배치 방법에 비해 테스트 데이터 세트에서의 성능이 종종 더 나쁩니다. 우리의 실험에서,
일반화 성능 저하(일반화 격차라고도 함)는 소규모 네트워크에서도 최대 5%에 달하는 것으로 확인되었습니다.

In this paper, we present numerical results that shed light into this drawback of large-batch methods. We observe that the generalization gap is correlated with a marked sharpness of the minimizers obtained by large-batch methods. This motivates efforts at remedying the generalization problem, as a training algorithm that employs large batches without sacrificing generalization performance would have the ability to scale to a much larger number of nodes than is possible today. This could potentially reduce the training time by orders-of-magnitude; we present an idealized performance model in the Appendix C to support this claim.
본 논문에서는 대규모 배치 학습법의 이러한 단점을 조명하는 수치적 결과를 제시한다.
우리는 일반화 격차가 대규모 배치 학습법으로 얻어진 최소화점의 현저한 날카로움과 상관관계가 있음을 관찰한다.
이는 일반화 문제 해결을 위한 노력을 촉진하는데,
일반화 성능을 희생하지 않으면서 대규모 배치를 활용하는 학습 알고리즘은
현재 가능한 것보다 훨씬 더 많은 노드 수로 확장할 수 있는 능력을 갖출 수 있기 때문이다. 이는 훈련 시간을 수십 배 단위로 단축할 잠재력을 지닙니다. 부록 C에서 이 주장을 뒷받침하는 이상화된 성능 모델을 제시합니다.

The paper is organized as follows. In the remainder of this section, we define the notation used in this paper, and in Section 2 we present our main findings and their supporting numerical evidence. In Section 3 we explore the performance of small-batch methods, and in Section 4 we briefly discuss the relationship between our results and recent theoretical work. We conclude with open questions concerning the generalization gap, sharp minima, and possible modifications to make large-batch training viable. In Appendix E, we present some attempts to overcome the problems of large-batch training.
본 논문은 다음과 같이 구성된다. 본 절의 나머지 부분에서는 본 논문에서 사용되는 표기법을 정의하고, 제2절에서는 주요 연구 결과와 이를 뒷받침하는 수치적 증거를 제시한다.
제3절에서는 소규모 배치 학습법의 성능을 탐구하며, 제4절에서는 본 연구 결과와 최근 이론적 연구 간의 관계를 간략히 논의한다. 마지막으로 일반화 격차, 최솟값의 날카로움, 대규모 배치 학습의 실현 가능성을 위한 수정 방안과 관련된 개방형 질문으로 결론을 맺는다. 부록 E에서는 대규모 배치 학습의 문제점을 극복하기 위한 몇 가지 시도를 제시한다.

---
#### 1.1 NOTATION
We use the notation fi to denote the composition of loss function and a prediction function corresponding to the i th data point. The vector of weights is denoted by x and is subscripted by k to denote an iteration. We use the term small-batch (SB) method to denote SGD, or one of its variants like ADAM (Kingma & Ba, 2015) and ADAGRAD (Duchi et al., 2011), with the proviso that the gradient approximation is based on a small mini-batch. In our setup, the batch Bk is randomly sampled and its size is kept fixed for every iteration. We use the term large-batch (LB) method to denote any training algorithm that uses a large mini-batch. In our experiments, ADAM is used to explore the behavior of both a small or a large batch method.
우리는 i번째 데이터 포인트에 대응하는 손실 함수와 예측 함수의 합성을 fi로 표기한다. 가중치 벡터는 x로 표기하며, 반복을 나타내기 위해 k로 첨자를 붙인다. 소배치(SB) 방법이란 SGD 또는 그 변형인 ADAM(Kingma & Ba, 2015) 및 ADAGRAD(Duchi et al., 2011)를 지칭하며,
단, 기울기 근사치가 소규모 미니배치를 기반으로 한다는 조건을 붙입니다. 본 설정에서는 배치 Bk를 무작위로 추출하며, 그 크기는 각 반복마다 고정됩니다. 대규모 미니배치를 사용하는 모든 학습 알고리즘을 대규모 배치(LB) 방법이라 칭합니다. 본 실험에서는 ADAM을 활용하여 소규모 배치 및 대규모 배치 방법의 양측 특성을 탐구합니다.

---
### 2 DRAWBACKS OF LARGE-BATCH METHODS
#### 2.1 OUR MAIN OBSERVATION
As mentioned in Section 1, practitioners have observed a generalization gap when using large-batch methods for training deep learning models. Interestingly, this is despite the fact that large-batch methods usually yield a similar value of the training function as small-batch methods. One may put forth the following as possible causes for this phenomenon: (i) LB methods over-fit the model; (ii) LB methods are attracted to saddle points; (iii) LB methods lack the explorative properties of SB methods and tend to zoom-in on the minimizer closest to the initial point; (iv) SB and LB methods converge to qualitatively different minimizers with differing generalization properties. The data presented in this paper supports the last two conjectures.
제1절에서 언급한 바와 같이, 실무자들은 딥러닝 모델 훈련에 대량 배치 방법을 사용할 때 일반화 격차를 관찰해왔다. 흥미롭게도 이는 대량 배치 방법이 일반적으로 소량 배치 방법과 유사한 훈련 함수 값을 산출한다는 사실에도 불구하고 발생하는 현상이다. 이 현상의 가능한 원인으로는 다음과 같은 것들을 제시할 수 있다: (i) LB 방법이 모델을 과적합시킨다; (ii) LB 방법이 안장점에 끌린다; (iii) LB 방법이 SB 방법의 탐색적 특성을 결여하여 초기점에 가장 가까운 최소화점에 집중하는 경향이 있음; (iv) SB와 LB 방법이 서로 다른 일반화 특성을 지닌 질적으로 다른 최소화점으로 수렴함. 본 논문에 제시된 데이터는 마지막 두 가설을 뒷받침한다.

The main observation of this paper is as follows:
본 논문의 주요 관찰 결과는 다음과 같다:
>The lack of generalization ability is due to the fact that large-batch methods tend to converge to sharp minimizers of the training function. These minimizers are characterized by a significant number of large positive eigenvalues in ∇2f(x), and tend to generalize less well. In contrast, small-batch methods converge to flat minimizers characterized by having numerous small eigenvalues of ∇2f(x). We have observed that the loss function landscape of deep neural networks is such that large-batch methods are attracted to regions with sharp minimizers and that, unlike small-batch methods, are unable to escape basins of attraction of these minimizers.
>일반화 능력 부족은 대량 배치 방법이 훈련 함수의 급격한 최소화점에 수렴하는 경향이 있기 때문이다. 이러한 최소화점은 ∇²f(x)에 상당한 수의 큰 양의 고유값이 존재하는 특징을 가지며, 일반적으로 덜 잘 일반화한다. 반면 소량 배치 방법은 ∇²f(x)에 수많은 작은 고유값을 가지는 평탄한 최소화점으로 수렴한다. 우리는 딥 뉴럴 네트워크의 손실 함수 지형이 대량 배치 방법이 날카로운 최소화점이 있는 영역으로 끌려가며, 소량 배치 방법과 달리 이러한 최소화점의 끌어당김 영역에서 벗어날 수 없다는 점을 관찰했습니다.

The concept of sharp and flat minimizers have been discussed in the statistics and machine learning literature. (Hochreiter & Schmidhuber, 1997) (informally) define a flat minimizer x¯ as one for which the function varies slowly in a relatively large neighborhood of x¯. In contrast, a sharp minimizer xˆ is such that the function increases rapidly in a small neighborhood of xˆ. A flat minimum can be described with low precision, whereas a sharp minimum requires high precision. The large sensitivity of the training function at a sharp minimizer negatively impacts the ability of the trained model to generalize on new data; see Figure 1 for a hypothetical illustration. This can be explained through the lens of the minimum description length (MDL) theory, which states that statistical models that require fewer bits to describe (i.e., are of low complexity) generalize better (Rissanen, 1983). Since flat minimizers can be specified with lower precision than to sharp minimizers, they tend to have better generalization performance. Alternative explanations are proffered through the Bayesian view of learning (MacKay, 1992), and through the lens of free Gibbs energy; see e.g. Chaudhari et al. (2016).
통계학 및 기계 학습 문헌에서는 날카로운 최소화점과 평탄한 최소화점 개념이 논의되어 왔다. (Hochreiter & Schmidhuber, 1997)는 평탄한 최소점 x¯을 (비공식적으로) 함수가 x¯의 비교적 넓은 근방에서 천천히 변화하는 점으로 정의한다. 반면, 날카로운 최소점 xˆ은 함수가 xˆ의 작은 근방에서 급격히 증가하는 점이다. 평탄한 최소점은 낮은 정밀도로 설명될 수 있는 반면, 날카로운 최소점은 높은 정밀도를 요구한다. 예측 함수가 급격한 최소화점에서 보이는 높은 민감도는
훈련된 모델이 새로운 데이터에 대해 일반화하는 능력에 부정적인 영향을 미칩니다;
가상적인 예시는 그림 1을 참조하십시오. 이는 최소 기술 길이(MDL) 이론의 관점에서 설명될 수 있는데,
이 이론은 기술하는 데 더 적은 비트(즉, 낮은 복잡도)를 요구하는 통계적 모델이
더 잘 일반화된다고 주장합니다(Rissanen, 1983). 평탄한 최소화점은
날카로운 최소화점보다 낮은 정밀도로 지정될 수 있으므로, 일반적으로 더 우수한 일반화 성능을 보입니다.
베이즈 학습 관점(MacKay, 1992)과 자유 깁스 에너지 관점에서도 대체 설명이 제시됩니다.
예를 들어 Chaudhari 등(2016)을 참조하십시오.


Figure 1: A Conceptual Sketch of Flat and Sharp Minima. The Y-axis indicates value of the loss function and the X-axis the variables (parameters)
그림 1: 평탄한 최소점과 날카로운 최소점의 개념적 스케치. Y축은 손실 함수의 값을 나타내고, X축은 변수(매개변수)를 나타낸다.

---
#### 2.2 NUMERICAL EXPERIMENTS
In this section, we present numerical results to support the observations made above. To this end, we make use of the visualization technique employed by (Goodfellow et al., 2014b) and a proposed heuristic metric of sharpness (Equation (4)). We consider 6 multi-class classification network configurations for our experiments; they are described in Table 1. The details about the data sets and network configurations are presented in Appendices A and B respectively. As is common for such problems, we use the mean cross entropy loss as the objective function f.
본 절에서는 위에서 제시한 관찰 결과를 뒷받침하는 수치적 결과를 제시한다. 이를 위해
(Goodfellow et al., 2014b)에서 사용한 시각화 기법과 제안된 선명도 경험적 지표(식 (4))를 활용한다. 실험을 위해 6가지 다중 클래스 분류 네트워크 구성을 고려하며, 이는 표 1에 설명되어 있다. 데이터 세트와 네트워크 구성에 대한 세부 사항은 각각 부록 A와 B에 제시된다. 이러한 문제에서 흔히 그렇듯이, 우리는 평균 교차 엔트로피 손실을 목적 함수 f로 사용한다.

The networks were chosen to exemplify popular configurations used in practice like AlexNet (Krizhevsky et al., 2012) and VGGNet (Simonyan & Zisserman, 2014). Results on other networks
실무에서 널리 사용되는 구성의 예시로 AlexNet(Krizhevsky et al., 2012) 및 VGGNet(Simonyan & Zisserman, 2014)과 같은 네트워크가 선정되었습니다. 다른 네트워크에 대한 결과는

Table 1: Network Configurations

| **Name** | **Network Type**        | **Architecture** | **Data set**                          |
| :------- | :---------------------- | :--------------- | :------------------------------------ |
| F1       | Fully Connected         | Section B.1      | MNIST (LeCun et al., 1998a)           |
| F2       | Fully Connected         | Section B.2      | TIMIT (Garofolo et al., 1993)         |
| C1       | (Shallow) Convolutional | Section B.3      | CIFAR-10 (Krizhevsky & Hinton, 2009)  |
| C2       | (Deep) Convolutional    | Section B.4      | CIFAR-10                              |
| C3       | (Shallow) Convolutional | Section B.3      | CIFAR-100 (Krizhevsky & Hinton, 2009) |
| C4       |  (Deep) Convolutional   | Section B.4      | CIFAR-100                             |
and using other initialization strategies, activation functions, and data sets showed similar behavior. Since the goal of our work is not to achieve state-of-the-art accuracy or time-to-solution on these tasks but rather to characterize the nature of the minima for LB and SB methods, we only describe the final testing accuracy in the main paper and ignore convergence trends.
다른 초기화 전략, 활성화 함수 및 데이터 세트를 사용해도 유사한 행동 양상을 보였습니다.
본 연구의 목표는 해당 작업에서 최첨단 정확도나 해결 시간 달성보다는 LB 및 SB 방법의 최소값 특성을 규명하는 것이므로,
본 논문에서는 최종 테스트 정확도만 기술하고 수렴 경향은 생략합니다.

For all experiments, we used 10% of the training data as batch size for the large-batch experiments and 256 data points for small-batch experiments. We used the ADAM optimizer for both regimes. Experiments with other optimizers for the large-batch experiments, including ADAGRAD (Duchi et al., 2011), SGD (Sutskever et al., 2013) and adaQN (Keskar & Berahas, 2016), led to similar results. All experiments were conducted 5 times from different (uniformly distributed random) starting points and we report both mean and standard-deviation of measured quantities. The baseline performance for our setup is presented Table 2. From this, we can observe that on all networks, both approaches led to high training accuracy but there is a significant difference in the generalization performance. The networks were trained, without any budget or limits, until the loss function ceased to improve.
모든 실험에서 대규모 배치 실험에는 훈련 데이터의 10%를 배치 크기로 사용했으며,
소규모 배치 실험에는 256개의 데이터 포인트를 사용했습니다. 두 경우 모두 ADAM 최적화기를 적용했습니다.
대규모 배치 실험에서 ADAGRAD(Duchi et al., 2011), SGD(Sutskever et al., 2013), adaQN(Keskar & Berahas, 2016) 등 다른 최적화기를 사용한 실험도 유사한 결과를 보였습니다. 모든 실험은 서로 다른 (균일 분포된 랜덤)
시작점에서 5회 반복 수행되었으며, 측정된 양의 평균과 표준편차를 모두 보고합니다. 본 설정의 기준 성능은
표 2에 제시되어 있습니다. 이를 통해 모든 네트워크에서 두 접근법 모두 높은 훈련 정확도를 보였으나
일반화 성능에는 상당한 차이가 있음을 관찰할 수 있습니다. 네트워크는 손실 함수의 개선이 멈출 때까지
예산이나 제한 없이 훈련되었습니다.

Table 2: Performance of small-batch (SB) and large-batch (LB) variants of ADAM on the 6 networks listed in Table 1
표 2: 표 1에 나열된 6개 네트워크에서 ADAM의 소규모 배치(SB) 및 대규모 배치(LB) 변형의 성능

|          | Training Accuracy | -              | Testing Accuracy | -              |
| :------- | :---------------- | :------------- | :--------------- | :------------- |
| **Name** | SB                | LB             | SB               | LB             |
| F1       | 99.66% ± 0.05%    | 99.92% ± 0.01% | 98.03% ± 0.07%   | 97.81% ± 0.07% |
| F2       | 99.99% ± 0.03%    | 98.35% ± 2.08% | 64.02% ± 0.20%   | 59.45% ± 1.05% |
| C1       | 99.89% ± 0.02%    | 99.66% ± 0.20% | 80.04% ± 0.12%   | 77.26% ± 0.42% |
| C2       | 99.99% ± 0.04%    | 99.99% ± 0.01% | 89.24% ± 0.12%   | 87.26% ± 0.07% |
| C3       | 99.56% ± 0.44%    | 99.88% ± 0.30% | 49.58% ± 0.39%   | 46.45% ± 0.43% |
| C4       | 99.10% ± 1.23%    | 99.57% ± 1.84% | 63.08% ± 0.50%   | 57.81% ± 0.17% |
We emphasize that the generalization gap is not due to over-fitting or over-training as commonly observed in statistics. This phenomenon manifest themselves in the form of a testing accuracy curve that, at a certain iterate peaks, and then decays due to the model learning idiosyncrasies of the training data. This is not what we observe in our experiments; see Figure 2 for the training–testing curve of the F2 and C1 networks, which are representative of the rest. As such, early-stopping heuristics aimed at preventing models from over-fitting would not help reduce the generalization gap. The difference between the training and testing accuracies for the networks is due to the specific choice of the network (e.g. AlexNet, VGGNet etc.) and is not the focus of this study. Rather, our goal is to study the source of the testing performance disparity of the two regimes, SB and LB, on a given network model.
우리는 일반화 격차가 통계학에서 흔히 관찰되는 과적합이나 과훈련 때문이 아님을 강조한다.
이러한 현상은 특정 반복 횟수에서 정점을 찍은 후 모델이 훈련 데이터의 특이점을 학습함에 따라
감소하는 테스트 정확도 곡선의 형태로 나타난다. 이는 본 실험에서 관찰된 바와 다릅니다. 나머지 네트워크를 대표하는 F2 및 C1 네트워크의 훈련-테스트 곡선은 그림 2를 참조하십시오. 따라서 모델의 과적합을 방지하기 위한 조기 종료(early-stopping) 기법은 일반화 격차 감소에 도움이 되지 않습니다. 네트워크의 훈련 정확도와 테스트 정확도 차이는
특정 네트워크 선택(예: AlexNet, VGGNet 등)에 기인하며 본 연구의 초점이 아닙니다.
오히려 우리의 목표는 주어진 네트워크 모델에서
SB와 LB 두 체제의 테스트 성능 차이가 발생하는 원인을 연구하는 것입니다.

---
##### 2.2.1 PARAMETRIC PLOTS

We first present parametric 1-D plots of the function as described in (Goodfellow et al., 2014b). Let $x_s^*$ and $x_l^*$ indicate the solutions obtained by running ADAM using small and large batch sizes respectively. We plot the loss function, on both training and testing data sets, along a line-segment containing the two points. Specifically, for α ∈ \[−1, 2], we plot the function $f(αx_l^* + (1 − α)x_s^* )$ and also superimpose the classification accuracy at the intermediate points; see Figure $3^1$ . For this

experiment, we randomly chose a pair of SB and LB minimizers from the 5 trials used to generate the data in Table 2. The plots show that the LB minima are strikingly sharper than the SB minima in this one-dimensional manifold. The plots in Figure 3 only explore a linear slice of the function, but in Figure 7 in Appendix D, we plot $f(sin( απ / 2 )x_l^* + cos( απ 2 )x_s^* )$ to monitor the function along a curved path between the two minimizers . There too, the relative sharpness of the minima is evident.

##### 2.2.2 SHARPNESS OF MINIMA
### 3 SUCCESS OF SMALL-BATCH METHODS
### 4 DISCUSSION AND CONCLUSION
