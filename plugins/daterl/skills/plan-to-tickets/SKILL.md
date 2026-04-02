---
name: dt:tickets
description: >
  Use when the user has a completed plan (plans/{slug}-plan.md) and wants to
  decompose it into GitHub milestones and issues. Triggers: "티켓 만들어줘",
  "이슈 생성", "plan to tickets", "마일스톤 만들어", "GitHub 이슈로 분해",
  "/dt:tickets", "개발 준비", "프로젝트 세팅"
---

# Plan-to-Tickets

기획서(`plans/{slug}-plan.md`)를 GitHub 레포 + 마일스톤 + 이슈로 변환하는 스킬.

## CRITICAL Rules

1. **플랜 파일 필수**: `plans/{slug}-plan.md`가 없으면 STOP. 사용자에게 `/dt:plan` 먼저 실행 안내.
2. **한국어 대화**: 모든 질문·산출물은 한국어. GitHub 이슈 제목/본문은 한국어 기본, 사용자 선호 시 영어.
3. **턴당 AskUserQuestion 1회만**. 호출 후 STOP.
4. **gh CLI 필수**: `gh auth status` 확인 후 진행. 실패 시 `gh auth login` 안내.
5. **Dry-run 우선**: Step 2 완료 후 반드시 사용자 확인. 승인 없이 GitHub 생성 금지.
6. **멱등성**: 이미 존재하는 레포/마일스톤/이슈는 스킵. 중복 생성 금지.
   - 레포: `gh repo view {org}/{repo}` 로 존재 여부 확인
   - 마일스톤: `gh api repos/{org}/{repo}/milestones` 로 기존 목록 조회
   - 라벨: `gh label list --repo {org}/{repo}` 로 기존 라벨 확인
   - 존재하면 스킵하고 사용자에게 알림

---

## Step 0: 사전 확인

1. `gh auth status` 실행으로 GitHub CLI 인증 확인
2. `plans/` 디렉토리 스캔 → 플랜 파일 탐색
3. 인자로 slug가 주어지면 해당 파일 사용, 없으면 AskUserQuestion으로 선택

---

## Step 1: 플랜 파싱

`plans/{slug}-plan.md`를 읽고 다음을 추출한다:

| 추출 항목 | 플랜 섹션 | 매핑 대상 |
|-----------|----------|-----------|
| 프로젝트명 | 제목 (H1) | 레포명 + README |
| 배경/목표 | 배경 + 목표 | README + 레포 설명 |
| 핵심 기능 | 핵심 기능 (H2/H3) | **마일스톤** |
| 세부 항목 | 기능 하위 bullet | **이슈** |
| 일정 | 일정 섹션 | 마일스톤 due_date |
| 기술 스택 | 아키텍처/기술 제약 | 레포 topics + README |
| 성공 기준 | 성공 기준 | 이슈 acceptance criteria |
| 리스크 | 리스크 + 대응 | 이슈 라벨 (risk) |

### 분해 원칙

**마일스톤 = 독립 배포/검증 가능한 기능 단위**
- 플랜의 H2/H3 기능 섹션 또는 일정의 Day 블록을 마일스톤으로
- 일정이 있으면 `due_on` 설정
- 마일스톤 수: 3~7개 (너무 적으면 합치고, 너무 많으면 그룹핑)

**이슈 = 1인이 1일 내 완료 가능한 작업 단위**
- 각 마일스톤 하위에 3~10개 이슈
- 이슈 제목: `[마일스톤명] 구체적 작업` 형식
- 이슈 본문: 목표, 작업 내용, 완료 조건 (acceptance criteria)
- 의존성이 있으면 이슈 본문에 `depends on #N` 명시

### 의존성 감지 기준
- 데이터/인프라 이슈 → 해당 데이터를 사용하는 기능 이슈에 의존
- 환경 세팅 이슈 → 모든 기능 이슈의 선행 조건
- UI 이슈 → 해당 백엔드/로직 이슈에 의존
- 마일스톤 간 의존성: M1 완료 → M2 시작 (별도 명시 불필요, 마일스톤 순서로 표현)

### 라벨 체계

| 라벨 | 색상 | 용도 |
|------|------|------|
| `setup` | `#0E8A16` | 환경 세팅, 초기 구성 |
| `feature` | `#1D76DB` | 기능 구현 |
| `data` | `#5319E7` | 데이터 처리, ETL |
| `ui` | `#FBCA04` | UI/UX 구현 |
| `infra` | `#B60205` | 인프라, 배포 |
| `test` | `#D93F0B` | 테스트, QA |
| `docs` | `#0075CA` | 문서화 |
| `risk` | `#E4E669` | 리스크 대응 |

---

## Step 2: Dry-run 출력

GitHub에 아무것도 생성하지 않고, 분해 결과를 사용자에게 보여준다:

```markdown
## Plan-to-Tickets 프리뷰

**레포**: {org}/{repo-name}
**설명**: {한 줄 설명}
**Topics**: {topic1}, {topic2}, ...

### 마일스톤 ({N}개)

#### M1: {마일스톤명} (due: {날짜})
- [ ] #{순번} {이슈 제목} [`label`]
- [ ] #{순번} {이슈 제목} [`label`]
...

#### M2: {마일스톤명} (due: {날짜})
...

### 라벨 ({N}개)
{라벨명} — {용도}

### 의존성 그래프
{이슈 간 depends on 관계 요약}
```

**사용자 확인 필수**: "이 구조로 GitHub에 생성할까요? 수정할 부분이 있으면 말씀해주세요."

수정 요청이 오면 반영 후 다시 프리뷰. 승인될 때까지 반복.

---

## Step 3: GitHub 생성

승인 후 순차 실행:

### 3-1. 레포 생성
```bash
gh repo create {org}/{repo-name} --public --description "{설명}" --clone
```
- `--public`/`--private`는 사용자 선호. 기본값: private.
- clone 후 README.md 생성 및 초기 커밋

### 3-2. README 생성
플랜에서 추출한 정보로 README.md 작성:
- 프로젝트명, 배경, 목표, 기술 스택, 일정, 팀 구성

### 3-3. 라벨 생성
```bash
gh label create {name} --color {color} --description "{desc}" --repo {org}/{repo}
```

### 3-4. 마일스톤 생성
```bash
gh api repos/{org}/{repo}/milestones -f title="{name}" -f due_on="{date}" -f description="{desc}"
```
- 생성 후 응답의 `number` 필드를 캡처하여 이슈 생성 시 사용
- **절대 마일스톤 번호를 추측하지 않는다** — 반드시 API 응답에서 확인

### 3-5. 이슈 생성
마일스톤별로 순차 생성 (의존성 번호 매핑을 위해):
```bash
gh issue create --repo {org}/{repo} --title "{title}" --body "{body}" --label "{labels}" --milestone "{milestone-title}"
```
- `--milestone`에는 **마일스톤 제목(문자열)**을 사용 (번호 추측 금지)
- 이슈 생성 후 실제 번호를 기록하여 후속 이슈의 `depends on #N` 업데이트
- 의존 이슈가 아직 생성 안 된 경우: 선행 이슈부터 생성

### 실행 순서
```
레포 생성 → README 커밋 → 라벨 생성 (병렬) → 마일스톤 생성 (순차) → 이슈 생성 (마일스톤별 순차)
```

---

## Step 4: 결과 리포트

```markdown
## Plan-to-Tickets 완료

**레포**: [{org}/{repo}]({url})
**마일스톤**: {N}개 생성
**이슈**: {N}개 생성
**라벨**: {N}개 생성

### 생성된 마일스톤
| # | 마일스톤 | Due Date | 이슈 수 |
|---|---------|----------|---------|
| 1 | {name}  | {date}   | {N}     |
...

### 다음 단계
- GitHub Projects에서 보드 뷰 설정
- 팀원 assign
- 첫 마일스톤 착수
```

### 로컬 백업
`plans/{slug}-tickets.md`에 생성된 이슈 목록 + URL 저장.

---

## 에러 처리

| 상황 | 대응 |
|------|------|
| `gh auth status` 실패 | 스킬 중단, `gh auth login` 안내 |
| 레포 이미 존재 | AskUserQuestion: 기존 레포에 추가할지, 새 레포명 지정할지 |
| 마일스톤 중복 | 스킵 + 사용자 알림 |
| 이슈 생성 실패 | 실패 이슈 목록 출력 + 재시도 옵션 |
| Rate limit | 1초 대기 후 재시도 (최대 3회) |
| 플랜 파일 구조 비표준 | 최선 추측으로 파싱 + [추정] 마크 + 사용자 확인 |

---

## Anti-patterns

| Anti-pattern | Fix |
|---|---|
| 플랜 없이 이슈 생성 | 반드시 plans/{slug}-plan.md 기반. 없으면 /plan 먼저 |
| 사용자 확인 없이 생성 | Step 2 dry-run 필수. 승인 후 Step 3 |
| 이슈 너무 큼 (2일+) | 1인 1일 단위로 분해 |
| 이슈 너무 작음 (30분 미만) | 관련 작업 합치기 |
| 마일스톤 없이 이슈만 | 반드시 마일스톤 구조화 |
| 영어/한국어 혼용 | 일관성 유지. 기본 한국어, 사용자 선호 시 영어 |

## Red Flags — STOP

- plans/{slug}-plan.md 파일이 존재하지 않음
- gh CLI 미인증 상태
- 사용자 승인 없이 GitHub 리소스 생성 시도
- org 접근 권한 없음
