[English](README.md) | [한국어](README.ko.md) | [中文](README.zh.md) | 日本語 | [Español](README.es.md)

# docs-guide

<p align="center">
  <img src="assets/docs-guide-hero-01.png" alt="docs-guide" width="320">
</p>

> **公式ドキュメントを、記憶ではなくライブで取得。**

AI が正しい API を思い出してくれると信じるのは、もうやめましょう。docs-guide は公式ドキュメントサイトから実際のソースを直接取得し、本物のドキュメントに根ざした回答を返します。ソースを取得できないときはその旨を明示し、モデル知識へのフォールバックを必ず宣言します——黙って切り替えることはありません。

[クイックスタート](#クイックスタート) • [なぜ docs-guide なのか](#なぜ-docs-guide-なのか) • [仕組み](#仕組み) • [コマンド / スキル](#コマンド--スキル) • [要件](#要件) • [ライセンス](#ライセンス)

---

## クイックスタート

### 1. マーケットプレイスを追加

```
/plugin marketplace add https://github.com/fivetaku/gptaku_plugins.git
```

### 2. プラグインをインストール

```
/plugin install docs-guide
```

### 3. Claude Code を再起動

キャッシュは起動時に読み込まれます——インストール後は再起動が必要です。

### 4. 好きなライブラリについて質問する

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

## なぜ docs-guide なのか

- **AI の記憶ではなく公式ソース** —— ドキュメントをライブで取得するので、実際に使っているバージョンに合った回答が得られます
- **バージョン対応** —— `package.json` や `requirements.txt` などの依存関係ファイルを読み取り、インストール済みバージョンを検出して対応するドキュメントを取得します
- **68 以上のライブラリを事前マッピング** —— React、Next.js、Vue、Django、Stripe、Prisma、Supabase、LangChain など、検証済みの llms.txt URL がすぐに使えます
- **スマートなフォールバックチェーン** —— llms.txt が使えないときは GitHub raw マークダウン、sitemap.xml、プラットフォーム別インデックス、WebSearch の順に切り替えます
- **常に出典を明記** —— すべての回答の最後に、取得した URL・検出したバージョン・使用した取得方法を表示します
- **自然に動作** —— スラッシュコマンド不要。「公式ドキュメント」と言うか、ライブラリに関する質問をするだけで動きます

---

## 仕組み

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

**llms.txt** は AI が読みやすいドキュメント標準です——`robots.txt` に似た、サイトルートに置かれる機械向けインデックスです。これを公開しているサイトでは、AI がハルシネーションなしにドキュメントを正確にたどれます。llms.txt がないサイトでは、GitHub、sitemap、WebSearch へと段階的にフォールバックします。

---

## コマンド / スキル

### コマンド

| コマンド | 引数 | 説明 |
|---------|------|------|
| `/docs-guide` | `[ライブラリ] [質問]` | 任意のライブラリの公式ドキュメントを取得して解説します |

**使用例:**

```
/docs-guide react useEffect
/docs-guide next.js app router caching
/docs-guide django ORM
/docs-guide stripe webhooks
```

引数なしで実行すると、ライブラリとトピックを尋ねます。

### エージェント

| エージェント | 役割 |
|-------|------|
| `docs-guide` | コアエンジン——プロジェクトのバージョンを検出し、llms.txt またはフォールバックでドキュメントを取得して、出典付きで解説します |

### スキル

| スキル | 役割 |
|-------|------|
| `docs-guide-knowledge` | llms.txt パターンの知識、68 以上の検証済みサイト URL、フォールバック戦略のインデックス |

---

## 対応ライブラリ（抜粋）

| 分野 | ライブラリ |
|--------|-----------|
| フロントエンド | React, Next.js, Vue, Svelte, Angular, Astro, Nuxt |
| バックエンド | Django, FastAPI, Hono |
| データベース | Prisma, Supabase, Drizzle ORM, MongoDB, Redis |
| 決済 / 認証 | Stripe, Clerk, Auth0 |
| クラウド | Vercel, Docker, AWS, Cloudflare, Netlify |
| AI / ML | LangChain, CrewAI, OpenAI, Mistral |
| ビルドツール | Vite, Vitest, Bun, Deno, Turborepo |
| モバイル | React Native, Expo |

全リスト: `skills/docs-guide-knowledge/references/llms-txt-sites.md`

リストにないライブラリも、公式サイトが `llms.txt` を公開していれば自動的に対応します。ない場合はフォールバックチェーンが処理します。

---

## 要件

- [Claude Code](https://docs.anthropic.com/claude-code) CLI
- Claude Max/Pro サブスクリプション、または対応する Claude API キー

追加の依存関係なし。ビルド手順なし。

---

## ライセンス

MIT — [fivetaku](https://github.com/fivetaku)

---

<div align="center">

**ドキュメントはライブで取得されるから、回答は常に本物のソースを追いかける。あなたの AI もそうあるべきです。**

</div>
