---
name: docs-knowledge
description: This skill should be used when a user asks about the llms.txt standard, wants to find or fetch official documentation for a library or framework, or discusses documentation accessibility for AI systems. Example queries include "llms.txt란 뭐야", "React 공식 문서 찾아줘", "fetch the Next.js docs", "documentation for LLMs", "AI용 문서 접근성", "라이브러리 문서 가져와", "which sites have llms.txt", "공식 문서 URL 알려줘", "docs index for Stripe".
---

# Documentation Knowledge Base

Provide knowledge about the llms.txt standard and official documentation access patterns for AI/LLM systems.

## What is llms.txt

llms.txt is a convention where websites provide an AI-readable index of their documentation at their root URL (similar to robots.txt for search engines). It enables LLMs to accurately reference official documentation instead of relying on potentially outdated training data.

### llms.txt vs llms-full.txt

Two variants exist:

| File | Content | When to use |
|------|---------|-------------|
| `llms.txt` | Index/table of contents with links to individual pages | When looking for a specific topic — fetch index, find relevant link, then fetch that page |
| `llms-full.txt` | Entire documentation concatenated into one file | When needing broad overview or the index doesn't contain enough detail. Caution: can be very large |

Always prefer `llms.txt` (the index) first. Only fetch `llms-full.txt` if the index doesn't contain what is needed or if the user explicitly wants comprehensive coverage.

### docs_map.md (variant)

Some sites use a different filename with the same purpose. For example, Claude Code uses `claude_code_docs_map.md` instead of `llms.txt`. The format differs but the concept is identical: an AI-readable index of all documentation pages.

## Documentation Retrieval Strategy

### Step 1: Check known sites list

Refer to `${CLAUDE_PLUGIN_ROOT}/skills/docs-knowledge/references/llms-txt-sites.md` for the maintained list of known llms.txt URLs. If the requested library is in the list with "verified" status, use that URL directly.

### Step 2: Try llms.txt URL patterns

If the library is not in the known list, identify the official documentation site and try these URLs in order:

1. `{official-site}/llms.txt`
2. `{official-site}/docs/llms.txt`
3. `{official-site}/llms-full.txt`
4. `{official-site}/docs/llms-full.txt`

A successful fetch returns a structured document containing links to documentation pages. If a URL returns 404 or non-documentation content, move to the next pattern.

### Step 3: WebSearch fallback

If no llms.txt variant is found:

1. WebSearch: `{library name} official documentation {topic}`
2. Identify the official domain from search results (avoid third-party tutorials)
3. WebFetch the official documentation page directly

### Step 4: Present results

After fetching documentation content:

1. Extract the section relevant to the user's question
2. Explain clearly in the user's language
3. Include code examples from the docs when available
4. Cite the source URL at the end of the response

## Error Handling

| Situation | Action |
|-----------|--------|
| llms.txt returns 404 | Silently try next URL pattern, then fall back to WebSearch |
| WebFetch fails or times out | Try an alternative URL or inform user |
| Content is not documentation | Discard and try WebSearch instead |
| Redirect to different domain | Follow the redirect and fetch from new URL |
| No documentation found at all | Inform user and ask them to provide the docs URL |

## Response Format

Present documentation findings as:

```
[Clear explanation based on fetched documentation]

[Code examples if present in the docs]

---
Source: [URL(s) fetched]
```

Match the user's language. If they ask in Korean, explain in Korean. If English, respond in English.

## Known Sites

Refer to `${CLAUDE_PLUGIN_ROOT}/skills/docs-knowledge/references/llms-txt-sites.md` for the maintained list. Entries marked "verified" have been confirmed to serve valid llms.txt content. Entries marked "unverified" are best-guess URLs that may or may not exist — always handle gracefully with fallback.
