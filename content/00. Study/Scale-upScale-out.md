---
aliases:
  - Scale-up/Scale-out
---
- Scale-up
	- 한 서버 내부
	- e.g. GPU 간 데이터 교환
		- PCIe P2P
		- NVLink
			- NVSwitch
			- AMD Infinity Fabric
- Scale-out
	- 여러 서버 간
	- e.g. 서버 간 고속 데이터 전송
		- InfiniBand
		- Ethernet (TCP/IP)
		- RoCE

| 기술                  | 연결 범위            | 주요 특징                          |
| ------------------- | ---------------- | ------------------------------ |
| PCIe P2P            | 서버 내부            | GPU 간 직접 데이터 전송. NVLink 없이도 가능 |
| NVLink              | 서버 내부 중심         | NVIDIA GPU 간 고대역폭·저지연 연결       |
| NVSwitch            | 서버 내부 및 지원 시스템 간 | 다수 GPU를 NVLink 패브릭으로 연결        |
| AMD Infinity Fabric | AMD GPU 내부·GPU 간 | AMD의 고속 인터커넥트                  |
| InfiniBand          | 서버 간             | RDMA 지원, HPC·대규모 AI 클러스터       |
| Ethernet (TCP/IP)   | 서버 간             | 범용 네트워크, 구축 용이, CPU 처리 부담 가능   |
| RoCE                | 서버 간             | Ethernet 환경에서 RDMA 지원          |

NVLink/NVSwitch의 연결 범위는 세대와 시스템 구성에 따라 달라짐, 
일부 구성은 서버 간 NVLink 연결도 지원.

# parallelization
| 구분        | 기술                                 | 역할                                  |
| --------- | ---------------------------------- | ----------------------------------- |
| 물리적 연결    | PCIe, NVLink, InfiniBand, Ethernet | 데이터를 이동시키는 경로                       |
| 메모리 접근·전송 | P2P, DMA, RDMA, GPUDirect RDMA     | 메모리 사이 데이터를 접근·전송하는 방식              |
| 통신 라이브러리  | NCCL, RCCL, MPI                    | All-Reduce, Broadcast 등 병렬 통신 연산 수행 |
