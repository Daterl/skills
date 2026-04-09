# AI 전문성 루브릭 (A1 · A2 · A3) — 25점

> 읽는 시점: AI 전문성 카테고리 평가 시

---

## A1 — Cortex를 적절한 용도로 활용 (9점)

**평가 대상: Cortex 6개 기능** (ML FORECAST는 A1이 아닌 R3에서 논리성 검증)

| 기능 | 적절한 용도 | 부적절한 용도 (감점) |
|------|-----------|-------------------|
| AI_COMPLETE | 자유형 생성, 요약, 분류, 질의응답 | 단순 문자열 concatenation 대체 |
| AI_CLASSIFY | 사전 정의 카테고리 분류 (지원 티켓, 콘텐츠 분류) | binary TRUE/FALSE만 필요한 경우 |
| AI_SENTIMENT | 측면별(aspect-based) 감성 분석 | 단순 긍/부정 분류 (0.92 정확도 낭비) |
| Cortex Search | 비정형 텍스트 하이브리드(벡터+키워드) 검색, RAG | 단순 SQL LIKE 검색 대체 |
| Cortex Analyst | NL2SQL (자연어 → SQL), semantic model 필수 | YAML 없는 직접 SQL 생성 |
| Cortex Agent | Search + Analyst 오케스트레이션, Plan→Route→Reflect 사이클 | 단순 챗봇 (단일 기능 호출만) |

**A1 점수 기준:**

| 점수 | 기준 |
|------|------|
| 9 | 5-6개 기능 모두 동작 + 각 기능이 적절한 용도로 사용됨 |
| 7 | 4-5개 동작, 1-2개 용도 부적합 또는 형식적 사용 |
| 5 | 3개 동작, 핵심 기능(Agent, Search) 미사용 또는 미구현 |
| 2 | 1-2개만 동작 |

### Cortex Agent 정적 코드 검증 (4단계 프레임워크)

런타임 검증이 아닌 SQL 파일 + 앱 코드 읽기로 확인한다.

**Step 1 — Agent DDL 존재 확인** (`*.sql` 파일 검색)
```sql
-- 이것이 있어야 Agent가 실제 정의된 것
CREATE OR REPLACE CORTEX AGENT <name> ...
  tools: [
    { cortex_search_service: ... },   -- Search 도구 등록?
    { analyst: { semantic_model: ... } }  -- Analyst 도구 등록?
  ]
```
→ 두 도구 모두 `tools:` 배열에 있으면 오케스트레이션 구성 완료

**Step 2 — Orchestration Instructions 라우팅 로직 확인**
- "structured" / "SQL" / "tabular" → Analyst 라우팅 명시 여부
- "unstructured" / "text" / "document" → Search 라우팅 명시 여부
- 라우팅 없으면: 단순 챗봇 수준 (A1 -2점)

**Step 3 — 앱 코드 호출 방식 확인** (`app.py` 또는 메인 앱 파일)
```python
# 오케스트레이션 패턴 (✅)
requests.post(".../cortex/agent:run", json={...})

# 단순 AI_COMPLETE 직접 호출 (❌ Agent 없음)
session.sql("SELECT AI_COMPLETE('mistral-7b', ...)")
```

**Step 4 — 추가 가산 항목**
- `warehouse` 필드 실제 WH명 있음? (`""` → 즉시 런타임 에러, -1점)
- `"auto"` 모델 선택? (A3 확장성과 연동)
- Budget(`max_tokens`, `timeout_secs`) 설정 있음?
- Cortex Search: `TARGET_LAG` 설정으로 증분 인덱싱 활성화? (50-70% 비용 절감)
- 다중 에이전트 패턴: Master Agent → Sub-Agent(UDF) 계층 구조? (가산)

> Reference: [Best Practices for Building Cortex Agents](https://www.snowflake.com/en/developers/guides/best-practices-to-building-cortex-agents/) — 4-stage config framework
> Reference: [Cortex Search Hybrid Search](https://www.snowflake.com/en/blog/cortex-search-ai-hybrid-search/) — Incremental indexing 50-70% cost savings

---

## A2 — AI를 가치 창출 구조로 활용 (8점)

| 점수 | 기준 |
|------|------|
| 8 | AI 함수가 Dynamic Table 내에서 선언적 파이프라인으로 실행되거나, Agent가 Search+Analyst를 오케스트레이션하여 사용자에게 새로운 가치 제공 |
| 6 | AI 결과를 저장하고 앱에서 조회하는 배치 패턴 구현 |
| 4 | AI를 직접 호출하지만 결과가 재사용되지 않음 (매 클릭마다 LLM 호출) |
| 2 | AI를 버즈워드 수준으로만 사용 |

**가치 창출 구조 예시:**
- `AI_SENTIMENT` + `AI_CLASSIFY` → Dynamic Table enriched 컬럼 저장 → Agent가 이 컬럼 쿼리 (읽기 비용 최소화)
- Cortex Analyst NL2SQL → Cortex Search 비정형 문서 검색 → Agent 결과 통합 응답
- "switching from Databricks to Snowflake AISQL: processing 900% faster, saved $500,000 annually" (Snowflake 고객 사례)

> Reference: [From Zero to Agents: End-to-End Data Pipelines](https://www.snowflake.com/en/developers/guides/from-zero-to-agents/)
> Reference: [Gen AI in Action: Cortex Customer Stories](https://www.snowflake.com/en/blog/gen-ai-cortex-customer-stories-outcomes/)

---

## A3 — AI 모델 확장성 (8점)

| 점수 | 기준 |
|------|------|
| 8 | 모델 `"auto"` 설정, semantic model이 YAML로 분리되어 코드 변경 없이 확장 가능, 데이터 소스 추가 경로 문서화 |
| 6 | 일부 설계가 확장 가능하지만 모델이 하드코딩됨 |
| 4 | 전체 설계가 현재 데이터/스키마에 고정됨 |
| 2 | 확장 고려 없음, 변경 시 전면 재작성 필요 |

**확장성 체크포인트:**
- Agent/AI_COMPLETE 모델 설정이 `"auto"` 또는 설정 파일로 분리됨?
- Semantic model(`*.yaml`)이 테이블 추가 시 JOIN 설정만으로 확장 가능한 구조?
- ML FORECAST가 새 지역/시리즈 추가 시 파이프라인 재실행으로 대응 가능?
- 앱 코드에 도메인별 하드코딩(고정 구 이름, 특정 컬럼명 등)이 최소화됨?

> Reference: [Cortex Agents Configure & Interact](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents-manage) — "auto" model for future-proof architecture
