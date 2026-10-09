## Index
1. [[01. Main and Arguments|Main and Arguments]]
2. [[02. Transformer Structures|Transformer Structures]]
3. [[03. Checkpoint Loading|Checkpoint Loading]]
4. [[04. Memory Management|Memory Management]]
5. [[05. Tokenizer|Tokenizer]]
6. [[06. Sampler|Sampler]]
7. [[07. Text Generation|Text Generation]]
8. [[08. Transformer Forward Pass|Transformer Forward Pass]]
9. [[09. Output and Decoding|Output and Decoding]]
10. [[10. Cleanup|Cleanup]]

## Execution Flow
```text
main()
  │
  ├── Parse Arguments
  │
  ├── build_transformer()
  │     ├── read_checkpoint()
  │     │     └── memory_map_weights()
  │     └── malloc_run_state()
  │
  ├── build_tokenizer()
  │
  ├── build_sampler()
  │
  ├── generate() / chat()
  │     ├── encode()
  │     ├── forward()
  │     ├── sample()
  │     └── decode()
  │
  ├── free_sampler()
  ├── free_tokenizer()
  └── free_transformer()
```