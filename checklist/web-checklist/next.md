# Next.js Checklist (Meta-Framework Layer)

> Next.js App Router — rendering model, file conventions, Server Components/Actions, middleware, metadata, build & deploy.
> Library layer: [[react]] (components, hooks, state, forms, testing, a11y) — tick that first, then this.
> General companion: [[web]] · Launch gate: [[Frontend Launch]]
> Last updated: 2026-09-14 (split from react-js.md — Next.js-specific concerns here, React concerns in [[react]])

> **Gate + link:** this file never re-states React items. If it's about components, hooks, client state, or forms behavior → [[react]]. If it's about *where code runs* (server vs client), routing files, or the Next build/deploy → here.

---

## 1. Project Setup (Next-Specific)

- [ ] **App Router** — Default for new projects. Pages Router only if migrating legacy or team refusal. Don't mix the two for new routes.
- [ ] **Next 15+ / React 19** — Async `params`/`searchParams`, `useActionState`, Turbopack dev. Don't start on 14 unless dependency-constrained.
- [ ] **Runtime decision per route** — Node.js runtime (default, full API) vs Edge runtime (middleware, geo-latency-sensitive routes, limited APIs). Don't default everything to Edge.
- [ ] **`server-only` / `client-only` package guards** — Import `server-only` in modules that must never reach the client bundle. Build fails loudly on violation.

## 2. Rendering Model (The Critical Decision)

- [ ] **Server Components by default** — Components are Server Components by default. Add `"use client"` only when you need hooks, event handlers, browser APIs, or context. Push client boundaries down to the leaves.
- [ ] **Server Components for data** — Fetch data in Server Components. Pass as props to Client Components. No API routes just to bridge data. No `useEffect` + `fetch` for initial page data → [[react]] §4.
- [ ] **Streaming with Suspense** — `loading.tsx` for route-level. `<Suspense fallback={<Skeleton />}>` for component-level. Users see content faster, no white screens.
- [ ] **Server Actions** — For form mutations. `action={myServerAction}` instead of `onSubmit` + `fetch`. But know the limits: complex validation or third-party integrations → explicit Route Handler.
- [ ] **ISR / SSG / SSR per route** — Static for marketing pages (`export const revalidate = 3600` or `generateStaticParams`) — not SSR'd every request. Dynamic for dashboards. SSR for user-specific pages.
- [ ] **Caching model understood** — Next's layered caches (Request Memoization, Data Cache, Full Route Cache, Router Cache). Know what `dynamic = 'force-dynamic'`, `revalidate`, and `cache: 'no-store'` actually disable. Surprises here = stale data in prod.

## 3. Server Routes & Backend Surface

- [ ] **Route Handlers (`app/api/**/route.ts`)** — For machine-to-machine endpoints, webhooks, SSE streams. Auth + validation on every one — they're public endpoints.
- [ ] **Authenticate Server Actions like API routes** — Every Server Action re-checks auth/permissions. `"use server"` functions are public endpoints, not private methods.
- [ ] **Validate action input with Zod** — Server Action arguments come from the client. Same schema as the form (shared package) → [[react]] §9.
- [ ] **`revalidatePath` / `revalidateTag` after mutations** — Invalidate the right cache, or users see stale data after writes.
- [ ] **SSE via Route Handler** — `ReadableStream` response for one-way streams (notifications, LLM tokens) → [[react]] §5.
- [ ] **`after()` for non-blocking work** — Logging, analytics, webhooks that shouldn't delay the response.

## 4. Routing & File Conventions

- [ ] **Nested layouts** — `layout.tsx` preserves state across navigations. Auth layout, dashboard layout, settings layout.
- [ ] **Parallel routes & intercepting routes** — For modals on top of existing pages (photo lightbox, quick-edit drawer). URL changes but background page persists.
- [ ] **Loading UI per route** — `loading.tsx` near `page.tsx`. Automatic Suspense boundary.
- [ ] **Error UI per route** — `error.tsx` near `page.tsx`. Automatic Error Boundary. `reset()` function to retry.
- [ ] **Not found per route** — `not-found.tsx` for route-specific 404 content.
- [ ] **Dynamic routes with typed params** — `/user/[id]` → typed `params.id` (awaited in Next 15). No stringly-typed route params.
- [ ] **Middleware** — `middleware.ts` for auth guards, redirects, locale detection. Runs on Edge runtime (limited APIs). Keep it fast; heavy authz belongs in layouts/Server Actions.

## 5. SEO & Metadata

- [ ] **Metadata API** — `export const metadata` (static) / `generateMetadata` (dynamic) per route. Unique `<title>` (50–60 chars) and description (140–160), generated from data, never hard-coded strings.
- [ ] **Open Graph + Twitter cards** — `openGraph` in metadata; `og:image` 1200×630 (generate per-route with `next/og` / ImageResponse for dynamic cards). Test unfurls in Slack/Discord/X validators.
- [ ] **Canonical URLs** — `alternates.canonical` on any page with duplicate/parameterized content.
- [ ] **`robots.txt` + sitemap** — `app/robots.ts` and `app/sitemap.ts` (or `generateSitemaps` for large sites). Sitemap referenced in robots.txt.
- [ ] **JSON-LD structured data** — Script tags via metadata or layout. `Product`, `Article`, `BreadcrumbList`, `FAQPage` where applicable. Validate with Google Rich Results Test.
- [ ] **Crawlability** — Public indexable pages render content in initial HTML (SSR/SSG). Verify with "View source", not DevTools.
- [ ] **Indexability control** — `robots: { index: false }` on authenticated, duplicate, staging pages. Staging behind auth + `X-Robots-Tag: noindex`.
- [ ] **hreflang** — `alternates.languages` when i18n exists → [[web]] §17.
- [ ] **Search Console** — Registered, sitemap submitted, crawl errors monitored after launch.

## 6. Performance (Next-Specific)

- [ ] **Image optimization** — `next/image` with `sizes`, `priority` on LCP image, `placeholder="blur"` for local images. Remote images: configure `remotePatterns` in `next.config.ts`.
- [ ] **Font optimization** — `next/font` with `subset` and `display: 'swap'`. Self-host fonts, don't fetch from Google Fonts. Hoist static I/O (fonts, logos) to module level.
- [ ] **Script optimization** — `next/script` with `afterInteractive`/`lazyOnload` for third-party analytics. Never a blocking `<script>` in the document head.
- [ ] **Component-level code splitting** — `next/dynamic(() => import('./HeavyChart'), { loading: () => <Skeleton /> })` for heavy components below the fold.
- [ ] **Server-side caching** — `React.cache()` for per-request dedup of repeated fetches. LRU cache (or Valkey/Redis) for cross-request caching of stable data. `unstable_cache`/`cacheTag` for Data Cache control.
- [ ] **Minimize RSC → client serialization** — Pass only the fields the client needs, not whole ORM objects. Serialized props must be serializable (no functions/Dates-as-objects surprises). No duplicate data across props.
- [ ] **No module-level mutable request state** — Leaks across requests in RSC/SSR. Per-request context only.
- [ ] **Parallelize server fetches** — Component composition streams in parallel; where data is co-fetched, `Promise.all`. Nested per-item fetches chained inside `Promise.all`.
- [ ] **Bundle analysis** — `@next/bundle-analyzer`. Watch First Load JS per route; catch accidentally-large deps.

## 7. Security (Next-Specific)

- [ ] **Env var prefix discipline** — `NEXT_PUBLIC_*` ships to the browser (that's the design). Server-only vars have no prefix and stay in Server Components/Actions/Route Handlers. Never prefix a secret.
- [ ] **Headers** — `next.config.ts` `headers()`: CSP (report-only first), `X-Content-Type-Options: nosniff`, `Referrer-Policy`, `Permissions-Policy`, HSTS. No `X-Powered-By` (`poweredByHeader: false`).
- [ ] **Server Actions hardened** — `serverActions.allowedOrigins` if cross-origin; body size limit; rate-limit mutation-heavy actions.
- [ ] **Auth cookies** — HTTP-only, Secure, SameSite. Session read in Server Components/middleware, never shipped to client state → [[react]] §13.
- [ ] **Middleware is not the only gate** — Edge middleware can be bypassed for some routes; defense in depth: layout-level + Server Action-level auth checks too.

## 8. Build & Deploy

- [ ] **Local production build tested** — `next build && next start` before shipping. No server/client boundary errors (`'use client'` leaks fail here, not in dev).
- [ ] **Environment variables** — Set per environment in the host dashboard; documented which are required/optional. Runtime env for server code where supported (avoid full rebuilds per env).
- [ ] **Output mode chosen** — Default (Node server / serverless), `output: 'standalone'` (Docker — self-hosted homelab), or `output: 'export'` (fully static). Don't default to `export` if you use Server Actions/middleware/ISR.
- [ ] **CI/CD** — Lint → type-check → unit test → build → deploy preview (Vercel/Cloudflare) → E2E → promote to production.
- [ ] **Preview deployments per PR/commit** — Vercel/Netlify/Cloudflare. Shareable URLs for stakeholders.
- [ ] **Static assets** — Content-hashed automatically. Immutable cache headers for `/_next/static/*`. Optional `assetPrefix`/CDN for self-hosting.
- [ ] **Source maps** — `productionBrowserSourceMaps: false` (default); upload to Sentry via `@sentry/nextjs` wizard, not served publicly.
- [ ] **Observability wired** — `instrumentation.ts` with `@sentry/nextjs` (client + server + edge). `useReportWebVitals` → analytics.

## 9. AI/LLM Integration (Server Side)

- [ ] **Vercel AI SDK** — `streamText` + `toDataStreamResponse()` in Route Handlers; `useChat` on the client → [[react]] §16.
- [ ] **RSC streaming** — AI SDK v4+ streams into Server Components (`toUIMessageStream`). `<Suspense>` boundaries with streaming content. Don't fall back to polling.
- [ ] **Never expose provider keys** — No `NEXT_PUBLIC_OPENAI_API_KEY`. All LLM calls in Server Actions/Route Handlers with server-only env vars.
- [ ] **Context & prompt management** — System prompts composed server-side, never in client bundles.
- [ ] **Rate-limit AI endpoints** — Per-user limits on Route Handlers/Actions; 429 UX on the client → [[react]] §16.

---

## Quick Sanity Check Before Launch

- [ ] All pages have a `<title>` and `<meta name="description">` (Metadata API)
- [ ] LCP image has `priority` and `fetchpriority="high"`
- [ ] `next build && next start` clean — no server/client boundary errors
- [ ] No console errors in production build
- [ ] Lighthouse score ≥ 90 on mobile
- [ ] 404 page exists and is helpful (not default Next.js page)
- [ ] `robots.txt` and `sitemap.xml` resolve (app/robots.ts, app/sitemap.ts)
- [ ] OG tags unfurl correctly (Slack/Discord test)
- [ ] Staging/authenticated pages are `noindex`
- [ ] Security headers present (check response with securityheaders.com)
- [ ] No `NEXT_PUBLIC_` secret leaked into client bundle
- [ ] React library items → [[react]] Quick Sanity Check

---

## Project Tier Scoping Matrix

> **How to use this table:** Pick your tier first (→ [[react]] has the full tier flowchart), then focus only on the sections marked ✅ (required) or 🟡 (recommended). Skip ❌ sections entirely.
>
> **Legend:** ✅ Required · 🟡 Recommended / partial · ❌ Skip

### Checklist Applicability by Tier

| # | Section | 🧪 POC | 🔧 Prototype | 🏠 Internal | 🟢 Small Prod | 🔵 Medium Prod | 🟣 Production Grade | 🔴 Mission-Critical |
|---|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 1 | Project Setup (Next) | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 2 | Rendering Model | 🟡 SPA-like static | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 3 | Server Routes & Backend Surface | 🟡 if used | 🟡 | ✅ | ✅ | ✅ + rate limits | ✅ + hardened | ✅ + audit |
| 4 | Routing & File Conventions | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 5 | SEO & Metadata | ❌ | 🟡 if public | 🟡 if public | ✅ if public | ✅ | ✅ + structured data | ✅ |
| 6 | Performance (Next) | ❌ | 🟡 basic CWV | ✅ | ✅ + budgets | ✅ + profiling | ✅ + SLO | ✅ + capacity |
| 7 | Security (Next) | 🟡 no secrets | 🟡 essentials | ✅ | ✅ + headers/CSP | ✅ + pentest | ✅ + hardened | ✅ + formal audit |
| 8 | Build & Deploy | ❌ | 🟡 basic build | ✅ + CI | ✅ + previews | ✅ + canary + flags | ✅ + full pipeline | ✅ + signed artifacts |
| 9 | AI/LLM (Server Side) | 🟡 if AI is the POC | 🟡 | 🟡 if used | ✅ if used | ✅ | ✅ + guardrails | ✅ + audit trail |

---

## Sources

- Library layer: [[react]] · General companion: [[web]] · Launch gate: [[Frontend Launch]]
- Split 2026-09-14 from `react-js.md`: Next.js-specific concerns here, React library concerns in [[react]].
- Performance rules informed by Vercel Engineering's `react-best-practices` (vercel-labs/agent-skills): waterfalls & bundle = CRITICAL, server-side perf = HIGH.
- Next.js official production checklist: nextjs.org/docs/app/guides/production-checklist
