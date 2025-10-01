### 1. Numpy 기반 증강

#### 1.1 회전 (Rotation)
- `np.rot90()` 활용 (90°, 180°, 270°)

		
#### 1.2 반전 (Flip)
- `numpy.flip()`
	
- `np.flipud()` (상하)
	
- `np.fliplr()` (좌우)
	
- 슬라이싱(`[::-1]`) 방식

#### 1.3 밝기 조절 (Brightness) 
- 픽셀 값 스케일링 (`img * x`)
	
- `np.clip()` + `astype(np.uint8)`
        
#### 1.4 대비 조절 (Contrast)
- 평균값 기반: `mean_val + x * (img - mean_val)`
        
#### 1.5 노이즈 추가 (Noise)
- `np.random.normal()` 활용

#### 1.6 크롭 (Crop)
- 중앙 크롭 구현
        
#### 1.7 임의 각도 회전 (Custom Rotation)
- 회전 행렬 직접 정의   
- 중심점 기준 좌표 변환
---
### 2. PyTorch 기반 증강
#### 2.1 Numpy ↔ Tensor 변환  
- `torch.from_numpy()` + `permute()`
        
- Tensor → Numpy 변환
        
#### 2.2 기본 변환
- 밝기 조절 (`torch.clamp(tensor * x, 0, 1)`)

- 색상 반전 (`torch.flip(dims=[0])`)

- 상하 반전 (`torch.flip(dims=[1])`)

- 좌우 반전 (`torch.flip(dims=[2])`)
        
#### 2.3 노이즈 추가
- `torch.randn_like()` 활용 후 `torch.clamp()`