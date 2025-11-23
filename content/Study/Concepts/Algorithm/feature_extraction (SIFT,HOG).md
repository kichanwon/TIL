---
draft:
---
## 1. SIFT (Scale Invariant Feature Transform)
### 개념 및 동작 원리
- 영상에서 코너점이나 뚜렷한 특징점(keypoints)을 먼저 검출하고, 각 특징점 주변의 로컬 패치(local patch)에 대해 특징 벡터(feature vector)를 추출한다.
- 특징점 주변 패치를 4×4 블록으로 나누고, 각 블록 내에서 픽셀들의 그래디언트(gradient) 방향 및 크기에 대한 히스토그램을 구한다. 이 히스토그램의 모든 bin 값을 이어 붙여서 128차원 벡터로 만든다.
- 크기 변화(scale), 회전(rotation), 밝기 변화 등에 대해 **강인성(invariance)** 을 갖도록 설계되었다. 즉, 물체의 크기나 방향이 바뀌어도 특징 매칭이 가능하도록 한다. 
### 강점 및 제약
- **강점**: 복잡한 내부 패턴을 가진 물체에서 특징점이 풍부할 경우 뛰어난 구분력(discriminative power)을 갖는다. 밝기 변화나 회전 등에 비교적 강하다.
- **제약**: 계산량이 많고 실시간 처리에 부담이 있을 수 있으며, 단순한 윤곽이나 패턴만 있는 경우 과한 표현이 될 수 있다.
### 사용 맥락
- 물체 인식(object recognition)·매칭(matching)·스테레오 영상(stereo vision) 등의 분야에서 특징점 기반 매칭이 필요할 때 적합하다.
- 내부 패턴이 복잡하고 특징점이 풍부하게 존재하는 대상에 유리하다.

## 2. HOG (Histogram of Oriented Gradients)
### 개념 및 동작 원리
- 입력 영상의 대상 영역(region of interest)을 일정 크기의 셀(cell) 단위로 나눈다. 각 셀 내에서 엣지(edge) 또는 그래디언트 크기가 일정 이상인 픽셀들의 방향(orientation)에 대한 히스토그램을 구한다.
- 히스토그램을 블록(block) 단위로 정규화(normalization)하여 조명 변화(lighting change) 등에 대한 영향을 줄이고, 이들 정규화된 히스토그램을 일렬로 이어붙여 특징 벡터(feature vector)를 구성한다.
- 템플릿 매칭(template matching)과 히스토그램 매칭(histogram matching)의 중간적 성격을 갖는다: 블록 단위로 기하학적 정보를 일부 유지하면서, 셀 내부에서는 분포(histogram) 정보만을 사용한다.
### 강점 및 제약
- **강점**: 윤곽선(contour)이나 형태(silhouette)가 뚜렷한 대상(human, 자동차 등)에서 유리하다. 엣지 기반이므로 밝기 변화나 조명 변화에도 비교적 강하다.
- **제약**: 물체가 회전되거나 형태 변화가 크면 매칭이 어려울 수 있다. 내부 패턴이 복잡하거나 특징점이 많은 경우에는 SIFT 등 보다 강력한 방법이 유리할 수 있다.
### 사용 맥락
- 사람 검출(human detection)·차량 검출(vehicle detection) 등 윤곽과 형태가 중요한 객체 검출(object detection) 분야에서 많이 사용된다.
- 물체의 형태 변화나 회전이 적고, 윤곽 중심의 단순한 내부 구조를 갖는 대상에 적합하다.