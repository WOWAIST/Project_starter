# AI Project Architect — Workflow

파이프라인과 실행 규칙의 단일 정의. 모든 AI 도구 진입점(CLAUDE.md, `.cursorrules`, AGENTS.md 등)은
`core/system_prompt.md`(정체성·원칙)와 이 파일을 로드한다. 여기에 없는 판단 기준은 system_prompt.md를 따른다.

## 파이프라인

```
사용자 입력 (app/ UI 또는 직접 작성)
    ↓
intake.md                     [capabilities/intake-generator]
    ↓
project.blueprint.md          [capabilities/blueprint-generator]   ← 아키텍처 단일 기준 (source of truth)
    ↓
artifact.plan.md              [capabilities/artifact-planner]      ← 생성할 산출물과 순서 결정
    ↓
계획된 산출물들                [capabilities/artifact-generator]    ← plan 실행 오케스트레이션 (생성은 개별 생성기에 위임)
    ↓
AI-specific Prompt            [prompt-generator]                   ← 예정 (adapters/ai 사용)
```

각 capability는 산출물 파일을 통해서만 연결된다. 모듈끼리 서로의 프롬프트를 직접 참조하지 않는다.

## 실행 규칙

사용자가 문서 생성을 요청하면:

1. `core/system_prompt.md`를 읽고 AI Project Architect로서 작업한다.
2. `intake.md`를 읽는다. 비어 있거나 "미정"인 항목은 되묻지 말고 합리적으로 추론하되, 아키텍처·기술 선택·워크플로우 등 **중요한 결정을 바꾸는 정보만** 사용자에게 질문한다.
3. **Blueprint를 먼저 생성한다.** `capabilities/blueprint-generator/capability.md`의 지침에 따라 intake.md를 `project.blueprint.md`(프로젝트 루트)로 변환한다.
   - 이미 존재하면 다시 만들지 않고 그대로 사용한다. 단, intake.md가 크게 바뀌었다면 갱신을 제안한다.
4. **Artifact Plan을 생성한다.** `capabilities/artifact-planner/capability.md`의 지침에 따라 project.blueprint.md를 `artifact.plan.md`(프로젝트 루트)로 변환한다. 이 문서가 어떤 산출물을 어떤 순서로 만들지 정하는 실행 계획이다.
   - blueprint와 마찬가지로 이미 존재하면 재사용하고, blueprint가 크게 바뀌었다면 갱신을 제안한다.
5. **Artifact Plan을 실행한다.** `capabilities/artifact-generator/capability.md`의 지침에 따라 계획된 산출물을 의존 순서대로, 각 산출물의 담당 생성기에 위임해 생성한다. 하위 문서는 **artifact.plan.md에 계획된 것만**, **intake.md가 아니라 `project.blueprint.md`에서 파생**시킨다. intake.md는 blueprint에 없는 세부 정보를 보충할 때만 참조한다. 표준 4종 문서는 document-generator의 아래 매핑을 기본으로 하고, plan에만 있는 문서(예: architecture.md, roadmap.md)는 blueprint를 근거로 생성한다:

   | 생성 문서 | 역할 프롬프트 | 구조 템플릿 |
   |---|---|---|
   | `product.md` | `capabilities/document-generator/roles/planner.md` | `capabilities/document-generator/templates/product.md` |
   | `design.md` | `capabilities/document-generator/roles/designer.md` | `capabilities/document-generator/templates/design.md` |
   | `development.md` | `capabilities/document-generator/roles/developer.md` | `capabilities/document-generator/templates/development.md` |
   | `collaboration.md` | (별도 역할 없음 — Architect가 직접) | `capabilities/document-generator/templates/collaboration.md` |

6. 출력 위치: `project.blueprint.md`와 `artifact.plan.md`는 프로젝트 루트. 나머지 산출물의 **확정 위치는 artifact.plan.md가 지정**한다(예: 문서 → `docs/`, AI 진입점 → 루트). plan에 지정이 없으면 `output/`에 생성하되, `output/`은 검토용 임시 위치다 — 검토 후 plan에 위치를 기록하고 옮긴다.

## 생성 시 판단 기준

- **산출물 언어는 intake.md의 언어를 따른다.** 프롬프트·템플릿이 어떤 언어로 쓰였는지와 무관하다 (intake가 한국어면 모든 산출물도 한국어).
- **충돌 시 Blueprint가 우선한다.** 하위 문서가 `project.blueprint.md`와 어긋나면 blueprint에 맞춘다. blueprint 자체를 바꿔야 하는 상황이면 사용자에게 먼저 알린다.
- **가치 없는 섹션은 생성하지 않는다.** "보통 프로젝트에 있으니까" 만드는 문서·섹션은 금지.
- **모든 추천에는 근거를 붙인다.** 기술/아키텍처 추천 시 왜, 트레이드오프, (필요시) 대안을 함께 쓴다.
- **팀 규모에 맞게 스케일한다.** 1인 개발이면 프로세스·협업 문서를 최소화하고, 팀이 커질수록 조율·거버넌스를 강화한다.
- **프로젝트 유형에 맞게 적응한다.** 웹/모바일/익스텐션/CLI/API 등 해당 없는 플랫폼 섹션은 삭제한다.
- **완료 전 리뷰한다.** 중복 정보, 약한 추천, 범용적 조언을 제거한 뒤 결과를 전달한다.

## capability 추가 / 변경 시

- 새 capability는 `capabilities/<이름>/` 폴더를 추가하고 이 파일의 파이프라인에 등록한다. 컨벤션: `docs/adding-a-capability.md`
- 파이프라인이 바뀌면 `app/` UI의 스텝 구성도 **같은 변경에서** 함께 수정한다 (UI ↔ workflow drift 방지).
