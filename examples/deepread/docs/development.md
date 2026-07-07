# Technical Architecture - DeepRead

## 기술 스택

### Framework / Runtime
* Next.js 15 (App Router) — 풀스택 단일 코드베이스 (blueprint Technology Direction)
* React 19 / TypeScript (strict)

### UI
* TailwindCSS + shadcn/ui + Lucide

### 웹앱 (Web)
* 배포: Railway (intake §8 명시 — git push 배포, Postgres 애드온)

### AI
* Claude API (intake §5 명시)
* 모델: claude-sonnet-4-6 — 질문 생성·설명 평가
* Lite 모델: claude-haiku-4-5 — 용어 추출, 청킹 보조

### Storage
MVP: PostgreSQL (Railway) + Prisma
Future: Redis — 평가 큐가 병목일 때만

---

# 프로젝트 구조

```text
deepread
├── prisma/schema.prisma
├── src
│   ├── app
│   │   ├── (marketing)/page.tsx
│   │   ├── library/  read/[docId]/  dashboard/
│   │   └── api/
│   ├── features
│   │   ├── ingest      ← 문서 가져오기·청킹
│   │   ├── reading     ← 학습 루프
│   │   ├── terms       ← 용어 카드
│   │   └── stats       ← 학습 기록·독립성 지표
│   ├── lib
│   │   ├── ai/         ← AI 파이프라인
│   │   ├── auth.ts
│   │   └── db.ts
│   └── shared
│       ├── types  constants  utils
└── package.json
```

---

# 주요 도메인

## Document
```ts
interface Document {
  id: string;
  userId: string;
  sourceUrl: string | null;
  title: string;
  status: "ingesting" | "ready" | "completed";
  chunkCount: number;
  createdAt: Date;
}
```

## Chunk
```ts
interface Chunk {
  id: string;
  documentId: string;
  order: number;
  content: string;            // 원문 그대로 — 절대 가공하지 않는다
  question: string | null;
}
```

## Attempt
```ts
interface Attempt {
  id: string;
  chunkId: string;
  userId: string;
  explanation: string;
  verdict: "understood" | "partial" | "reread";
  feedback: string;
  hintUsed: number;           // 독립성 지표 원천
  createdAt: Date;
}
```

## TermCard
```ts
interface TermCard {
  id: string;
  chunkId: string;
  term: string;
  explanation: string;        // 개념 설명 — 번역이 아님
}
```

## ReadingStats
```ts
interface ReadingStats {
  userId: string;
  level: 1 | 2 | 3;
  documentsCompleted: number;
  hintRatePerChunk: number;   // 최근 4주 이동 평균
  updatedAt: Date;
}
```

이 타입을 먼저 확정한 뒤 개발을 시작한다. 이 타입이 API 계약서 역할을 한다.

---

# API

## POST /api/documents
```text
{ sourceUrl } 또는 { rawText, title }
↓ 본문 추출·정제 ↓ 청킹(haiku) → Chunk[] 저장
↓ Document(status: "ready") + 첫 Chunk
```

## POST /api/chunks/:id/question
```text
chunkId ↓ 레벨 확인 → 질문 생성 대상 판정
↓ question-gen(sonnet) → 스키마 검증 ↓ Chunk.question 저장
```

## POST /api/attempts (핵심)
```text
{ chunkId, explanation } + Chunk.content
↓ evaluator(sonnet) → verdict + 원문 인용 feedback (검증, 실패 시 1회 재시도)
↓ Attempt 저장 + ReadingStats 갱신 → { verdict, feedback }
```

## GET /api/stats
```text
userId ↓ Attempt 집계(hintRate 4주 이동평균, 레벨 산정) ↓ ReadingStats + 추이
```

---

# AI 프롬프트 모듈 구조

```text
src/lib/ai
├── client.ts           ← 재시도·스키마 검증·비용 로깅
├── chunker.ts
├── question-gen.ts
├── evaluator.ts
└── term-extractor.ts
```

각 모듈은 독립적으로 관리한다. 하나의 거대한 프롬프트로 합치지 않는다.
모든 출력은 zod 스키마 검증 통과 후 노출한다 (blueprint 개발 원칙 4).

---

# 핵심 처리 플로우

```text
사용자 → URL 입력 → 인제스트(추출→청킹→저장) → 구간 표시
→ 이해 질문 생성 → 설명 입력 → 평가(판정+피드백+통계 갱신)
→ 이해함: 다음 구간 / 완독: 리포트
```

---

# 담당자 분업

## 개발자 A — 학습 경험 (Frontend & Reading UX)
담당: `src/app`, `src/features/reading`(UI), `src/features/stats`
기능: 리더 화면(구간 전환·설명·판정·피드백), 힌트/용어 카드 UI, 대시보드, 온보딩
책임: 학습 루프를 이탈 없이 완주할 수 있는 UX 품질

## 개발자 B — AI 파이프라인 & 데이터 (Backend)
담당: `src/lib/ai`, `src/app/api`, `src/features/ingest`, `prisma`
기능: 4개 프롬프트 모듈+스키마 검증, Prisma 스키마·집계 쿼리, OAuth, Railway 배포
책임: AI 응답의 스키마 안정성과 판정 품질

---

# 공통 작업

```text
src/shared/types
```

### 우선 작성
```ts
Document
Chunk
Attempt
TermCard
ReadingStats
```
