---
draft: true
---
early stopping

normalization
regulization

---
### **1. Introduction**
### **2. Related Work**

### **3. Deep Residual Learning**
- **3.1. Residual Learning**
- **3.2. Identity Mapping by Shortcuts**
- **3.3. Network Architectures**
- **3.4. Implementation**

### **4. Experiments**
- **4.1. ImageNet Classification**
- **4.2. CIFAR-10 and Analysis**
- **4.3. Object Detection on PASCAL and MS COCO**

## 1. Introduction
- 매우 깊은 네트워크
- Degradation problem
- Vanishing / Exploding gradients
- Normalized initialization
- Intermediate normalization (BatchNorm)
- Optimization difficulty
- Residual learning 필요성
## 2. Related Work
- Residual representations (VLAD, Fisher Vector)
- Multigrid / PDE residual solution
- Preconditioning
- Shortcut connections (early MLP shortcuts, auxiliary classifiers)
- Centering responses (skip connections)
- Inception shortcuts
- Highway networks (gated shortcuts vs identity shortcuts)
## 3. Deep Residual Learning
### 3.1 Residual Learning
- Underlying mapping H(x)H(x)H(x)
- Residual mapping F(x)=H(x)−xF(x) = H(x) - xF(x)=H(x)−x
- Identity mapping
- Optimization ease
- Solvers difficulty in fitting identity
- Preconditioning effect
- Small residual responses
### 3.2 Residual Network Architecture
- Basic block (2-layer)
- Shortcut connection
- Identity shortcut
- Projection shortcut (1×1 conv)
- Dimension matching
- Parameter-free shortcuts
- Element-wise addition
### 3.3 Network Architectures
- Plain network (VGG-style)
- 3×3 convolution rules
- Downsampling via stride
- Global average pooling
- FLOPs 비교 (VGG vs plain)
- Residual network counterpart
- Shortcut option A (zero-padding)
- Shortcut option B (projection)
- Shortcut option C (full projection)
- Convergence speed
- Degradation vs residual success
### 3.4 Implementation
- Data augmentation
- BatchNorm after conv
- He initialization
- SGD setup
- Learning rate schedule
- No dropout
- Multi-scale testing
- Fully convolutional inference
## 4. Experiments
### 4.1 ImageNet Classification
- 18-layer / 34-layer plain nets
- 18-layer / 34-layer ResNets
- Degradation in plain nets
- Residual learning solves degradation
- Training/validation error comparison
- Top-1 / Top-5 error
- Identity vs projection shortcuts
- Convergence improvement
- Residual effectiveness

