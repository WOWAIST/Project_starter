# examples/deepread

AI Project Architect 파이프라인이 처음부터 끝까지 정상 동작하는지 확인하기 위한 **가상 프로젝트 샘플**입니다. 실제 프로젝트가 아니며, 이 스타터 템플릿의 일부로 사용되지 않습니다.

```
사용자 입력 (app/ UI, 실제 폼으로 생성)
    ↓
intake.md
    ↓
project.blueprint.md
    ↓
artifact.plan.md
    ↓
docs/product.md, design.md, development.md, collaboration.md, tasks.md
CLAUDE.md (plan이 지정한 대로 루트 배치)
```

각 파일은 `app/index.html`의 실제 UI 흐름(Step 1~10)을 그대로 밟아 생성했습니다. 새 capability를 추가하거나 파이프라인을 바꿀 때 참고용 예시(few-shot)로 사용하세요.

> 생성일: 2026-07-08. 이후 core/capabilities가 바뀌면 이 예시는 갱신 전까지 스테일할 수 있습니다.
