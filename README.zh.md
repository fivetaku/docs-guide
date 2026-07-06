[English](README.md) | [한국어](README.ko.md) | 中文 | [日本語](README.ja.md) | [Español](README.es.md)

# docs-guide

<p align="center">
  <img src="assets/docs-guide-hero-01.png" alt="docs-guide" width="320">
</p>

> **官方文档，实时获取——而不是靠记忆。**

别再指望 AI 能凭记忆答对 API。docs-guide 直接从官方文档站点抓取真实内容，让回答建立在真正的文档之上。当某个来源无法获取时，它会如实说明，并明确声明回退到模型知识——绝不悄悄切换。

[快速开始](#快速开始) • [为什么选 docs-guide](#为什么选-docs-guide) • [工作原理](#工作原理) • [命令 / 技能](#命令--技能) • [环境要求](#环境要求) • [许可证](#许可证)

---

## 快速开始

### 1. 添加市场

```
/plugin marketplace add https://github.com/fivetaku/gptaku_plugins.git
```

### 2. 安装插件

```
/plugin install docs-guide
```

### 3. 重启 Claude Code

缓存在启动时加载——安装后必须重启。

### 4. 询问任意库

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

## 为什么选 docs-guide

- **官方来源，而非 AI 记忆** —— 实时抓取文档，回答匹配你实际使用的版本
- **版本感知** —— 读取 `package.json`、`requirements.txt` 等依赖文件，检测已安装版本并获取对应文档
- **预置 68+ 个库的映射** —— React、Next.js、Vue、Django、Stripe、Prisma、Supabase、LangChain 等已验证的 llms.txt URL 开箱即用
- **智能回退链** —— llms.txt 不可用时，依次回退到 GitHub raw Markdown、sitemap.xml、平台专属索引、WebSearch
- **始终标注来源** —— 每次回答末尾都会附上抓取的 URL、检测到的版本以及使用的获取方式
- **自然触发** —— 无需斜杠命令；提到"官方文档"或提出与某个库相关的问题即可

---

## 工作原理

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

**llms.txt** 是一种面向 AI 的文档标准——位于站点根目录、对机器友好的索引文件，类似 `robots.txt`。发布了它的站点，能让 AI 精准导航文档而不产生幻觉。对于没有 llms.txt 的站点，插件会优雅地回退到 GitHub、sitemap 和 WebSearch。

---

## 命令 / 技能

### 命令

| 命令 | 参数 | 说明 |
|---------|------|------|
| `/docs-guide` | `[库名] [问题]` | 抓取并讲解任意库的官方文档 |

**示例：**

```
/docs-guide react useEffect
/docs-guide next.js app router caching
/docs-guide django ORM
/docs-guide stripe webhooks
```

不带参数运行时，命令会询问库名和主题。

### 代理

| 代理 | 角色 |
|-------|------|
| `docs-guide` | 核心引擎——检测项目版本，通过 llms.txt 或回退方式抓取文档，并附带来源进行讲解 |

### 技能

| 技能 | 角色 |
|-------|------|
| `docs-guide-knowledge` | llms.txt 模式知识、68+ 个已验证站点 URL、回退策略索引 |

---

## 支持的库（示例）

| 领域 | 库 |
|--------|-----------|
| 前端 | React, Next.js, Vue, Svelte, Angular, Astro, Nuxt |
| 后端 | Django, FastAPI, Hono |
| 数据库 | Prisma, Supabase, Drizzle ORM, MongoDB, Redis |
| 支付 / 认证 | Stripe, Clerk, Auth0 |
| 云服务 | Vercel, Docker, AWS, Cloudflare, Netlify |
| AI / ML | LangChain, CrewAI, OpenAI, Mistral |
| 构建工具 | Vite, Vitest, Bun, Deno, Turborepo |
| 移动端 | React Native, Expo |

完整列表：`skills/docs-guide-knowledge/references/llms-txt-sites.md`

不在列表中的库，只要其官方站点发布了 `llms.txt` 就会自动支持。否则由回退链处理。

---

## 环境要求

- [Claude Code](https://docs.anthropic.com/claude-code) CLI
- Claude Max/Pro 订阅，或受支持的 Claude API 密钥

无额外依赖。无构建步骤。

---

## 许可证

MIT — [fivetaku](https://github.com/fivetaku)

---

<div align="center">

**文档是实时拉取的，所以你的回答始终紧跟真实来源。你的 AI 也应如此。**

</div>
