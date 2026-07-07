# Capability 추가 컨벤션

AI Project Architect의 새 기능은 기존 모듈을 수정하지 않고 **새 capability 모듈을 추가**하는 방식으로 확장합니다.

## 모듈 구조

```
capabilities/<kebab-case-이름>/
├── capability.md      필수 — 실행 프롬프트
├── README.md          선택 — 한 줄 계약: "입력 X → 출력 Y", 의존 capability, 상태
├── roles/             선택 — 하위 역할 프롬프트
└── templates/         선택 — 이 모듈 전용 출력 템플릿
```

- 이름은 산출물이나 동작을 드러내는 kebab-case로 짓습니다. (예: `blueprint-generator`, `artifact-planner`, `github-bootstrap`)
- capability 전용 템플릿·역할은 모듈 안에 둡니다. 최상위에 공유 폴더를 만들지 않습니다 — 모듈 간 결합을 막기 위해서입니다.
- 실행 프롬프트 없이 UI나 외부 도구가 인터페이스인 경우(예: intake-generator), capability.md 대신 README.md로 계약만 명시합니다.

## capability.md 공통 스키마

기존 capability들이 따르는 형식을 표준으로 합니다:

```
# PURPOSE          이 capability의 책임 (한 문단)
# INPUT            입력 산출물 — 파일명 명시 (예: project.blueprint.md)
# OBJECTIVE        출력 산출물 — 파일명 명시 (예: artifact.plan.md)
# DESIGN PRINCIPLES
# OUTPUT STRUCTURE 출력 문서의 섹션 구조
# QUALITY REVIEW   완료 전 자기 검토 기준
# ABSOLUTE RULE    이 capability의 절대 원칙
```

핵심 계약은 **INPUT/OBJECTIVE의 파일명**입니다. capability는 산출물 파일을 통해서만 연결되고,
다른 capability의 프롬프트를 직접 참조하지 않습니다.

## 추가 절차

1. `capabilities/<이름>/` 폴더와 capability.md를 만든다.
2. `core/workflow.md`의 파이프라인에 등록한다. — **기존 모듈 수정은 이 한 줄이 전부여야 합니다.**
3. 파이프라인에 사용자 단계가 생기면 `app/` UI의 스텝을 같은 변경에서 함께 추가한다.

## 생성기(generator) capability의 추가 규칙

산출물을 만드는 capability는 `capabilities/artifact-generator/capability.md`의 **Generator Contract**도 함께 따릅니다
(디스패치 패키지 수신 → 산출물 + 상태 반환). 오케스트레이터인 artifact-generator 자체는 수정하지 않습니다.

## 예정된 capability

prompt-generator · github-bootstrap · documentation-generator ·
architecture-planner · task-planner · ai-workflow · review-engine · release-manager

- AI별 적응 지식(Claude/Codex/Gemini용 프롬프트 변환 규칙)은 `adapters/ai/`에, 프로젝트 유형별 적응 지식(web/mobile/cli …)은 `adapters/project/`에 둡니다. 두 폴더는 첫 파일이 생길 때 만듭니다.
- 완성된 파이프라인 산출물 예시(intake → blueprint → plan)는 `examples/`에 둡니다.
