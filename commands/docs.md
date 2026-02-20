---
name: docs
description: Fetch and explain official documentation for any library or framework
argument-hint: "[library] [question]"
allowed-tools:
  - WebFetch
  - WebSearch
  - Read
  - Task
---

# /docs Command

The user wants to look up official documentation for a library or framework.

## Parse Arguments

The argument format is: `[library] [question]`

Examples:
- `/docs react useEffect` → Library: React, Question: useEffect
- `/docs next.js app router caching` → Library: Next.js, Question: app router caching
- `/docs fastapi dependency injection` → Library: FastAPI, Question: dependency injection
- `/docs` (no args) → Ask user what library and topic they want to look up

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
5. If llms.txt not found, WebSearch for `{library} official documentation {topic}`
6. WebFetch the top result from the official domain
7. Explain based on fetched content
8. Cite source URL(s)

## Response

- Match user's language (Korean → Korean, English → English)
- Include code examples from the docs when relevant
- Always cite source URL at the end
