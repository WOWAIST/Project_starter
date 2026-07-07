# Project Starter

새 프로젝트를 시작할 때 필요한 **기획 / 디자인 / 개발 / 협업 문서**를 Claude와 함께 빠르게 만들어주는 템플릿 레포입니다.

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

### Step 3 — Claude에게 문서 생성 요청

Claude Code(또는 Claude.ai)에서 아래와 같이 요청합니다.

```
# 전체 문서 한 번에 생성
Project_starter 레포를 참고해서 product/design/development/collaboration 문서를 생성해줘

# 개별 문서만 생성
intake.md를 바탕으로 prompts/planner.md 역할로 PRD를 작성해줘
intake.md를 바탕으로 prompts/designer.md 역할로 디자인 문서를 작성해줘
intake.md를 바탕으로 prompts/developer.md 역할로 개발 문서를 작성해줘
```

### Step 4 — 생성된 문서 확인 & 수정

Claude가 `output/` 폴더(또는 지정한 위치)에 문서를 생성합니다.  
팀과 함께 검토하고 필요한 부분을 수정하세요.

---

## 파일 구조

```
Project_starter/
│
├── intake.md                  ← ✏️  여기를 채워주세요 (프로젝트 정보 입력)
│
├── templates/                 ← 문서 구조 템플릿 (Claude가 참고)
│   ├── product.md             PRD 구조
│   ├── design.md              디자인 문서 구조
│   ├── development.md         개발 문서 구조
│   └── collaboration.md       협업 가이드 구조
│
├── prompts/                   ← Claude 역할 프롬프트
│   ├── planner.md             기획자 역할 지시
│   ├── designer.md            디자이너 역할 지시
│   └── developer.md           개발자 역할 지시
│
└── README.md
```

---

## 각 문서 설명

| 문서 | 내용 | 주요 독자 |
|------|------|---------|
| `product.md` | PRD — 기능 정의, 사용자 스토리, 성공 지표 | 전체 팀 |
| `design.md` | 디자인 시스템, 컬러/타이포, 주요 화면 | 디자이너, 프론트 |
| `development.md` | 아키텍처, API 설계, 기술 스택, 배포 전략 | 개발팀 |
| `collaboration.md` | Git 컨벤션, PR 규칙, 회의 방식 | 전체 팀 |

---

## 팁

- **intake.md를 자세히 채울수록** 문서 품질이 높아집니다.
- 문서 생성 후 팀 전체가 함께 리뷰하고 수정하는 것을 권장합니다.
- 프로젝트 진행 중 요구사항이 바뀌면 intake.md를 업데이트하고 다시 생성할 수 있습니다.
- `prompts/` 파일을 직접 수정해 Claude의 문서 작성 방식을 커스터마이징할 수 있습니다.
