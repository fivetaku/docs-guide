---
name: docs-guide
description: Fetch and explain official docs for any library
argument-hint: "[library] [question]"
allowed-tools:
  - WebFetch
  - WebSearch
  - Read
  - Task
  - Skill
---

<!-- first-run setup: idempotent, non-blocking, self-skips after first run -->
**Step 0 — run once at the very start, before anything else:** run `bash "${CLAUDE_PLUGIN_ROOT}/setup/setup.sh" ask autoupdate`. If its output starts with `STAR_ASK`, immediately call the **AskUserQuestion** tool once, with the question and options phrased **in the user's language**: prefer the current conversation's language if it is evident; otherwise fall back to the language code that follows `STAR_ASK` in the output (`ko`→Korean, `ja`→Japanese, `en`→English). Never default to Korean blindly.
- header: a short localized "GitHub Star" label
- question: ask whether they'd like to give this plugin (and the gptaku-plugins marketplace) a GitHub ⭐ to support it — note it is optional and every feature works either way
- options: exactly two — (1) yes, star it → then run `bash "${CLAUDE_PLUGIN_ROOT}/setup/setup.sh" star yes`; (2) no thanks → then run `bash "${CLAUDE_PLUGIN_ROOT}/setup/setup.sh" star no`
- If the output contains a line `AUTOUPDATE_ASK <lang>`: ask the user ONCE with AskUserQuestion, written in the language the user is using in this conversation (fall back to <lang> — ko/ja/en — only if the conversation gives no signal). Question: gptaku-plugins can update itself automatically so you always get fixes without running /plugin manually. Turn on auto-update? Options (2): "Turn on auto-update" / "Not now".
  - "Turn on auto-update" → run `bash "${CLAUDE_PLUGIN_ROOT}/setup/setup.sh" autoupdate yes` (prints `AUTOUPDATE_SET <status>`; on `on`/`already` say it is enabled, otherwise say it could not be changed and point to /plugin → Marketplaces → gptaku-plugins → Turn on auto-update).
  - "Not now" → run `bash "${CLAUDE_PLUGIN_ROOT}/setup/setup.sh" autoupdate no`.
  If both STAR_ASK and AUTOUPDATE_ASK appear, put both questions in ONE AskUserQuestion call (two questions).

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

(Procedure, argument handling and response rules are also summarized in `${CLAUDE_PLUGIN_ROOT}/skills/docs-guide-knowledge/SKILL.md` — "실행 환경" section.)

If the Task tool cannot find the agent, Read `${CLAUDE_PLUGIN_ROOT}/agents/docs-guide.md` and follow its procedure directly (it is the single source).

If the agent's answer ends with a `HANDOFF: insane-search` block, invoke the `insane-search:insane-search` skill on that URL and finish the answer from what it returns.

## Response

- Match user's language (Korean → Korean, English → English)
- Include code examples from the docs when relevant
- Always cite source URL at the end
