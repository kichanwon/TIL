## 슬라이드 1. Title



오늘 발표할 논문은  
**MASt3R-SLAM: Real-Time Dense SLAM with 3D Reconstruction Priors**입니다.

이 논문은 CVPR 2025 Highlight로 선정되었고, Best Demo 부문에서도 Honorable Mention을 받은 연구입니다.

핵심적으로는 MASt3R라는 two-view 3D reconstruction 모델을 단순한 이미지 쌍 복원에 사용하는 것이 아니라, 실제로 연속적인 영상을 처리하는 **real-time dense SLAM 시스템으로 확장한 연구**입니다.

발표에서는 기존 SLAM이 어떤 한계를 가지고 있었고, MASt3R의 3D reconstruction prior를 이용해서 이를 어떻게 해결했는지를 중심으로 설명드리겠습니다.

---

## 슬라이드 2. Contents

발표는 다음 순서로 진행하겠습니다.

먼저 Abstract와 Demo를 통해 논문의 전체적인 아이디어와 실제 동작을 간단히 살펴보겠습니다.

그다음 Introduction과 Related Work에서 기존 SLAM의 문제와 이 논문이 해결하고자 하는 연구 공백을 설명하겠습니다.

이후 Method에서 MASt3R-SLAM의 핵심 기술들을 살펴보고,

마지막으로 실험 결과와 한계, 그리고 결론 순서로 발표를 진행하겠습니다.

---

## 슬라이드 3. Abstract

먼저 논문의 전체 내용을 간단하게 정리하겠습니다.

이 논문은 MASt3R라는 **two-view 3D reconstruction 모델을 기반으로 한 real-time dense monocular SLAM 시스템**을 제안합니다.

기존 monocular SLAM에서는 주로 이미지의 일부 특징점만 이용해 sparse map을 만드는 경우가 많았습니다.

반면 MASt3R는 두 이미지를 입력하면 각 pixel에 대응되는 dense 3D pointmap을 직접 예측할 수 있습니다.

MASt3R-SLAM은 이 dense 3D 정보를 tracking과 mapping에 직접 활용합니다.

또 하나의 특징은 정확한 camera intrinsic이나 특정한 parametric camera model을 반드시 알고 있어야 하는 구조가 아니라는 점입니다.

기본적으로 모든 ray가 하나의 camera center를 통과한다는 조건만 사용하기 때문에 calibration이 정확하지 않은 환경에서도 동작할 수 있도록 설계되었습니다.

Method는 크게

**Pointmap Matching, Tracking과 Local Fusion, Graph와 Loop Closure, 그리고 Global Optimisation**

으로 구성됩니다.

최종적으로 전역적으로 일관된 camera pose와 dense 3D reconstruction을 생성하면서 약 15 FPS 수준으로 동작하는 것이 이 시스템의 핵심입니다.

---

## 슬라이드 4. Demo

이제 본격적인 설명 전에 실제 시스템이 어떻게 동작하는지 데모를 먼저 보겠습니다.

왼쪽 위 영상이 SLAM 시스템에 입력되는 단안 RGB 영상입니다.

시스템은 현재 frame과 이전 keyframe을 계속 비교하면서 카메라의 위치와 주변의 3D 구조를 동시에 추정합니다.

큰 화면에 보이는 것이 이 과정에서 생성되는 dense 3D reconstruction입니다.

여기서 작은 카메라 형태는 현재 추정된 camera pose를 나타내고, 카메라가 이동하면서 장면의 3D 구조가 계속 누적되는 것을 확인할 수 있습니다.

즉, 이 데모에서 볼 부분은 단순히 3D reconstruction만 수행하는 것이 아니라,

**단안 RGB 영상이 들어오는 동안 카메라를 tracking하면서 동시에 dense 3D map을 실시간으로 생성한다는 점**입니다.

---

## 슬라이드 5. Simultaneous Localization and Mapping

이제 논문의 배경이 되는 SLAM부터 간단히 설명하겠습니다.

SLAM은 Simultaneous Localization and Mapping의 약자로,

카메라나 로봇이 **현재 자신의 위치를 추정하는 Localization과 주변 환경의 지도를 만드는 Mapping을 동시에 수행하는 기술**입니다.

예를 들어 오른쪽의 로봇청소기처럼 SLAM이 없다면 주변 공간을 이해하지 못하고 이동하지만, SLAM을 사용하면 방의 구조를 파악하면서 자신의 위치를 추정할 수 있습니다.

여기서 중요한 것은 Localization과 Mapping이 서로 독립된 문제가 아니라는 점입니다.

정확한 위치를 추정하려면 정확한 지도가 필요하고, 반대로 정확한 지도를 만들기 위해서는 카메라의 정확한 위치를 알아야 합니다.

따라서 이 두 문제를 동시에 안정적으로 해결하는 것이 SLAM의 핵심입니다.

기존에는 이를 정확하게 수행하기 위해 calibration된 카메라뿐만 아니라 IMU나 LiDAR 같은 추가 센서를 함께 사용하는 경우도 많습니다.

하지만 이 논문에서는 **단일 RGB 카메라만으로 dense 3D map까지 실시간으로 만들 수 있는가**에 초점을 맞춥니다.

---

## 슬라이드 6. Matching And Stereo 3D Reconstruction — MASt3R

다음으로 이 논문의 기반 모델인 MASt3R를 설명하겠습니다.

기존의 two-view 3D reconstruction에서는 두 이미지로부터 3D 구조를 얻기 위해 여러 단계가 필요합니다.

먼저 feature를 추출하고 두 이미지 사이에서 matching을 수행한 뒤, 카메라의 상대 pose를 계산하고 마지막으로 triangulation을 통해 3D point를 생성합니다.

즉,

**Feature extraction → Matching → Pose estimation → Triangulation**

과 같은 단계적인 geometry pipeline을 사용합니다.

DUSt3R는 이 과정을 학습 기반으로 바꿨습니다.

두 이미지를 입력하면 네트워크가 각 pixel이 3D 공간에서 어디에 위치하는지를 직접 예측합니다.

MASt3R는 DUSt3R를 발전시킨 모델로, 여기에 보다 정확한 correspondence를 찾기 위한 matching 기능을 강화했습니다.

즉 MASt3R의 핵심은

**두 이미지를 입력받아 두 이미지가 공유하는 3D 공간을 직접 예측할 수 있다는 것**입니다.

이렇게 학습된 dense 3D reconstruction prior를 SLAM의 tracking과 mapping에 이용하는 것이 MASt3R-SLAM의 출발점입니다.

---

## 슬라이드 7. Introduction — 문제 배경

그렇다면 왜 MASt3R 같은 3D reconstruction prior가 SLAM에서 필요한지를 살펴보겠습니다.

단일 RGB 카메라를 사용하는 SLAM에서는 크게 세 가지를 동시에 해결해야 합니다.

첫 번째는 정확한 camera pose,

두 번째는 전역적으로 일관된 dense 3D geometry,

세 번째는 이를 실시간으로 처리할 수 있는 계산 속도입니다.

기존 방법에서는 이 세 가지를 동시에 만족시키기가 어렵습니다.

기존 single-view prior는 한 장의 이미지에서 depth를 예측하기 때문에 깊이 모호성과 view 간 consistency 문제가 있습니다.

Multi-view 방식은 여러 이미지의 관계를 이용하지만, 관측되는 2D motion 안에 camera motion과 scene depth가 함께 섞여 있기 때문에 pose와 geometry를 분리해서 추정하기 어렵습니다.

또한 전통적인 SLAM은 정확한 camera model과 calibration을 요구하는 경우가 많습니다.

여기서 저자들이 주목한 점은

**카메라의 pose는 계속 변하지만 동일한 장면의 실제 3D 구조는 변하지 않는다**는 것입니다.

그래서 이미지 평면의 2D motion을 중심으로 문제를 풀기보다,

여러 이미지에서 공통으로 유지되는 **3D geometry 자체를 prior로 사용하자**는 방향으로 접근합니다.

결국 이 연구의 핵심 전환은

**2D motion prior 중심 SLAM에서 학습된 3D reconstruction prior 중심 SLAM으로의 전환**

이라고 볼 수 있습니다.

---

## 슬라이드 8. Related Work

관련 연구를 보면 기존 방식의 장단점을 더 명확하게 볼 수 있습니다.

먼저 Classical Sparse SLAM은 특징점과 camera pose를 함께 최적화하기 때문에 정확하고 빠르게 동작할 수 있습니다.

하지만 일부 landmark만 사용하기 때문에 장면 전체의 dense 3D geometry를 제공하기 어렵습니다.

Learning-based geometry에서는 single-view depth prediction이나 optical flow와 같은 방법들이 사용되었습니다.

Single-view 방식은 한 장의 이미지에서 발생하는 depth ambiguity가 문제이고,

multi-view 방식은 camera motion과 scene depth가 함께 결합되어 있다는 문제가 있습니다.

DROID-SLAM과 같은 learning-based SLAM도 높은 성능을 보여주지만, 명시적인 3D geometry prior가 부족합니다.

반대로 NeRF나 Gaussian Splatting은 높은 품질의 3D reconstruction을 만들 수 있지만 실시간 SLAM으로 사용하기에는 계산 비용이 큽니다.

그리고 최근 DUSt3R와 MASt3R가 두 이미지에서 직접 dense pointmap을 예측하는 강력한 3D prior를 제공하기 시작했습니다.

하지만 이 모델들은 기본적으로 독립적인 two-view reconstruction 모델이지 SLAM 시스템은 아닙니다.

따라서 이 논문의 연구 공백은

**강력한 two-view 3D reconstruction prior는 존재하지만, 이를 실시간으로 동작하고 전역 일관성을 유지하는 SLAM으로 만드는 방법은 부족하다**

는 것으로 정리할 수 있습니다.

---

## 슬라이드 9. Preliminaries

Method에 들어가기 전에 뒤에서 계속 사용하는 몇 가지 개념만 먼저 설명하겠습니다.

먼저 DUSt3R는 두 이미지를 입력하면 각 이미지에 대한 pointmap과 confidence를 출력합니다.

여기서 pointmap은 간단하게 말하면 **각 pixel이 3D 공간에서 어느 위치에 있는지를 나타낸 값**입니다.

그리고 confidence는 그 3D 위치 예측을 얼마나 신뢰할 수 있는지를 나타냅니다.

MASt3R는 여기에 feature head를 추가해서 **feature descriptor와 feature confidence**도 함께 출력합니다.

Feature descriptor는 각 pixel의 특징을 숫자 벡터로 나타낸 값으로, 서로 다른 이미지에서 같은 지점을 찾는 correspondence matching에 사용됩니다.

두 번째는 scale 문제입니다.

Two-view reconstruction에서는 이미지 쌍마다 예측되는 3D 구조의 scale이 서로 달라질 수 있습니다.

그래서 MASt3R-SLAM은 pose를 일반적인 rotation과 translation만 가지는 SE(3)가 아니라, scale까지 포함하는 **Sim(3)** 공간에서 표현합니다.

즉 rotation, translation, scale을 함께 최적화합니다.

마지막은 camera model입니다.

이 논문은 모든 ray가 하나의 camera center를 통과한다는 **generic central camera**만 가정합니다.

그리고 3D point를 unit ray 방향으로 변환해서 사용하기 때문에 특정 pinhole projection이나 정확한 intrinsic에 대한 의존성을 줄일 수 있습니다.

이 세 가지 개념,

**pointmap, Sim(3), 그리고 ray representation**

을 기억하면 이후 Method를 이해하기가 훨씬 쉽습니다.

---

## 슬라이드 10~14. Method

이 부분은 직전에 작성한 대본을 그대로 사용합니다.

- System Diagram
    
- Pointmap Matching
    
- Tracking and Pointmap Fusion
    
- Graph Construction and Loop Closure
    
- Backend Optimisation
    
- Relocalisation
    
- Known Calibration
    

---

## 슬라이드 15. 평가 데이터셋 및 평가 방법

이제 실험 결과를 살펴보겠습니다.

먼저 camera pose 평가는 TUM RGB-D, 7-Scenes, ETH3D-SLAM, EuRoC와 같은 데이터셋을 사용했습니다.

실험 환경은 Intel i9 CPU와 RTX 4090 GPU이고, 시스템은 약 15 FPS 수준으로 동작합니다.

Pose 정확도를 평가하기 위해서는 **Absolute Trajectory Error, ATE**를 사용합니다.

ATE는 추정된 카메라 trajectory와 실제 trajectory 사이의 위치 차이를 전체 frame에서 계산한 뒤 RMSE로 나타내는 지표입니다.

값이 작을수록 카메라 위치를 정확하게 추정한 것입니다.

Dense geometry는 Accuracy, Completion, Chamfer Distance 세 가지 지표를 사용합니다.

Accuracy는 재구성한 point가 실제 surface에 얼마나 가까운지,

Completion은 실제 장면을 얼마나 빠짐없이 복원했는지를 나타냅니다.

Chamfer Distance는 이 두 값을 함께 고려한 전체적인 reconstruction 품질 지표입니다.

따라서 이후 표에서는 기본적으로 **값이 낮을수록 좋은 결과**라고 보면 됩니다.

---

## 슬라이드 16. Camera Pose Estimation & Dense Geometry Evaluation

이제 핵심 결과를 보겠습니다.

표 전체의 숫자를 하나씩 보기보다는 몇 가지 결과만 보겠습니다.

먼저 위쪽 TUM RGB-D의 calibrated 조건에서 MASt3R-SLAM의 평균 ATE는 **0.030 m**로, 비교 방법 중 매우 높은 trajectory 정확도를 보입니다.

반면 calibration을 사용하지 않는 조건에서도 MASt3R-SLAM은 **0.060 m**이고, 같은 조건의 DROID-SLAM은 **0.158 m**입니다.

즉 정확한 intrinsic을 사용하지 않아도 tracking이 크게 무너지지 않는다는 것을 보여줍니다.

왼쪽 아래 7-Scenes에서도 calibrated MASt3R-SLAM의 평균 ATE는 **0.047 m** 수준으로 경쟁력 있는 결과를 보입니다.

오른쪽의 geometry 평가를 보면 pose accuracy뿐만 아니라 dense geometry에서도 낮은 Chamfer Distance를 보여주고 있습니다.

따라서 이 실험에서 중요한 결과는 단순히 trajectory만 정확한 것이 아니라,

**camera tracking과 dense 3D reconstruction을 동시에 수행하면서 높은 성능을 유지했다는 것**입니다.

다만 모든 데이터셋과 모든 조건에서 항상 압도적인 성능을 보이는 것은 아닙니다.

따라서 결과는

**calibrated 조건에서 강한 trajectory 성능을 보이고, uncalibrated 환경에서도 안정적으로 동작하며, 동시에 dense geometry까지 제공한다**

정도로 해석하는 것이 적절합니다.

---

## 슬라이드 17. Limitations and Future Work

다음은 논문의 한계와 향후 연구 방향입니다.

첫 번째 한계는 **전역적인 dense geometry optimisation이 부족하다는 점**입니다.

현재는 여러 frame에서 얻은 pointmap을 fusion해서 local geometry를 개선하지만, 전체 surface를 대상으로 dense bundle adjustment를 수행하는 구조는 아닙니다.

따라서 전체 map의 surface consistency가 완전히 보장되지는 않습니다.

두 번째는 MASt3R 자체가 주로 pinhole camera 이미지를 기반으로 학습되었다는 점입니다.

SLAM framework 자체는 다양한 camera model을 처리할 수 있지만, 기반 network가 fisheye나 큰 distortion에 충분히 학습되지 않았다면 geometry prediction 성능이 떨어질 수 있습니다.

세 번째는 계산량입니다.

MASt3R decoder를 full resolution으로 실행하기 때문에 tracking이나 loop closure, relocalisation 과정에서 network inference가 주요 병목이 될 수 있습니다.

따라서 향후 연구에서는 크게 세 방향이 중요합니다.

먼저 decoder를 경량화해서 inference 속도를 높이는 것,

두 번째는 pose graph뿐만 아니라 dense pointmap 자체까지 전역적으로 최적화하는 것,

세 번째는 fisheye나 wide-angle 등 다양한 camera model을 포함하도록 기반 MASt3R 모델 자체를 확장하는 것입니다.

즉 현재 연구는 real-time dense SLAM의 가능성을 보여줬지만,

**속도와 global geometry consistency, 그리고 camera domain generalisation은 여전히 남아 있는 과제**라고 볼 수 있습니다.

---

## 슬라이드 18. Conclusion

마지막으로 전체 내용을 정리하겠습니다.

기존 learning-based SLAM의 많은 연구는 DROID-SLAM과 같이 optical flow나 2D correspondence를 중심으로 pose와 depth를 추정하는 방향으로 발전해 왔습니다.

반면 이 논문은 MASt3R의 **two-view 3D reconstruction prior 자체를 SLAM의 핵심 representation으로 사용했다는 점**에서 기존 접근과 차이가 있습니다.

즉 MASt3R가 예측한 dense pointmap을 이용해 correspondence를 찾고,

ray 기반으로 camera pose를 추정하며,

여러 frame의 pointmap을 fusion하고,

loop closure와 global optimisation을 통해 장기적인 일관성을 유지했습니다.

결과적으로 이 시스템은

**calibration 없이도 동작 가능한 구조, real-time dense SLAM, 높은 trajectory accuracy, 그리고 dense 3D reconstruction**

을 하나의 시스템 안에서 구현했습니다.

제가 생각하는 이 논문의 가장 중요한 기여는 단순히 MASt3R를 SLAM에 넣었다는 것이 아니라,

**학습된 two-view 3D reconstruction 모델의 출력을 실시간 tracking과 mapping, 그리고 global optimisation까지 연결할 수 있는 SLAM framework로 확장했다는 점**입니다.

즉 향후 SLAM 연구가 기존의 2D motion 중심 접근뿐만 아니라, 학습된 3D geometry prior를 직접 활용하는 방향으로도 발전할 수 있다는 가능성을 보여준 연구라고 정리할 수 있습니다.

---

## 슬라이드 19. 감사합니다

이상으로 MASt3R-SLAM 발표를 마치겠습니다.

감사합니다.