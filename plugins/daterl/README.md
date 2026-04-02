# daterl

Daterl 팀 기획·프로젝트 관리 스킬 번들

## 설치

```bash
# 마켓플레이스 추가
/plugin marketplace add Daterl/skills

# 플러그인 설치
/plugin install daterl@skills
```

## 스킬 목록

| 스킬 | 사용법 | 설명 |
|------|--------|------|
| plan-agent | `/daterl:plan-agent` | 모호한 아이디어를 Notion 기획서로 변환 |
| plan-to-tickets | `/daterl:plan-to-tickets` | 기획서를 GitHub 마일스톤 + 이슈로 분해 |

## 워크플로우

```
아이디어 → /daterl:plan-agent → Notion 기획서 → /daterl:plan-to-tickets → GitHub 이슈
```

## 업데이트

```bash
claude plugin marketplace update skills
claude plugin update daterl@skills
```
