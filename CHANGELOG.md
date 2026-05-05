# Changelog

## [1.3.3] - 2026-05-05 — Spec-Level Drill-Down + URL extraction protocol

> Council 토론 결과 (Codex + Gemini + Claude Chairman) 적용. 이전 실패 사례
> ("Gemini 최신 모델" 인덱스만 fetch → "확인 필요" / 자연 이름 추측 → 404) 재발 방지.

### Added — agents/docs-guide.md
- **Spec-Level Drill-Down Requirement** 신규 섹션
  - Spec-level triggers 14건 (latest/current, exact API ID, pricing, deprecation,
    context window, region, endpoint compatibility, modalities, SDK version, schema, sunset)
  - Drill-down protocol 5단계 (index → href 추출 → detail fetch → cite detail URL)
  - **Drill-down cap (claim-type-bounded)**: general 1+1-2 / spec 1+3 / latest 1+5 / matrix max 8
  - Stop condition: "각 exact claim마다 detail-page source 1+"
- **Quality Gate 분기**: general questions vs spec-level questions
- **Self-reflection checklist**: 4 항목 (index fetched, detail fetched per claim,
  hrefs extracted not guessed, source line cites detail)
- **Error Handling 추가 행 2건**: "Item listed but no detail URL" / "404 on guessed URL → STOP"

### Added — references (신규 파일 2개)
- `references/webfetch-prompts.md` — 표준 WebFetch prompt 3 템플릿
  - Template 1: href extraction (preview/beta/canary 접미사 보존)
  - Template 2: detail page claim extraction (verbatim 인용 강제)
  - Template 3: multi-source cross-validation (authority chain 정의)
  - Anti-patterns (NEVER) / OK patterns (always)
- `references/regression-cases.md` — 8 골든 케이스 (매 릴리즈 전 수동 smoke test)
  1. Gemini 최신 모델
  2. Preview/beta suffix model IDs
  3. Pricing query
  4. Deprecation date / sunset
  5. Context window / token limit
  6. Endpoint compatibility
  7. Migration / breaking changes
  8. Marketing index (Neo4j 등)

### Added — references/llms-txt-sites.md
- ⚠️ LLM Provider 모델 페이지 주의 라벨 (preview/beta 접미사 unguessable, regression cases 안내)
- ⚠️ LLM Provider 문서 업데이트 비대칭 경고 (overview/pricing/changelog 시점 차이)

### Updated
- **agents/docs-guide.md** Quality Gate, Error Handling 강화 (위 참조)
- **skills/docs-guide-knowledge/SKILL.md**:
  - URL Fix Patterns 표 +2 행 (href 추출 / 404 STOP)
  - Quality Gate 분기 (general / spec-level)
- **commands/docs-guide.md**:
  - Direct fallback 경로에 4-1 (spec-level detail fetch), 4-2 (href 추출) 신규 단계
  - Step 8 (cite source) — spec-level은 detail URL 명시

### Council 권고 (적용된 핵심 원리)
- **Index is not evidence** — 인덱스는 라우팅 자료, 인용 근거 아님
- **Names are not locators** — 자연 이름을 URL/API ID로 변환 금지
- **Claim-type gates** — 일반 설명 / exact spec / 가격·날짜·지원상태 각각 다른 증거 기준
- **Bounded drilldown** — 무한 fetch 금지, claim 충족 기준 cap
- **Fail closed on exact facts** — 정확한 값 필요 질문에서 모르면 모른다고

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
