# Technical Architecture - {{프로젝트 이름}}

## 기술 스택

### Framework / Runtime

* {{예: Next.js 15 / Spring Boot 3 / Express / Flutter}}
* {{예: React / Vue / 없음(서버 전용)}}
* {{예: TypeScript / Java / Kotlin}}

### UI
<!-- 웹/앱이 아닌 경우 이 섹션 삭제 -->

* {{예: TailwindCSS / MUI / NativeWind}}
* {{예: shadcn/ui / Radix UI}}

### 플랫폼별 추가 항목
<!-- 해당하는 항목만 남기고 나머지 삭제 -->

**웹앱 (Web)**
* 배포: {{예: Vercel / AWS Amplify / Nginx}}
* SSR / CSR / SSG 전략: {{선택}}

**모바일앱 (Mobile)**
* 플랫폼: {{iOS / Android / 크로스플랫폼}}
* 빌드 도구: {{예: Expo / Xcode / Android Studio}}
* 스토어 배포: {{예: App Store / Google Play}}

**크롬 익스텐션 (Chrome Extension)**
* Manifest: {{V3}}
* Content Script
* Background Service Worker
* Side Panel / Popup

**데스크탑 (Desktop)**
* 프레임워크: {{예: Electron / Tauri}}
* 패키징: {{예: electron-builder}}

**CLI / 서버 전용**
* 런타임: {{예: Node.js / Python / Go}}
* 실행 방식: {{예: CLI 커맨드 / cron / daemon}}

### AI
<!-- AI 기능이 없으면 삭제 -->

* {{예: OpenAI API / Claude API / Gemini}}
* 모델: {{예: gpt-4o / claude-sonnet-4-6}}
* Lite 모델: {{예: gpt-4o-mini — 빠른 응답용}}

### Storage

MVP

* {{예: Chrome Storage / localStorage / SQLite / In-memory}}

Future

* {{예: Supabase / PostgreSQL / Redis}}

---

# 프로젝트 구조

```text
project-root

├── {{플랫폼 폴더 — 예: extension / android / electron}}
│
│   ├── {{플랫폼 진입점 파일}}
│   │
│   └── {{플랫폼별 핵심 폴더}}
│       ├── {{파일 1}}
│       └── {{파일 2}}
│
├── src
│
│   ├── app
│
│   ├── api
│   │   ├── {{엔드포인트 1}}
│   │   │   └── route.ts
│   │   │
│   │   └── {{엔드포인트 2}}
│   │       └── route.ts
│
│   ├── features
│   │
│   │   ├── {{도메인 1}}
│   │   ├── {{도메인 2}}
│   │   └── {{도메인 3}}
│
│   ├── lib
│   │
│   │   ├── {{핵심 유틸 1}}
│   │   └── {{핵심 유틸 2}}
│
│   └── shared
│
│       ├── types
│       ├── constants
│       └── utils
│
└── package.json
```

---

# 주요 도메인

## {{도메인 1}}

```ts
interface {{도메인 1}} {
  id: string;
  {{필드명}}: {{타입}};
  {{필드명}}: {{타입}};
}
```

## {{도메인 2}}

```ts
interface {{도메인 2}} {
  id: string;
  {{필드명}}: {{타입}};
  {{필드명}}: {{타입}};
}
```

## {{도메인 3}}

```ts
interface {{도메인 3}} {
  {{필드명}}: {{타입}};
  {{필드명}}: {{타입}};
  {{옵셔널 필드}}?: {{타입}};
}
```

이 타입을 먼저 확정한 뒤 개발을 시작한다.

이 타입이 API 계약서 역할을 한다.

---

# API
<!-- REST API가 없으면 이 섹션 삭제 또는 WebSocket / gRPC 등으로 교체 -->

## POST /api/{{엔드포인트 1}}

{{한 줄 설명}}

```text
{{입력값 설명}}
↓
{{처리 단계 1}}
↓
{{처리 단계 2}}
↓
{{출력값 설명}}
```

---

## POST /api/{{엔드포인트 2}}

{{한 줄 설명}}

```text
{{입력값}}
↓
{{처리 단계}}
↓
{{출력값}}
```

---

## POST /api/{{엔드포인트 3}}

{{한 줄 설명}}

```text
{{입력값 A}}
+
{{입력값 B}}
↓
{{처리 결과}}
```

---

# {{핵심 모듈}} 구조
<!-- AI 프롬프트, 파서, 어댑터 등 핵심 내부 모듈이 있을 때 사용 -->
<!-- 없으면 삭제 -->

```text
src/lib/{{모듈 폴더}}

├── {{파일 1}}.ts
├── {{파일 2}}.ts
├── {{파일 3}}.ts
└── {{파일 4}}.ts
```

각 {{모듈}}은 독립적으로 관리한다.

절대로 하나의 거대한 {{모듈}}로 합치지 않는다.

---

# 핵심 처리 플로우

```text
사용자

↓

{{진입점 — 예: 버튼 클릭 / API 호출 / 앱 실행}}

↓

{{처리 단계 1}}

↓

{{처리 단계 2}}

↓

{{처리 단계 3}}

↓

{{처리 단계 4}}

↓

{{처리 단계 5}}

↓

{{최종 결과 상태}}
```

---

# 담당자 분업

## 개발자 A — {{담당 역할 이름}}

### 담당 폴더

```text
{{폴더 1}}

{{폴더 2}}

{{폴더 3}}
```

### 담당 기능

{{기능 그룹 1}}

```text
{{세부 기능 A}}

{{세부 기능 B}}
```

---

{{기능 그룹 2}}

```text
{{세부 기능 A}}

{{세부 기능 B}}
```

---

### 책임

{{이 개발자가 최종적으로 책임지는 품질 기준 한 문장}}

---

## 개발자 B — {{담당 역할 이름}}

### 담당 폴더

```text
{{폴더 1}}

{{폴더 2}}
```

### 담당 기능

{{기능 그룹 1}}

```text
{{세부 기능 A}}

{{세부 기능 B}}
```

---

{{기능 그룹 2}}

```text
{{세부 기능 A}}

{{세부 기능 B}}
```

---

### 책임

{{이 개발자가 최종적으로 책임지는 품질 기준 한 문장}}

---

# 공통 작업

```text
src/shared/types
```

### 우선 작성

```ts
{{타입 1}}
{{타입 2}}
{{타입 3}}
{{타입 4}}
{{타입 5}}
```
