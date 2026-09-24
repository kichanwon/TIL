# Linear Algebra
## Definition
- 선형대수(Linear Algebra)는 벡터, 행렬, 선형방정식, 선형변환과 그 관계를 다루는 수학 분야
- 다차원 데이터를 표현하고 변환하는 기본 도구

## Intuition
- 여러 값을 **벡터와 행렬 형태로 구조화**해서 다루는 방법
- 벡터는 데이터나 방향을 표현
- 행렬은 벡터를 변환하는 연산을 표현
- 고차원 공간의 관계를 계산 가능한 형태로 바꾸는 수학적 언어

## Mathematical Definition
- 벡터
$$[  
\mathbf{x}=  
\begin{bmatrix}  
x_1\  
x_2\  
\vdots\  
x_n  
\end{bmatrix}  
]$$

- 행렬
$$[  
A=  
\begin{bmatrix}  
a_{11} & a_{12}\  
a_{21} & a_{22}  
\end{bmatrix}  
]
$$
- 선형변환의 대표적인 표현
$$[  
\mathbf{y}=A\mathbf{x}  
]$$

- 여러 선형방정식은 다음과 같이 표현 가능
$$[  
A\mathbf{x}=\mathbf{b}  
]$$

## Example
- 한 샘플의 여러 특성
$$[  
\mathbf{x}=  
\begin{bmatrix}  
\text{height}\  
\text{weight}\  
\text{age}  
\end{bmatrix}  
]$$
- 흑백 이미지는 픽셀 값의 행렬로 표현 가능
- RGB 이미지는 여러 행렬이 쌓인 Tensor 형태로 표현 가능
- 행렬을 이용해 벡터의 회전, 확대, 축소 등의 변환을 표현 가능

## Related Concepts
- [[Vector]]
- [[Matrix]]
- [[Linear Transformation]]
- [[Computer Vision/Homogeneous Coordinates]]

## Applications in AI
- 신경망의 기본 가중치 연산
$$[  
\mathbf{z}=W\mathbf{x}+\mathbf{b}  
]$$
- Transformer의 Attention에서 행렬 곱 사용
$$[  
QK^T  
]$$
- 이미지와 특징 데이터를 벡터, 행렬, Tensor로 표현
- PCA, SVD를 이용한 차원 축소
- Embedding을 벡터 공간에 표현
- Computer Vision에서 좌표 변환과 기하 계산에 사용

