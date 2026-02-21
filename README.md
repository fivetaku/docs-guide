# docs-guide

> 아무 라이브러리의 **공식문서**를 가져와서 정확하게 설명해주는 Claude Code 플러그인

## 이게 뭔가요?

코딩하다가 "이거 어떻게 쓰는 거지?" 싶을 때, 보통 구글링하거나 ChatGPT에 물어봅니다.
문제는 **AI가 옛날 정보**나 **잘못된 정보**를 알려줄 때가 있다는 겁니다.

이 플러그인은 다릅니다:

1. 라이브러리의 **공식 문서 사이트**에 직접 접속합니다
2. 실제 문서 내용을 **그 자리에서 읽어옵니다**
3. 읽은 내용을 바탕으로 **정확하게** 설명합니다
4. 어디서 가져왔는지 **출처 URL**을 항상 보여줍니다

즉, AI의 기억에 의존하지 않고 **공식문서 원본**을 기반으로 답변합니다.

## 왜 필요한가요?

Claude Code에는 `claude-code-guide`라는 내장 에이전트가 있지만, 이건 Claude Code 자체 문서만 가져옵니다. React, Next.js, Django, Stripe 같은 **다른 라이브러리** 문서는 가져오지 못합니다.

이 플러그인을 설치하면 **68개 이상의 라이브러리** 공식문서를 바로 가져올 수 있고, 목록에 없는 라이브러리도 자동으로 찾아봅니다.

### 지원하는 라이브러리 예시

| 분야 | 라이브러리 |
|------|-----------|
| 프론트엔드 | React, Next.js, Vue, Svelte, Angular, Astro, Nuxt |
| 백엔드 | Django, Hono |
| 데이터베이스 | Prisma, Supabase, Drizzle ORM, MongoDB, Redis |
| 결제/인증 | Stripe, Clerk, Auth0 |
| 클라우드 | Vercel, Docker, AWS, Cloudflare, Netlify |
| AI/ML | LangChain, CrewAI, OpenAI, Mistral |
| 빌드 도구 | Vite, Vitest, Bun, Deno, Turborepo |
| 모바일 | React Native, Expo |

전체 목록은 `skills/docs-knowledge/references/llms-txt-sites.md`에서 확인할 수 있습니다.
목록에 없는 라이브러리도 공식 사이트에 `llms.txt`가 있으면 자동으로 지원되고, 없으면 WebSearch로 찾아봅니다.

## 어떻게 동작하나요?

```
"React useEffect 공식문서 기반으로 설명해줘"
        ↓
1. react.dev/llms.txt 접속 → 문서 페이지 목록 확보
        ↓
2. 목록에서 useEffect 관련 페이지 URL 찾기
        ↓
3. 해당 페이지에 직접 접속 → 실제 문서 내용 읽기
        ↓
4. 읽은 내용 기반으로 설명 + 코드 예제 + 출처 URL 표시
```

`llms.txt`는 AI가 문서를 쉽게 읽을 수 있도록 만들어진 표준 형식입니다.
이 파일이 없는 사이트는 GitHub 소스, sitemap.xml, WebSearch 순서로 자동 대체합니다.

## 설치 방법

### 방법 1: Claude Code에게 말하기 (가장 쉬움)

Claude Code를 열고 아래처럼 입력하세요:

```
이 플러그인 설치해줘: https://github.com/fivetaku/gptaku_plugins.git
```

Claude Code가 마켓플레이스 등록과 설치를 알아서 진행합니다.
설치가 끝나면 Claude Code를 **재시작**하세요.

### 방법 2: 슬래시 명령어로 설치

마켓플레이스 등록 후 한 줄로 설치:

```
/plugin marketplace add https://github.com/fivetaku/gptaku_plugins.git
/plugin install docs-guide
```

설치 후 Claude Code를 **재시작**하세요.

### 방법 3: 수동 설치

```bash
git clone https://github.com/fivetaku/gptaku_plugins.git
claude --plugin-dir /path/to/gptaku_plugins/docs-guide
```

또는 플러그인 디렉토리에 직접 복사:

```bash
cp -r docs-guide ~/.claude/plugins/local/docs-guide
```

## 사용법

슬래시 명령어를 외울 필요 없습니다. **한국어든 영어든** 자연스럽게 물어보면 됩니다.

### 그냥 물어보기 (추천)

```
Next.js App Router 캐싱 공식문서 확인해줘
```
```
Stripe webhook 설정하는 법 알려줘
```
```
Prisma에서 relation 어떻게 쓰는지 공식문서 기반으로
```
```
Explain FastAPI dependency injection from official docs
```

"공식문서", "official docs", "문서 확인" 같은 키워드가 있으면 자동으로 이 플러그인이 동작합니다.
키워드 없이 라이브러리 관련 질문을 해도 필요하다고 판단하면 공식문서를 가져옵니다.

### /docs 명령어 (직접 실행)

```
/docs react useEffect
/docs next.js caching
/docs django ORM
/docs stripe webhooks
```

형식: `/docs [라이브러리] [궁금한 내용]`

## 구성요소

| 종류 | 이름 | 역할 |
|------|------|------|
| Agent | `docs-guide` | 핵심 엔진. 문서 사이트 접속 → 내용 읽기 → 설명 |
| Skill | `docs-knowledge` | llms.txt 패턴 지식 + 68개 사이트 URL 데이터베이스 |
| Command | `/docs` | 수동 실행용 슬래시 명령어 |

## 응답 형식

```
[공식문서 기반 설명]

[관련 코드 예제]

---
Source: https://react.dev/reference/react/useEffect
(version: React 19 | method: llms.txt)
```

항상 **출처 URL**, **버전 정보**, **어떤 방법으로 가져왔는지**를 표시합니다.
문서를 직접 확인하고 싶으면 Source 링크를 클릭하면 됩니다.

## 프로젝트 버전 자동 감지

프로젝트 폴더에 `package.json`, `requirements.txt` 등이 있으면 설치된 라이브러리 버전을 자동으로 감지합니다.

예: `package.json`에 `"next": "15.1.0"`이 있으면 Next.js 15 문서를 가져옵니다.

## 문서를 못 가져오는 경우

일부 사이트는 JavaScript로만 렌더링되어 내용을 읽을 수 없습니다 (예: Apple Developer, Oracle Docs).
이런 경우 AI 지식 기반으로 답변하되, "공식 문서에서 직접 확인하지 못했습니다"라고 알려줍니다.

## 만든 사람

- [fivetaku](https://github.com/fivetaku)
