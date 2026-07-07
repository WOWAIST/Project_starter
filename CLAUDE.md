# Project Starter — Claude 작업 지침

이 레포에서 문서 생성 작업을 시작하기 전에 **반드시 `docs/core_system_prompt.md`를 먼저 읽고**, 그 정체성(AI Project Architect)과 원칙을 따르세요. 이 파일은 워크플로우만 정의하며, 판단 기준은 모두 core_system_prompt.md가 우선합니다.

## 레포 구조

```
docs/core_system_prompt.md   ← 최상위 정체성 & 원칙 (모든 작업의 기준)
intake.md                    ← 사용자가 입력한 프로젝트 정보
prompts/                     ← 문서별 역할 프롬프트 (planner / designer / developer)
templates/                   ← 문서 구조 템플릿 (product / design / development / collaboration)
```

## 문서 생성 워크플로우

사용자가 문서 생성을 요청하면:

1. `docs/core_system_prompt.md`를 읽고 AI Project Architect로서 작업한다.
2. `intake.md`를 읽는다. 비어 있거나 "미정"인 항목은 되묻지 말고 합리적으로 추론하되, 아키텍처·기술 선택·워크플로우 등 **중요한 결정을 바꾸는 정보만** 사용자에게 질문한다.
3. 역할 프롬프트와 템플릿을 조합해 문서를 생성한다:

   | 생성 문서 | 역할 프롬프트 | 구조 템플릿 |
   |---|---|---|
   | `product.md` | `prompts/planner.md` | `templates/product.md` |
   | `design.md` | `prompts/designer.md` | `templates/design.md` |
   | `development.md` | `prompts/developer.md` | `templates/development.md` |
   | `collaboration.md` | (별도 역할 없음 — Architect가 직접) | `templates/collaboration.md` |

4. 출력 위치: 사용자가 지정한 폴더, 지정이 없으면 `output/`.

## 생성 시 판단 기준

- **가치 없는 섹션은 생성하지 않는다.** "보통 프로젝트에 있으니까" 만드는 문서·섹션은 금지.
- **모든 추천에는 근거를 붙인다.** 기술/아키텍처 추천 시 왜, 트레이드오프, (필요시) 대안을 함께 쓴다.
- **팀 규모에 맞게 스케일한다.** 1인 개발이면 프로세스·협업 문서를 최소화하고, 팀이 커질수록 조율·거버넌스를 강화한다.
- **프로젝트 유형에 맞게 적응한다.** 웹/모바일/익스텐션/CLI/API 등 해당 없는 플랫폼 섹션은 삭제한다.
- **완료 전 리뷰한다.** 중복 정보, 약한 추천, 범용적 조언을 제거한 뒤 결과를 전달한다.
