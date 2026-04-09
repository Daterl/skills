# Snowflake 전문성 루브릭 (S1 · S2 · S3) — 25점

> 읽는 시점: Snowflake 전문성 카테고리 평가 시

---

## S1 — 플랫폼 기능 적절 활용 및 최적화 (9점)

| 점수 | 기준 |
|------|------|
| 9 | Dynamic Tables + AISQL 함수 내장, 또는 Streams/Tasks를 용도에 맞게 정확히 선택. `CREATE AGENT` DDL 정의 존재 |
| 7 | 다양한 Snowflake 기능 사용 (6개 이상), 대부분 적절히 활용 |
| 5 | 기능 3-5개, 단순 SQL SELECT + 기본 함수 위주 |
| 2 | Snowflake를 외부 DB처럼 사용 (로드만 하고 처리는 외부) |
| 0 | Snowflake 기능 미활용 또는 파일 없음 |

**평가 체크포인트:**
- Dynamic Tables vs Streams/Tasks 선택이 용도에 맞는가?
  - 자동 incremental refresh 필요 → Dynamic Tables
  - 이벤트 기반 트리거 필요 → Streams + Tasks
- Warehouse 크기가 작업에 최적화되었는가? (AISQL에는 MEDIUM 이하 권장, XSMALL 충분한 경우 많음)
- SQL 파일이 기능별로 체계화되었는가? (파이프라인 순서 추적 가능)

**기술 적합성 체크 — 문제 유형 → 권장 Snowflake 기능**

평가 전 프로젝트가 어떤 문제를 푸는지 파악한 뒤, 해당 문제 유형에 맞는 기능을 선택했는지 확인한다.
미사용이 감점은 아니지만, 해당 기능이 명백히 필요한 도메인임에도 쓰지 않았다면 S1 점수를 보수적으로 산정한다.

| 문제 유형 | 권장 Snowflake 기능 | CoCo 대응 스킬 | 확인 방법 |
|----------|-------------------|--------------|---------|
| 텍스트 분석 / 감성 / 요약 / OCR | `AI_COMPLETE`, `AI_SENTIMENT`, `AI_CLASSIFY` | cortex-ai-functions | SQL 파일에 AISQL 함수 존재 여부 |
| 시계열 예측 / ML 추론 | `ML FORECAST`, `ML CLASSIFICATION`, Model Registry | machine-learning | `SNOWFLAKE.ML.*` 호출 여부 |
| 비용 모니터링 / 크레딧 추적 | `QUERY_HISTORY`, `METERING_HISTORY`, `AI_COUNT_TOKENS` | cost-intelligence | 비용 추정 쿼리 또는 docs 문서 존재 여부 |
| 데이터 품질 검증 / 이상 탐지 | Data Metric Functions (DMF) | data-quality | `CREATE DATA METRIC FUNCTION` 또는 `APPLY METRIC POLICY` |
| 데이터 출처 추적 / 영향 분석 | Column-level Lineage, `OBJECT_DEPENDENCIES` | lineage | `SELECT * FROM SNOWFLAKE.ACCOUNT_USAGE.OBJECT_DEPENDENCIES` |
| 외부 API / 스토리지 연결 | External Stage, Kafka Connector, REST API | integrations | `CREATE STAGE` + External 설정, Snowpipe |
| 데이터 보안 / 컴플라이언스 | Dynamic Data Masking, Row Access Policy, Tag | data-governance | `CREATE MASKING POLICY`, `CREATE ROW ACCESS POLICY` |
| 협업 / 데이터 공유 | Data Clean Rooms, Secure Data Sharing | data-cleanrooms | `CREATE CLEANROOM` 또는 `SHARE` DDL |
| 노트북 기반 탐색 / 프로토타이핑 | Notebooks in Snowflake Workspaces | notebooks-in-workspaces | `.ipynb` 파일 또는 Notebook DDL |
| dbt 파이프라인 운영 | dbt on Snowflake, Snowflake Git Integration | dbt-projects-on-snowflake | `dbt_project.yml` + Snowflake connection 설정 |

> Reference: [Snowflake Dynamic Tables Complete Guide](https://dataengineerhub.blog/articles/snowflake-dynamic-tables-complete-guide-2025) — 200,000+ in production, 2,900+ customers
> Reference: [Cortex Code (CoCo) 10 Skills Overview](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-code) — CoCo skill → Snowflake feature mapping

---

## S2 — 데이터 자산 혁신적 활용 (8점)

| 점수 | 기준 |
|------|------|
| 8 | Marketplace 데이터를 AISQL 파이프라인에 통합하거나 cross-domain JOIN으로 새 인사이트 창출 |
| 6 | 외부 데이터셋 활용하지만 단순 조회 수준 |
| 4 | Snowflake 내부 데이터만 사용, Marketplace 미활용 |
| 2 | 데이터 소스 1개, 분석 없음 |

**혁신적 활용 vs 단순 활용 구분:**

| 단순 활용 | 혁신적 활용 |
|----------|------------|
| Marketplace에서 데이터 구독·조회만 | 여러 Marketplace 데이터를 JOIN하여 기존에 없던 지표 생성 |
| 단일 데이터 소스 분석 | Cross-domain: 상권+인구+부동산 등 다차원 결합 |
| 수동 ETL | Dynamic Tables + AISQL 함수 내장으로 선언적 파이프라인 |
| 외부 ML 플랫폼으로 데이터 이동 | Snowflake 내에서 학습-추론-서빙 완결 |

---

## S3 — Snowflake로만 가능한 해결책 (8점)

| 점수 | 기준 |
|------|------|
| 8 | Zero-copy sharing, Cortex AI, SiS, ML FORECAST 등 Snowflake 전용 기능이 핵심 역할 |
| 6 | Snowflake가 중요하지만 다른 플랫폼으로도 구현 가능한 부분이 있음 |
| 4 | Snowflake는 저장소 역할만, 실제 처리는 외부 |
| 2 | Snowflake 대신 다른 플랫폼 사용 시 동일 결과 |

**"Snowflake 필수" 판별 기준:**

| 구현 패턴 | 판정 |
|----------|------|
| AISQL 함수(`AI_CLASSIFY`, `AI_SENTIMENT`, `AI_COMPLETE`)가 SQL 파이프라인에 내장 | ✅ 필수 |
| Cortex Agent가 Search + Analyst 실제 오케스트레이션 | ✅ 필수 |
| Streamlit in Snowflake(SiS)로 배포 + 데이터 플랫폼 밖 미유출 | ✅ 필수 |
| 외부 Python 스크립트(`openai`, `langchain`)로 동일 결과 가능 | ❌ 감점 |
| Snowflake는 `COPY INTO`만 사용, 나머지는 외부 | ❌ 감점 |

> Reference: [Snowflake Doubles Down on Developers](https://www.snowflake.com/en/news/press-releases/snowflake-doubles-down-on-developers-with-end-to-end-capabilities-for-building-enterprise-grade-pipelines-models-and-ai-powered-apps/)
