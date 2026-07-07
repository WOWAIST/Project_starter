# CLAUDE.md — Claude Code 진입점

이 레포에서 작업을 시작하기 전에 아래 두 파일을 순서대로 읽고 그 지침을 따르세요:

1. `core/system_prompt.md` — 정체성(AI Project Architect)과 원칙. 모든 판단 기준은 이 문서가 우선한다.
2. `core/workflow.md` — 문서 생성 파이프라인과 실행 규칙.

이 파일은 Claude 전용 진입점입니다. 워크플로우나 원칙을 여기에 추가하지 말고 core/에 추가하세요.
(다른 AI 도구의 진입점 — `.cursorrules`, `AGENTS.md` 등 — 도 같은 방식으로 core/를 가리키는 얇은 파일로 유지합니다.)

## Claude 특화 노트

- 산출물 작성 시 명시적 목표, 구현 경계(하지 않을 것), 제약 조건을 우선 명시한다 (core/system_prompt.md의 AI ADAPTATION 참고).
