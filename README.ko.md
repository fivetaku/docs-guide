[English](README.md) | 한국어

# docs-guide

> **공식문서를 기억이 아닌 실시간으로 가져옵니다.**

AI가 API를 제대로 기억할 거라는 믿음은 접어두세요. docs-guide는 공식 문서 사이트에서 실제 내용을 직접 가져와, 버전에 맞는 정확한 답변을 제공합니다.

[빠른 시작](#빠른-시작) • [왜 docs-guide인가?](#왜-docs-guide인가) • [동작 방식](#동작-방식) • [명령어 / 스킬](#명령어--스킬) • [요구사항](#요구사항) • [라이선스](#라이선스)

---

## 빠른 시작

### 1. 마켓플레이스 등록 (처음 한 번만)

```
/plugin marketplace add https://github.com/fivetaku/gptaku_plugins.git
```

### 2. 플러그인 설치

```
/plugin install docs-guide
```

### 3. Claude Code 재시작

캐시는 시작 시 로드됩니다 — 설치 후 반드시 재시작하세요.

### 4. 라이브러리에 대해 물어보기

```
Next.js App Router 캐싱 공식문서 기반으로 설명해줘
```
```
/docs-guide stripe webhooks
```
```
/docs-guide fastapi dependency injection
```

---

## 왜 docs-guide인가?

- **AI 기억이 아닌 공식 출처** — 문서를 실시간으로 가져오므로, 실제로 사용 중인 버전에 맞는 답변을 받을 수 있습니다
- **버전 자동 감지** — `package.json`, `requirements.txt` 등 의존성 파일을 읽어 설치된 버전을 파악하고, 해당 버전 문서를 가져옵니다
- **68개 이상 라이브러리 사전 매핑** — React, Next.js, Vue, Django, Stripe, Prisma, Supabase, LangChain 등 검증된 llms.txt URL 목록이 내장되어 있습니다
- **스마트 폴백 체인** — llms.txt가 없으면 GitHub raw 마크다운 → sitemap.xml → 플랫폼별 인덱스 → WebSearch 순으로 자동 대체합니다
- **항상 출처 표시** — 모든 응답 마지막에 가져온 URL, 감지된 버전, 사용한 방법을 표시합니다
- **자연스럽게 동작** — 슬래시 명령어 없이도 "공식문서", "official docs" 등의 키워드나 라이브러리 관련 질문만으로 자동 실행됩니다

---

## 동작 방식

```
사용자: "React useEffect cleanup 공식문서 기반으로 설명해줘"
            │
            ▼
  Step 0: 프로젝트 의존성 확인
  (package.json → React 19 감지)
            │
            ▼
  Step 1: 알려진 사이트 목록 확인
  (react.dev — 68개 이상 검증된 URL 중 확인)
            │
            ▼
  Step 2: react.dev/llms.txt 가져오기
  (모든 문서 페이지 인덱스)
            │
            ▼
  Step 3: 관련 페이지 URL 찾기
  → /reference/react/useEffect
            │
            ▼
  Step 4: 해당 페이지 WebFetch
  (실제 문서 내용 읽기)
            │
            ▼
  응답: 설명 + 코드 예제
  Source: https://react.dev/reference/react/useEffect
  (version: React 19 | method: llms.txt)
```

**llms.txt**는 AI가 문서를 쉽게 읽을 수 있도록 만들어진 표준입니다. `robots.txt`처럼 사이트 루트에 위치하는 AI용 문서 인덱스로, 이 파일이 있으면 AI가 추측 없이 정확한 페이지를 찾아갈 수 있습니다. llms.txt가 없는 사이트는 GitHub, sitemap, WebSearch 순으로 자동 대체합니다.

---

## 명령어 / 스킬

### 명령어

| 명령어 | 인수 | 설명 |
|--------|------|------|
| `/docs-guide` | `[라이브러리] [질문]` | 어떤 라이브러리든 공식문서를 가져와 설명합니다 |

**사용 예시:**

```
/docs-guide react useEffect
/docs-guide next.js app router caching
/docs-guide django ORM
/docs-guide stripe webhooks
```

인수 없이 실행하면 라이브러리와 주제를 물어봅니다.

### 에이전트

| 에이전트 | 역할 |
|----------|------|
| `docs-guide` | 핵심 엔진 — 프로젝트 버전 감지 후 llms.txt 또는 폴백 방식으로 문서를 가져와 출처와 함께 설명 |

### 스킬

| 스킬 | 역할 |
|------|------|
| `docs-guide-knowledge` | llms.txt 패턴 지식, 68개 이상 검증된 사이트 URL, 폴백 전략 인덱스 |

---

## 지원 라이브러리 (예시)

| 분야 | 라이브러리 |
|------|-----------|
| 프론트엔드 | React, Next.js, Vue, Svelte, Angular, Astro, Nuxt |
| 백엔드 | Django, FastAPI, Hono |
| 데이터베이스 | Prisma, Supabase, Drizzle ORM, MongoDB, Redis |
| 결제 / 인증 | Stripe, Clerk, Auth0 |
| 클라우드 | Vercel, Docker, AWS, Cloudflare, Netlify |
| AI / ML | LangChain, CrewAI, OpenAI, Mistral |
| 빌드 도구 | Vite, Vitest, Bun, Deno, Turborepo |
| 모바일 | React Native, Expo |

전체 목록: `skills/docs-guide-knowledge/references/llms-txt-sites.md`

목록에 없는 라이브러리도 공식 사이트에 `llms.txt`가 있으면 자동으로 지원됩니다. 없으면 폴백 체인이 처리합니다.

---

## 요구사항

- [Claude Code](https://docs.anthropic.com/claude-code) CLI
- Claude Max/Pro 구독 또는 지원되는 Claude API 키

추가 의존성 없음. 빌드 과정 없음.

---

## 라이선스

MIT — [fivetaku](https://github.com/fivetaku)

---

<div align="center">

**문서는 항상 최신입니다. AI도 그래야 합니다.**

</div>
