# Nuxt Checklist (Meta-Framework Layer)

> Nuxt 3 — rendering modes, file-based routing, Nitro server routes, SEO, hybrid caching, build & deploy.
> Library layer: [[vue]] (Composition API, components, Pinia, forms, testing, a11y) — tick that first, then this.
> General companion: [[web]] · Launch gate: [[Frontend Launch]]
> Last updated: 2026-09-14 (split from vue-js.md — library concerns here/vue, Nuxt concerns in nuxt)

> **Gate + link:** this file never re-states Vue items. If it's about components, reactivity, Pinia, composables, or forms behavior → [[vue]]. If it's about *where code runs* (server vs client), `pages/` routing files, SEO/hydration, or the Nitro build/deploy → here.

---

## 1. Project Setup (Nuxt-Specific)

- [ ] **`nuxi init` + Nuxt 3.x/4** — TypeScript by default. Dev with `nuxt dev`, typecheck with `nuxt typecheck` (`vue-tsc` under the hood → [[vue]] §1).
- [ ] **Nuxt modules** — `@nuxt/image`, `@nuxt/fonts`, `@nuxt/scripts`, `nuxt-security`. Don't DIY. Each module audited for bundle cost before adding.
- [ ] **Nuxt layers (`extends`)** — Share config/components/modules across apps without copy-paste. `composables/` = logic, `components/` = UI, `server/` = API; server code never imports from the app side.
- [ ] **Directory boundaries** — Feature code stays in feature folders (→ [[vue]] §2); `pages/` stay thin — they compose feature components, they don't contain business logic.
- [ ] **Auto-import caveats** — Auto-imports hide dependencies and break when code moves into shared packages or libraries. Explicit imports in `shared/` and anything extracted from the app. Auto-imports only scan certain directories (`composables/`, `utils/`, `components/`) — files nested deeper or elsewhere need manual imports.
- [ ] **`nuxt.config.ts` discipline** — `app.head` for global meta, `routeRules` for per-route rendering/caching, `nitro.prerender` for static. Config reviewed in PRs like code.

## 2. Rendering Modes (SSR / SSG / Hybrid)

- [ ] **Rendering decision per route** — SSR default for dynamic/public pages; `ssr: false` (SPA mode) only for authenticated app shells with no SEO need; prerender for marketing/docs.
- [ ] **Hybrid rendering via `routeRules`** — `routeRules: { '/blog/**': { swr: 3600 }, '/admin/**': { ssr: false } }`. Per-route strategy: `swr`/`isr` for stale-while-revalidate caching, `prerender` for static.
- [ ] **SSG / prerender** — `nitro.prerender` routes or crawl-links for fully static output. Crawlability: public indexable pages must have content in the initial HTML — verify with "View source" (not DevTools).
- [ ] **Server components / islands** — `<NuxtIsland>` / island components for static pieces inside dynamic pages (heavy, non-interactive content rendered server-only, zero client JS).
- [ ] **Hydration understood** — No client-only data rendered during SSR without `<ClientOnly>` or a placeholder (hydration mismatch errors). No `Date.now()`/random values in SSR-rendered templates.
- [ ] **`ssr: false` is a last resort** — Whole-app SPA mode loses SEO and first-paint benefits; prefer per-route `routeRules`.

## 3. Nitro Server Routes

- [ ] **Server routes** — `server/api/` directory, `defineEventHandler`. Good for lightweight APIs, proxies, webhooks. Auth + Zod validation on every one — they're public endpoints.
- [ ] **Server middleware** — `server/middleware/` for cross-cutting concerns (auth session, logging, request IDs). Keep it fast; per-route authz still re-checked in handlers.
- [ ] **SSE via `createEventStream`** — `server/api/` with h3's `createEventStream` + `defineEventHandler`. Correct `text/event-stream` headers, no proxy buffering. Client side → [[vue]] §7.
- [ ] **Route rules for server endpoints** — `routeRules` can also cache/proxy server routes (`{ '/api/proxy/**': { proxy: '...' } }`).
- [ ] **Server utils shared** — `server/utils/` auto-imported into handlers. DB clients, auth helpers, rate limiters live here — never in app-side code.

## 4. File-Based Routing & Conventions

- [ ] **`pages/` directory** — File-based routes. `[id].vue` dynamic params via `useRoute().params`. `[...slug].vue` catch-all. Route names typed with `nuxt prepare` generated types.
- [ ] **`layouts/`** — `default.vue` plus named layouts (`dashboard.vue`, `auth.vue`). `definePageMeta({ layout: 'dashboard' })` per page. Layouts persist across navigations.
- [ ] **`middleware/`** — Route middleware for auth guards/redirects (`definePageMeta({ middleware: 'auth' })`) and global middleware for cross-cutting checks. Runs on server during SSR and on client navigation — keep it isomorphic and fast.
- [ ] **`error.vue`** — Custom app-level error page with `clearError({ redirect: '/' })`. Not the default Nuxt error screen.
- [ ] **Navigation** — `<NuxtLink>` (prefetches in viewport) over `<a>`; `navigateTo()` in middleware/handlers, `useRouter()` for imperative client nav (→ [[vue]] §8).
- [ ] **`app.vue` / entry conventions** — `<NuxtLayout>` + `<NuxtPage>` wiring understood; `app.config.ts` for reactive app-level config (theme, feature toggles).

## 5. SEO & Metadata

- [ ] **`useHead` / `useSeoMeta`** — Nuxt composables for per-page meta (SSR-safe, reactive). `@unhead/vue` for plain Vite SPAs → [[vue]]. `<Title>`, `<Meta>`, `<Link>`, `<Script>` components for declarative head in templates.
- [ ] **Metadata per route** — Unique `<title>` (50–60 chars), `<meta name="description">` (140–160), canonical URL. Generated from route/page data via `useSeoMeta`, never hard-coded strings.
- [ ] **Open Graph + Twitter cards** — `og:title`, `og:description`, `og:image` (1200×630), `twitter:card` — `useSeoMeta` covers most of it. Dynamic per-route OG images via `nuxt-og-image`. Test unfurls in Slack/Discord/X validators before launch.
- [ ] **Structured data (JSON-LD)** — Inject via `useHead({ script: [{ type: 'application/ld+json', innerHTML: JSON.stringify(schema) }] }` or `useSchemaOrg`. `Organization`, `Product`, `Article`, `BreadcrumbList`, `FAQPage` where applicable. Validate with Google Rich Results Test; only mark up content visible on the page.
- [ ] **`robots.txt` + XML sitemap** — `@nuxtjs/sitemap` module (auto-generated with `lastmod`); sitemap referenced in robots.txt. Segmented sitemaps for large sites (> 50K URLs per file).
- [ ] **hreflang for multi-locale** — With `@nuxtjs/i18n`: correct language/region pairs, self-referencing entries, `x-default`. Only when i18n exists.
- [ ] **Indexability control** — `noindex` on authenticated, duplicate, parameterized, and staging pages. Staging behind auth + `X-Robots-Tag: noindex` (never rely on robots.txt alone to hide content).
- [ ] **Search Console + Bing Webmaster** — Registered, sitemap submitted, crawl errors and 404s monitored after launch.
- [ ] **Core Web Vitals are SEO** — LCP/INP/CLS thresholds are ranking inputs. Lighthouse SEO audit in CI; metadata lint (unique titles, canonical present) for critical routes.

## 6. Performance (Nuxt-Specific)

- [ ] **`useFetch` vs `useAsyncData` discipline** — `useFetch(url)` = `useAsyncData` + `$fetch` shortcut. `useAsyncData` for non-HTTP work or custom fetchers. Never plain `fetch()`/`$fetch` in setup for page data — you lose SSR transfer, dedup, and caching.
- [ ] **Payload & dedup understood** — SSR-fetched data rides in the payload (`__NUXT_DATA__`) and is reused on hydration — no double fetch. Identical `useFetch` keys dedupe automatically; keep keys stable and explicit for shared queries.
- [ ] **Nuxt Image** — `@nuxt/image`: `<NuxtImg>`/`<NuxtPicture>` with `sizes`, `preload` on the LCP image, format negotiation (AVIF/WebP). Provider configured (ipx self-hosted or CDN).
- [ ] **Nuxt Fonts** — `@nuxt/fonts`: automatic self-hosting, subsetting, `font-display: swap`. No render-blocking Google Fonts requests.
- [ ] **Lazy components & islands** — `<LazyHeavyChart>` auto lazy-loads (component named with `Lazy` prefix). Islands (§2) for static heavy fragments. Dynamic imports for editor/chart libs.
- [ ] **Caching via `routeRules`** — `swr`/`isr` per route (§2), plus Nitro's `cachedEventHandler`/`defineCachedFunction` for expensive server computations. Know the TTL of every cached surface.
- [ ] **Payload trimming** — Server handlers return only the fields the client needs, not whole ORM objects. Smaller payload = faster hydration.
- [ ] **Bundle analysis** — `nuxt analyze`. Watch per-route client JS; catch accidentally-large deps and non-tree-shaken imports (→ [[vue]] §2 barrel rule).

## 7. Security (Nuxt-Specific)

- [ ] **Runtime config prefixes** — `runtimeConfig.public.*` (env `NUXT_PUBLIC_*`) ships to the browser — that's the design. Secrets go in non-public `runtimeConfig` (server-only, e.g. `NUXT_SECRET_*` / `nu.authToken`). Never a secret under `public`.
- [ ] **`nuxt-security` module** — Security headers (CSP report-only first, `X-Content-Type-Options`, `Referrer-Policy`, HSTS, `Permissions-Policy`), rate limiting, `X-Powered-By` removed. Configured per environment, not left at defaults.
- [ ] **Server route auth** — Every `server/api/` handler re-checks auth/permissions. Middleware is not the only gate — defense in depth at handler level. Session from HTTP-only cookies, never shipped to client state (→ [[vue]] §15).
- [ ] **Input validation server-side** — Zod on every handler's body/query (client validation is UX only → [[vue]] §11).
- [ ] **SSRF/proxy care** — Server routes that proxy user-supplied URLs validate/allowlist targets. Webhook handlers verify signatures.
- [ ] **Dependency audit** — `pnpm audit` in CI, zero critical/high CVEs; Dependabot/Renovate for patches. Server deps matter more here — Nitro runs your code with real privileges.

## 8. Data Fetching (Server-Side)

- [ ] **`useFetch` / `useAsyncData` built-in** — Auto-deduped, cached, SSR-safe. Replaces manual `fetch` + `useState`. Keys explicit and stable (`useFetch('/api/users', { key: 'users' })`).
- [ ] **`refresh()` for revalidation** — Re-run a query after mutations or real-time events instead of maintaining a parallel copy of the data (→ [[vue]] §7). `refreshNuxtData()` for page-wide refresh.
- [ ] **`useState` for SSR-safe shared state** — Cross-component state that must survive SSR → hydration (user session, theme, feature flags). Never module-level `ref()` shared across requests — it leaks between users on the server.
- [ ] **Fetch options discipline** — `server: false` for client-only data (auth-gated widgets), `lazy: true` for non-blocking below-the-fold data, `watch` for reactive re-fetch, `default()` for empty-state shapes.
- [ ] **No waterfalls** — Independent `useFetch` calls run in parallel by default in setup — don't `await` them sequentially; only await when a later fetch depends on an earlier result.
- [ ] **Error handling per fetch** — `const { data, error, pending } = useFetch(...)` — all four states (loading/empty/error/loaded) rendered. `createError` in server handlers for proper status codes.

## 9. Build & Deploy

- [ ] **Local production build tested** — `nuxt build && node .output/server/index.mjs` before shipping. Hydration mismatches and server/client boundary errors surface here, not in dev.
- [ ] **`nitro.preset` chosen** — `node-server` (default, self-host/VPS), `docker` via the node preset + multi-stage Dockerfile, `cloudflare-pages`/`cloudflare-module`, `vercel`, `netlify`. Preset matches the actual host — don't deploy a node build to a serverless host.
- [ ] **`.output` directory is the artifact** — Self-contained (server + public assets). Docker copies `.output` only; don't ship `node_modules`/source.
- [ ] **Environment variables** — `runtimeConfig` overridable at runtime via `NUXT_*` env vars (no rebuild per env). Documented which are required/optional. Nothing secret under `NUXT_PUBLIC_*` (§7).
- [ ] **SSR caching layer** — `routeRules` swr/isr (§2, §6) + CDN cache for static assets. Cache invalidation strategy defined for content updates.
- [ ] **CI/CD** — Lint → `nuxt typecheck` → unit test → `nuxt build` → deploy preview → E2E (Playwright against the built output) → promote. Preview URL per PR.
- [ ] **Source maps** — Not served publicly; uploaded to Sentry (`@sentry/nuxt` covers client + Nitro server).
- [ ] **Observability wired** — Sentry or equivalent on both client and server; Nitro request logging; Core Web Vitals from real users.

## 10. AI/LLM Integration (Server Side)

- [ ] **Streaming server route** — `server/api/chat.ts` with Vercel AI SDK `streamText` + `toDataStreamResponse()`; client `useChat` from `@ai-sdk/vue` → [[vue]] §18. Or SSE via `createEventStream` (§3).
- [ ] **Provider keys server-only** — Keys in non-public `runtimeConfig` (e.g. `NUXT_OPENAI_API_KEY` → `runtimeConfig.openaiKey`). Never `NUXT_PUBLIC_*` (→ [[vue]] §18). System prompts composed server-side, never in client bundles.
- [ ] **Rate-limit AI endpoints** — Per-user limits on AI server routes (`nuxt-security` rate limiter or handler-level); 429 UX on the client → [[vue]] §18.
- [ ] **Context & cost control** — Token budgets per request, conversation trimming server-side, cache identical prompts where safe.

---

## Quick Sanity Check Before Launch

- [ ] `nuxt build && node .output/server/index.mjs` clean — no hydration mismatch warnings
- [ ] Public indexable pages have content in "View source" HTML (not DevTools DOM)
- [ ] All pages have a `<title>` and `<meta name="description">` (useSeoMeta)
- [ ] LCP image uses `<NuxtImg>` with `preload`/`priority`
- [ ] `robots.txt` and `sitemap.xml` resolve (@nuxtjs/sitemap)
- [ ] OG tags unfurl correctly (Slack/Discord test)
- [ ] Staging/authenticated pages are `noindex`
- [ ] Security headers present (securityheaders.com check; nuxt-security configured)
- [ ] No `NUXT_PUBLIC_` secret leaked into client payload (`nuxt analyze` / view payload)
- [ ] Every `server/api/` route authenticated + Zod-validated
- [ ] `routeRules` caching TTLs correct — no stale content after publish
- [ ] 404 page is custom (`error.vue`), not the default Nuxt screen
- [ ] Vue library items → [[vue]] Quick Sanity Check

---

## Project Tier Scoping Matrix

> **How to use this table:** Pick your tier first (→ [[vue]] has the full tier descriptions + flowchart), then focus only on the sections marked ✅ (required) or 🟡 (recommended). Skip ❌ sections entirely.
>
> **Legend:** ✅ Required · 🟡 Recommended / partial · ❌ Skip

### Checklist Applicability by Tier

| # | Section | 🧪 POC | 🔧 Prototype | 🏠 Internal | 🟢 Small Prod | 🔵 Medium Prod | 🟣 Production Grade | 🔴 Mission-Critical |
|---|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 1 | Project Setup (Nuxt) | 🟡 | ✅ | ✅ | ✅ | ✅ + layers | ✅ | ✅ |
| 2 | Rendering Modes | 🟡 SPA-like | ✅ | ✅ | ✅ | ✅ + hybrid | ✅ + SLO | ✅ + HA |
| 3 | Nitro Server Routes | 🟡 if used | 🟡 | ✅ | ✅ | ✅ + rate limits | ✅ + hardened | ✅ + audit |
| 4 | File-Based Routing & Conventions | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 5 | SEO & Metadata | ❌ | 🟡 if public | 🟡 if public | ✅ if public | ✅ | ✅ + structured data | ✅ |
| 6 | Performance (Nuxt) | ❌ | 🟡 basic CWV | ✅ | ✅ + budgets | ✅ + profiling | ✅ + SLO | ✅ + capacity |
| 7 | Security (Nuxt) | 🟡 no secrets | 🟡 essentials | ✅ | ✅ + headers/CSP | ✅ + pentest | ✅ + hardened | ✅ + formal audit |
| 8 | Data Fetching (Server-Side) | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ + failover |
| 9 | Build & Deploy | ❌ | 🟡 basic build | ✅ + CI | ✅ + previews | ✅ + canary + flags | ✅ + full pipeline | ✅ + signed artifacts |
| 10 | AI/LLM (Server Side) | 🟡 if AI is the POC | 🟡 | 🟡 if used | ✅ if used | ✅ | ✅ + guardrails | ✅ + audit trail |

---

## Sources

- Nuxt 3 Docs — https://nuxt.com/
- Library layer: [[vue]] · General companion: [[web]] · Launch gate: [[Frontend Launch]]
- Split 2026-09-14 from `vue-js.md`: Nuxt-specific concerns here, Vue library concerns in [[vue]].
- Absorbs the former `vue-js.md` §15 "Nuxt 3 (SSR / Full-Stack)" plus SEO & Metadata (§7) and the Nitro/SSE items from §5.
