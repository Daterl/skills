# Daterl Skills

Daterl 팀 Claude Code 플러그인 마켓플레이스

## 개요

AI 기반 개발 도구를 위한 재사용 가능한 플러그인 저장소입니다.

## 저장소 구조

```
skills/
├── .claude-plugin/
│   └── marketplace.json
├── plugins/
│   └── daterl/
│       ├── .claude-plugin/
│       │   └── plugin.json
│       ├── README.md
│       ├── CHANGELOG.md
│       └── skills/
│           ├── plan-agent/
│           │   └── SKILL.md
│           └── plan-to-tickets/
│               └── SKILL.md
└── README.md
```

## 설치 방법

```bash
# 1. 마켓플레이스 추가
/plugin marketplace add Daterl/skills

# 2. daterl 플러그인 설치
/plugin install daterl@skills
```

## 사용법

```bash
# 기획서 작성
/daterl:plan-agent

# 기획서 → GitHub 이슈 변환
/daterl:plan-to-tickets
```

## 플러그인 업데이트

```bash
claude plugin marketplace update skills
claude plugin update daterl@skills
```

## 플러그인 목록

| 플러그인 | 버전 | 설명 |
|----------|------|------|
| daterl | 1.0.0 | 기획·프로젝트 관리 스킬 번들 |

## 새 스킬 추가

1. `plugins/daterl/skills/{skill-name}/SKILL.md` 생성
2. `plugins/daterl/.claude-plugin/plugin.json` 버전 업데이트
3. `.claude-plugin/marketplace.json` 버전 업데이트
4. `plugins/daterl/CHANGELOG.md` 업데이트
5. PR 제출
