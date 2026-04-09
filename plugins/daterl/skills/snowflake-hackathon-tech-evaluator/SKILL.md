---
name: snowflake-hackathon-tech-evaluator
description: >
  Use when scoring a Snowflake TECH TRACK hackathon submission before final review,
  identifying technical gaps against the official rubric, or reviewing a teammate's project quality.
  Triggers: "심사 기준 평가", "해커톤 점수 추정", "Snowflake 프로젝트 자가 평가",
  "hackathon submission review", "기술 평가", "갭 분석", "제출 전 점검", "심사표",
  "technical rubric", "제출 준비", "점수 추정".
---

# Snowflake Hackathon Technical Evaluator

## Overview

Snowflake TECH TRACK 심사표의 **기술 4개 카테고리(총 75점)** 기준으로 프로젝트를 자가 평가한다.
산출물은 **증거 기반 점수 + 갭 분석 + 즉각 액션**이다 — 막연한 낙관이나 의견이 아닌 파일·코드 인용.

> "An entry with zero novelty reinvents the wheel and does nothing new." — TAIKAI Hackathon Judging
> "A single query can consume thousands of credits without warning." — Seemore Data, Snowflake Cortex Cost Analysis

## When to Use

- 제출 **전** 자가 평가 및 수정 우선순위 파악
- 팀원 간 기술 수준 공유 및 리뷰
- 심사위원 관점에서 갭을 미리 파악할 때

**범위에서 제외**: 발표/스토리텔링(별도 루브릭), 코드 수정, 데모 스크립트 작성

## Quick Reference — 12개 심사 항목

| ID | 카테고리 | 항목 | 만점 |
|----|---------|------|------|
| C1 | 창의성 25% | 기존 접근법 대비 차별화된 문제 정의·솔루션 | 8점 |
| C2 | 창의성 25% | 문제 배경의 타당성 | 9점 |
| C3 | 창의성 25% | 새로운 아이디어 또는 기존 대비 개선점 | 8점 |
| S1 | Snowflake 전문성 25% | 플랫폼 기능 적절 활용 및 최적화 (단순 이용 ≠ 만점) | 9점 |
| S2 | Snowflake 전문성 25% | 데이터 자산 기반 혁신적 활용 방식 | 8점 |
| S3 | Snowflake 전문성 25% | Snowflake로만 가능한 해결책인가 | 8점 |
| A1 | AI 전문성 25% | Cortex를 적절한 용도로 활용 | 9점 |
| A2 | AI 전문성 25% | AI를 단순 기능이 아닌 가치 창출 구조로 활용 | 8점 |
| A3 | AI 전문성 25% | AI 모델 발전 속도에 맞춘 확장성 | 8점 |
| R1 | 현실성 15% | 구현 완성도 | 5점 |
| R2 | 현실성 15% | 자원·비용 합리성 및 지속 가능성 | 5점 |
| R3 | 현실성 15% | 문제 해결의 논리성·구현 가능성 | 5점 |

---

## 실행 절차

### Phase 1: 프로젝트 파일 수집

다음 파일을 순서대로 읽는다. 없으면 "파일 없음(감점 반영)" 처리 후 계속.

```
필수:
- CLAUDE.md / AGENTS.md / README.md   (프로젝트 맥락)
- 메인 앱 파일 (app.py, main.py 등)   (구현 현황)
- SQL 스키마 파일 (sql/schema/*.sql)   (Snowflake 기능 사용 범위)
- Semantic Model (*.yaml, *.yml)       (Cortex Analyst 설계)
- AI/Agent 설정 (cortex_agent.sql 등)  (Cortex Agent 구성)

가능하면:
- docs/*.md                            (문제 배경, 데이터 전략, 비용 계획)
- environment.yml / requirements.txt   (의존성 및 플랫폼 확인)
```

### Phase 2: 항목별 증거 기반 평가

**반드시 지킬 것:**
- 점수 산정 시 `파일명:라인번호` 또는 `파일명:함수명` 형식으로 증거 인용
- "잘 구현됨"처럼 모호한 표현 금지 → "app.py:309 — chat_input 구현, Cortex Search 미연동으로 A1에서 -2점"처럼 구체화
- 의심스러우면 점수를 보수적으로 산정 (실제 심사는 항상 예상보다 보수적)

---

## 카테고리별 평가 기준

### 창의성 25점 (C1 · C2 · C3)

**C1 — 차별화된 문제 정의·솔루션 (8점)**

| 점수 | 기준 |
|------|------|
| 8 | 기존 서비스와 명확한 차별점이 문서화됨. "기존 솔루션 X의 Y 한계를 Z로 해결" 형식으로 서술됨 |
| 6 | 참신한 아이디어지만 기존 서비스와의 차별점이 암묵적 (명시 없음) |
| 4 | 알려진 솔루션을 새 데이터/도메인에 적용 (novelty 낮음) |
| 2 | 기존 도구를 조합한 수준. 바퀴 재발명 |
| 0 | 차별점 없음 |

**판단 기준 (CHI 2025, TAIKAI):**
- "이 문제를 기존 서비스(구글 지도, 직방, 카카오맵 등)로 해결할 수 없는 이유"가 명시되어 있는가?
- Novelty(새로움) + Usefulness(유용성) 이중 기준 충족 여부
- 기존 접근법 대비 독창적 방법으로 알려진 문제를 해결해도 C1 만점 가능

> Reference: ["A truly novel idea surprises the listener and reveals a new way of thinking. An entry with zero novelty reinvents the wheel."](https://taikai.network/en/blog/hackathon-judging) — TAIKAI

**C2 — 문제 배경의 타당성 (9점)**

| 점수 | 기준 |
|------|------|
| 9 | 배경 + 정량 데이터(사용자 규모, 문제 빈도 등) + 이해관계자 니즈가 모두 서술됨 |
| 7 | 배경은 명확하지만 정량 근거 없음 |
| 5 | 배경이 주관적이거나 너무 광범위 |
| 2 | 문제 정의가 솔루션 중심 ("AI 챗봇을 만들고 싶어서") |
| 0 | 배경 서술 없음 |

**판단 기준:**
- "Challenge must describe the problem, not the solution" 원칙 준수 여부
- 문제가 너무 좁으면 창의성 억제, 너무 넓으면 솔루션이 피상적 → 적절한 범위

> Reference: [MLH Judging Plan](https://guide.mlh.io/general-information/judging-and-submissions/judging-plan) — Innovation/Creativity 25%, Problem Background 별도

**C3 — 새로운 아이디어 또는 개선점 (8점)**

| 점수 | 기준 |
|------|------|
| 8 | 업계 최초 또는 기존 대비 측정 가능한 개선(속도, 비용, 정확도 등) 제시 |
| 6 | 기존 방법보다 나은 접근이지만 정량 비교 없음 |
| 4 | 아이디어는 있으나 기존 대비 차별점이 불분명 |
| 2 | 영감은 있으나 실질 개선 없음 |

**판단 기준:**
- "Does the entry solve an unoriginal problem in an original way?" (TAIKAI)
- 새 데이터 도메인에 기존 기술 적용 = 부분 점수
- 기존 논문·서비스를 명시적으로 인용하고 개선점을 제시 = 만점 근접

---

### Snowflake 전문성 25점 (S1 · S2 · S3)

**S1 — 플랫폼 기능 적절 활용 및 최적화 (9점)**

| 점수 | 기준 |
|------|------|
| 9 | Dynamic Tables + AISQL 함수 내장, 또는 Streams/Tasks를 용도에 맞게 정확히 선택. `CREATE AGENT`로 Agent DDL 정의 |
| 7 | 다양한 Snowflake 기능 사용 (6개 이상), 대부분 적절히 활용 |
| 5 | 기능 3-5개, 단순 SQL SELECT + 기본 함수 위주 |
| 2 | Snowflake를 외부 DB처럼 사용 (로드만 하고 처리는 외부) |
| 0 | Snowflake 기능 미활용 또는 파일 없음 |

**평가 체크포인트:**
- Dynamic Tables vs Streams/Tasks 선택이 용도에 맞는가 (자동 incremental refresh 필요 → Dynamic Tables)
- Warehouse 크기가 작업에 최적화되었는가 (`XSMALL`~`MEDIUM`; AISQL에는 MEDIUM 이하 권장)
- SQL 파일이 기능별로 체계화되었는가 (파이프라인 순서 추적 가능)

> Reference: [Snowflake Dynamic Tables Complete Guide](https://dataengineerhub.blog/articles/snowflake-dynamic-tables-complete-guide-2025) — Dynamic Tables: 200,000+ in production, 2,900+ customers

**S2 — 데이터 자산 혁신적 활용 (8점)**

| 점수 | 기준 |
|------|------|
| 8 | Marketplace 데이터를 AISQL 파이프라인에 통합하거나 cross-domain JOIN으로 새 인사이트 창출 |
| 6 | 외부 데이터셋 활용하지만 단순 조회 수준 |
| 4 | Snowflake 내부 데이터만 사용, Marketplace 미활용 |
| 2 | 데이터 소스 1개, 분석 없음 |

**혁신적 활용 vs 단순 활용 구분 (Q2 연구 결과):**

| 단순 활용 | 혁신적 활용 |
|----------|------------|
| Marketplace에서 데이터 구독·조회만 | 여러 Marketplace 데이터를 JOIN하여 기존에 없던 지표 생성 |
| 단일 데이터 소스 분석 | Cross-domain: 상권+인구+부동산 등 다차원 결합 |
| 수동 ETL | Dynamic Tables + AISQL 함수 내장으로 선언적 파이프라인 |
| 외부 ML 플랫폼으로 데이터 이동 | Snowflake 내에서 학습-추론-서빙 완결 |

**S3 — Snowflake로만 가능한 해결책 (8점)**

| 점수 | 기준 |
|------|------|
| 8 | Zero-copy sharing, Cortex AI, SiS, ML FORECAST 등 Snowflake 전용 기능이 핵심 역할 |
| 6 | Snowflake가 중요하지만 다른 플랫폼으로도 구현 가능한 부분이 있음 |
| 4 | Snowflake는 저장소 역할만, 실제 처리는 외부 |
| 2 | Snowflake 대신 다른 플랫폼 사용 시 동일 결과 |

**"Snowflake 필수" 판별 기준 (Q2 연구 결과):**
- AISQL 함수(`AI_CLASSIFY`, `AI_SENTIMENT`, `AI_COMPLETE`)가 SQL 파이프라인에 내장됨 → 필수
- Cortex Agent가 Search + Analyst를 실제 오케스트레이션함 → 필수
- Streamlit in Snowflake(SiS)로 배포 + 데이터가 플랫폼 밖으로 나가지 않음 → 필수
- 외부 Python 스크립트로 동일 결과 가능 → 감점

> Reference: [Snowflake Doubles Down on Developers](https://www.snowflake.com/en/news/press-releases/snowflake-doubles-down-on-developers-with-end-to-end-capabilities-for-building-enterprise-grade-pipelines-models-and-ai-powered-apps/)

---

### AI 전문성 25점 (A1 · A2 · A3)

**A1 — Cortex를 적절한 용도로 활용 (9점)**

각 Cortex 기능의 "적절한 용도" 기준:

| 기능 | 적절한 용도 | 부적절한 용도 (감점) |
|------|-----------|-------------------|
| AI_COMPLETE | 자유형 생성, 요약, 분류, 질의응답 | 단순 문자열 concatenation 대체 |
| AI_CLASSIFY | 사전 정의 카테고리 분류 (지원 티켓, 콘텐츠 분류) | binary TRUE/FALSE만 필요한 경우 |
| AI_SENTIMENT | 측면별(aspect-based) 감성 분석 | 단순 긍/부정 분류 (0.92 정확도 낭비) |
| Cortex Search | 비정형 텍스트 하이브리드(벡터+키워드) 검색, RAG | 단순 SQL LIKE 검색 대체 |
| Cortex Analyst | NL2SQL (자연어 → SQL), semantic model 필수 | YAML 없는 직접 SQL 생성 |
| Cortex Agent | Search + Analyst 오케스트레이션, Plan→Route→Reflect 사이클 | 단순 챗봇 (단일 기능 호출) |
| ML FORECAST | 시계열 예측 (ARIMA 기반), 다중 시리즈 | 단일 포인트 예측, 학습 데이터 1년 미만 |

**A1 점수 기준:**

| 점수 | 기준 |
|------|------|
| 9 | 6개 기능 전수 동작 + 각 기능이 적절한 용도로 사용됨 |
| 7 | 5개 이상 동작, 1-2개 용도 부적합 또는 형식적 사용 |
| 5 | 3-4개 동작, 핵심 기능(Agent, Search) 미사용 또는 미구현 |
| 2 | 1-2개만 동작 |

**Cortex Agent 구성 품질 체크 (4단계 프레임워크):**
1. **Agent Description** (50-100단어): 에이전트 목적과 사용자 페르소나 명시
2. **Tool Description**: 각 도구(Search/Analyst)의 적합 시나리오 명시
3. **Orchestration Instructions**: 도구 선택 로직 ("structured query → Analyst, unstructured text → Search"), Budget(seconds, tokens) 설정
4. **Response Instructions**: 출력 형식(마크다운/표/서사형) 지정

**추가 체크포인트:**
- `"auto"` 모델 선택 (Snowflake 권장: 최신 모델 자동 적용)
- `warehouse` 필드가 빈 문자열(`""`)이면 즉시 실행 오류 → 감점
- 다중 에이전트 패턴: Master Agent → Sub-Agent(전문화된 UDF) 계층 구조 시 가산
- **Cortex Search 증분 인덱싱**: primary key 기반 → 전체 재색인 대비 50-70% 비용 절감 (TARGET_LAG 설정 확인)

> Reference: [Best Practices for Building Cortex Agents](https://www.snowflake.com/en/developers/guides/best-practices-to-building-cortex-agents/) — Snowflake Official Guide (4-stage config framework)
> Reference: [Cortex Search — State-of-the-art hybrid search](https://www.snowflake.com/en/blog/cortex-search-ai-hybrid-search/) — Incremental indexing cost analysis
> Reference: [Cortex Agents Multi-Agent Patterns](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents-overview) — Hierarchical UDF delegation

**A2 — AI를 가치 창출 구조로 활용 (8점)**

| 점수 | 기준 |
|------|------|
| 8 | AI 함수가 Dynamic Table 내에서 선언적 파이프라인으로 실행되거나, Agent가 Search+Analyst를 오케스트레이션하여 사용자에게 새로운 가치 제공 |
| 6 | AI 결과를 저장하고 앱에서 조회하는 배치 패턴 구현 |
| 4 | AI를 직접 호출하지만 결과가 재사용되지 않음 (매 클릭마다 LLM 호출) |
| 2 | AI를 버즈워드 수준으로만 사용 |

**가치 창출 구조 예시 (Q1 연구 결과):**
- `AI_SENTIMENT` + `AI_CLASSIFY` → Dynamic Table에 enriched 컬럼으로 저장 → Agent가 이 컬럼을 쿼리 (읽기 비용 최소화)
- Cortex Analyst가 semantic model로 NL2SQL → Cortex Search가 비정형 문서 검색 → Agent가 결과 통합 응답
- "switching from Databricks to Snowflake AISQL: processing 900% faster, saved $500,000 annually" (Snowflake 고객 사례)

> Reference: [From Zero to Agents: Building End-to-End Data Pipelines](https://www.snowflake.com/en/developers/guides/from-zero-to-agents/)
> Reference: [Gen AI in Action: Cortex Customer Stories](https://www.snowflake.com/en/blog/gen-ai-cortex-customer-stories-outcomes/)

**A3 — AI 모델 확장성 (8점)**

| 점수 | 기준 |
|------|------|
| 8 | 모델 `"auto"` 설정, semantic model이 YAML로 분리되어 코드 변경 없이 확장 가능, 데이터 소스 추가 경로 문서화 |
| 6 | 일부 설계가 확장 가능하지만 모델이 하드코딩됨 |
| 4 | 전체 설계가 현재 데이터/스키마에 고정됨 |
| 2 | 확장 고려 없음, 변경 시 전면 재작성 필요 |

**확장성 체크포인트:**
- Agent 모델 설정이 `"auto"` 또는 설정 파일로 분리됨?
- Semantic model(*.yaml)이 테이블 추가 시 JOIN 설정만으로 확장 가능한 구조?
- ML FORECAST가 새 지역/시리즈 추가 시 파이프라인 재실행으로 대응 가능?
- 앱 코드에 도메인별 하드코딩(고정 구 이름, 특정 컬럼명 등)이 최소화됨?

> Reference: [Cortex Agents Configure & Interact](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents-manage) — "auto" model selection for future-proof architecture

---

### 현실성 15점 (R1 · R2 · R3)

**R1 — 구현 완성도 (5점)**

| 점수 | 기준 |
|------|------|
| 5 | 핵심 기능 전수 동작, 에러 핸들링 포함, 배포 가능 상태, E2E 테스트 시나리오 존재 |
| 4 | 핵심 기능 동작, 일부 edge case 미처리 |
| 3 | 데모 수준 동작, 하드코딩된 값 존재 |
| 2 | 부분 동작, 주요 기능 미완성 |
| 1 | 실행 불가, 프로토타입 미완성 |

**체크포인트 (Q4 연구 결과):**
- API 키/시크릿이 하드코딩되지 않았는가
- `warehouse` 필드가 빈 문자열이 아닌가 (`""` → 즉시 에러)
- 앱이 SiS에서 실제 배포·실행되는가 (코드 존재 ≠ 배포 완료)
- SQL 파일이 검증 쿼리(`SHOW`, `DESCRIBE`)를 포함하는가

**R2 — 자원·비용 합리성 및 지속 가능성 (5점)**

| 점수 | 기준 |
|------|------|
| 5 | 비용 예산 계획, 배치 패턴(AI 1회 실행→결과 저장→조회), Warehouse AUTO_SUSPEND 설정, 실제 크레딧 소비 추적 |
| 4 | 배치 패턴 구현, 비용 계획 있음 |
| 3 | `@st.cache_data` 등 기본 최적화, 비용 계획 없음 |
| 2 | 매 클릭마다 LLM 호출 (크레딧 소모 무제한) |
| 1 | 비용 고려 없음 |

**Cortex AI 비용 최적화 체크리스트 (Q4 연구 결과):**
- `AI_COUNT_TOKENS`로 사전 비용 추정 여부
- 모델 선택 근거 (mistral-7b vs mistral-large2 vs claude — 최대 40배 차이)
- `CORTEX_FUNCTIONS_USAGE_HISTORY`로 모니터링 가능한 구조인가
- Warehouse: XSMALL (AISQL 사용), AUTO_SUSPEND=60 설정

> Reference: [The Hidden Cost of Snowflake Cortex AI — $5K single-query warning](https://seemoredata.io/blog/snowflake-cortex-ai/) — Seemore Data
> Reference: [Monitoring Snowflake AI Costs](https://select.dev/posts/monitoring-snowflake-ai-costs)

**R3 — 문제 해결의 논리성·구현 가능성 (5점)**

| 점수 | 기준 |
|------|------|
| 5 | 문제→기술 선택 논리가 명확하고 문서화됨, 측정 가능한 결과 지표 존재 |
| 4 | 논리는 있으나 일부 가정이 비현실적 |
| 3 | 개념은 실현 가능하나 문제-솔루션 연결이 약함 |
| 2 | 기술 먼저 선택하고 문제를 끼워 맞춘 흔적 |
| 1 | "AI-powered" 버즈워드 중심, 실질 문제 해결 없음 |

**"논리적 해결" vs "억지 솔루션" 판별 질문 (Q4 연구 결과):**
1. 이 문제가 AI/ML 없이도 해결 가능한가? → 가능하면 AI 필요성 설명이 있어야 함
2. ML FORECAST를 쓰는 경우: 학습 데이터가 최소 1개 전체 시즌 주기를 포함하는가?
3. 선택한 기술 스택이 문제의 특성에 적합한가? (분류 → AI_CLASSIFY, 검색 → Cortex Search)
4. 결과가 정량적으로 측정 가능한가? (정확도, 속도, 비용 등)

> Reference: [Snowflake ML Forecasting Documentation](https://docs.snowflake.com/en/user-guide/ml-functions/forecasting) — min 1 full season of training data
> Reference: [Things I Learnt by Participating in GenAI Hackathons](https://towardsdatascience.com/things-i-learnt-by-participating-in-genai-hackathons-over-the-past-6-months/)

---

## 산출물 형식

### 1단계: 요약 테이블 (항상 먼저 출력)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 Snowflake Hackathon 기술 자가 평가 — {프로젝트명} / {날짜}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

카테고리              예상점수   만점   핵심 갭 (파일명 인용)
─────────────────────────────────────────────────────────
창의성                {N}점     25점   {한 줄}
Snowflake 전문성      {N}점     25점   {한 줄}
AI 전문성             {N}점     25점   {한 줄}
현실성                {N}점     15점   {한 줄}
─────────────────────────────────────────────────────────
기술 총점             {N}점     90점

🎯 Top 5 즉각 액션 (점수 손실 영향순):
1. [{점수 손실}점] {파일명:라인/함수}: {수정 내용 1문장}
2. [{점수 손실}점] ...
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### 2단계: 카테고리별 세부 분석

항목(C1~R3)별로:
- **현재 상태**: 파일·코드 증거 인용
- **심사위원 시선**: "심사위원이 이 부분을 볼 때 어떻게 보이는가"
- **만점 받으려면**: 파일·문장 수준의 구체적 보완 방법

### 3단계: 총평

- 강점 3개 (점수가 높은 이유)
- 약점 3개 (즉시 수정하면 효과적인 이유)
- 제출 전 P0(필수) vs P1(선택) 액션 분리

---

## Common Mistakes

| 실수 | 결과 | 수정 방법 |
|------|------|----------|
| Agent SQL에 `warehouse: ""` | Agent 실행 시 런타임 에러 | 실제 WH명으로 채우기 |
| Cortex Search DDL 없이 Agent에서 참조 | DONGNE_SEARCH not found 에러 | CREATE CORTEX SEARCH SERVICE DDL 추가 |
| `mistral-large2` 모델 하드코딩 | A3 확장성 감점 | `"auto"` 또는 설정 파일로 분리 |
| 탭마다 LLM 직접 호출 | R2 비용 감점 | AI 결과 테이블에 저장 → SELECT 조회 패턴 |
| ML FORECAST에 학습 데이터 3개월 | 예측 불안정 | 최소 1개 시즌(연간 패턴 = 1년) 이상 |
| 파일 존재 ≠ 배포 완료 | R1 감점 | SiS 실제 SHOW STREAMLITS 확인 |

---

## Research References

| 항목 | 출처 |
|------|------|
| 창의성 평가 (C1-C3) | [TAIKAI Judging Criteria](https://taikai.network/en/blog/hackathon-judging), [CHI 2025 Creativity](https://arxiv.org/html/2503.04290), [MLH Contest Terms](https://guide.mlh.io/general-information/judging-and-submissions/judging-plan) |
| Cortex Agent 구성 | [Best Practices for Cortex Agents](https://www.snowflake.com/en/developers/guides/best-practices-to-building-cortex-agents/), [Cortex Agents Docs](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents) |
| Cortex Search + Analyst | [Cortex Search Hybrid](https://www.snowflake.com/en/blog/cortex-search-ai-hybrid-search/), [Cortex Analyst Docs](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-analyst) |
| AISQL 함수 | [AISQL GA Release](https://docs.snowflake.com/en/release-notes/2025/other/2025-11-04-cortex-aisql-operators-ga), [AI_SENTIMENT Docs](https://docs.snowflake.com/en/sql-reference/functions/ai_sentiment) |
| Dynamic Tables | [Dynamic Tables Guide 2025](https://dataengineerhub.blog/articles/snowflake-dynamic-tables-complete-guide-2025) |
| 비용 최적화 | [Hidden Cost of Cortex AI](https://seemoredata.io/blog/snowflake-cortex-ai/), [Monitoring AI Costs](https://select.dev/posts/monitoring-snowflake-ai-costs) |
| ML FORECAST | [ML Forecasting Docs](https://docs.snowflake.com/en/user-guide/ml-functions/forecasting) |
| 현실성 평가 | [GenAI Hackathon Lessons](https://towardsdatascience.com/things-i-learnt-by-participating-in-genai-hackathons-over-the-past-6-months/), [LLM Production Readiness](https://arxiv.org/html/2403.18958v1) |
| Snowflake 혁신 활용 | [Snowflake Agentic Products](https://www.snowflake.com/en/news/press-releases/snowflake-marketplace-adds-agentic-products-and-ai-ready-data-from-leading-news-research-and-market-data-providers/) |
