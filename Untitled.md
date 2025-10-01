| Syntax  | Description |
| ------- | ----------- |
| `- [ ]` | to-do       |
| `- [/]` | incomplete  |
| `- [x]` | done        |
| `- [-]` | canceled    |
| `- [>]` | forwarded   |
| `- [<]` | scheduling  |
| `- [?]` | question    |
| `- [!]` | important   |
| `- [*]` | star        |
| `- ["]` | quote       |
| `- [l]` | location    |
| `- [b]` | bookmark    |
| `- [i]` | information |
| `- [S]` | savings     |
| `- [I]` | idea        |
| `- [p]` | pros        |
| `- [c]` | cons        |
| `- [f]` | fire        |
| `- [k]` | key         |
| `- [w]` | win         |
| `- [u]` | up          |
| `- [d]` | down        |


딥러닝 구성요소
-  층
	- 입력층 : 데이터를 받아들이는 층
	- 은닉층 : 모든 입력 노드부터 입력 값을 받아 가중합을 계산, 활성화 함수 적용하여 출력층에 전달하는 층
	- 출력층 : 신경망의 최종 결괏값이 포함된 층
- 가중치 : 노드와 노드간 연결 강도
- 편향 : 가중합에 더해주는 상ㅅ, 하나의 뉴런에서 활성화 함수를거쳐 최종적으로 출력되는 값을 조절
- 가중합, 전달 함수 : 가중치와 신호의 곱을 합한 것
- 함수
	- 활성화 함수 : 신호를 입력받아 이를 적절히 처리하여 출력하는 함수
	- 손실 함수 : 가중치 학습을 위해 출력 함수의 결과와 실제 값 간의 오차를 측정하는 함수


가중치
- 입력 값이 연산 결과에 미치는 영향력을 조절하는 요소

가중합 또는 전달 함수
- 가중합 = 전달함수
- 각 노드에서 들어오는 신호에 가중치를 곱해서 다음 노드로 전달 ->이 값을 모두 더한 합계 = 가중합
- 노드의 가중합이 계산되면 이 가중합을 활성화 함수로 보내기 때문에 전달 함수(transfer function)

활성화 함수
- 전달 함수에서 전달 받은 값을 출력할 때 일정 기준에 따라 출력 값을 변화시키는 비선형 함수
	- sigmoid
		- 선형 함수의 결과 > 0~1 사이 비선형 형태로 변형
		- 로지스틱 회귀와 같은 분류 문제를 확률적으로 표현
		- 기울기 소멸 문제
	- hyperbolic tangent
		- 선형 함수의 결과 > -1~1 사이 비선형 형태로 변형
		- 로지스틱 회귀와 같은 분류 문제를 확률적으로 표현
		- 기울기 소멸 문제
	- ReLU
		- 음수일 때는 0, 양수일때는 _x_
		- 경사하강법에 영향을 주지 않아 학습 속도가 빠름
		- 기울기 소멸 문제 발생 X
	- Leaky ReLU
		- 음수일 때는 매우 작은 수
		- 입력 값이 수렴하는 구간 제거





---
There are 3 popular methods to calculate the derivative:
1. Numerical differentiation
2. Symbolic differentiation
3. Automatic differentiation

---
autograd(역방향 패스를 실행하고 기울기를 계산하는 함수 / Automatic differentiation)
torch.nn(신경망을 다루는 유틸리티 세트)

---

딥러닝 모델에서 Leaf Tensor는 모델의 학습 가능한 파라미터(가중치와 편향)
leaf tensor : 
- outdegree 가 0
- 연산 그래프의 말단에 위치한 텐서
- 다른 텐서의 연산 결과로 생성되지 않은 텐서
- 직접 생성된 텐서로, 사용자가 명시적으로 정의
- 모델의 학습 가능한 파라미터들이 일반적으로 리프 텐서
non-leaf tensor
- 다른 텐서의 연산 결과로 생성된 텐서

왜 가중치와 바이어스는 리프 텐서인가?
- 직접 정의됨: 
	- 모델의 가중치와 바이어스는 `torch.nn.Parameter`나 `torch.nn.Module`의 서브클래스에서 직접 생성
- 연산 결과가 아님: 
	- 이들은 다른 텐서들의 연산 결과로 생성되지 않습니다.

---

retrain_grad()
- backward를 호출하면 intermediate variables의 grad는 모두 삭제
- requires_grad_를 적용해주어도 backward 후에 grad 속성을 보면 None값
- (leaf node인 경우에만 grad 추적이 되도록 default 설정해둠 in torch) 
- **self.upsampled.retrain_grad()를 backward 메소드 호출 전에 추가해주면** grad를 제대로 잡아주는 걸 볼 수 있음.

1. .grad 속성은 모든 텐서에 존재하지만 기본값은 None
2. 기울기가 필요한 모든 리프 텐서(t.is_leaf())의 .grad 필드에 누적 (또는 retain_grad()를 호출한 텐서들인데, 이는 비리프 텐서의 .grad 필드를 채우는 데 사용)
3. 여러 개의 .backward()를 연달아 실행하면(중간에 .forward()가 없더라도) 모든 매개변수에 대한 .grad 속성이 누적됩니다.

---

**순전파(Forward Propagation)**: 순전파 단계에서, 신경망은 정답을 맞추기 위해 최선의 추측(best guess)을 합니다. 이렇게 추측을 하기 위해서 입력 데이터를 각 함수들에서 실행
**역전파(Backward Propagation)**: 역전파 단계에서, 신경망은 추측한 값에서 발생한 오류(error)에 비례하여(proportionate) 매개변수들을 적절히 조절(adjust)합니다. 출력(output)로부터 역방향으로 이동하면서 오류에 대한 함수들의 매개변수들의 미분값( _변화도(gradient)_ )을 수집하고, 경사하강법(gradient descent)을 사용하여 매개변수들을 최적화

---

연산 그래프(Computational Graph)
개념적으로, autograd는 데이터(텐서)의 및 실행된 모든 연산들(및 연산 결과가 새로운 텐서인 경우도 포함하여)의 기록을 Function 객체로 구성된 방향성 비순환 그래프(DAG; Directed Acyclic Graph)에 저장(keep)합니다. 이 방향성 비순환 그래프(DAG)의 잎(leave)은 입력 텐서이고, 뿌리(root)는 결과 텐서입니다. 이 그래프를 뿌리에서부터 잎까지 추적하면 연쇄 법칙(chain rule)에 따라 변화도를 자동으로 계산할 수 있습니다.

순전파 단계에서, autograd는 다음 두 가지 작업을 동시에 수행합니다:
- 요청된 연산을 수행하여 결과 텐서를 계산하고,
- DAG에 연산의 _변화도 기능(gradient function)_ 를 유지(maintain)합니다.

역전파 단계는 DAG 뿌리(root)에서 `.backward()` 가 호출될 때 시작됩니다. `autograd` 는 이 때:
- 각 `.grad_fn` 으로부터 변화도를 계산하고,
- 각 텐서의 `.grad` 속성에 계산 결과를 쌓고(accumulate),
- 연쇄 법칙을 사용하여, 모든 잎(leaf) 텐서들까지 전파(propagate)합니다.

다음은 지금까지 살펴본 예제의 DAG를 시각적으로 표현한 것입니다. 그래프에서 화살표는 순전파 단계의 방향을 나타냅니다. 노드(node)들은 순전파 단계에서의 각 연산들에 대한 역전파 함수들을 나타냅니다. 파란색 잎(leaf) 노드는 잎 텐서 `a` 와 `b` 를 나타냅니다.

![../../_images/dag_autograd.png](https://tutorials.pytorch.kr/_images/dag_autograd.png)

>참고
**PyTorch에서 DAG들은 동적(dynamic)입니다.** 주목해야 할 중요한 점은 그래프가 처음부터(from scratch) 다시 생성된다는 것입니다; 매번 `.backward()` 가 호출되고 나면, autograd는 새로운 그래프를 채우기(populate) 시작합니다. 이러한 점 덕분에 모델에서 흐름 제어(control flow) 구문들을 사용할 수 있게 되는 것입니다; 매번 반복(iteration)할 때마다 필요하면 모양(shape)이나 크기(size), 연산(operation)을 바꿀 수 있습니다.

---

DAG에서 제외하기
`torch.autograd` 는 `requires_grad` 플래그(flag)가 `True` 로 설정된 모든 텐서에 대한 연산들을 추적합니다. 따라서 변화도가 필요하지 않은 텐서들에 대해서는 이 속성을 `False` 로 설정하여 DAG 변화도 계산에서 제외합니다.

입력 텐서 중 단 하나라도 `requres_grad=True` 를 갖는 경우, 연산의 결과 텐서도 변화도를 갖게 됩니다.

```
x = torch.rand(5, 5)
y = torch.rand(5, 5)
z = torch.rand((5, 5), requires_grad=True)

a = x + y
print(f"Does `a` require gradients?: {a.requires_grad}")
b = x + z
print(f"Does `b` require gradients?: {b.requires_grad}")

Does `a` require gradients?: False
Does `b` require gradients?: True
```

신경망에서, 변화도를 계산하지 않는 매개변수를 일반적으로 **고정된 매개변수(frozen parameter)** 이라고 부릅니다. 이러한 매개변수의 변화도가 필요하지 않다는 것을 미리 알고 있으면, 신경망 모델의 일부를 《고정(freeze)》하는 것이 유용합니다. (이렇게 하면 autograd 연산량을 줄임으로써 성능 상의 이득을 제공합니다.)

미세조정(finetuning)을 하는 과정에서, 새로운 정답(label)을 예측할 수 있도록 모델의 대부분을 고정한 뒤 일반적으로 분류 계층(classifier layer)만 변경합니다. 이를 설명하기 위해 간단한 예제를 살펴보겠습니다. 이전과 마찬가지로 이미 학습된 resnet18 모델을 불러온 뒤 모든 매개변수를 고정합니다.

```
from torch import nn, optim

model = resnet18(weights=ResNet18_Weights.DEFAULT)
```

신경망의 모든 매개변수를 고정합니다

```
for param in model.parameters():
    param.requires_grad = False
```

10개의 정답(label)을 갖는 새로운 데이터셋으로 모델을 미세조정하는 상황을 가정해보겠습니다. resnet에서 분류기(classifier)는 마지막 선형 계층(linear layer)인 `model.fc` 입니다. 이를 새로운 분류기로 동작할 (고정되지 않은) 새로운 선형 계층으로 간단히 대체하겠습니다.

```
model.fc = nn.Linear(512, 10)
```

이제 `model.fc` 를 제외한 모델의 모든 매개변수들이 고정되었습니다. 변화도를 계산하는 유일한 매개변수는 `model.fc` 의 가중치(weight)와 편향(bias)뿐입니다.

분류기만 최적화합니다.
```
optimizer = optim.SGD(model.parameters(), lr=1e-2, momentum=0.9)
```

옵티마이저(optimizer)에 모든 매개변수를 등록하더라도, 변화도를 계산(하고 경사하강법으로 갱신)할 수 있는 매개변수들은 분류기의 가중치와 편향뿐입니다.

컨텍스트 매니저(context manager)에 torch.no_grad() 로도 똑같이 제외하는 기능을 사용할 수 있습니다.

---

tensor에 requires_grad=False가 있는 경우(DataLoader를 통해 얻었거나 전처리 또는 초기화가 필요했기 때문), tensor.requires_grad_()는 autograd가 tensor에 대한 작업을 기록하도록 합니다.