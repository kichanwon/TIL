---
draft: true
---
early stopping

---
## 1. Normalization
**목표**: 특성의 **값 범위를 일정 구간으로 재조정**  
**방식**: 최소–최대 기반 스케일 조정
- 대표식:
$$    x' = \frac{x - \min(x)}{\max(x) - \min(x)}$$
- 결과 범위: 보통 \[0, 1] 또는 \[-1, 1]
- 의미: 데이터의 **절대 크기(scale)** 만 바꿀 뿐, **분포(shape)** 는 유지
**효과층**: 모델의 입력 규모를 균일하게 맞춰 **계산 안정성 확보**
## 2. Standardization
**목표**: 특성을 **평균 0, 분산 1**을 갖는 정규화된 축으로 변환  
**방식**: z-score 변환
- 대표식:
$$x' = \frac{x - \mu}{\sigma}$$
- 결과 분포: 평균 0, 표준편차 1
    
- 의미: 데이터의 **분포 중심과 폭을 재정렬**하여 통계적 기준축(Standard Normal Space)으로 이동
    

**효과층**: 특성이 서로 다른 단위·스케일을 갖는 경우 **최적화 기울기 안정화**

---

## 3. Regularization

**목표**: 모델의 **과적합 제어**, 파라미터 복잡도 억제  
**방식**: 손실함수에 **패널티 항 추가**

- L1:
$$\lambda \sum |w|$$
    가중치 희소성 유도
- L2:
$$\lambda \sum w^2$$    
    큰 가중치 억제
- 의미: 입력 스케일 조정이 아니라 **모델 파라미터 공간에 제약**을 가함
**효과층**: 모델의 일반화 능력 향상

- **Normalization**: 데이터 **범위** 조정
- **Standardization**: 데이터 **분포** 조정
- **Regularization**: 모델 **파라미터** 조정

---

# geeknews 1
- 2025년은 **대형 언어 모델(LLM)** 과 **에이전트 프레임워크**가 폭발적으로 성장한 해로, Python 생태계 전반의 혁신이 가속화됨
- LLM 중심 흐름 속에서도 **일반 개발용**과 **AI/ML/Data 분야**를 균형 있게 다룬 **Top 10 라이브러리 목록**을 선정
- Rust 기반의 초고속 **타입 체커 ‘ty’** , 코드 복잡도 분석 도구 **complexipy**, 문서 처리 프레임워크 **Kreuzberg** 등이 일반용 부문을 대표
- AI/ML 부문에서는 **MCP Python SDK**, **TOON**, **Deep Agents**, **smolagents**, **LlamaIndex Workflows** 등이 LLM 통합과 에이전트 개발 혁신을 주도
- 이 리스트는 Python이 여전히 **데이터 처리·성능·개발자 경험** 전반에서 진화 중임을 보여주는 지표

---

## 개요

- Tryolabs는 매년 Python 생태계의 주요 라이브러리를 선정하며, 이번이 **11번째 연례 리스트**임
- 2025년은 LLM과 에이전트 관련 도구가 급증했으나, 선정팀은 **LLM 편중을 피하고 Python의 폭넓은 발전**을 반영함
- 결과적으로 **일반 개발용 10선**, **AI/ML/Data 10선**, **러너업(Runners-up)** , **롱테일(Long tail)** 카테고리로 구성됨

## 일반용 Top 10 라이브러리

- **ty** — Rust로 작성된 초고속 Python 타입 체커
    
    - 프로젝트 구조 자동 인식, `.venv` 탐지, `pyproject.toml` 지원
    - **Salsa 기반 함수 단위 증분 분석**으로 IDE 반응성 향상
    - Astral 팀의 **Ruff**, **uv**에 이은 툴링 현대화 시도
- **complexipy** — 코드의 **인지 복잡도(cognitive complexity)** 측정 도구
    
    - SonarSource 연구 기반으로, 인간이 이해하기 어려운 구조를 수치화
    - Rust 구현으로 대규모 코드베이스도 빠르게 분석
    - CLI, Python API, VS Code 확장, CI/CD 통합 지원
- **Kreuzberg** — 다언어 문서 인텔리전스 프레임워크
    
    - PDF, Office, 이미지, HTML 등 **50여 파일 포맷** 지원
    - Python·TypeScript·Go 등 언어 바인딩 제공
    - CLI, REST API, Docker, MCP 서버 등 다양한 배포 형태
- **throttled-py** — **5가지 알고리듬**(Fixed/Sliding Window, Token/Leaky Bucket, GCRA) 기반 요청 속도 제어
    
    - 메모리·Redis 스토리지 지원, 동기/비동기 코드 모두 호환
    - **2.5~4.5배 빠른 성능**과 간결한 설정 구조 제공
- **httptap** — HTTP 요청의 **세부 타이밍 분석 및 시각화**
    
    - DNS, TCP, TLS, 서버 대기, 응답 전송 단계별 측정
    - 터미널 워터폴 뷰, JSON/metrics 출력, 리디렉션 추적 지원
- **fastapi-guard** — FastAPI용 **보안 미들웨어 통합 솔루션**
    
    - IP 화이트/블랙리스트, 속도 제한, XSS·SQLi 탐지, 지리적 필터링
    - Redis 통합으로 분산 환경 지원, OWASP 헤더 자동 설정
- **modshim** — **모듈 오버레이 방식**으로 기존 라이브러리 확장
    
    - 소스 수정 없이 기능 추가 가능, **monkey-patching 대안**
    - import 시스템 후킹으로 가상 병합 모듈 생성
- **Spec Kit** — GitHub의 **명세 기반 개발(Spec-Driven Development)** 도구
    
    - 명세를 실행 가능한 청사진으로 변환, AI 에이전트가 구현 수행
    - Copilot, Claude Code 등 다양한 AI 도구와 호환
- **skylos** — **죽은 코드 탐지 및 보안 취약점 분석** 도구
    
    - 사용되지 않는 함수·클래스·임포트 탐지, SQLi 등 위험 패턴 검사
    - **신뢰도 점수(0–100)** 기반 결과 제공, VS Code·CI/CD 통합
- **FastOpenAPI** — **모든 웹 프레임워크에서 OpenAPI 문서 자동 생성**
    
    - Flask, Django, Tornado 등 8개 프레임워크 지원
    - FastAPI 스타일의 데코레이터 라우팅과 Pydantic v2 검증 제공

## AI/ML/Data Top 10 라이브러리

- **MCP Python SDK & FastMCP** — LLM을 외부 데이터와 연결하는 **Model Context Protocol** 구현
    
    - Anthropic 공식 SDK와 Prefect의 FastMCP 2.0이 상호 보완
    - OAuth 2.1, 엔터프라이즈 인증, OpenAPI/FastAPI 통합 지원
- **TOON (Token-Oriented Object Notation)** — **LLM용 압축 JSON 대체 포맷**
    
    - YAML식 들여쓰기와 CSV형 배열 구조로 **40~60% 토큰 절감**
    - JSON과 완전 호환, 다언어 구현 진행 중
- **Deep Agents** — LangChain 기반 **장기 작업형 LLM 에이전트 프레임워크**
    
    - 계획 수립, 파일시스템 접근, **서브에이전트 위임** 기능 내장
    - LangGraph 통합으로 스트리밍·지속 메모리 지원
- **smolagents** — Hugging Face의 **경량 코드 실행형 에이전트 프레임워크**
    
    - 약 1,000줄 규모의 단순 구조, Python 코드로 행동 실행
    - **E2B·Docker·WASM 샌드박스** 등 안전 실행 환경 제공
- **LlamaIndex Workflows** — **이벤트 기반 AI 워크플로우 프레임워크**
    
    - `@step`과 `Event`로 구성된 비동기 구조, 병렬 실행 지원
    - Context 객체로 상태 관리 및 체크포인트 복원 가능
- **Batchata** — OpenAI·Anthropic·Gemini용 **통합 배치 처리 API**
    
    - 비용 제한, 재시도, 중단 복구, **Pydantic 기반 구조화 출력** 지원
- **MarkItDown** — Microsoft의 **문서→Markdown 변환기**
    
    - PDF, Word, PPT, Excel, 이미지, 오디오 등 다수 포맷 지원
    - LLM 친화적 구조 유지, Azure Document Intelligence 통합
- **Data Formulator** — Microsoft Research의 **AI 기반 데이터 시각화 도구**
    
    - 시각적 인터페이스와 자연어 결합, **자동 데이터 변환 코드 생성**
    - Vega-Lite 기반 시각화, pandas/SQL 코드 투명 공개
- **LangExtract** — Google의 **정확한 텍스트 구조 추출 라이브러리**
    
    - **원문 문자 위치 매핑**으로 추출 근거 시각화
    - Gemini·OpenAI·Ollama 등 다수 모델 지원, 병렬 처리 최적화
- **GeoAI** — OpenGeos의 **AI-지리정보 통합 분석 프레임워크**
    
    - PyTorch·Transformers·Leafmap 통합, **위성 이미지 학습·시각화** 지원
    - 토지 피복 분류, 변화 탐지 등 주요 지리 분석 작업 간소화

## Runners-up 주요 예시

- **AuthTuna** — 비동기 Python용 인증·인가 프레임워크
- **FastRTC** — Python 함수를 실시간 오디오·비디오 스트림으로 변환
- **hexora** — 악성 코드 패턴 탐지용 정적 분석 도구
- **opentemplate** — 최신 개발·보안·CI/CD 설정을 포함한 프로젝트 템플릿
- **Pyrefly** — Meta의 Rust 기반 고성능 타입 체커

## Long Tail 개요

- 수백 개의 **틈새 라이브러리**를 분야별로 정리
- AI 에이전트, 비동기 처리, 데이터 파이프라인, 웹 개발, 테스트 등 세분화
- Python 생태계의 **폭넓은 실험과 세대 교체 흐름**을 보여줌

## 결론

- 2025년 Python 생태계는 **Rust 기반 성능 향상**, **LLM 통합**, **에이전트 자동화**, **보안·유지보수성 강화**가 핵심 트렌드로 부상
- Tryolabs의 리스트는 Python이 여전히 **AI 혁신과 범용 개발의 교차점**에 있음을 입증함


---

# KUding 기술 스택 요약

| 구분            | 기술                      |
| ------------- | ----------------------- |
| Frontend      | React 18                |
| Frontend 언어   | TypeScript              |
| 서버 상태 관리      | TanStack Query v5       |
| 로컬 상태 관리      | Zustand                 |
| 라우팅           | React Router v6         |
| UI 라이브러리      | MUI v6 또는 Ant Design v5 |
| 빌드 도구         | Vite v5                 |
| Backend 런타임   | Node.js                 |
| Backend 프레임워크 | NestJS (라이트 사용)         |
| ORM           | Prisma                  |
| Database      | MySQL 또는 PostgreSQL     |
| 인증            | JWT                     |
| 권한 관리         | Role-based Guard        |
| 채점 서버         | Python + Docker         |
| 배포            | Docker / Docker Compose |
| 리버스 프록시       | Nginx                   |

1단계: Backend 기본 설정
├─ NestJS 프로젝트 초기화
├─ Prisma 설정 + 스키마 작성
├─ Docker Compose로 개발 DB 구성
└─ 기본 모듈 구조 생성

2단계: 핵심 기능 구현
├─ 인증 (JWT)
├─ 시험 CRUD
└─ 문제 관리

3단계: Frontend 설정 및 연동
├─ Vite + React 초기화
└─ API 연동


```bash
cd back

# NestJS CLI 설치 (전역)
npm i -g @nestjs/cli

# NestJS 프로젝트 초기화 (현재 폴더에)
nest new . --package-manager npm

# Prisma 및 필요한 패키지 설치
npm install @prisma/client
npm install -D prisma

# JWT 관련 패키지
npm install @nestjs/jwt @nestjs/passport passport passport-jwt
npm install -D @types/passport-jwt

# 설정 관련
npm install @nestjs/config

# Validation
npm install class-validator class-transformer

# Prisma 초기화
npx prisma init
```