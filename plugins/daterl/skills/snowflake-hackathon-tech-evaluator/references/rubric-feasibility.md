# 현실성 루브릭 (R1 · R2 · R3) — 15점

> 읽는 시점: 현실성 카테고리 평가 시

---

## R1 — 구현 완성도 (5점)

| 점수 | 기준 |
|------|------|
| 5 | 핵심 기능 전수 동작, 에러 핸들링 포함, 배포 가능 상태, E2E 테스트 시나리오 존재 |
| 4 | 핵심 기능 동작, 일부 edge case 미처리 |
| 3 | 데모 수준 동작, 하드코딩된 값 존재 |
| 2 | 부분 동작, 주요 기능 미완성 |
| 1 | 실행 불가, 프로토타입 미완성 |

**체크포인트:**
- API 키/시크릿이 하드코딩되지 않았는가
- `warehouse` 필드가 빈 문자열이 아닌가 (`""` → 즉시 에러, -1점)
- 앱이 SiS에서 실제 배포·실행되는가 (코드 존재 ≠ 배포 완료)
  - 확인: `SHOW STREAMLITS` 쿼리 또는 Git integration 설정 존재 여부
- SQL 파일이 검증 쿼리(`SHOW`, `DESCRIBE`)를 포함하는가

---

## R2 — 자원·비용 합리성 및 지속 가능성 (5점)

| 점수 | 기준 |
|------|------|
| 5 | 비용 예산 계획, 배치 패턴(AI 1회 실행→결과 저장→조회), Warehouse AUTO_SUSPEND 설정, 실제 크레딧 소비 추적 |
| 4 | 배치 패턴 구현, 비용 계획 있음 |
| 3 | `@st.cache_data` 등 기본 최적화, 비용 계획 없음 |
| 2 | 매 클릭마다 LLM 호출 (크레딧 소모 무제한) |
| 1 | 비용 고려 없음 |

**Cortex AI 비용 최적화 체크리스트:**

| 항목 | 확인 방법 | 미충족 시 감점 |
|------|----------|--------------|
| `AI_COUNT_TOKENS`로 사전 비용 추정 | SQL 파일 grep | -0.5점 |
| 모델 선택 근거 문서화 | docs/ 또는 주석 (mistral-7b vs large2 최대 40배 차이) | -0.5점 |
| `CORTEX_FUNCTIONS_USAGE_HISTORY` 모니터링 가능 구조 | Snowflake 계정 접근 또는 쿼리 존재 | 참고 |
| Warehouse: XSMALL, AUTO_SUSPEND=60 | SQL DDL 확인 | -0.5점 |
| AI 결과 테이블 저장 후 SELECT 패턴 | 배치 SQL + 앱 코드 | -1점 |

> Reference: [The Hidden Cost of Snowflake Cortex AI — $5K single-query warning](https://seemoredata.io/blog/snowflake-cortex-ai/) — Seemore Data
> Reference: [Monitoring Snowflake AI Costs](https://select.dev/posts/monitoring-snowflake-ai-costs)

---

## R3 — 문제 해결의 논리성·구현 가능성 (5점)

| 점수 | 기준 |
|------|------|
| 5 | 문제→기술 선택 논리가 명확하고 문서화됨, 측정 가능한 결과 지표 존재 |
| 4 | 논리는 있으나 일부 가정이 비현실적 |
| 3 | 개념은 실현 가능하나 문제-솔루션 연결이 약함 |
| 2 | 기술 먼저 선택하고 문제를 끼워 맞춘 흔적 |
| 1 | "AI-powered" 버즈워드 중심, 실질 문제 해결 없음 |

**"논리적 해결" vs "억지 솔루션" 판별 질문:**
1. 이 문제가 AI/ML 없이도 해결 가능한가? → 가능하면 AI 필요성 설명이 있어야 함
2. ML FORECAST를 쓰는 경우: 학습 데이터가 최소 1개 전체 시즌 주기를 포함하는가?
3. 선택한 기술 스택이 문제 특성에 적합한가? (분류 → AI_CLASSIFY, 검색 → Cortex Search)
4. 결과가 정량적으로 측정 가능한가? (정확도, 속도, 비용 등)

> Reference: [Snowflake ML Forecasting](https://docs.snowflake.com/en/user-guide/ml-functions/forecasting) — min 1 full season of training data required
> Reference: [Things I Learnt by Participating in GenAI Hackathons](https://towardsdatascience.com/things-i-learnt-by-participating-in-genai-hackathons-over-the-past-6-months/)
