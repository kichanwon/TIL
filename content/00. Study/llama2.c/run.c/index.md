- _**run.c**_: Llama 2의 Transformer 추론(Inference)을 C 언어로 구현한 프로그램
- _**주요 기능**_: 모델 초기화, 토큰화, Transformer 연산, 텍스트 생성 및 메모리 관리
- _**실행 방식**_: 학습된 가중치를 불러와 다음 토큰을 반복적으로 예측하여 텍스트 생성
## Index
1. [[01. Main and Arguments|Main and Arguments]]
    - 프로그램 진입점 `main()` 및 명령행 인자 처리        
2. [[02. Transformer Structures|Transformer Structures]]
    - 모델 설정(`Config`), 가중치(`Weights`), 추론 상태(`RunState`) 구조 정의
3. [[03. Checkpoint Loading|Checkpoint Loading]]
    - 모델 체크포인트 읽기 및 가중치 메모리 매핑
4. [[04. Memory Management|Memory Management]]
    - 추론용 메모리 할당 및 모델 초기화
5. [[05. Tokenizer|Tokenizer]]
    - 입력 텍스트를 Token ID로 변환 및 토크나이저 관리
6. [[06. Sampler|Sampler]]
    - Logits를 기반으로 다음 토큰 선택
7. [[07. Text Generation|Text Generation]]
    - `generate()` / `chat()`을 통한 토큰 생성 반복 제어
8. [[08. Transformer Forward Pass|Transformer Forward Pass]]
    - Embedding, Attention, FFN 등 Transformer 순전파 연산
9. [[09. Output and Decoding|Output and Decoding]]
    - 생성된 Token ID를 텍스트로 변환 및 출력
10. [[10. Cleanup|Cleanup]]
    - 동적 메모리 해제 및 파일 자원 정리

## Execution Flow
```text
main()
  │
  ├── Parse Arguments                  // 실행 옵션 설정
  │
  ├── Model Initialization             // 모델 초기화
  │     └── build_transformer()
  │           ├── read_checkpoint()    // 모델 파일 읽기
  │           │     └── memory_map_weights()
  │           └── malloc_run_state()   // 추론용 메모리 할당
  │
  ├── Tokenizer / Sampler Initialization
  │     ├── build_tokenizer()          // 토크나이저 초기화
  │     └── build_sampler()            // 샘플러 초기화
  │
  ├── Text Generation                  // 텍스트 생성
  │     └── generate() / chat()
  │           ├── encode()             // 텍스트 → Token ID
  │           │
  │           └── Generation Loop      // 토큰 생성 반복
  │                 ├── forward()      // Transformer 추론
  │                 ├── sample()       // 다음 토큰 선택
  │                 └── decode()       // Token ID → 텍스트
  │
  └── Memory Cleanup                   // 자원 해제
        ├── free_sampler()
        ├── free_tokenizer()
        └── free_transformer()
```