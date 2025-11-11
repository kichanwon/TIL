---
creation_date: 2025-11-10
draft: true
---
# 📁 content/Study/Concepts/Network Index
```dataview
list from "content/Study/Concepts/Network"
where file.name != this.file.name
sort file.name asc
```
기본형
 ├─ FNN (기초)
 ├─ CNN (공간 데이터)
 ├─ RNN ──┬─ LSTM
 │         └─ GRU
 ├─ Autoencoder ──┬─ VAE
 │                 └─ AE 변형
 ├─ GAN (생성형)
 ├─ Transformer (Attention 기반)
 └─ GNN (그래프 기반)

---
- **FNN**: 일반 데이터
- **CNN**: 이미지(공간 구조)
- **RNN/LSTM**: 텍스트·음성(순차 구조)
- **Transformer**: 모든 시퀀스 데이터 (현재 주류)
- **GAN/VAE**: 데이터 생성
- **GNN**: 관계 중심 데이터
