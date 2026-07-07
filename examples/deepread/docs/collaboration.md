# DeepRead — Collaboration Guide

> 작성일: 2026-07-08 | 버전: 1.0 | 2~3인 팀 기준 — 규칙은 최소한만.

---

## 1. 팀 구성 & 역할

| 역할 | 주요 책임 |
|------|---------|
| 개발자 A — 학습 경험 (FE·UX) | 리더·대시보드 화면, 학습 루프 UX 품질 |
| 개발자 B — AI 파이프라인·데이터 (BE) | 프롬프트 모듈, API, DB, 배포 |

기획·디자인은 겸업: product.md/design.md가 결정 기준. 문서에 없는 결정은 주간 회의에서 정한 뒤 문서에 반영한다.

---

## 2. 커뮤니케이션

(추론 — intake에 소통 도구 미정)

- **일상 소통**: 카카오톡 또는 Discord DM
- **이슈 관리**: GitHub Issues (intake §7 명시) + tasks.md
- **정기 회의**: 주 1회 30분 — 마일스톤 점검 & 다음 주 태스크 확정

---

## 3. Git 워크플로우

### 3.1 브랜치 전략 — GitHub Flow (intake §7 명시)

```
main ─────────────────────────── (항상 배포 가능 — Railway 자동 배포)
  │        │        │
feature/*──┘  fix/*─┘
```

| 브랜치 | 설명 | 병합 대상 |
|--------|------|---------|
| `main` | 프로덕션 (push = 배포) | - |
| `feature/{{기능}}` | 기능 개발 | `main` |
| `fix/{{버그}}` | 버그 수정 | `main` |

### 3.2 브랜치 네이밍
```
feature/12-chunk-reader
fix/31-evaluator-schema-error
```

---

## 4. 커밋 컨벤션

```
<타입>(<범위>): <요약>
```

| 타입 | 사용 상황 |
|------|---------|
| `feat` | 새 기능 |
| `fix` | 버그 수정 |
| `docs` | 문서 |
| `refactor` | 리팩터링 |
| `test` | 테스트 |
| `chore` | 빌드/설정 |

**예시**: `feat(reading): 구간 판정 배지와 원문 인용 피드백 표시`

---

## 5. PR 규칙

- 하나의 PR은 하나의 목적만
- 리뷰어 30분 내 검토 가능한 크기
- **1명** 승인 후 머지 (서로가 유일한 리뷰어)
- Claude 코드 리뷰 1차 → 사람 리뷰 (blueprint AI Development Strategy)

### PR 템플릿
```markdown
## 변경 사항
## 테스트 방법
## 체크리스트
- [ ] 셀프 리뷰 완료
- [ ] shared/types 변경 시 상대에게 사전 공유함
```

### 코드 리뷰 가이드
- 당일 내 리뷰 시작
- `[Required]` / `[Optional]` / `[Question]` 접두어 사용
- shared/types와 src/lib/ai 변경은 반드시 상호 리뷰

---

## 6. 코드 컨벤션

- 들여쓰기 스페이스 2칸 / ESLint+Prettier 기본값 준수

| 대상 | 규칙 | 예시 |
|------|------|------|
| 변수/함수 | camelCase | `evaluateAttempt` |
| 컴포넌트 | PascalCase | `ChunkReader` |
| 상수 | UPPER_SNAKE_CASE | `MAX_HINT_PER_CHUNK` |
| 유틸 파일 | kebab-case | `term-extractor.ts` |

---

## 7. 이슈 관리

| 레이블 | 설명 |
|--------|------|
| `bug` | 버그 |
| `feature` | 기능 |
| `blocked` | 상대 작업 대기 중 |

---

## 8. 스프린트 & 회의

- **주기**: 1주 (blueprint Milestones가 주 단위)
- **주간 회의**: 월요일 30분
- **데일리**: 비동기 텍스트 한 줄 (한 일/할 일/블로커)
- **회고**: 마일스톤 종료 시 (M0~M3)만 — KPT 간단히
