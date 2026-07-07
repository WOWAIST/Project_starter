# Project Starter

새 프로젝트를 시작할 때 필요한 **기획 / 디자인 / 개발 / 협업 문서**를 AI와 함께 빠르게 만들어주는 템플릿 레포입니다.

---

## 사용 방법

### Step 1 — 이 레포를 새 프로젝트에 복사

```bash
# 방법 A: 클론 후 git 히스토리 제거
git clone <this-repo-url> my-project
cd my-project && rm -rf .git && git init

# 방법 B: GitHub "Use this template" 버튼 사용
```

### Step 2 — intake.md 작성

`intake.md` 파일을 열어 프로젝트 정보를 채워주세요.  
모르는 항목은 비워두거나 "미정"으로 표기해도 됩니다.

```
intake.md
├── 1. 프로젝트 기본 정보
├── 2. 문제 & 목표
├── 3. 사용자
├── 4. 핵심 기능
├── 5. 기술 스택
├── 6. 디자인 방향
├── 7. 협업 & 워크플로우
└── 8. 제약 사항
```

### Step 3 — AI에게 문서 생성 요청

사용 중인 AI 코딩 도구(Claude Code, Cursor, Codex 등)에서 아래와 같이 요청합니다.

```
# 전체 문서 한 번에 생성 (Blueprint → Artifact Plan → 문서 순으로 진행됩니다)
Project_starter 레포를 참고해서 product/design/development/collaboration 문서를 생성해줘

# 단계별로 생성
intake.md를 바탕으로 capabilities/blueprint-generator/capability.md에 따라 project.blueprint.md를 생성해줘
project.blueprint.md를 바탕으로 capabilities/artifact-planner/capability.md에 따라 artifact.plan.md를 생성해줘

# 개별 문서만 생성 (project.blueprint.md가 있어야 합니다)
project.blueprint.md를 바탕으로 capabilities/document-generator/roles/planner.md 역할로 PRD를 작성해줘
project.blueprint.md를 바탕으로 capabilities/document-generator/roles/designer.md 역할로 디자인 문서를 작성해줘
project.blueprint.md를 바탕으로 capabilities/document-generator/roles/developer.md 역할로 개발 문서를 작성해줘
```

### Step 4 — 생성된 문서 확인 & 수정

AI가 프로젝트 루트에 `project.blueprint.md`와 `artifact.plan.md`를, `output/` 폴더(또는 지정한 위치)에 나머지 문서를 생성합니다.  
팀과 함께 검토하고 필요한 부분을 수정하세요. 특히 **project.blueprint.md는 모든 문서의 기준**이므로 가장 먼저 확정하는 것을 권장합니다.

---

## 파일 구조

```
Project_starter/
│
├── intake.md                        ← ✏️  여기를 채워주세요 (프로젝트 정보 입력)
│
├── core/                            ← 정체성과 오케스트레이션
│   ├── system_prompt.md             최상위 시스템 프롬프트 (AI Project Architect)
│   └── workflow.md                  파이프라인 정의 — capability 실행 순서와 규칙
│
├── capabilities/                    ← 독립 능력 모듈 (기능 추가 = 폴더 추가)
│   ├── intake-generator/            사용자 폼 → intake.md (인터페이스는 app/ UI)
│   ├── blueprint-generator/         intake.md → project.blueprint.md
│   ├── artifact-planner/            project.blueprint.md → artifact.plan.md
│   └── document-generator/          계획된 문서 생성 (예정 — 재료는 준비됨)
│       ├── roles/                   기획자 / 디자이너 / 개발자 역할 프롬프트
│       └── templates/               product / design / development / collaboration 구조
│
├── app/
│   └── index.html                   인테이크 폼 UI
│
├── docs/
│   └── adding-a-capability.md       capability 추가 컨벤션
│
├── CLAUDE.md                        ← Claude Code 진입점 (core/를 가리키는 얇은 어댑터)
└── README.md
```

`adapters/`(AI별·프로젝트 유형별 적응 지식)와 `examples/`(완성 산출물 예시)는 첫 파일이 생길 때 추가됩니다 — 위치와 용도는 `docs/adding-a-capability.md`에 정의되어 있습니다.

---

## 문서 생성 파이프라인

모든 문서는 intake.md에서 바로 만들어지지 않고, **Blueprint를 거쳐** 생성됩니다.

```
사용자 입력 (app/ UI)
    ↓  capabilities/intake-generator
intake.md (입력)
    ↓  capabilities/blueprint-generator
project.blueprint.md (아키텍처 단일 기준 — source of truth)
    ↓  capabilities/artifact-planner
artifact.plan.md (생성할 산출물과 순서 결정 — 실행 계획)
    ↓  capabilities/document-generator (roles × templates)
product.md / design.md / development.md / collaboration.md ...
```

Blueprint는 "무엇을 / 왜 / 누구를 위해 / 어떻게 만들 것인가"를 한 문서로 확정합니다. 이후 문서가 Blueprint와 충돌하면 Blueprint가 우선합니다.
Artifact Plan은 이 프로젝트에 실제로 가치 있는 산출물만 골라 생성 순서와 담당 AI를 정합니다 — 계획에 없는 문서는 만들지 않습니다.

## 프롬프트 구조 (3계층)

AI는 아래 3계층을 조합해 문서를 생성합니다.

```
core/system_prompt.md          정체성 & 원칙 — "AI Project Architect"로서 판단하는 기준
        ↓
capabilities/*/capability.md   능력 — 각 파이프라인 단계의 실행 프롬프트 (+ roles/)
        ↓
capabilities/*/templates/      구조 — 각 문서의 포맷과 섹션
```

- **core/system_prompt.md**: 모호함 최소화, 가치 없는 산출물 생략, 모든 추천에 근거 제시, 팀 규모·프로젝트 유형에 맞는 적응 등 전체 작업의 판단 기준을 정의합니다.
- **core/workflow.md**: capability들의 실행 순서와 산출물 계약을 정의합니다.
- **CLAUDE.md**: Claude Code 사용 시 자동으로 로드되는 얇은 진입점 — core/ 두 파일을 가리킵니다. 다른 AI 도구(Cursor, Codex, Gemini 등)를 사용할 경우 해당 도구의 규칙 파일(`.cursorrules`, `AGENTS.md` 등)을 같은 방식의 얇은 포인터로 만들거나, core/ 파일을 대화에 직접 첨부하세요.

---

## 각 문서 설명

| 문서 | 내용 | 주요 독자 |
|------|------|---------|
| `project.blueprint.md` | 아키텍처 방향, 스코프, 마일스톤 — 모든 문서의 기준 | 전체 팀 |
| `artifact.plan.md` | 생성할 산출물 목록, 순서, 담당 AI — 문서 생성 실행 계획 | 전체 팀 |
| `product.md` | PRD — 기능 정의, 사용자 스토리, 성공 지표 | 전체 팀 |
| `design.md` | 디자인 시스템, 컬러/타이포, 주요 화면 | 디자이너, 프론트 |
| `development.md` | 아키텍처, API 설계, 기술 스택, 배포 전략 | 개발팀 |
| `collaboration.md` | Git 컨벤션, PR 규칙, 회의 방식 | 전체 팀 |

---

## 팁

- **intake.md를 자세히 채울수록** 문서 품질이 높아집니다.
- 문서 생성 후 팀 전체가 함께 리뷰하고 수정하는 것을 권장합니다.
- 프로젝트 진행 중 요구사항이 바뀌면 intake.md를 업데이트하고 다시 생성할 수 있습니다.
- `capabilities/` 안의 프롬프트를 직접 수정해 AI의 문서 작성 방식을 커스터마이징할 수 있습니다.
- 새 기능을 추가하려면 `docs/adding-a-capability.md`의 컨벤션을 따라 capability 모듈을 추가하세요.
