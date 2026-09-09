# 이미지 형성의 기하학
3D의 한 점이 이미지 평면(image plane)에 투영되는 방법에 관하여

## 가정

방 안에 3차원 점 $P$가 있을 때, 
카메라가 촬영한 이미지에서 점($P$)가 어느 픽셀 좌표 $(u, v)$에 나타낼 때
네 가지 좌표계가 사용됨.
1. World Coordinate System / $(P_w=(X_w,Y_w,Z_w))$
    - 실제 공간 기준 좌표계
2. Camera Coordinate System / $(P_c=(X_c,Y_c,Z_c))$
    - 카메라 중심 3차원 좌표계
3. Normalized Image Coordinate System / $(x_n,y_n)$
    - 카메라 내부 파라미터의 영향을 제거한 정규화된 2차원 좌표계
4. Pixel Image Coordinate System / $(u,v)$
    - 실제 이미지 파일에서 사용하는 2차원 픽셀 좌표계

전체 흐름은 다음과 같음.
$$  
P_w \rightarrow P_c \rightarrow (x_n,y_n) \rightarrow (u,v)  
$$
즉 
$$  
\text{월드 좌표} \rightarrow \text{카메라 좌표} \rightarrow \text{정규 이미지 좌표} \rightarrow \text{픽셀 좌표}  
$$
![[Pasted image 20260605100413.png]]

---
## World Coordinate System
![[Pasted image 20260529154725.png]]
월드 좌표계와 카메라 좌표계는 회전과 이동에 의해 서로 연결됨.
회전을 위한 3개의 파라미터와 이동을 위한 3개의 파라미터,
총 6개의 파라미터를 카메라의 **외부 파라미터**라고 함.

방의 모서리처럼 임의의 한 점을 원점으로 정하고 그 원점을 기준으로 점 $P$의 위치를 좌표로 표현.

---
## Camera Coordinate System

이미지는 카메라가 보는 장면이기에 변환이 필요.
$$P_w \rightarrow P_c$$
실제 공간 기준 좌표계에서 **카메라 중심 좌표계**로의 변환.

e.g)
- $P_w=(3,2,1)$: 실제 세계의 원점에서 오른쪽으로 3m, 앞쪽으로 2m, 위로 1m
- $P_c=(1,0.5,4)$: 카메라 기준으로 오른쪽으로 1m, 위/아래 방향으로 0.5m, 카메라 앞쪽으로 4m

카메라가 방의 축 방향과 다르게 돌아가 있으면 **회전 $R$** 이 필요.
카메라가 방의 원점과 다른 위치에 있으면 **이동 $t$** 이 필요.

$$P_c=R(Pw−C)=RP_w+t, t=−RC$$
행렬 표현: 
- $\mathbf X_c=R\mathbf X_w+t$
- $\begin{bmatrix} X_c \\ Y_c \\ Z_c \end{bmatrix} = \mathbf{R} \begin{bmatrix} X_w \\ Y_w \\ Z_w \end{bmatrix} + \mathbf{t}$

- $R$: $3 \times 3$ 회전 행렬
- $t$: $3 \times 1$ 이동 벡터
$R$과 $t$를 합쳐서 **외부 파라미터**라고 함. (카메라가 실제 공간에서 어디에 있고 어느 방향을 바라보는지)

회전과 이동을 하나의 행렬로 합친 것이 **외부 행렬**. $\mathbf{P} = [\mathbf{R} \mid \mathbf{t}]$ 
외부 행렬의 크기 $3 \times 4$
- 앞의 $3 \times 3$ 부분: 회전 행렬 $R$
- 마지막 $3 \times 1$ 부분: 이동 벡터 $t$

$\begin{bmatrix} X_w \\ Y_w \\ Z_w \\ 1 \end{bmatrix}$ 3차원 좌표 뒤에 $1$을 붙이면 [[Homogeneous Coordinates|동차 좌표]]로 표현할 수 있음.
[[Homogeneous Coordinates|동차 좌표]]를 사용하면 회전과 이동을 하나의 행렬 곱으로 계산할 수 있음.
- [[Homogeneous Coordinates|동차 좌표]] 표현: $\begin{bmatrix} X_c \\ Y_c \\ Z_c \end{bmatrix} = [\mathbf{R} \mid \mathbf{t}] \begin{bmatrix} X_w \\ Y_w \\ Z_w \\ 1 \end{bmatrix}$
마지막의 $1$은 실제 좌표값이 아니라, 이동 벡터 $t$를 행렬 곱 안에 포함시키기 위한 보조값.
t는 **월드 좌표계의 원점이 카메라 좌표계에서 어디에 보이는지를 나타내는 이동 벡터**

---
## Image Coordinate System

이미지 좌표계는 **카메라 기준 3차원 점 $P_c$** 를 이미지 평면 위의 좌표와 최종 **2차원 픽셀 좌표 $(u, v)$** 로 바꾸는 과정과 관련된 좌표계.

앞 단계까지는 월드 좌표를 카메라 좌표로 변환함.

$$
P_w \rightarrow P_c
$$

이제 해야 할 일은 카메라 기준 3차원 좌표를 이미지 위의 2차원 좌표로 변환하는 것.

$$
P_c \rightarrow (u, v)
$$

즉,

$$
(X_c, Y_c, Z_c) \rightarrow (u, v)
$$

카메라 좌표계에서 점 $P_c$는 다음과 같이 표현됨.

$$
P_c = (X_c, Y_c, Z_c)
$$

- $X_c$: 카메라 기준 좌우 방향 위치
- $Y_c$: 카메라 기준 위아래 방향 위치
- $Z_c$: 카메라 앞쪽 깊이 방향 거리

이미지에 찍히는 위치는 $X_c$, $Y_c$만으로 결정되지 않음.
깊이 $Z_c$가 함께 고려되어야 함.
	가까운 물체는 크게 보이고, 먼 물체는 작게 보이기 때문.

핀홀 카메라 모델에서는 카메라 기준 3차원 점을 이미지 평면 위의 좌표 $(x, y)$로 투영함.

![[Pasted image 20260604142340.png]]

$$
x = f \frac{X_c}{Z_c}, y = f \frac{Y_c}{Z_c}
$$
- $f$: 초점거리
- $\frac{X_c}{Z_c}$: **카메라에서 봤을 때 점이 좌우로 얼마나 벌어져 보이는지**를 나타내는 정규화 좌표 성분
- $\frac{Y_c}{Z_c}$: **카메라에서 봤을 때 점이 위아래로 얼마나 벌어져 보이는지**를 나타내는 정규화 좌표 성분

즉, $X_c$, $Y_c$를 깊이 $Z_c$로 나누는 과정이 **3D 점을 2D 방향 정보로 바꾸는 핵심**이고, 여기에 초점거리 $f$가 반영되어 이미지 평면 좌표 $(x, y)$가 정해짐.

위 식으로 얻은 $(x, y)$는 이상적인 이미지 평면 좌표.
하지만 실제 이미지에서 필요한 값은 픽셀 좌표. $(u, v)$

![[Pasted image 20260604142347.png]]

- $(x, y)$: 카메라의 광학 중심을 기준으로 한 **이미지 평면 위의 좌표**
- $(u, v)$: **실제 이미지 파일에서의 픽셀 좌표**
- 실제 이미지 좌표계는 보통 왼쪽 위를 원점 $(0, 0)$으로 사용

따라서 이미지 평면 좌표 $(x, y)$를 실제 픽셀 좌표 $(u, v)$로 바꾸는 과정이 필요함.
이때 카메라의 초점거리, 주점, 센서 특성을 반영하기 위해 내부 행렬 $K$를 사용함.

내부 행렬은 카메라 기준 3차원 좌표를 이미지에 투영하고 실제 픽셀 좌표로 변환하는 데 필요한 **카메라 내부 파라미터를 모은 행렬**.
즉, 카메라의 렌즈와 센서 특성을 나타냄.

$$
K =
\begin{bmatrix}
f_x & \gamma & c_x \\
0 & f_y & c_y \\
0 & 0 & 1
\end{bmatrix}
$$

- $f_x$: x축 방향 초점거리
- $f_y$: y축 방향 초점거리
- $c_x$: 이미지에서 광학 중심의 x좌표
- $c_y$: 이미지에서 광학 중심의 y좌표
- $\gamma$: 센서 x축과 y축 사이의 비틀림

대부분의 경우 $\gamma$는 거의 $0$으로 둠.
따라서 일반적으로는 다음과 같이 단순화하여 사용함.

$$
K =
\begin{bmatrix}
f_x & 0 & c_x \\
0 & f_y & c_y \\
0 & 0 & 1
\end{bmatrix}
$$

카메라 좌표 $(X_c, Y_c, Z_c)$를 깊이 $Z_c$로 나누면 정규화 이미지 좌표가 됨.
$$
\left(\frac{X_c}{Z_c}, \frac{Y_c}{Z_c}\right)
$$
정규화 이미지 좌표를 [[Homogeneous Coordinates|동차 좌표]]로 표현하면 다음과 같다.
$$  
\begin{bmatrix}  
x_n \  
y_n \  
1  
\end{bmatrix}

\begin{bmatrix}  
\frac{X_c}{Z_c} \  
\frac{Y_c}{Z_c} \  
1  
\end{bmatrix}  
$$

여기에 내부 행렬 $K$를 적용하면 픽셀 좌표를 얻을 수 있다.
$$  
\begin{bmatrix}  
u \  
v \  
1  
\end{bmatrix}

K  
\begin{bmatrix}  
x_n \  
y_n \  
1  
\end{bmatrix}  
$$

따라서 $\gamma = 0$일 때는 다음과 같다.
$u = f_x x_n + c_x$
$v = f_y y_n + c_y$

여기에 정규화 이미지 좌표를 대입하면 최종식은 다음과 같다.
$u = f_x \frac{X_c}{Z_c} + c_x$
$v = f_y \frac{Y_c}{Z_c} + c_y$

같은 과정을 카메라 좌표에서 바로 [[Homogeneous Coordinates|동차 좌표]] 형태로 쓰면 다음과 같이 표현할 수도 있다.
$$
\begin{bmatrix}  
u' \  
v' \  
w'  
\end{bmatrix}

K  
\begin{bmatrix}  
X_c \  
Y_c \  
Z_c  
\end{bmatrix}
$$

이때 $\gamma = 0$이면 내부 행렬 $K$는 다음과 같다.
$$  
K =  
\begin{bmatrix}  
f_x & 0 & c_x \  
0 & f_y & c_y \  
0 & 0 & 1  
\end{bmatrix}  
$$

따라서 계산 결과는 다음과 같다.
$u' = f_x X_c + c_x Z_c$
$v' = f_y Y_c + c_y Z_c$
$w' = Z_c$

[[Homogeneous Coordinates|동차 좌표]]를 실제 픽셀 좌표로 바꾸기 위해 마지막 성분 $w'$로 나누면 된다.
$u = \frac{u'}{w'}$
$v = \frac{v'}{w'}$

따라서 다음과 같다.
$u = \frac{f_x X_c + c_x Z_c}{Z_c}$
$v = \frac{f_y Y_c + c_y Z_c}{Z_c}$

이를 정리하면 다음과 같다.
$u = f_x \frac{X_c}{Z_c} + c_x$
$v = f_y \frac{Y_c}{Z_c} + c_y$

결국 카메라 기준 3차원 점을 이미지 픽셀 좌표로 바꾸는 전체 과정은 다음과 같다.
$$  
(X_c, Y_c, Z_c)  
\rightarrow  
\left(\frac{X_c}{Z_c}, \frac{Y_c}{Z_c}\right)  
\rightarrow  
(u, v)  
$$

즉, 먼저 깊이 $Z_c$로 나누어 정규화 이미지 좌표를 만들고, 이후 내부 행렬 $K$를 통해 초점거리와 주점 위치를 반영하여 실제 픽셀 좌표 $(u, v)$를 얻는다.

따라서 Image Coordinate System은 **카메라 기준 3D 좌표를 정규화 이미지 좌표로 바꾸고, 내부 행렬 $K$를 적용하여 실제 이미지 픽셀 좌표 $(u, v)$로 변환하는 과정**.

---
## Normalized Image Coordinate System

정규화된 이미지 좌표계는 카메라 내부 파라미터의 영향을 제거한 가상의 이미지 좌표계.
카메라 좌표계에서 점 $P$가 다음과 같다고 하자.

$$  
P_c=(X_c,Y_c,Z_c)  
$$

이 점을 깊이 $Z_c$로 나누면 정규 이미지 좌표가 됨.

$$  
x_n=\frac{X_c}{Z_c}  
$$

$$  
y_n=\frac{Y_c}{Z_c}  
$$

따라서 정규 이미지 좌표는 다음과 같음.

$$  
(x_n,y_n)=\left(\frac{X_c}{Z_c},\frac{Y_c}{Z_c}\right)  
$$

정규화된 이미지 좌표계는 초점거리, 주점, 픽셀 크기, 해상도 같은 카메라 내부 특성이 아직 반영되지 않은 좌표계.

즉, 카메라가 보는 방향 정보만 남긴 좌표계.
- $x_n$: 카메라 정면 기준으로 좌우 방향으로 얼마나 벗어나 보이는지
- $y_n$: 카메라 정면 기준으로 위아래 방향으로 얼마나 벗어나 보이는지

정규 이미지 좌표계는 초점거리 $f=1$인 가상의 이미지 평면으로 볼 수 있음.
즉, 카메라 초점에서 거리 1만큼 떨어진 가상의 이미지 평면에 점을 투영한 좌표.
이 좌표계는 실제 픽셀 좌표가 아니며 단위도 pixel이 아님.


