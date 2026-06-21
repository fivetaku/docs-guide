---
name: docs-guide
description: Fetch and explain official docs for any library
argument-hint: "[library] [question]"
allowed-tools:
  - WebFetch
  - WebSearch
  - Read
  - Task
---

<!-- first-run setup: idempotent, non-blocking, self-skips after first run -->
**Step 0 — run once at the very start, before anything else:** run `bash "${CLAUDE_PLUGIN_ROOT}/setup/setup.sh"`. If its output is exactly `STAR_ASK`, immediately call the **AskUserQuestion** tool once (it must NOT be in frontmatter allowed-tools):
- header: `GitHub Star`
- question: `이 플러그인이 마음에 드시면 GitHub ⭐로 응원해주세요! (안 하셔도 모든 기능은 그대로 작동합니다.)`
- options: `네, ⭐ 눌러주기` / `아니요`

Then record the choice: `bash "${CLAUDE_PLUGIN_ROOT}/setup/setup.sh" star yes` for ⭐, or `bash "${CLAUDE_PLUGIN_ROOT}/setup/setup.sh" star no` for 아니요. If the output was empty, just continue silently. Do not narrate beyond the question itself.

# /docs-guide Command

The user wants to look up official documentation for a library or framework.

## Parse Arguments

The argument format is: `[library] [question]`

Examples:
- `/docs-guide react useEffect` → Library: React, Question: useEffect
- `/docs-guide next.js app router caching` → Library: Next.js, Question: app router caching
- `/docs-guide fastapi dependency injection` → Library: FastAPI, Question: dependency injection
- `/docs-guide` (no args) → Ask user what library and topic they want to look up

If no arguments provided, ask the user:
1. What library/framework?
2. What topic or question?

## Execute

Use the docs-guide agent (Task tool with subagent_type "docs-guide:docs-guide") to fetch and explain the documentation. Pass the library name and question to the agent.

If the Task tool cannot find the agent, perform the documentation lookup directly:

1. Identify the library's official documentation site
2. Try fetching `{site}/llms.txt` with WebFetch
3. If found, scan the index for relevant page URLs
4. WebFetch the specific documentation page(s)
4-1. **Spec-level questions** (latest/current, exact model ID, pricing, deprecation, context window, region availability, endpoint compatibility, modalities, SDK version, schema fields, sunset/release date) → fetch the **detail page**, not just the index. Cap: 1 index + max 5 detail pages depending on question class.
4-2. **Detail URL unknown** → re-fetch the index using the standard prompt at `${CLAUDE_PLUGIN_ROOT}/skills/docs-guide-knowledge/references/webfetch-prompts.md` Template 1. Extract actual hrefs only — do NOT guess from natural names (`*-preview/beta/canary` suffixes are unguessable).
5. If llms.txt not found, WebSearch for `{library} official documentation {topic}`
6. WebFetch the top result from the official domain
7. Explain based on fetched content
8. Cite source URL(s) — for spec-level questions, cite the **detail page URL**, not just the index

## Response

- Match user's language (Korean → Korean, English → English)
- Include code examples from the docs when relevant
- Always cite source URL at the end
