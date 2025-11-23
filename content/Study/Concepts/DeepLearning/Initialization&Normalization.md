---
draft:
---
현대의 초기화(Xavier/He)와 정규화(BatchNorm)는  
**폭발/소실되지 않도록 weight를 0 근처의 작은 값으로 초기화한다.**
- Xavier/Glorot 초기화 (2010)
- He/Kaiming 초기화 (2015, ReLU 최적화)

둘 다 **weight의 분산을 매우 작게 설정**해서  
초기 weight가 **0 근처의 작은 random 값**이 되도록 설계되었다.
그 이유는:
- weight가 너무 크면 → forward 값/gradient가 **폭발(explode)**
- weight가 너무 작으면 → 값/gradient가 **소실(vanish)**

따라서 “폭발도 소실도 일어나지 않게 하는 **균형점**”이  
결과적으로 **0 근처의 작은 값**이 된다.

BatchNorm도 같은 맥락에서:
- activation의 분포를 정규화하여 gradient가 안정적으로 흐르도록 만든다
- BN의 gamma, beta도 초기값이 **1, 0 = 거의 identity**라 입력을 크게 바꾸지 않는다

즉, **초기화 + BN 두 조합 덕분에 깊은 네트워크도 처음부터 거의 ‘변형 없는’ 상태로 시작한다.**