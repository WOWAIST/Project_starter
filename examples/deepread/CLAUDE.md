# CLAUDE.md — DeepRead

DeepRead는 한국 개발자가 영어 기술 문서를 AI 보조 학습으로 스스로 읽게 만드는 웹앱입니다.
번역기가 아닙니다 — **문장 번역을 제공하는 코드를 절대 작성하지 마세요.** (제품 철학: docs/product.md §10)

## 기준 문서

작업 전 순서대로 참조:

1. `project.blueprint.md` — 아키텍처 단일 기준. 모든 문서·코드와 충돌 시 이 문서가 우선
2. `docs/development.md` — 타입 계약, 폴더 구조, API 설계
3. `docs/product.md` — 기능 규칙 (판정 3단계, 힌트 단계 규칙, AI 금지 규칙)

## 스택

Next.js 15 (App Router) · TypeScript strict · TailwindCSS + shadcn/ui · Prisma + PostgreSQL (Railway) · Claude API (sonnet: 평가·질문, haiku: 청킹·용어)

## 구현 규칙

- **타입 먼저**: `src/shared/types`의 도메인 타입(Document, Chunk, Attempt, TermCard, ReadingStats)이 계약이다.
- **프롬프트는 모듈**: AI 프롬프트는 `src/lib/ai/` 파일 단위로 관리. 하나로 합치지 않는다.
- **AI 응답 검증**: 모든 Claude 응답은 zod 스키마 검증 후 사용 (1회 재시도 → 폴백 UI).
- **Chunk.content는 불변**: 원문은 저장 후 절대 가공·수정·번역하지 않는다.

## 하지 않을 것

- 번역 기능, 소셜 기능, 모바일 앱 관련 코드 (MVP 스코프 밖)
- Claude API 클라이언트 호출 (AI 호출은 서버 전용 — 키 보호)
- shared/types를 우회하는 임시 타입 정의

## 커밋·PR

`<타입>(<범위>): <요약>` 컨벤션, feature 브랜치 → main PR, 승인 1명. 상세: docs/collaboration.md
