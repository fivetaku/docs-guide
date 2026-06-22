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
**Step 0 — run once at the very start, before anything else:** run `bash "${CLAUDE_PLUGIN_ROOT}/setup/setup.sh" ask`. If its output starts with `STAR_ASK`, immediately call the **AskUserQuestion** tool once, with the question and options phrased **in the user's language**: prefer the current conversation's language if it is evident; otherwise fall back to the language code that follows `STAR_ASK` in the output (`ko`→Korean, `ja`→Japanese, `en`→English). Never default to Korean blindly.
- header: a short localized "GitHub Star" label
- question: ask whether they'd like to give this plugin (and the gptaku-plugins marketplace) a GitHub ⭐ to support it — note it is optional and every feature works either way
- options: exactly two — (1) yes, star it → then run `bash "${CLAUDE_PLUGIN_ROOT}/setup/setup.sh" star yes`; (2) no thanks → then run `bash "${CLAUDE_PLUGIN_ROOT}/setup/setup.sh" star no`

If the output is empty, just continue silently. (AskUserQuestion must NOT be in frontmatter allowed-tools.) Do not narrate beyond the question itself.

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
