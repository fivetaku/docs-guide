---
name: docs-guide-knowledge
description: This skill should be used when a user asks about the llms.txt standard, wants to find or fetch official documentation for a library or framework, or discusses documentation accessibility for AI systems. Example queries include "llms.txt란 뭐야", "React 공식 문서 찾아줘", "fetch the Next.js docs", "documentation for LLMs", "AI용 문서 접근성", "라이브러리 문서 가져와", "which sites have llms.txt", "공식 문서 URL 알려줘", "docs index for Stripe".
---

# Documentation Knowledge Base

Provide knowledge about the llms.txt standard and official documentation access patterns for AI/LLM systems.

## What is llms.txt

llms.txt is a convention where websites provide an AI-readable index of their documentation at their root URL (similar to robots.txt for search engines). It enables LLMs to accurately reference official documentation instead of relying on potentially outdated training data.

### llms.txt vs llms-full.txt

| File | Content | When to use |
|------|---------|-------------|
| `llms.txt` | Index/table of contents with links to individual pages | When looking for a specific topic — fetch index, find relevant link, then fetch that page |
| `llms-full.txt` | Entire documentation concatenated into one file | When needing broad overview. Caution: can be very large and consume context window |

Always prefer `llms.txt` (the index) first. Only fetch `llms-full.txt` if the index doesn't contain what is needed or if the user explicitly wants comprehensive coverage.

### docs_map.md (variant)

Some sites use a different filename with the same purpose. For example, Claude Code uses `claude_code_docs_map.md` instead of `llms.txt`. The format differs but the concept is identical: an AI-readable index of all documentation pages.

## Smart Broad Approach

This skill uses a "Smart Broad" strategy for documentation retrieval:

### When to fetch external docs
- Questions about specific library/framework APIs, configuration, or features
- Setup/installation, migration guides, version-specific behavior
- API references, auth flows, payment integration, DB queries
- User explicitly asks for official documentation

### When to skip fetch
- Basic language syntax (for loop, array methods)
- General CS concepts (REST, closures, design patterns)
- Architecture discussions not tied to a specific library version

### When to disambiguate
- Question uses a generic term (e.g., "Router", "ORM", "auth") that could refer to multiple libraries
- Check project dependencies (package.json, requirements.txt etc.) to narrow down
- If still ambiguous, ask the user to clarify

When confidence is low, answer from knowledge first and offer to fetch official docs as follow-up.

## Project Context Detection

Before doc lookup, the agent can scan the working directory for dependency files:
- `package.json`, `requirements.txt`, `pyproject.toml`, `go.mod`, `Cargo.toml`, `pom.xml`

This enables:
- **Auto version detection**: Fetch docs matching the installed version
- **Disambiguation**: Identify which library the user means
- **Version normalization**: `react 18`, `React v18`, `^18.2.0` → all mean React 18.x

## Documentation Retrieval Strategy

### Step 1: Check known sites list

Refer to `${CLAUDE_PLUGIN_ROOT}/skills/docs-guide-knowledge/references/llms-txt-sites.md` for the maintained list of 68+ known llms.txt URLs.

### Step 2: Try llms.txt URL patterns

If the library is not in the known list, identify the official documentation site and try these URLs in order:

1. `{official-site}/llms.txt`
2. `{official-site}/docs/llms.txt`
3. `{official-site}/llms-full.txt`
4. `{official-site}/docs/llms-full.txt`

### Step 3: Fallback strategies (priority order)

If no llms.txt variant is found, check `${CLAUDE_PLUGIN_ROOT}/skills/docs-guide-knowledge/references/fallback-strategies.md`:

1. **Per-technology strategy** — 40+ technologies with tested best URLs
2. **GitHub raw markdown** — Most reliable for open source (LLM-native format, no rendering issues)
3. **sitemap.xml** — Universal fallback, filter for `/docs/` patterns
4. **Platform-specific signals** — MkDocs `search_index.json`, Sphinx `objects.inv`
5. **WebSearch** — Last resort, prefer official domains

### Step 4: Present results

After fetching documentation content:

1. Extract only the section relevant to the user's question
2. Explain clearly in the user's language
3. Include code examples from the docs when available
4. Note the documentation version if identifiable
5. Cite the source URL and retrieval method at the end

## Quality Gate

- Minimum: at least 1 official URL + 1 specific fact/code from the source
- If insufficient evidence: explicitly state "공식 문서에서 확인하지 못했습니다"
- If user rejects result ("이 문서 아니야"): try next fallback strategy

## Error Handling

| Situation | Action |
|-----------|--------|
| llms.txt returns 404 | Silently try next URL pattern, then fall back |
| llms.txt linked URL returns 404 | Strip `.md` extension and retry |
| Specific path 404 | Try parent path for table of contents |
| GitHub `main` branch 404 | Try `master`, then version branches |
| WebFetch returns empty/JS content | Try GitHub source or answer from knowledge |
| Content is not documentation | Discard and try next strategy |
| No documentation found at all | Inform user, answer from knowledge, suggest they provide URL |

## Known Limitations

- **JS-rendered sites**: `developer.apple.com`, `docs.oracle.com` — WebFetch returns empty. Answer from knowledge.
- **Marketing llms.txt**: `neo4j.com/llms.txt` is marketing index, not docs. Use `neo4j.com/docs/` directly.
- **Hugo sites**: No detectable platform signals. Skip platform detection, go to sitemap/GitHub.

## Response Format

```
[Clear explanation based on fetched documentation]

[Code examples if present in the docs]

---
Source: [URL(s) fetched]
(version: X.Y | method: llms.txt/GitHub/sitemap/WebSearch)
```

Match the user's language. If they ask in Korean, explain in Korean. If English, respond in English.

## Version Awareness

Documentation changes across versions. Always identify the correct version before fetching.

### Version Detection Priority

1. **Project dependencies**: Check `package.json`, `requirements.txt`, `pyproject.toml`, `go.mod` etc. for installed version
2. **User mention**: Parse version from query — "React 19", "Django 5.0", "@18", "^18.2.0"
3. **Normalize variants**: `react 18`, `React v18`, `@18`, `^18.2.0` → all mean React 18.x
4. **Version-specific URLs**: Some sites have versioned paths:
   - Next.js: `/docs/14/getting-started`
   - Django: `/en/5.0/topics/`
   - Python: `/3.12/library/`
5. **Default**: If no version info, use latest stable and note which version

When documentation shows version-specific behavior (e.g., "New in 19.0"), mention it explicitly so the user knows whether it applies to their project.

### Common Version Pitfalls

| Situation | Risk | Action |
|-----------|------|--------|
| User says "React" without version | React 18 vs 19 have significant API differences | Check package.json first |
| Library has LTS and current | User may need LTS-specific docs | Ask if unclear |
| Pre-release/canary docs | May contain unstable APIs | Warn the user |
| Archived docs (e.g., Create React App) | Deprecated project | Note that the project is no longer maintained |

## Disambiguation Patterns

When a user query maps to multiple possible libraries, resolve before fetching.

### By Project Context

Scan dependency files to narrow down:
- "Router" in React project → `react-router-dom`
- "Router" in Express project → `express.Router`
- "ORM" in Python project → check for `sqlalchemy`, `django`, `tortoise-orm`
- "auth" → `next-auth`, `passport`, `firebase-auth`, `supabase-auth` etc.

### By Explicit Ask

If project context is insufficient or ambiguous:
- Ask: "React Router와 Express Router 중 어떤 건가요?" / "Which router — React Router or Express?"
- Provide 2-3 specific options, not open-ended

### Ecosystem Conventions

Some terms have strong ecosystem defaults:
- "middleware" in Next.js → `middleware.ts` (edge middleware)
- "middleware" in Express → `app.use()` pattern
- "store" in Vue → Pinia (modern) or Vuex (legacy)
- "state management" in React → useState/useReducer (built-in) or Zustand/Redux (external)

## Retrieval Optimization

### Token-Aware Fetching

Large documentation files can exhaust context. Follow these practices:

1. **Index first**: Always fetch `llms.txt` (index) before `llms-full.txt` (full dump)
2. **Targeted fetch**: From the index, identify the single most relevant page URL
3. **Section extraction**: After fetching a page, extract only the relevant section — do not include entire page content in response
4. **Progressive depth**: Answer from first fetch. If user asks "더 자세히" or "more detail", fetch additional pages
5. **Multi-page strategy**: For broad topics, fetch up to 3 pages maximum. Summarize connections between pages.

### URL Fix Patterns

Documentation URLs often have inconsistencies. Handle these automatically:

| Pattern | Fix |
|---------|-----|
| `llms.txt` links end in `.md` but 404 | Strip `.md` extension and retry |
| Path returns 404 | Try parent path for table of contents |
| GitHub `main` branch 404 | Try `master`, then version-specific branches (e.g., `v8.17`) |
| URL has trailing slash issues | Try both with and without trailing `/` |

## Concurrency with Other Agents

When the docs-guide agent is invoked alongside other skills or agents:
- Docs-guide handles all external documentation lookup
- Built-in `claude-code-guide` handles Claude Code, Claude Agent SDK, and Claude API questions
- If a question spans both (e.g., "How to use Stripe with Claude Code MCP"), docs-guide handles the Stripe part

## Known Sites

Refer to `${CLAUDE_PLUGIN_ROOT}/skills/docs-guide-knowledge/references/llms-txt-sites.md` for the maintained list of 68+ verified sites.

## Fallback Reference

For technologies without llms.txt, refer to `${CLAUDE_PLUGIN_ROOT}/skills/docs-guide-knowledge/references/fallback-strategies.md` for 40+ technology-specific strategies and platform detection methods.
