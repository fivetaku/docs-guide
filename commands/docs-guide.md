---
name: docs-guide
description: Fetch and explain official docs for any library
argument-hint: "[library] [question]"
allowed-tools:
  - WebFetch
  - WebSearch
  - Read
  - Agent
---

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

Use the docs-guide agent (Agent tool with subagent_type "docs-guide:docs-guide") to fetch and explain the documentation. Pass the library name and question to the agent.

## Response

- Match user's language (Korean → Korean, English → English)
- Include code examples from the docs when relevant
- Always cite source URL at the end
