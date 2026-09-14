# Svelte Launch Checklist

> Tick every box before a Svelte 5 app hits production. Framework companion to [[Frontend Launch]]. Svelte 5 introduced runes — reactive state without `$:` or stores.
> Last updated: 2026-09-14 (added Architecture & Code Organization, Real-Time & Live Data, SEO & Metadata — cascade from [[web]])

---

## 1. Project Setup

- [ ] **SvelteKit** — `npx sv create`. SvelteKit is to Svelte what Next.js is to React. Use it for everything except embeddable widgets.
- [ ] **TypeScript** — `lang="ts"` in `<script>` blocks. `svelte.config.js`: `vitePlugin: { inspector: true }` for devtools.
- [ ] **pnpm** — fast, strict. Lockfile committed.
- [ ] **ESLint + Prettier** — `@sveltejs/eslint-config`. `svelte-check` for type checking `.svelte` files.
- [ ] **Svelte 5 runes mode** — `compilerOptions: { runes: true }` in config. Runes are the new reactivity model. No `$:`, no `$store`, use `$state`, `$derived`, `$effect`.

---

## 2. Architecture & Code Organization

- [ ] **Feature-based folders** — `src/lib/features/users/`, `src/lib/features/orders/` with components, stores, api, and Zod schemas co-located per feature. Routes in `src/routes/` stay thin — thin `+load` server functions, `+page.svelte` composes feature modules, no business logic in components.
- [ ] **`$lib` alias discipline** — `$lib` → `src/lib`. Shared code in `$lib/shared`, features in `$lib/features`. No deep relative imports (`../../../`) crossing feature boundaries.
- [ ] **Module boundaries enforced in CI** — `eslint-plugin-boundaries` or `dependency-cruiser`: `$lib/shared` → `$lib/features` → `src/routes`. Nothing imports upward or sideways across features except through the feature's public export.
- [ ] **Dependency rule points inward** — Pure TS domain modules (logic, types, Zod schemas) never import Svelte or SvelteKit. Components import from logic, never the reverse.
- [ ] **Limited barrel files** — One `index.ts` per feature public boundary at most. Deep-import within a feature. Barrels hurt tree-shaking and slow down Vite HMR.
- [ ] **Shared code promoted on third use** — Don't abstract on first or second use. `$lib/shared` holds API client, generated types, schemas, design tokens, utils — not speculative "common" components.
- [ ] **Tests co-located** — `user-card.test.ts` next to `UserCard.svelte`. Vitest + `@testing-library/svelte` pick them up by convention.
- [ ] **API types generated, not hand-written** — `openapi-typescript` / orval from the backend's OpenAPI spec. SvelteKit also generates `./$types` (`PageData`, `LayoutData`) for load data — use it instead of hand-maintained response types that drift.

---


## 3. Runes (Svelte 5 Reactivity)

- [ ] **`$state()`** — reactive variable. `let count = $state(0)`. Any reassignment triggers reactivity. Works in `.svelte`, `.svelte.ts`, `.svelte.js`.
- [ ] **`$derived()`** — computed value. `let double = $derived(count * 2)`. Auto-tracks deps. Lazy — only recomputed when read.
- [ ] **`$effect()`** — side effects. `$effect(() => console.log(count))`. Runs after DOM updates. Auto-cleanup on destroy. `$effect.pre` for pre-DOM effects.
- [ ] **`$props()`** — component props. `let { name, count = 0 } = $props()`. Replaces `export let`. Default values, rest props (`...rest`).
- [ ] **`$bindable()`** — two-way binding prop. `let { value = $bindable(0) } = $props()`. Parent can `<Component bind:value={myVal} />`.
- [ ] **`$inspect()`** — debug reactive values. `$inspect(count)`. Logs whenever value changes. Stripped in production builds.
- [ ] **`$state.snapshot()`** — plain object copy of reactive state. For serialization, logging, non-reactive consumers.

---

## 4. State Management

- [ ] **Runes for local state** — `$state()` in components, `$state()` in `.svelte.ts` modules. No store library needed for most cases.
- [ ] **Shared state** — export `$state` from `.svelte.ts` files. Import anywhere. Truly reactive cross-component state without stores or context APIs.
- [ ] **Svelte stores (legacy)** — `writable`, `readable`, `derived`. Still work but runes are the future. `$store` auto-subscribe syntax still valid.
- [ ] **TanStack Query (Svelte)** — for server state. `createQuery`, `createMutation`. Caching, refetch, optimistic updates.
- [ ] **URL state** — SvelteKit `$page.url.searchParams`. Built-in. No library needed.

---

## 5. SvelteKit — Routing & Data Loading

- [ ] **File-based routing** — `src/routes/` directory. `+page.svelte` for UI, `+page.ts` for data loading, `+layout.svelte` for persistent layouts.
- [ ] **`load` functions** — `+page.ts`: `export async function load({ params, fetch, url }) { return { user: await fetchUser(params.id) } }`. Data flows to `+page.svelte` via `let { data } = $props()`.
- [ ] **Universal vs server loads** — `+page.ts` = universal (runs on server AND client for navigation). `+page.server.ts` = server-only (secrets, DB access).
- [ ] **Form actions** — `+page.server.ts`: `export const actions = { default: async ({ request }) => { ... } }`. Form `method="POST" action="?/actionName"`. Progressive enhancement — works without JS.
- [ ] **`enhance`** — SvelteKit's form enhancer. `use:enhance={({ formElement, formData, action, result }) => ...}`. Ajaxy forms without writing fetch calls.
- [ ] **Error handling** — `+error.svelte` per route segment. `throw error(404, 'Not found')` in load. `throw redirect(302, '/login')`.
- [ ] **Hooks** — `src/hooks.server.ts`: `handle()`, `handleFetch()`, `handleError()`. Auth guards, header injection, error logging.

---

## 6. Real-Time & Live Data

- [ ] **SSE vs WebSocket** — SSE for one-way server→client streams (notifications, activity feeds, LLM tokens) — trivial in a SvelteKit `+server.ts` route returning a `ReadableStream`. WebSocket only for bidirectional (chat, collaboration, presence). Default to SSE — simpler, works over HTTP/2, auto-reconnects natively.
- [ ] **Events write into the server-state cache** — On message: update the TanStack Query Svelte cache (`queryClient.setQueryData` / `invalidateQueries`) or call `invalidateAll()` so `$page.data` refreshes. Never a parallel store copy of live data — one source of truth.
- [ ] **Runes for ephemeral socket UI state** — Svelte 5 `$state()` for connection status, unread counts, typing indicators; `$derived()` for computed views of the stream; `$effect()` for subscribe/unsubscribe side effects.
- [ ] **Reconnection with backoff + resume** — Exponential backoff with jitter. `Last-Event-ID` (SSE) or resume token (WS) so a reconnect doesn't duplicate or drop messages. Browser `EventSource` auto-reconnects; custom WS clients don't.
- [ ] **Ordering & dedup** — Sequence numbers on server events, client dedupes by event ID. Chatty streams: batch/throttle UI updates (backpressure).
- [ ] **Optimistic + server-authoritative** — Optimistic UI for the user's own actions, reconciled on server ack. Server wins on conflict.
- [ ] **Auth over the channel** — Cookie-authenticated SSE/WS; SvelteKit's `handle()` hook in `hooks.server.ts` is the natural auth point. Never a token in the query string (leaks into logs).
- [ ] **Cleanup** — Close the socket in `onDestroy` (or the `onMount` return function), remove listeners, call `AbortController.abort()`. No zombie connections after route changes.
- [ ] **Scale awareness** — WebSockets don't scale on serverless adapters — use SSE, Pusher/Ably, or `adapter-node` / a dedicated WS server. Pub/sub fan-out via Valkey/Redis on the backend.

---


## 7. Components

- [ ] **Template syntax** — `{#if}`, `{#each items as item (item.id)}`, `{#await promise}`, `{#snippet name()}...{/snippet}`. No JSX — HTML-first.
- [ ] **`{@render}`** — render snippets or components. `{@render children()}`. Replaces `<slot>` (Svelte 4).
- [ ] **`{@html}`** — raw HTML. Must sanitize. `{@html DOMPurify.sanitize(content)}`. Never bare `{@html userInput}`.
- [ ] **Event handlers** — `onclick={handler}` (lowercase). Modifiers: `onclick|preventDefault={handler}`, `onclick|once={handler}`.
- [ ] **Styling** — `<style>` in `.svelte` files is component-scoped by default. Zero config. `:global()` for escaping.
- [ ] **`bind:` directive** — `bind:value={name}` for inputs. `bind:this={element}` for DOM refs. `bind:open={isOpen}` on `<dialog>`.

---

## 8. Styling

- [ ] **Tailwind CSS** — works natively. `@tailwind base/components/utilities` in `app.css`. Or UnoCSS (lighter, tree-shakable).
- [ ] **Component library** — Melt UI (headless, accessible primitives) + shadcn-svelte (styled). Or Skeleton UI (Tailwind-native). Don't build from scratch.
- [ ] **Dark mode** — `tailwind.config`: `darkMode: 'class'`. Toggle `dark` class on `<html>`. Or use SvelteKit's `handle` hook to read `prefers-color-scheme`.

---

## 9. Forms

- [ ] **SvelteKit form actions** — no client-side form library needed for basic cases. `export const actions` + `use:enhance`.
- [ ] **Superforms** — `sveltekit-superforms` + `zod`. `const form = await superValidate(request, zodSchema)`. Client: `const { form, enhance, errors } = superForm(data.form)`. Field-level errors built-in.
- [ ] **Progressive enhancement** — forms work without JavaScript. `use:enhance` adds AJAX + validation on top.
- [ ] **Form states** — Superforms provides `$submitting`, `$errors`, `$message`, `$tainted`. Every state is tracked and accessible in template.

---

## 10. Performance

- [ ] **No virtual DOM** — Svelte compiles to direct DOM manipulation. This is the default. No extra configuration needed.
- [ ] **Build-time optimization** — dead code elimination at compile time. Unused CSS purged. Reactive declarations optimized to minimal DOM updates. All automatic — no plugins required.
- [ ] **Code splitting** — route-level splitting is automatic in SvelteKit. `import()` for component-level splitting.
- [ ] **`{#key}`** — force re-render when expression changes. `{#key item.id}<ExpensiveComponent />{/key}`.
- [ ] **Image optimization** — `@sveltejs/enhanced-img`. `<enhanced:img src="./pic.jpg" alt="..." />`. Auto resizes, generates srcset, lazy-loads.
- [ ] **LCP < 2.5s** — Svelte's compile-time approach typically produces smaller bundles than React/Vue frameworks by default. Verify with Lighthouse.

---

## 11. Routing

- [ ] SvelteKit file-based routing — `+page.svelte`, `+layout.svelte`. Nested layouts preserved across navigations
- [ ] Dynamic routes — `[id]` folders. Typed params in `load` functions
- [ ] Navigation guards — `hooks.server.ts` for auth checks
- [ ] Error boundaries per route — `+error.svelte`
- [ ] 404 — `+error.svelte` in root with `$page.status === 404`

---

## 12. SEO & Metadata

- [ ] **Metadata per route** — Unique `<title>` (50–60 chars) + `<meta name="description">` (140–160) on every page, generated from `load` data via `<svelte:head>`, Svelte 5 snippets-based head management, or `@unhead` — never hard-coded strings.
- [ ] **Crawlability first** — SvelteKit SSRs by default; `export const prerender = true` for public, indexable pages. CSR-only content is invisible to some crawlers and social unfurlers — verify with "View source" (not DevTools) that content is in the initial HTML.
- [ ] **Open Graph + Twitter cards** — `og:title`, `og:description`, `og:image` (1200×630), `twitter:card` per route. Test unfurls in Slack/Discord/X validators before launch.
- [ ] **Structured data (JSON-LD)** — `<script type="application/ld+json">{@html JSON.stringify(schema)}</script>` in the head. `Organization`, `Product`, `Article`, `BreadcrumbList` where applicable. Validate with Google Rich Results Test.
- [ ] **`robots.txt` + XML sitemap** — Static files, adapter-generated output (`@sveltejs/adapter-static`), or `svelte-sitemap` at build time. Sitemap with `lastmod`, referenced in robots.txt.
- [ ] **hreflang for multi-locale** — Correct language/region pairs, self-referencing entries, `x-default` — built with `$app/paths` when i18n exists.
- [ ] **Indexability control** — `noindex` on authenticated, duplicate, parameterized, and staging pages. Staging behind auth + `X-Robots-Tag: noindex` set in `hooks.server.ts`.
- [ ] **Search Console + Bing Webmaster** — Registered, sitemap submitted, crawl errors and 404s monitored after launch.
- [ ] **Core Web Vitals are SEO** — LCP/INP/CLS are ranking inputs. Svelte ships less JS than most frameworks by default — keep it that way. Lighthouse SEO audit in CI.

---


## 13. Testing

- [ ] **Vitest** — fast, Vite-native, works with Svelte. `@sveltejs/vite-plugin-svelte`.
- [ ] **`@testing-library/svelte`** — `render(Component, { props })`. `screen.getByRole()`, `screen.getByText()`. Test behavior, not structure.
- [ ] **Playwright** — E2E. SvelteKit has first-class support. `page.goto('/login')`, `page.fill()`, `page.click()`.
- [ ] **`svelte-check`** — type-check `.svelte` files. Runs in CI. `svelte-check --tsconfig ./tsconfig.json`.

---

## 14. Accessibility

- [ ] Semantic HTML — `{#if}`/`{#each}` encourage native HTML structures. `<button>` for actions, `<a>` for navigation.
- [ ] Svelte accessibility warnings — compiler warns about missing `alt`, unlabeled inputs, positive tabindex, missing `lang`. Fix all warnings — they're free a11y audits.
- [ ] Focus management — `bind:this={element}` + `element.focus()` after conditional renders.

---

## 15. Security

- [ ] No `{@html}` without DOMPurify — `{@html DOMPurify.sanitize(userContent)}`.
- [ ] No secrets in `$env/static/public` — these are inlined at build time. `$env/static/private` for server-only.
- [ ] CSP — SvelteKit outputs static HTML + minimal JS. Works well with strict CSPs (no `unsafe-inline` needed once `enhanced:img` generates proper `srcset` without inline styles).

---

## 16. SvelteKit Adapters

- [ ] **Adapter choice** — `adapter-auto` (detects environment). `adapter-node` (self-hosted Node). `adapter-vercel`, `adapter-cloudflare`, `adapter-netlify` (serverless edge). `adapter-static` (SPA or fully static).
- [ ] **Edge deployment** — `adapter-cloudflare-workers` or `adapter-vercel`. SvelteKit is edge-ready. SSR at the edge with minimal cold start.

## 17. AI/LLM Integration

- [ ] **Vercel AI SDK Svelte** — `@ai-sdk/svelte` `useChat` for chat UIs. SvelteKit `+server.ts` route streams with `streamText` + `toDataStreamResponse()`.
- [ ] **Never expose provider keys** — `$env/static/public` ships to the browser. LLM keys live in `$env/static/private` / `$env/dynamic/private` only, accessed in `+server.ts` or `+page.server.ts`.
- [ ] **Rune-based AI state** — `$state()` for messages and streaming status, `$derived()` for computed UI state, `$effect()` for scroll-to-bottom side effects. `$state.snapshot()` for persistence.
- [ ] **Streaming UX** — SSE from server route, `useChat` parses the stream. Typing indicator, partial markdown, stop button, regenerate + edit messages.
- [ ] **Markdown rendering** — `marked` or `mdsvex` + DOMPurify. Never bare `{@html}` on model output — always `{@html DOMPurify.sanitize(content)}`.
- [ ] **Non-chat AI calls** — TanStack Query Svelte `createMutation` for classify/extract/summarize. Cache identical prompts (response dedup).
- [ ] **Graceful degradation** — Error state with retry, cached fallback, "AI can be wrong" disclaimers where user-facing. Rate-limit UX on 429.

## 18. Data Privacy & Compliance (Frontend-Specific)

- [ ] **Error monitoring scrubbing** — Sentry `beforeSend` strips PII (emails, tokens, form values) from error payloads.
- [ ] **Cookie consent** — GDPR/CCPA banner before analytics fire. Load Plausible/Umami/PostHog only after opt-in.
- [ ] **PII minimization** — Don't store user data in localStorage/IndexedDB unnecessarily. Mask sensitive data in UI previews.
- [ ] **Third-party script inventory** — `$env/dynamic/public` + `app.html` audit: what loads, what it collects, where it's sent (EU/US). Remove dead scripts.
- [ ] **Data retention UI** — "Delete my data" / "Export my data" flows calling backend erasure/export endpoints via form actions or `fetch`.
- [ ] **Privacy policy & terms** — Up-to-date, linked in footer. Cover collection, retention, rights (access/erasure/portability).
- [ ] **Do Not Track / GPC** — Respect `navigator.doNotTrack` and Global Privacy Control where feasible.

---

## Quick Sanity Check

- [ ] `svelte-check` passes — zero type errors in `.svelte` files
- [ ] `vite build` succeeds — zero build errors
- [ ] No Svelte compiler warnings in console (`a11y-*`, `css-unused-selector`)
- [ ] Lighthouse ≥ 90 on mobile
- [ ] Forms work without JavaScript (try disabling JS in DevTools)
- [ ] Back button works (no redirect loops)
- [ ] `{#each}` always has a `key` — `{#each items as item (item.id)}`
- [ ] All `<img>` have `alt`, all inputs have labels
- [ ] `+layout.svelte` provides consistent chrome without re-renders — SvelteKit preserves layout state across navigations by default
- [ ] Private env vars in `$env/static/private` — never leaked through `$env/static/public` or `$env/dynamic/public`


---

## Project Tier Scoping Matrix

> **How to use this table:** Pick your tier first, then focus only on the sections marked ✅ (required) or 🟡 (recommended). Skip ❌ sections entirely — they'd be over-engineering for your context.
>
> **Legend:** ✅ Required · 🟡 Recommended / partial · ❌ Skip

### Tier Descriptions

| # | Tier | Description | Typical Team | Users | Lifespan |
|---|---|---|---|---|---|
| 1 | 🧪 **POC / Spike** | Validate an idea. Throwaway code. `console.log` is fine. | 1 dev | Internal only | Days–weeks |
| 2 | 🔧 **Prototype / MVP** | Waiting for integration or user validation. Might become real. | 1–2 devs | Beta testers | Weeks–months |
| 3 | 🏠 **Internal Tool** | Real users (employees), real traffic. No external exposure or paying customers. | 1–3 devs | Employees | Ongoing |
| 4 | 🟢 **Small Production** | Single app, few pages, low traffic. Real users, maybe early revenue. | 1–2 devs | < 1K users | Ongoing |
| 5 | 🔵 **Medium Production** | Multiple apps or higher traffic. Real revenue or user base that matters. | 2–5 devs | 1K–100K users | Ongoing |
| 6 | 🟣 **Production Grade** | Full rigor — high-stakes SaaS, enterprise product, or large user base. | 5+ devs | 100K+ users | Long-term |
| 7 | 🔴 **Mission-Critical / Regulated** | Healthcare (HIPAA), finance (PCI-DSS), safety systems. Failure = severe harm. Adds formal verification, regulatory audit. | 10+ devs | Varies | Decades |

### Which Tier Am I?

```mermaid
flowchart TD
    A[Is this throwaway / exploratory?] -->|Yes| T1[🧪 Tier 1 or 2<br/>POC / Prototype]
    A -->|No| B[Are the users internal<br/>employees?]
    B -->|Yes| T3[🏠 Tier 3<br/>Internal Tool]
    B -->|No| C[Do paying users or real<br/>revenue depend on it?]
    C -->|No| T4[🟢 Tier 4<br/>Small Production]
    C -->|Yes| D[Multiple apps or<br/>1K+ users?]
    D -->|No| T4
    D -->|Yes| E[Enterprise / high-stakes<br/>/ regulated industry?]
    E -->|No| T5[🔵 Tier 5<br/>Medium Production]
    E -->|Yes| F[Failure could cause<br/>severe harm?]
    F -->|No| T6[🟣 Tier 6<br/>Production Grade]
    F -->|Yes| T7[🔴 Tier 7<br/>Mission-Critical]
    
    style T1 fill:#e1f5ff
    style T3 fill:#fff4e1
    style T4 fill:#e8f5e9
    style T5 fill:#e3f2fd
    style T6 fill:#f3e5f5
    style T7 fill:#ffebee
```

### Checklist Applicability by Tier

| # | Section | 🧪 POC | 🔧 Prototype | 🏠 Internal | 🟢 Small Prod | 🔵 Medium Prod | 🟣 Production Grade | 🔴 Mission-Critical |
|---|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 1 | Project Setup | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 2 | Architecture & Code Organization | 🟡 | 🟡 | ✅ | ✅ | ✅ + boundaries | ✅ + enforced CI | ✅ + formal review |
| 3 | Runes (Svelte 5 Reactivity) | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 4 | State Management | 🟡 | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ |
| 5 | SvelteKit Routing & Data Loading | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 6 | Real-Time & Live Data | ❌ | 🟡 if used | 🟡 if used | ✅ if used | ✅ + scale | ✅ + load testing | ✅ + HA/failover |
| 7 | Components | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 8 | Styling | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ + design system |
| 9 | Forms | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ + audit trail |
| 10 | Performance | ❌ | 🟡 basic CWV | ✅ | ✅ + budgets | ✅ + profiling | ✅ + SLO | ✅ + capacity |
| 11 | Routing | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 12 | SEO & Metadata | ❌ | 🟡 if public | 🟡 if public | ✅ if public | ✅ | ✅ + structured data | ✅ |
| 13 | Testing | ❌ maybe smoke | 🟡 unit | ✅ + component | ✅ + E2E | ✅ + visual reg | ✅ + a11y in CI | ✅ + formal verification |
| 14 | Accessibility | ❌ | 🟡 basics | ✅ | ✅ WCAG AA | ✅ + audits | ✅ + WCAG AA certified | ✅ + legal/regulatory |
| 15 | Security (Frontend) | 🟡 no secrets | 🟡 essentials | ✅ | ✅ + CSP | ✅ + pentest | ✅ + hardened | ✅ + formal audit |
| 16 | SvelteKit Adapters | ❌ | 🟡 adapter-auto | ✅ + node | ✅ + platform | ✅ + edge | ✅ + multi-region | ✅ + signed artifacts |
| 17 | AI/LLM Integration | 🟡 if AI is the POC | 🟡 | 🟡 if used | ✅ if used | ✅ | ✅ + guardrails | ✅ + audit trail |
| 18 | Data Privacy & Compliance | ❌ | ❌ | 🟡 minimal | ✅ consent + PII | ✅ + DPA | ✅ full compliance | ✅ + regulatory framework |

---

## Sources

- Svelte 5 Docs — https://svelte.dev/
- SvelteKit Docs — https://kit.svelte.dev/
- `[[Frontend Launch]]` — general frontend checklist (tick first)
