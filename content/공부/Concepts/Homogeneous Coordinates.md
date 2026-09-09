# 동차 좌표
일반 좌표에 스케일 성분을 추가하여, 같은 점으로 투영되는 좌표들을 하나의 표현 체계 안에서 다루는 방법.

## 가정
컴퓨터 비전에서 카메라 영상은 3차원 공간의 점을 2차원 이미지 평면에 투영한 결과.

투영 과정에서는 서로 다른 3차원 점들이 같은 이미지 점으로 보일 수 있음.
예를 들어 카메라 중심에서 이미지 평면의 한 점을 지나는 투영선 위의 모든 점은 이미지에서는 같은 점으로 나타남.

[[Homogeneous Coordinates|동차 좌표]]는 이런 투영 관계를 다루기 위해 좌표에 스케일 값을 하나 더 붙여 표현함.

2차원 점의 경우:

$$
(x,y) \leftrightarrow (wx,wy,w), \quad w \neq 0
$$

3차원 점의 경우:

$$
(X,Y,Z) \leftrightarrow (wX,wY,wZ,w), \quad w \neq 0
$$

즉, 동차 좌표에서는 스케일이 달라도 같은 점을 나타낼 수 있음.

$$
(x,y,1) \sim (wx,wy,w)
$$

---
## Homogeneous Coordinates

일반적인 2차원 좌표 $(x,y)$는 동차 좌표로 다음과 같이 표현할 수 있음.

$$
\begin{bmatrix}
x \\
y \\
1
\end{bmatrix}
$$

하지만 동차 좌표에서 마지막 성분은 실제 좌표축이 아니라 스케일을 나타내는 보조 성분.
따라서 다음 좌표들은 모두 같은 2차원 점을 의미함.

$$
(x,y,1), (2x,2y,2), (3x,3y,3), \cdots
$$

일반적으로 동차 좌표가 다음과 같을 때,

$$
p =
\begin{bmatrix}
x \\
y \\
w
\end{bmatrix}
$$

$w \neq 0$이면 실제 2차원 좌표는 마지막 성분으로 나누어 얻음.

$$
\left(\frac{x}{w}, \frac{y}{w}\right)
$$

즉, 동차 좌표를 일반 좌표로 바꾸는 과정은 마지막 성분이 $1$이 되도록 스케일을 정규화하는 과정.

$$
(x,y,w) \sim \left(\frac{x}{w}, \frac{y}{w}, 1\right)
$$

3차원 좌표도 같은 방식으로 표현됨.

$$
\begin{bmatrix}
X \\
Y \\
Z
\end{bmatrix}
\rightarrow
\begin{bmatrix}
X \\
Y \\
Z \\
1
\end{bmatrix}
$$

그리고 일반적인 동차 좌표는 다음과 같음.

$$
\begin{bmatrix}
wX \\
wY \\
wZ \\
w
\end{bmatrix}
$$

$w \neq 0$이면 실제 3차원 좌표는 다음과 같이 복원됨.

$$
\left(\frac{wX}{w}, \frac{wY}{w}, \frac{wZ}{w}\right) = (X,Y,Z)
$$

---
## Projection

동차 좌표가 투영과 연결되는 이유는 **같은 이미지 점으로 투영되는 점들의 집합**을 자연스럽게 표현할 수 있기 때문.

정규 이미지 평면 위의 한 점을 다음과 같이 두자.

$$
p'=(u,v)
$$

이 점의 동차 좌표 표현은 다음과 같음.

$$
\begin{bmatrix}
u \\
v \\
1
\end{bmatrix}
$$

카메라 좌표계에서 보면, 카메라 중심과 이 점을 잇는 투영선 위의 3차원 점들은 다음과 같이 표현됨.

$$
\lambda
\begin{bmatrix}
u \\
v \\
1
\end{bmatrix}
=
\begin{bmatrix}
\lambda u \\
\lambda v \\
\lambda
\end{bmatrix}
$$

여기서 $\lambda$는 카메라 중심에서 해당 점까지의 스케일 또는 깊이 방향 거리로 볼 수 있음.
즉, $\lambda$가 달라져도 이미지 평면에서는 같은 점 $(u,v)$로 투영됨.

따라서 카메라 좌표계의 3차원 점이 다음과 같을 때,

$$
\begin{bmatrix}
x \\
y \\
z
\end{bmatrix}
$$

이미지 평면으로의 투영은 마지막 성분으로 나누는 형태가 됨.

$$
(x,y,z) \rightarrow \left(\frac{x}{z}, \frac{y}{z}\right)
$$

반대로 이미지 평면의 점 $(u,v)$에서 출발하여 그 점으로 투영될 수 있는 3차원 점들을 표현하면 다음과 같음.

$$
(u,v) \rightarrow \lambda(u,v,1)
$$

즉,
- 동차 좌표에서 마지막 성분으로 나누는 과정은 projection
- 이미지 점에 임의의 스케일을 곱하는 과정은 inverse projection

---
## Matrix Representation

동차 좌표를 사용하는 중요한 이유는 이동, 회전, affine 변환, perspective 변환을 하나의 행렬 곱으로 표현할 수 있기 때문.

2차원 affine 변환은 다음과 같이 쓸 수 있음.

$$
\begin{bmatrix}
x' \\
y' \\
1
\end{bmatrix}
=
\begin{bmatrix}
a & b & t_x \\
c & d & t_y \\
0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
1
\end{bmatrix}
$$

일반 좌표만 사용하면 이동 $t_x,t_y$는 행렬 곱 안에 넣기 어렵지만, 동차 좌표에서는 마지막 성분 $1$을 이용해 이동까지 하나의 행렬로 처리할 수 있음.

3차원 카메라 좌표 변환도 같은 방식으로 표현됨.

$$
\begin{bmatrix}
X_c \\
Y_c \\
Z_c
\end{bmatrix}
=
[\mathbf{R} \mid \mathbf{t}]
\begin{bmatrix}
X_w \\
Y_w \\
Z_w \\
1
\end{bmatrix}
$$

- $\mathbf{R}$: 회전 행렬
- $\mathbf{t}$: 이동 벡터
- $[\mathbf{R} \mid \mathbf{t}]$: 회전과 이동을 합친 외부 행렬

Perspective 변환도 동차 좌표 형태로 표현한 뒤 마지막 성분으로 나누어 실제 좌표를 얻음.

$$
\begin{bmatrix}
u' \\
v' \\
w'
\end{bmatrix}
=
\mathbf{H}
\begin{bmatrix}
x \\
y \\
1
\end{bmatrix}
$$

$$
u=\frac{u'}{w'}, \quad v=\frac{v'}{w'}
$$

여기서 $\mathbf{H}$는 2차원 평면 사이의 projective 변환을 나타내는 행렬로 볼 수 있음.

---
## Projective Geometry

동차 좌표는 사영 기하학(projective geometry)에서 사용하는 좌표계.
그래서 동차 좌표를 projective coordinate라고도 부름.

사영 기하학은 투영 변환에서 유지되는 기하적 성질을 다루는 기하학.
컴퓨터 비전에서는 주로 3차원 공간의 점들이 2차원 이미지 평면에 투영되는 상황과 연결됨.

유클리드 기하학에서는 길이, 각도, 평행성과 같은 개념이 중요함.
하지만 3차원 장면을 2차원 이미지로 투영하면 다음 성질들은 일반적으로 보존되지 않음.

- 길이
- 각도
- 평행성

반면 다음 성질은 투영 후에도 의미 있게 유지됨.

- 점은 점으로 투영됨
- 직선은 직선으로 투영됨
- 점이 직선 위에 있다는 포함 관계는 유지됨

평행성이 보존되지 않는다는 점은 이미지에서 소실점(vanishing point)으로 나타남.
실제 3차원 공간에서는 평행한 직선들이 만나지 않지만, 이미지에서는 한 점으로 모이는 것처럼 보일 수 있음.

동차 좌표에서는 무한대의 점도 유한한 좌표로 표현할 수 있음.
예를 들어 $(u,v)$ 방향의 무한대 점은 다음과 같이 표현됨.

$$
(u,v,0)
$$

마지막 성분이 $0$이면 일반 좌표로 복원할 수 없지만, 사영 기하학에서는 이것을 특정 방향의 무한원점으로 해석함.

---
## Line Representation

2차원 직선의 일반적인 방정식은 다음과 같음.

$$
ax + by + c = 0
$$

동차 좌표 $p=(x,y,w)^T$를 사용하면 직선 방정식은 다음과 같이 표현됨.

$$
ax + by + cw = 0
$$

직선을 벡터로 표현하면 다음과 같음.

$$
l =
\begin{bmatrix}
a \\
b \\
c
\end{bmatrix}
$$

점을 동차 좌표로 표현하면 다음과 같음.

$$
p =
\begin{bmatrix}
x \\
y \\
w
\end{bmatrix}
$$

그러면 점 $p$가 직선 $l$ 위에 있다는 조건은 내적으로 간단히 표현됨.

$$
l^T p = 0
$$

즉,

$$
\begin{bmatrix}
a & b & c
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
w
\end{bmatrix}
= ax + by + cw = 0
$$

---
## Cross Product

동차 좌표에서는 직선과 점 사이의 관계를 외적(cross product)으로 계산할 수 있음.

두 직선 $l_1$, $l_2$의 교점은 다음과 같음.

$$
p = l_1 \times l_2
$$

왜냐하면 외적 결과는 두 벡터 $l_1$, $l_2$에 모두 수직이기 때문.
따라서 다음 조건을 동시에 만족함.

$$
l_1^T p = 0
$$

$$
l_2^T p = 0
$$

이는 점 $p$가 두 직선 위에 모두 놓인다는 뜻.

반대로 두 점 $p_1$, $p_2$를 지나는 직선은 다음과 같이 구할 수 있음.

$$
l = p_1 \times p_2
$$

즉,
- 두 직선의 교점: $p = l_1 \times l_2$
- 두 점을 지나는 직선: $l = p_1 \times p_2$

두 직선이 다음과 같다고 하자.

$$
l_1 =
\begin{bmatrix}
a_1 \\
b_1 \\
c_1
\end{bmatrix},
\quad
l_2 =
\begin{bmatrix}
a_2 \\
b_2 \\
c_2
\end{bmatrix}
$$

그러면 교점의 동차 좌표는 다음과 같음.

$$
p =
l_1 \times l_2
=
\begin{bmatrix}
b_1c_2-b_2c_1 \\
a_2c_1-a_1c_2 \\
a_1b_2-a_2b_1
\end{bmatrix}
$$

마지막 성분이 $0$이 아니면 실제 교점은 다음과 같이 얻음.

$$
\left(
\frac{b_1c_2-b_2c_1}{a_1b_2-a_2b_1},
\frac{a_2c_1-a_1c_2}{a_1b_2-a_2b_1}
\right)
$$

마지막 성분이 $0$이면 일반적인 유클리드 좌표로는 교점이 없음.
이 경우 사영 기하학에서는 두 직선이 무한대에서 만난다고 해석함.

---
## 핵심 정리

동차 좌표는 일반 좌표에 스케일 성분을 추가한 표현.
스케일이 다른 동차 좌표라도 마지막 성분으로 나누었을 때 같은 일반 좌표가 되면 같은 점을 나타냄.

$$
(x,y,w) \sim \left(\frac{x}{w}, \frac{y}{w}, 1\right)
$$

카메라 기하학에서는 하나의 이미지 점이 하나의 3차원 점이 아니라 투영선 위의 모든 점에 대응됨.
동차 좌표는 이 관계를 $\lambda(u,v,1)$처럼 자연스럽게 표현함.

또한 동차 좌표를 사용하면 이동과 투영을 행렬 곱으로 통합할 수 있고, 직선과 점의 관계도 $l^Tp=0$, $l_1 \times l_2$, $p_1 \times p_2$처럼 간단하게 다룰 수 있음.

---
## 참고

- [다크 프로그래머 - [영상 Geometry #2] Homogeneous Coordinates](https://darkpgmr.tistory.com/78)
