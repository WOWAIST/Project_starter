# DeepRead — Tasks

> blueprint Milestones를 착수 가능한 태스크로 분해. 담당: A(학습 경험·FE), B(AI 파이프라인·BE).

## M0 — 골격과 배포 (1주차)

| 태스크 | 담당 | 의존 |
|---|---|---|
| shared/types 도메인 타입 5종 확정 | A+B | — |
| Next.js + Tailwind/shadcn 셋업 | A | 타입 확정 |
| Prisma 스키마 + Railway Postgres 연결 | B | 타입 확정 |
| GitHub OAuth (NextAuth) | B | — |
| Railway 배포 파이프라인 확인 | B | 스캐폴딩 |
| 라우트 골격 (/library, /read, /dashboard) | A | 스캐폴딩 |

**완료 기준**: 로그인해서 빈 라이브러리가 보이는 앱이 Railway에 떠 있다.

## M1 — 읽기 파이프라인 (2~4주차)

| 태스크 | 담당 | 의존 |
|---|---|---|
| lib/ai/client.ts | B | M0 |
| 인제스트: URL 본문 추출·정제 | B | client |
| chunker.ts + Document/Chunk 저장 | B | 인제스트 |
| question-gen.ts | B | client |
| 리더 화면: 구간 표시·전환·진행률 | A | M0 |
| 문서 입력 UI + 라이브러리 목록 | A | M0 |

**완료 기준**: 실제 URL을 넣으면 구간 단위로 읽고 질문을 받는다.

## M2 — 학습 루프 (5~7주차)

| 태스크 | 담당 | 의존 |
|---|---|---|
| evaluator.ts | B | M1 |
| POST /api/attempts + ReadingStats 갱신 | B | evaluator |
| term-extractor.ts + 용어 카드 | B | M1 |
| 설명 입력 → 판정 배지·피드백 UI | A | attempts API |
| 힌트 단계 UI | A | M1 |
| 완독 리포트 화면 | A | attempts API |

**완료 기준**: 문서 하나를 처음부터 끝까지 학습 루프로 완독할 수 있다.

## M3 — 공개 준비 (8주차)

| 태스크 | 담당 | 의존 |
|---|---|---|
| 대시보드: 독립성 지표·레벨 표시 | A | M2 |
| 레벨 자동 제안 로직 | B | M2 |
| 온보딩·Empty State·에러 폴백 | A | M2 |
| AI 비용 상한·레이트 리밋 | B | M2 |
| README.md 작성 + 공개 배포 점검 | A+B | 전체 |

**완료 기준**: 팀 밖의 개발자가 안내 없이 가입→완독까지 도달할 수 있다.

---

리스크 메모: M2 evaluator 품질이 제품 성패를 좌우 — 5주차 기준 미달이면 M3 자동 제안을 잘라 시간 확보 (우선순위: 학습 루프 > 지표 > 자동화).
