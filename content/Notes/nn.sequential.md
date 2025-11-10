### sequential 장단점
## **정리 표**

| 구분                   | `직접 정의` 방식     | `nn.Sequential` 방식 |
| -------------------- | -------------- | ------------------ |
| 코드 가독성               | 중간 연산 많아 복잡    | 깔끔하고 구조적           |
| forward 수정           | 자유로움           | 제약 있음              |
| Residual / 병렬 구조     | 구현 가능          | 어렵거나 불가            |
| feature extractor 분리 | 약간 번거로움        | 매우 쉬움              |
| transfer learning    | 수동 지정          | 인덱싱으로 간단           |
| 디버깅 용이성              | layer별 확인 쉬움   | 인덱스 접근 필요          |
| 유지보수                 | 레이어 추가 시 수정 많음 | 구조 확장 쉬움           |

| 상황                              | 추천 방식                    |
| ------------------------------- | ------------------------ |
| **단순 CNN (CIFAR-10처럼 직선 구조)**   | nn.Sequential (깔끔하고 효율적) |
| **Residual, Skip, 병렬 등 복잡한 모델** | 직접 정의 (유연함)              |

---
#### sequential 적용
~~~ python
import torch
import torch.nn as nn

class SimpleCNN_Sequential(nn.Module):
    def __init__(self, num_classes=10, activation='relu', dropout_p=0.5):
        super(SimpleCNN_Sequential, self).__init__()
        
        # 활성화 함수 선택
        if activation == 'relu':
            act = nn.ReLU()
        elif activation == 'tanh':
            act = nn.Tanh()
        elif activation == 'sigmoid':
            act = nn.Sigmoid()
        else:
            raise ValueError("activation must be 'relu', 'tanh', or 'sigmoid'")
        
        # Feature extractor
        self.features = nn.Sequential(
            nn.Conv2d(3, 32, 3, padding=1),
            nn.BatchNorm2d(32),
            act,

            nn.Conv2d(32, 64, 3, padding=1),
            nn.BatchNorm2d(64),
            act,
            nn.MaxPool2d(2, 2),

            nn.Conv2d(64, 128, 3, padding=1),
            nn.BatchNorm2d(128),
            act,

            nn.Conv2d(128, 128, 3, padding=1),
            nn.BatchNorm2d(128),
            act,
            nn.MaxPool2d(2, 2),

            nn.Conv2d(128, 256, 3, padding=1),
            nn.BatchNorm2d(256),
            act,

            nn.Conv2d(256, 256, 3, padding=1),
            nn.BatchNorm2d(256),
            act,
            nn.MaxPool2d(2, 2)
        )

        # Classifier
        self.classifier = nn.Sequential(
            nn.Flatten(),
            nn.Linear(256 * 4 * 4, 256),
            act,
            nn.Dropout(dropout_p),
            nn.Linear(256, num_classes)
        )

    def forward(self, x):
        x = self.features(x)
        x = self.classifier(x)
        return x
~~~

#### sequential 미적용
~~~ python
import torch
import torch.nn as nn

class SimpleCNN_Manual(nn.Module):
    def __init__(self, num_classes=10, activation='relu', dropout_p=0.5):
        super(SimpleCNN_Manual, self).__init__()
        
        # 활성화 함수 선택
        if activation == 'relu':
            self.act = nn.ReLU()
        elif activation == 'tanh':
            self.act = nn.Tanh()
        elif activation == 'sigmoid':
            self.act = nn.Sigmoid()
        else:
            raise ValueError("activation must be 'relu', 'tanh', or 'sigmoid'")

        # Conv 블록 정의
        self.conv1 = nn.Conv2d(3, 32, 3, padding=1)
        self.bn1 = nn.BatchNorm2d(32)

        self.conv2 = nn.Conv2d(32, 64, 3, padding=1)
        self.bn2 = nn.BatchNorm2d(64)

        self.conv3 = nn.Conv2d(64, 128, 3, padding=1)
        self.bn3 = nn.BatchNorm2d(128)

        self.conv4 = nn.Conv2d(128, 128, 3, padding=1)
        self.bn4 = nn.BatchNorm2d(128)

        self.conv5 = nn.Conv2d(128, 256, 3, padding=1)
        self.bn5 = nn.BatchNorm2d(256)

        self.conv6 = nn.Conv2d(256, 256, 3, padding=1)
        self.bn6 = nn.BatchNorm2d(256)

        self.pool = nn.MaxPool2d(2, 2)
        self.dropout = nn.Dropout(dropout_p)
        self.fc1 = nn.Linear(256 * 4 * 4, 256)
        self.fc2 = nn.Linear(256, num_classes)

    def forward(self, x):
        # Block 1~2
        x = self.act(self.bn1(self.conv1(x)))
        x = self.act(self.bn2(self.conv2(x)))
        x = self.pool(x)  # 32 → 16

        # Block 3~4
        x = self.act(self.bn3(self.conv3(x)))
        x = self.act(self.bn4(self.conv4(x)))
        x = self.pool(x)  # 16 → 8

        # Block 5~6
        x = self.act(self.bn5(self.conv5(x)))
        x = self.act(self.bn6(self.conv6(x)))
        x = self.pool(x)  # 8 → 4

        # FC
        x = x.view(x.size(0), -1)
        x = self.act(self.fc1(x))
        x = self.dropout(x)
        x = self.fc2(x)
        return x
~~~
