[English](README.md) | [한국어](README.ko.md) | [中文](README.zh.md) | [日本語](README.ja.md) | Español

# docs-guide

<p align="center">
  <img src="assets/docs-guide-hero-01.png" alt="docs-guide" width="320">
</p>

> **Documentación oficial, obtenida en vivo — no de memoria.**

Deja de confiar en que la IA recuerde la API correcta. docs-guide obtiene la fuente real directamente de los sitios de documentación oficial, para que tus respuestas se apoyen en los documentos de verdad. Cuando una fuente no se puede obtener, lo dice y recurre explícitamente al conocimiento del modelo — nunca en silencio.

[Inicio rápido](#inicio-rápido) • [¿Por qué docs-guide?](#por-qué-docs-guide) • [Cómo funciona](#cómo-funciona) • [Comandos](#comandos--skills) • [Requisitos](#requisitos) • [Licencia](#licencia)

---

## Inicio rápido

### 1. Añade el marketplace

```
/plugin marketplace add https://github.com/fivetaku/gptaku_plugins.git
```

### 2. Instala el plugin

```
/plugin install docs-guide
```

### 3. Reinicia Claude Code

La caché se carga al arrancar — es necesario reiniciar después de instalar.

### 4. Pregunta sobre cualquier biblioteca

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

## ¿Por qué docs-guide?

- **Fuente oficial, no memoria de la IA** — Obtiene la documentación en vivo, de modo que las respuestas coinciden con la versión que realmente estás usando
- **Consciente de la versión** — Lee tu `package.json`, `requirements.txt` u otros archivos de dependencias para detectar la versión instalada y obtener la documentación correspondiente
- **68+ bibliotecas premapeadas** — React, Next.js, Vue, Django, Stripe, Prisma, Supabase, LangChain y muchas más tienen URLs de llms.txt verificadas y listas para usar
- **Cadena de fallback inteligente** — Cuando llms.txt no está disponible, recurre a markdown raw de GitHub, sitemap.xml, índices específicos de cada plataforma y, por último, WebSearch
- **Siempre cita la fuente** — Cada respuesta termina con la URL obtenida, la versión detectada y el método de recuperación utilizado
- **Funciona con naturalidad** — Sin comandos de barra obligatorios; basta con mencionar "documentación oficial" o hacer una pregunta sobre una biblioteca

---

## Cómo funciona

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

**llms.txt** es un estándar de documentación legible por IA — un índice apto para máquinas en la raíz del sitio, similar a `robots.txt`. Los sitios que lo publican permiten que los sistemas de IA naveguen su documentación con precisión y sin alucinaciones. Para los sitios sin llms.txt, el plugin recurre de forma ordenada a GitHub, sitemaps y WebSearch.

---

## Comandos / Skills

### Comandos

| Comando | Argumentos | Descripción |
|---------|-----------|-------------|
| `/docs-guide` | `[biblioteca] [pregunta]` | Obtiene y explica la documentación oficial de cualquier biblioteca |

**Ejemplos:**

```
/docs-guide react useEffect
/docs-guide next.js app router caching
/docs-guide django ORM
/docs-guide stripe webhooks
```

Si se invoca sin argumentos, el comando pregunta por la biblioteca y el tema.

### Agente

| Agente | Rol |
|-------|------|
| `docs-guide` | Motor principal — detecta la versión del proyecto, obtiene la documentación vía llms.txt o fallbacks, y la explica citando la fuente |

### Skill

| Skill | Rol |
|-------|------|
| `docs-guide-knowledge` | Conocimiento de patrones llms.txt, 68+ URLs de sitios verificadas, índice de estrategias de fallback |

---

## Bibliotecas compatibles (muestra)

| Ámbito | Bibliotecas |
|--------|-----------|
| Frontend | React, Next.js, Vue, Svelte, Angular, Astro, Nuxt |
| Backend | Django, FastAPI, Hono |
| Bases de datos | Prisma, Supabase, Drizzle ORM, MongoDB, Redis |
| Pagos / Auth | Stripe, Clerk, Auth0 |
| Nube | Vercel, Docker, AWS, Cloudflare, Netlify |
| IA / ML | LangChain, CrewAI, OpenAI, Mistral |
| Herramientas de build | Vite, Vitest, Bun, Deno, Turborepo |
| Móvil | React Native, Expo |

Lista completa: `skills/docs-guide-knowledge/references/llms-txt-sites.md`

Las bibliotecas que no están en la lista se soportan automáticamente si su sitio oficial publica `llms.txt`. En caso contrario, la cadena de fallback se encarga de la recuperación.

---

## Requisitos

- CLI de [Claude Code](https://docs.anthropic.com/claude-code)
- Suscripción Claude Max/Pro o una clave de API de Claude compatible

Sin dependencias adicionales. Sin paso de build.

---

## Licencia

MIT — [fivetaku](https://github.com/fivetaku)

---

<div align="center">

**La documentación se obtiene en vivo, así que tus respuestas siguen la fuente real. Tu IA también debería.**

</div>
