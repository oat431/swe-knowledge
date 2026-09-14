# SvelteKit Checklist (Meta-Framework Layer)

> SvelteKit — rendering modes, file routing, load functions, server routes, hooks, SEO, adapters, build & deploy.
> Library layer: [[svelte]] (runes, components, stores, forms, testing, a11y) — tick that first, then this.
> General companion: [[web]] · Launch gate: [[Frontend Launch]]
> Last updated: 2026-09-14 (split from fused svelte.md — library concerns in [[svelte]], SvelteKit concerns here)

> **Gate + link:** this file never re-states Svelte items. If it's about runes, components, stores, client-side data fetching, or form behavior → [[svelte]]. If it's about *where code runs* (server vs client), routing files, load functions, or the SvelteKit build/deploy → here.

---

## 1. SvelteKit Setup

- [ ] **Scaffold** — `npx sv create`. SvelteKit is to Svelte what Next.js is to React: use it for everything except embeddable widgets and pure internal SPAs (Vite path → [[svelte]] §1).
- [ ] **Adapter choice** — `adapter-auto` (detects environment), `adapter-node` (self-hosted Node), `adapter-vercel` / `adapter-cloudflare` / `adapter-netlify` (serverless/edge), `adapter-static` (fully static or SPA). Decide early — server features (form actions, `+server.ts`, SSR) require a non-static adapter or full prerendering.
- [ ] **`$lib` alias** — `$lib` → `src/lib`. Shared code in `$lib/shared`, features in `$lib/features` (→ [[svelte]] §2). No deep relative imports (`../../../`) crossing feature boundaries.
- [ ] **`$env` static vs dynamic** — `$env/static/*` is inlined at build time; `$env/dynamic/*` is read at runtime. `PUBLIC_`-prefixed vars ship to the browser by design — never a secret. Private modules (`$env/static/private`, `$env/dynamic/private`) are server-only and fail the build if imported into client code.

## 2. Rendering Modes

- [ ] **SSR by default** — Pages render on the server and hydrate on the client. Keep the default for anything public; don't blanket-disable SSR.
- [ ] **Prerender** — `export const prerender = true` for public, indexable, change-rarely pages (marketing, docs). Config default `kit.prerender.entries` for whole-site prerendering with `adapter-static`.
- [ ] **CSR-only routes** — `export const ssr = false` per route (or `kit.csr` config) for authenticated dashboards with no SEO value. Content then needs client fetch — invisible to some crawlers (§7).
- [ ] **Per-route overrides via hooks** — `handle()`'s `resolveOptions` (or `resolve()`'s second argument) can set `ssr`/`csr`/`preload` dynamically per request (e.g. disable SSR for bots-vs-users splits).
- [ ] **Hydration correctness** — Server-rendered HTML must match client output (no `Date.now()`/`Math.random()` in first render) or hydration warnings and flicker.

## 3. File Routing & Form Actions

- [ ] **File-based routing** — `src/routes/` directory. `+page.svelte` for UI, `+page.ts` for universal data loading, `+page.server.ts` for server-only logic, `+layout.svelte` for persistent layouts. Nested layouts preserved across navigations — shared chrome doesn't re-render.
- [ ] **Dynamic routes** — `[id]` folders with typed params in `load` functions. Rest routes `[...path]`, optional params `[[lang]]`.
- [ ] **Errors per route segment** — `+error.svelte` per segment. `throw error(404, 'Not found')` in load. `throw redirect(302, '/login')` for auth flows. Root `+error.svelte` handles 404 with `$page.status === 404`.
- [ ] **Form actions** — `+page.server.ts`: `export const actions = { default: async ({ request }) => { ... } }`. Form `method="POST" action="?/actionName"`. Progressive enhancement — works without JavaScript.
- [ ] **`use:enhance`** — SvelteKit's form enhancer: `use:enhance={({ formElement, formData, action, result }) => ...}`. Ajaxy forms without writing fetch calls. Superforms builds on this → [[svelte]] §10.
- [ ] **Navigation guards** — auth checks in `hooks.server.ts` (§6) or layout `load` (throw `redirect`), not only in UI components. One place checks; pages don't re-check.

## 4. Load Functions & Data Loading

- [ ] **`load` functions** — `+page.ts`: `export async function load({ params, fetch, url }) { return { user: await fetchUser(params.id) } }`. Data flows to `+page.svelte` via `let { data } = $props()`.
- [ ] **Universal vs server loads** — `+page.ts` = universal (runs on server AND client for navigation). `+page.server.ts` = server-only (secrets, DB access). Same shape, different trust boundary.
- [ ] **Parallel loading, no waterfalls** — Sibling route `load` functions run in parallel automatically; inside one load use `Promise.all` for independent fetches. Never sequential awaits.
- [ ] **`$page.data` + invalidation** — After mutations: `invalidateAll()` refreshes every load's data, `invalidate(url|fn)` targets one. Real-time events should call these (or write into the TanStack Query cache → [[svelte]] §7) — one source of truth, never a parallel copy.
- [ ] **Streaming via promises** — Return un-awaited promises from `load`; SvelteKit streams them. Page shell renders immediately, slow data arrives inside `{#await}` blocks.
- [ ] **Typed load data** — Import generated `./$types` (`PageData`, `LayoutData`) instead of hand-maintained response types that drift.
- [ ] **`event.fetch` for server fetches** — Deduplicated and cache-aware within a request; carries cookies. Don't use global `fetch` in server loads when `event.fetch` fits.

## 5. Server Routes (`+server.ts`)

- [ ] **API endpoints** — `export async function GET/POST/...({ request, cookies, params })`. Auth + validation on every one — they're public endpoints.
- [ ] **SSE streams** — Return a `ReadableStream` response for one-way server→client streams (notifications, activity feeds, LLM tokens). Default to SSE over WebSocket — simpler, works over HTTP/2, `EventSource` auto-reconnects natively (client side → [[svelte]] §7).
- [ ] **Cookies API** — `cookies.get/set/delete` with `httpOnly`, `secure`, `sameSite` flags. Session tokens never readable by JS.
- [ ] **WebSocket scale awareness** — WebSockets don't scale on serverless adapters. Use SSE, Pusher/Ably, or `adapter-node` / a dedicated WS server. Pub/sub fan-out via Valkey/Redis on the backend.

## 6. `hooks.server.ts`

- [ ] **`handle()` — the auth point** — Every request passes through it: auth guards, header injection, locale detection. Cookie-authenticated SSE/WS sessions are validated here. Never a token in the query string (leaks into logs).
- [ ] **`X-Robots-Tag` for staging** — Set `X-Robots-Tag: noindex` in `handle()` on staging/preview deployments so they never get indexed.
- [ ] **`handleFetch()`** — Control server-side `fetch` from client loads: rewrite internal URLs, attach credentials, enforce allowlists.
- [ ] **`handleError()`** — Centralized error logging (Sentry server SDK), PII scrubbing, safe messages to the client.
- [ ] **Keep it fast** — `handle()` runs on every request. Heavy authorization belongs in `+page.server.ts`/`+server.ts` too — defense in depth, not only middleware-level checks.

## 7. SEO & Metadata

- [ ] **Metadata per route** — Unique `<title>` (50–60 chars) + `<meta name="description">` (140–160) on every page, generated from `load` data via `<svelte:head>`, Svelte 5 snippets-based head management, or `@unhead` — never hard-coded strings. Canonical URLs on any duplicate/parameterized content.
- [ ] **Crawlability first** — SvelteKit SSRs by default; `export const prerender = true` for public, indexable pages. CSR-only content is invisible to some crawlers and social unfurlers — verify with "View source" (not DevTools) that content is in the initial HTML.
- [ ] **Open Graph + Twitter cards** — `og:title`, `og:description`, `og:image` (1200×630), `twitter:card` per route. Test unfurls in Slack/Discord/X validators before launch.
- [ ] **Structured data (JSON-LD)** — `<script type="application/ld+json">{@html JSON.stringify(schema)}</script>` in the head. `Organization`, `Product`, `Article`, `BreadcrumbList` where applicable. Validate with Google Rich Results Test.
- [ ] **`robots.txt` + XML sitemap** — Static files, adapter-generated output (`@sveltejs/adapter-static`), or `svelte-sitemap` at build time. Sitemap with `lastmod`, referenced in robots.txt.
- [ ] **hreflang for multi-locale** — Correct language/region pairs, self-referencing entries, `x-default` — built with `$app/paths` when i18n exists.
- [ ] **Indexability control** — `noindex` on authenticated, duplicate, parameterized, and staging pages. Staging behind auth + `X-Robots-Tag: noindex` set in `hooks.server.ts` (§6).
- [ ] **Search Console + Bing Webmaster** — Registered, sitemap submitted, crawl errors and 404s monitored after launch.
- [ ] **Core Web Vitals are SEO** — LCP/INP/CLS are ranking inputs. Svelte ships less JS than most frameworks by default — keep it that way. Lighthouse SEO audit in CI.

## 8. SvelteKit Performance

- [ ] **Streaming load data** — Un-awaited promises from `load` (§4): faster TTFB, shell paints while slow data resolves.
- [ ] **Preload code + data links** — SvelteKit emits `<link rel="modulepreload">` for route assets; tune with `resolveOptions`' `preloadCode`/`preloadData`. Anchor-level prefetch via `data-sveltekit-preload-data="hover|tap|viewport|off"` and `preloadData()`/`preloadCode()` from `$app/navigation`.
- [ ] **Route-level code splitting** — Automatic per route. Don't defeat it with barrel imports pulling every feature into the entry chunk.
- [ ] **`$app/navigation` awareness** — `beforeNavigate`/`afterNavigate` for teardown/setup at route boundaries; `goto()` with `invalidateAll`/`keepFocus`/`noScroll` options instead of full page loads.
- [ ] **Client-side perf fundamentals** — Compiler optimizations, `{#key}`, image handling, LCP budgets → [[svelte]] §8. SvelteKit adds streaming + splitting on top.

## 9. SvelteKit Security

- [ ] **CSRF protection is built-in** — SvelteKit checks the `Origin` header on form actions and POSTs (`csrf.checkOrigin`, on by default). Understand it; never disable it without a documented reason (e.g. genuine cross-origin webhook — then protect that route yourself).
- [ ] **Protect server routes in the handlers, not just load** — Auth/authorization inside `+page.server.ts` actions and `+server.ts` handlers. A `load` guard alone doesn't protect the mutation endpoint — actions are public POST targets.
- [ ] **No secrets in `$env/static/public` / `PUBLIC_` vars** — inlined into the client bundle at build time. Secrets live in `$env/static/private` / `$env/dynamic/private`, accessed only in `+server.ts`, `+page.server.ts`, and `hooks.server.ts`.
- [ ] **Security headers** — CSP, `X-Content-Type-Options: nosniff`, `Referrer-Policy`, HSTS set in `handle()` (§6) or the platform config. SvelteKit outputs static HTML + minimal JS and works well with strict CSPs (no `unsafe-inline` needed once `enhanced:img` generates proper `srcset` without inline styles).
- [ ] **Cookie flags** — `httpOnly`, `secure`, `sameSite` on every session cookie set via the cookies API (§5). Client-side token storage rules → [[svelte]] §13.

## 10. Build & Deploy

- [ ] **Adapter output is the deploy artifact** — No Nitro-style extra server layer: `adapter-node` → `node build/` behind a process manager (host/port via `HOST`/`PORT`); `adapter-static` → `build/` directory to any CDN/static host; platform adapters zip to their runtime.
- [ ] **Node vs static differences understood** — `adapter-static` requires every page prerenderable (or a SPA `fallback: 'index.html'`); no per-request server code, no form actions, no `+server.ts` dynamism. If you need any of those → `adapter-node`/serverless adapter.
- [ ] **Edge deployment** — `adapter-cloudflare` or `adapter-vercel`. SvelteKit is edge-ready: SSR at the edge with minimal cold start.
- [ ] **Local production build tested** — `vite build && node build` (or platform preview command) before shipping. Prerender failures and `$env` misuse surface here, not in dev.
- [ ] **Environment variables per environment** — `$env/static/*` bakes at build time (rebuild per env); prefer `$env/dynamic/*` + platform runtime env where supported. Documented which are required/optional.
- [ ] **CI/CD** — Lint → `svelte-check` → unit tests → build → deploy preview → E2E → promote. Preview URL per PR.
- [ ] **Source maps** — Uploaded to Sentry via the SvelteKit wizard, never served publicly.

## 11. AI/LLM Integration (Server Side)

- [ ] **Streaming server routes** — Vercel AI SDK: `streamText` + `toDataStreamResponse()` in a `+server.ts` route. Client `useChat` from `@ai-sdk/svelte` parses the stream → [[svelte]] §16.
- [ ] **Never expose provider keys** — `$env/static/public` ships to the browser. LLM keys live in `$env/static/private` / `$env/dynamic/private` only, accessed in `+server.ts` or `+page.server.ts`.
- [ ] **Context & prompt management** — System prompts composed server-side, never in client bundles.
- [ ] **Rate-limit AI endpoints** — Per-user limits on the streaming route (in `handle()` or the route itself); 429 UX on the client → [[svelte]] §16.

---

## Quick Sanity Check

- [ ] Forms work without JavaScript (try disabling JS in DevTools)
- [ ] Back button works (no redirect loops)
- [ ] `+layout.svelte` provides consistent chrome without re-renders — layouts preserve state across navigations by default
- [ ] SSR HTML contains the content — verify with "View source", not DevTools
- [ ] Private env vars in `$env/static/private` — never leaked through `$env/static/public` or `$env/dynamic/public`
- [ ] `robots.txt` and sitemap resolve; OG tags unfurl in Slack/Discord
- [ ] Staging/authenticated pages are `noindex` (meta + `X-Robots-Tag`)
- [ ] 404 `+error.svelte` exists and is helpful
- [ ] `vite build` + production run (`node build` / preview) clean
- [ ] Svelte library items → [[svelte]] Quick Sanity Check

---

## Project Tier Scoping Matrix

> **How to use this table:** Pick your tier first (→ [[svelte]] has the full tier descriptions and flowchart), then focus only on the sections marked ✅ (required) or 🟡 (recommended). Skip ❌ sections entirely.
>
> **Legend:** ✅ Required · 🟡 Recommended / partial · ❌ Skip

### Checklist Applicability by Tier

| # | Section | 🧪 POC | 🔧 Prototype | 🏠 Internal | 🟢 Small Prod | 🔵 Medium Prod | 🟣 Production Grade | 🔴 Mission-Critical |
|---|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 1 | SvelteKit Setup | 🟡 adapter-auto | ✅ | ✅ + node | ✅ + platform | ✅ + edge | ✅ + multi-region | ✅ + signed artifacts |
| 2 | Rendering Modes | 🟡 | ✅ | ✅ | ✅ + prerender plan | ✅ | ✅ | ✅ + formal review |
| 3 | File Routing & Form Actions | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ + audit trail |
| 4 | Load Functions & Data Loading | 🟡 | ✅ | ✅ | ✅ + streaming | ✅ | ✅ + typed contracts | ✅ + formal verification |
| 5 | Server Routes (+server.ts) | 🟡 if used | 🟡 | ✅ + auth | ✅ + rate limits | ✅ + hardened | ✅ + pentest | ✅ + audit |
| 6 | hooks.server.ts | ❌ | 🟡 basics | ✅ auth | ✅ + headers | ✅ + logging | ✅ + hardened | ✅ + formal audit |
| 7 | SEO & Metadata | ❌ | 🟡 if public | 🟡 if public | ✅ if public | ✅ | ✅ + structured data | ✅ |
| 8 | SvelteKit Performance | ❌ | 🟡 basic CWV | ✅ | ✅ + budgets | ✅ + profiling | ✅ + SLO | ✅ + capacity |
| 9 | SvelteKit Security | 🟡 no secrets | 🟡 essentials | ✅ | ✅ + CSP | ✅ + pentest | ✅ + hardened | ✅ + formal audit |
| 10 | Build & Deploy | ❌ | 🟡 basic build | ✅ + CI | ✅ + previews | ✅ + canary | ✅ + full pipeline | ✅ + signed artifacts |
| 11 | AI/LLM (Server Side) | 🟡 if AI is the POC | 🟡 | 🟡 if used | ✅ if used | ✅ | ✅ + guardrails | ✅ + audit trail |

---

## Sources

- Library layer: [[svelte]] · General companion: [[web]] · Launch gate: [[Frontend Launch]]
- Split 2026-09-14 from fused `svelte.md`: SvelteKit-specific concerns here, Svelte library concerns in [[svelte]].
- SvelteKit Docs — https://kit.svelte.dev/ (routing, load, form actions, hooks, adapters, security/CSRF)
