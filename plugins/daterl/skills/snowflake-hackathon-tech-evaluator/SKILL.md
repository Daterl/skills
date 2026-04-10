---
name: dt:snowflake-hackathon-tech-evaluator
description: >
  Use when scoring a Snowflake TECH TRACK hackathon submission before final review,
  identifying technical gaps against the official rubric, or reviewing a teammate's project quality.
  Triggers: "심사 기준 평가", "해커톤 점수 추정", "Snowflake 프로젝트 자가 평가",
  "hackathon submission review", "기술 평가", "갭 분석", "제출 전 점검", "심사표",
  "technical rubric", "제출 준비", "점수 추정".
---

# Snowflake Hackathon Technical Evaluator

## Overview

Snowflake TECH TRACK 심사표의 **기술 4개 카테고리(90점)** 기준으로 프로젝트를 자가 평가한다.
산출물은 **증거 기반 점수 + 갭 분석 + 즉각 액션**이다 — 막연한 낙관이 아닌 파일·코드 인용.

> "An entry with zero novelty reinvents the wheel." — TAIKAI Hackathon Judging
> "A single query can consume thousands of credits without warning." — Seemore Data

**범위에서 제외**: 발표/스토리텔링, 코드 수정, 데모 스크립트 작성

---

## Quick Reference — 12개 심사 항목

| ID | 카테고리 | 항목 | 만점 |
|----|---------|------|------|
| C1 | 창의성 | 기존 접근법 대비 차별화된 문제 정의·솔루션 | 8점 |
| C2 | 창의성 | 문제 배경의 타당성 | 9점 |
| C3 | 창의성 | 새로운 아이디어 또는 기존 대비 개선점 | 8점 |
| S1 | Snowflake 전문성 | 플랫폼 기능 적절 활용 및 최적화 | 9점 |
| S2 | Snowflake 전문성 | 데이터 자산 기반 혁신적 활용 방식 | 8점 |
| S3 | Snowflake 전문성 | Snowflake로만 가능한 해결책인가 | 8점 |
| A1 | AI 전문성 | Cortex 6개 기능을 적절한 용도로 활용 | 9점 |
| A2 | AI 전문성 | AI를 단순 기능이 아닌 가치 창출 구조로 활용 | 8점 |
| A3 | AI 전문성 | AI 모델 발전 속도에 맞춘 확장성 | 8점 |
| R1 | 현실성 | 구현 완성도 | 5점 |
| R2 | 현실성 | 자원·비용 합리성 및 지속 가능성 | 5점 |
| R3 | 현실성 | 문제 해결의 논리성·구현 가능성 | 5점 |

---

## 실행 절차

### Phase 1: 프로젝트 파일 수집

아래 파일을 순서대로 읽는다. 없으면 "파일 없음(감점 반영)" 처리 후 계속.

```
필수:
- CLAUDE.md / AGENTS.md / README.md   (프로젝트 맥락)
- 메인 앱 파일 (app.py, main.py 등)   (구현 현황)
- sql/ 하위 *.sql 전체               (Snowflake 기능 범위)
- *.yaml / *.yml                      (Cortex Analyst Semantic Model)

가능하면:
- docs/*.md                           (문제 배경, 데이터 전략, 비용 계획)
- environment.yml / requirements.txt  (의존성 및 플랫폼 확인)
```

### Phase 2: 항목별 증거 기반 평가

각 카테고리 루브릭 파일을 로드하여 평가한다:

| 평가 카테고리 | 루브릭 파일 |
|-------------|-----------|
| 창의성 (C1·C2·C3) | `references/rubric-creativity.md` |
| Snowflake 전문성 (S1·S2·S3) | `references/rubric-snowflake.md` |
| AI 전문성 (A1·A2·A3) | `references/rubric-ai.md` |
| 현실성 (R1·R2·R3) | `references/rubric-feasibility.md` |

채점 추적은 `references/scoring-worksheet.md` 형식을 사용한다.

**반드시 지킬 것:**
- 점수 산정 시 `파일명:라인번호` 또는 `파일명:함수명` 형식으로 증거 인용
- "잘 구현됨"처럼 모호한 표현 금지 → "app.py:309 — chat_input 구현, Agent 미연동으로 A1에서 -2점"처럼 구체화
- 의심스러우면 보수적으로 산정 (실제 심사는 항상 예상보다 보수적)
- A1: Cortex Agent 오케스트레이션은 런타임이 아닌 정적 코드로 검증 (`rubric-ai.md` Step 1-4 참조)

### Phase 3: 리포트 저장

평가 완료 후 `evaluation-report-{YYYY-MM-DD}.md`를 프로젝트 루트에 저장한다.
파일 구조: 요약 테이블 → 카테고리별 세부 분석 → 총평 → P0/P1 액션

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
- **P0 (제출 전 필수)** vs **P1 (여력 있을 때)** 액션 분리

---

## Common Mistakes

| 실수 | 결과 | 수정 방법 |
|------|------|----------|
| Agent SQL에 `warehouse: ""` | Agent 실행 시 런타임 에러 | 실제 WH명으로 채우기 |
| Cortex Search DDL 없이 Agent에서 참조 | SERVICE not found 에러 | `CREATE CORTEX SEARCH SERVICE` DDL 추가 |
| `mistral-large2` 모델 하드코딩 | A3 확장성 감점 | `"auto"` 또는 설정 파일로 분리 |
| 탭마다 LLM 직접 호출 | R2 비용 감점 | AI 결과 테이블 저장 → SELECT 조회 패턴 |
| ML FORECAST에 학습 데이터 3개월 | 예측 불안정 | 최소 1개 시즌(연간 패턴 = 1년) 이상 |
| 파일 존재 ≠ 배포 완료 | R1 감점 | SiS `SHOW STREAMLITS` 실제 확인 |
| Agent tools:에 Search만 등록 | A1 오케스트레이션 미달 | Analyst service도 tools: 배열에 추가 |

---

## References

| 카테고리 | 참조 파일 |
|---------|---------|
| 창의성 루브릭 전체 | `references/rubric-creativity.md` |
| Snowflake 전문성 루브릭 전체 | `references/rubric-snowflake.md` |
| AI 전문성 루브릭 + Agent 검증 | `references/rubric-ai.md` |
| 현실성 루브릭 + 비용 체크리스트 | `references/rubric-feasibility.md` |
| 채점 추적 템플릿 | `references/scoring-worksheet.md` |
