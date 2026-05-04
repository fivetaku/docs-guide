# Changelog

## [1.3.2] - 2026-05-04 — llms.txt list expansion 70 → 92

### Added (+22 verified URLs)
- **LLM Providers (+4)**: xAI (Grok), Groq, Ollama, Anyscale
- **AI Frameworks/Agents (+7)**: LangGraph, Vercel AI SDK, Haystack, DSPy, Inspect AI, Mastra, LangChain canonical
- **AI Observability (+6)**: LangFuse, Helicone, PromptLayer, Galileo, Phoenix (Arize), Braintrust
- **AI Dev Tools/Infra (+4)**: Cline, Cerebras, Baseten, NVIDIA NIM
- **Vector / Graph DB (+2)**: Vespa, Zilliz Cloud

### Updated
- **OpenAI**: `platform.openai.com/docs/llms.txt` → `developers.openai.com/api/docs/llms.txt` (canonical, was 301)
- **Anthropic Claude API**: 신규 등록 — `platform.claude.com/llms.txt` (was redirected from docs.anthropic.com)
- **Google Gemini API**: 신규 등록 — `ai.google.dev/gemini-api/docs/llms.txt` (비표준 경로, 폴백 체인 미발견)
- Routing note: claude-code-guide = Claude Code/SDK, docs-guide = Claude API/모델 정보

### Removed
- **Astro**: `docs.astro.build/llms.txt` 404 (2026-05-04 검증). fallback-strategies.md 사용.

### Verified
- 기존 70 URL 검증: 68 OK + 1 fail (Astro 제거) — pumasi/Codex 병렬 검증
- 48 신규 후보 탐색: 32 verified, 22 truly new (10 중복 + 1 HTML 반환 제외)
- 3 NOT_FOUND: deepseek, lancedb, typesense (자체 llms.txt 없음)

## [1.3.1] - 2026-05-04

### Changed
- knowledge SKILL.md "Response Format" 섹션 → "권장 — 도메인에 맞춰 가변 가능" qualifier 추가 (fossil v3 처치)

### Preserved
- Source 표기 (URL + 방법) — reproducibility 본질
- llms.txt 표준 활용 + version awareness
- 플랫폼 자동 감지 (Marketing llms.txt 회피, Hugo sites 등)

## [1.3.0] - 2026-02-25

### Changed
- CCPS v2.0 호환: 에이전트 경로, 커맨드 설명, 스킬 확장

## [1.2.0] - 2026-02-24

### Changed
- knowledge 폴더 이름 변경: docs-knowledge → docs-guide-knowledge

## [1.1.0] - 2026-02-23

### Changed
- 커맨드 이름 변경: `/docs` → `/docs-guide` (일관성)

## [1.0.0] - 2026-02-22

### Added
- 최초 릴리스
- llms.txt 기반 공식 문서 검색 스킬
- docs-guide-knowledge 에이전트
- README 한글화
