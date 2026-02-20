# docs-guide

> Fetch and explain official documentation for any library/framework.
> 아무 라이브러리의 공식문서를 가져와서 설명하는 플러그인.

## What it does / 뭐하는 건가

Built-in `claude-code-guide` only covers Claude Code docs. This plugin extends the same pattern to **any library or framework** using the llms.txt standard.

`claude-code-guide` 내장 에이전트는 Claude Code 문서만 가져옴. 이 플러그인은 같은 패턴을 **아무 라이브러리**로 확장.

## How it works / 작동 방식

```
User: "React useEffect 공식문서 기반으로 설명해줘"
        ↓
1. react.dev/llms.txt fetch → 문서 인덱스 확보
        ↓
2. 인덱스에서 관련 페이지 URL 찾기
        ↓
3. 해당 페이지 WebFetch → 실제 문서 읽기
        ↓
4. 읽은 내용 기반으로 정확하게 설명 + 출처 표시
```

If llms.txt doesn't exist, falls back to WebSearch.

## Components / 구성요소

| Component | Name | Purpose |
|-----------|------|---------|
| Agent | `docs-guide` | Core: fetch docs via llms.txt → WebFetch → explain |
| Skill | `docs-knowledge` | llms.txt pattern knowledge + known URL database |
| Command | `/docs` | Manual trigger: `/docs react hooks` |

## Usage / 사용법

### Automatic (recommended)

Just ask naturally:
```
"Next.js App Router 캐싱 공식문서 확인해줘"
"Explain FastAPI dependency injection from official docs"
"Tailwind CSS 커스텀 색상 설정 공식문서 기반으로"
```

### Manual

```
/docs react useEffect
/docs next.js caching
/docs fastapi dependency-injection
```

## Installation / 설치

```bash
# Local testing
claude --plugin-dir /path/to/docs-guide

# Or copy to plugins directory
cp -r docs-guide ~/.claude/plugins/local/docs-guide
```

## Supported sites / 지원 사이트

See `skills/docs-knowledge/references/llms-txt-sites.md` for the full list.

Any site with llms.txt is automatically supported. Sites without llms.txt use WebSearch fallback.
