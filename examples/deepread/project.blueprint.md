# DeepRead — Project Blueprint

> 아키텍처 단일 기준(source of truth). `(추론)`은 intake에 없어 합리적 기본값으로 채운 결정.

# Project Overview

DeepRead는 영어 기술 문서를 읽어야 하는 한국 개발자를 위한 학습 웹 앱이다. 번역 대신 AI가 문서를 구간으로 나눠 이해도를 질문·평가하며, 사용자가 결국 AI 없이 원문을 읽는 상태로 이끈다. 2~3명 풀스택 팀이 2개월 안에 MVP를 완성해 Railway에 배포한다.

# Vision

한국 개발자가 영어 문서 앞에서 번역기 대신 원문을 여는 것을 기본값으로 만든다. 성공은 사용자가 DeepRead를 더 이상 필요로 하지 않는 상태(독립적 읽기)에 도달하는 것이다.

# Goals

1. 학습 루프 완성 — 입력→청킹→질문→설명→평가가 끊김 없이 동작
2. 독립성의 가시화 — AI 도움 사용량 감소 추세를 사용자가 확인 가능
3. 2개월 내 Railway 공개 배포

# Success Criteria

(추론) 첫 문서 완독률 40%+, 완독자 2주 재방문율 30%+, 세션당 학습 구간 5개+, 4주 이상 사용자의 힌트 요청 하락 추세

# Target Users

Primary: 업무상 영어 공식 문서를 읽어야 하지만 번역기에 의존하는 주니어~미들 한국 개발자.

# Problem Statement

1. 번역기로 읽으면 이해는 되지만 독해력이 늘지 않는다
2. 원문으로 읽어도 이해를 검증할 방법이 없다
3. (추론) 기술 용어는 사전식 번역이 오히려 혼란을 준다
4. (추론) 일반 영어 학습 앱은 실무 문서와 동떨어져 있다

# Core Value Proposition

번역해주는 도구가 아니라 번역이 필요 없어지게 만드는 도구. 실무 문서 자체가 교재이고, AI가 이해를 검증하며, 도움 사용량 감소가 곧 성장의 증거다.

# Scope

**Included (MVP)**: 문서 입력·청킹 / 이해 질문 생성 / 설명 평가·피드백 / 용어 추출·개념 설명 / 학습 기록·독립성 대시보드 / (추론) GitHub OAuth 로그인

**Excluded**: 전문 번역 기능(철학 충돌) / 모바일 앱·익스텐션(기간 제약) / 소셜 기능(루프 검증 우선) / 영어 외 원문

**Future**: 팀/스터디 모드, 브라우저 익스텐션, 난이도 개인화

# Architecture Direction

**단일 풀스택 웹 앱(모놀리스)**. Why: 2~3인·2개월 규모에서 프론트/백 분리는 통신·배포 비용만 늘린다. 경계: [브라우저 UI] ↔ [앱 서버(API+AI 파이프라인)] ↔ [PostgreSQL / Claude API]. AI 호출은 서버 전용(키 보호).

# Technology Direction

(추론 — intake 프론트/백/DB 미정, 인프라는 제약사항에 Railway로 명시됨)

| 선택 | 근거 | 대안 |
|---|---|---|
| Next.js(App Router)+TypeScript | 풀스택 단일 코드베이스, Railway 배포 단순 | FE/BE 분리: 2~3인 팀엔 운영비 과다 |
| TailwindCSS+shadcn/ui | 디자이너 없는 팀의 빠른 일관성 | 직접 디자인 시스템: 기간 내 불가 |
| PostgreSQL(Railway)+Prisma | intake 제약사항의 "Railway, Postgres 애드온" 명시 반영 | — |
| Claude API | intake 명시 (외부 API + 개발 AI 도구 모두 Claude) | — |

# Development Principles

1. 타입이 계약이다 — 도메인 타입 먼저 확정
2. 프롬프트는 모듈이다 — AI 프롬프트를 파일 단위로 관리
3. 매 마일스톤은 배포 가능 상태로 끝낸다
4. AI 응답을 신뢰하지 않는다 — 스키마 검증 후 사용, 실패 시 폴백

# Documentation Plan

product.md / design.md / development.md / collaboration.md / CLAUDE.md(Claude Code 사용 — intake 명시) / tasks.md. roadmap·architecture는 이 규모에서 본 문서·development.md와 중복이라 생략.

# AI Development Strategy

개발 AI: **Claude Code** (intake §7 명시). Planning은 Claude Code로 본 파이프라인 유지, Implementation은 CLAUDE.md에 명시적 목표·경계·제약 기술, Review는 Claude 1차 리뷰 후 사람 승인, Documentation은 blueprint에서 파생.

# Milestones

| 기간 | 목표 | 완료 기준 |
|---|---|---|
| M0 (1주) | 골격+배포 | 타입 계약, 로그인, Railway 빈 앱 배포 |
| M1 (2~4주) | 읽기 파이프라인 | 입력→청킹→질문이 실제 문서로 동작 |
| M2 (5~7주) | 학습 루프 | 설명→평가→용어→기록 저장 완성 |
| M3 (8주) | 공개 준비 | 대시보드, 온보딩, 공개 배포 |

# Deliverables

Railway 배포된 DeepRead 웹 서비스 / AI-ready DeepRead 레포(CLAUDE.md 포함) / 학습 데이터 스키마·독립성 지표 정의

# NEXT DOCUMENTS

```
artifact.plan.md → product.md → design.md/development.md(병렬) → collaboration.md/CLAUDE.md/tasks.md
```

product.md가 먼저다 — 화면과 구현은 학습 루프 규칙 확정 후 가능하다.
