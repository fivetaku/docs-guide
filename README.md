English | [한국어](README.ko.md)

# docs-guide

> **Official documentation, fetched live — not from memory.**

Stop trusting AI to recall the right API. docs-guide fetches the actual source directly from official documentation sites so you always get accurate, version-matched answers.

[Quick Start](#quick-start) • [Why docs-guide?](#why-docs-guide) • [How it works](#how-it-works) • [Commands](#commands--skills) • [Requirements](#requirements) • [License](#license)

---

## Quick Start

### 1. Add the marketplace

```
/plugin marketplace add https://github.com/fivetaku/gptaku_plugins.git
```

### 2. Install the plugin

```
/plugin install docs-guide
```

### 3. Restart Claude Code

Cache loads on startup — a restart is required after installation.

### 4. Ask about any library

```
Explain Next.js App Router caching from official docs
```
```
/docs-guide stripe webhooks
```
```
/docs-guide fastapi dependency injection
```

---

## Why docs-guide?

- **Official source, not AI memory** — Fetches live documentation so answers match the version you are actually using
- **Version-aware** — Reads your `package.json`, `requirements.txt`, or other dependency files to detect the installed version and fetch matching docs
- **68+ libraries pre-mapped** — React, Next.js, Vue, Django, Stripe, Prisma, Supabase, LangChain, and many more have verified llms.txt URLs ready to go
- **Smart fallback chain** — When llms.txt is unavailable, falls back to GitHub raw markdown, sitemap.xml, platform-specific indexes, then WebSearch
- **Always cites the source** — Every response ends with the URL fetched, version detected, and retrieval method used
- **Works naturally** — No slash command required; just mention "official docs" or ask a library-specific question

---

## How it works

```
User: "React useEffect cleanup — official docs"
            │
            ▼
  Step 0: Scan project deps
  (package.json → React 19 detected)
            │
            ▼
  Step 1: Check known sites list
  (react.dev in 68+ verified URLs)
            │
            ▼
  Step 2: Fetch react.dev/llms.txt
  (index of all doc pages)
            │
            ▼
  Step 3: Find relevant page URL
  → /reference/react/useEffect
            │
            ▼
  Step 4: WebFetch the page
  (reads actual documentation content)
            │
            ▼
  Response: explanation + code examples
  Source: https://react.dev/reference/react/useEffect
  (version: React 19 | method: llms.txt)
```

**llms.txt** is an AI-readable documentation standard — a machine-friendly index at the site root, similar to `robots.txt`. Sites that publish it let AI systems navigate their docs precisely without hallucination. For sites without llms.txt, the plugin falls back gracefully through GitHub, sitemaps, and WebSearch.

---

## Commands / Skills

### Commands

| Command | Arguments | Description |
|---------|-----------|-------------|
| `/docs-guide` | `[library] [question]` | Fetch and explain official docs for any library |

**Examples:**

```
/docs-guide react useEffect
/docs-guide next.js app router caching
/docs-guide django ORM
/docs-guide stripe webhooks
```

If called without arguments, the command prompts for library and topic.

### Agent

| Agent | Role |
|-------|------|
| `docs-guide` | Core engine — detects project version, fetches docs via llms.txt or fallbacks, explains with source citation |

### Skill

| Skill | Role |
|-------|------|
| `docs-guide-knowledge` | llms.txt pattern knowledge, 68+ verified site URLs, fallback strategy index |

---

## Supported Libraries (sample)

| Domain | Libraries |
|--------|-----------|
| Frontend | React, Next.js, Vue, Svelte, Angular, Astro, Nuxt |
| Backend | Django, FastAPI, Hono |
| Database | Prisma, Supabase, Drizzle ORM, MongoDB, Redis |
| Payments / Auth | Stripe, Clerk, Auth0 |
| Cloud | Vercel, Docker, AWS, Cloudflare, Netlify |
| AI / ML | LangChain, CrewAI, OpenAI, Mistral |
| Build tools | Vite, Vitest, Bun, Deno, Turborepo |
| Mobile | React Native, Expo |

Full list: `skills/docs-guide-knowledge/references/llms-txt-sites.md`

Libraries not in the list are supported automatically if their official site publishes `llms.txt`. Otherwise the fallback chain handles retrieval.

---

## Requirements

- [Claude Code](https://docs.anthropic.com/claude-code) CLI
- Claude Max/Pro subscription or a supported Claude API key

No additional dependencies. No build step.

---

## License

MIT — [fivetaku](https://github.com/fivetaku)

---

<div align="center">

**The docs are always current. Your AI should be too.**

</div>
