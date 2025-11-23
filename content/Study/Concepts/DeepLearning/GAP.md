---
title: "Global Average Pooling: Spatial Abstraction and Feature Compression"
---
GAP은 “Tensor의 공간 평균을 내는 연산”
**GAP은 평균을 내기 때문에 공간 정보를 삭제하는 연산**
7×7×2048 → GAP → 1×1×2048
- 각 채널의 7×7 공간 정보를 **평균값 하나로 압축**
- 즉 정보 손실 발생
- 대신 “공간 위치 무관한 고수준 특징”만 남음
**큰 정보 손실이 있지만, 분류에서는 오히려 이게 장점**  
(불필요한 위치 정보가 줄어들어서 overfitting 감소)



**GAP을 쓰면 안 되는 모델 (예: Object Detection, Segmentation)**
이런 모델은 feature map의 **공간 정보(H×W)가 필수**  
따라서 **평균내서 공간을 없애면 안 됨.**
- Faster R-CNN
- Mask R-CNN
- U-Net 
- YOLO series
- FCN, DeepLab 등

**Transformer 입력으로 넣을 때**
transformer는 1D 토큰을 넣어야 하므로:
`H×W×C → flatten → sequence 만들기`
- ViT (Vision Transformer)
- CNN + Transformer hybrid 모델


**Fully Connected 구조 기반의 Dense 모델**
Flatten 후 Dense layer 여러 개 쌓는 구조에서는 GAP 불가.

