# DeepRead — Artifact Plan

> blueprint에서 파생. 계획에 없는 산출물은 생성하지 않는다.

# Planning Summary

2~3인·2개월·Claude Code 개발이라는 조건에 맞춰 6개 산출물만 계획한다. 문서 4종은 표준 매핑, AI 개발 환경 산출물 2종(CLAUDE.md, tasks.md)을 추가한다. roadmap/architecture는 blueprint와 중복이라 제외.

# Required Artifacts

| # | Name | Purpose | Reason | Priority | Dependencies | Generator |
|---|------|---------|--------|----------|--------------|-----------|
| 1 | product.md | 학습 루프 규칙·MVP 경계 확정 | 평가 기준이 전 기능의 판단 기준 | P1 | blueprint | document-generator/planner |
| 2 | design.md | 리더 화면 중심 UI 정의 | 디자이너 없는 팀의 일관성 기준 | P2 | product.md | document-generator/designer |
| 3 | development.md | 타입 계약·AI 파이프라인·분업 | 타입이 계약(개발 원칙 1) | P1 | product.md | document-generator/developer |
| 4 | collaboration.md | 소규모 협업 규칙 | 리뷰·머지 임의화 방지 | P3 | 없음 | document-generator (직접) |
| 5 | CLAUDE.md | Claude Code 진입점 | 개발 AI가 Claude Code로 확정 (intake §7) | P1 | development.md | artifact-generator (임시) |
| 6 | tasks.md | 마일스톤별 착수 태스크 | 2개월 고정, 방향 상실 방지 | P2 | product.md, development.md | artifact-generator (임시) |

**출력 위치**: product/design/development/collaboration/tasks → `docs/`. CLAUDE.md → 레포 루트 (Claude Code 요구사항).

# Generation Workflow

```
product.md → design.md/development.md(병렬) → tasks.md/CLAUDE.md → collaboration.md
```

# AI Assignment

전 산출물 Claude Code 담당 (intake 명시, 복수 AI 운용 불필요).

# Optional Future Artifacts

README.md(M3 공개 시점), roadmap.md(Phase 2 확정 시), architecture.md(서비스 분리 시)
