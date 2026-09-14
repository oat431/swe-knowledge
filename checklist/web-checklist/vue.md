# Vue Checklist (Library Layer)

> Vue 3 the UI library — Composition API, components, reactivity, Pinia, forms, testing, SPA routing. Framework-agnostic to the meta-framework.
> Meta-framework layer: [[nuxt]] (Nuxt 3) · General companion: [[web]] · Launch gate: [[Frontend Launch]]
> Last updated: 2026-09-14 (split from vue-js.md — library concerns here/vue, Nuxt concerns in nuxt)

---

## 1. Project Setup (Vite SPA Path)

- [ ] **Scaffold choice** — Meta-framework (Nuxt → [[nuxt]]) for anything public/SEO/full-stack. Vite + Vue Router for pure SPA (internal dashboard, no SEO). Lighter, faster dev.
- [ ] **Vite + Vue 3.5+** — `create-vue` (official scaffold). Composition API + `<script setup>` by default.
- [ ] **TypeScript strict** — `tsconfig.json`: `strict: true`. `vue-tsc` for type checking `.vue` files (slower but safer).
- [ ] **pnpm** — strict, fast, disk-efficient. Lockfile committed.
- [ ] **ESLint + Prettier** or **Biome** — Biome is gaining. `@vue/eslint-config-typescript`. Pre-commit: `lint-staged`.
- [ ] **Vue DevTools** — browser extension + Vite plugin (`vite-plugin-vue-devtools`). Browser extension is legacy now.

## 2. Architecture & Code Organization

- [ ] **Feature-based folders** — Organize by domain (`src/features/users/`, `src/features/orders/`) with components, composables, api, and stores co-located per feature. Route pages stay thin — they compose feature components, they don't contain business logic (in Nuxt: `pages/` → [[nuxt]]). Type-based (`components/`, `composables/` split at root) dies at ~20 files.
- [ ] **Module boundaries enforced in CI** — `eslint-plugin-boundaries` or `dependency-cruiser`: `shared/` → `features/` → app-shell. Nothing imports upward or sideways across features except through a feature's public export.
- [ ] **Dependency rule points inward** — Domain logic, types, and Zod schemas don't import Vue, Nuxt, or UI details. Components and composables import from logic, never the reverse.
- [ ] **Limited barrel files** — One `index.ts` per feature public boundary at most. Deep-import within a feature. Barrels drag whole libraries into client bundles and kill tree-shaking.
- [ ] **Shared code promoted on third use** — Don't abstract on first or second use. `shared/` (or `packages/shared` in a monorepo) holds validated types, API client, Zod schemas, design tokens, utils — not speculative "common" components.
- [ ] **Tests co-located** — `UserCard.test.ts` next to `UserCard.vue`. Vitest picks them up by convention.
- [ ] **API types generated, not hand-written** — `openapi-typescript` / orval from the backend's OpenAPI spec. Hand-maintained response types drift.

## 3. Composition API & Script Setup

- [ ] **`<script setup lang="ts">`** — default. No `setup()` function boilerplate.
- [ ] **`ref` vs `reactive`** — `ref()` for primitives and single values. `reactive()` only for objects with known shape (rare — `ref` with object works fine).
- [ ] **`computed`** — derived state. Never mutate a computed. Use writable computed for v-model support.
- [ ] **`watch` vs `watchEffect`** — `watch` for explicit deps. `watchEffect` for auto-tracking. `watchEffect` runs immediately.
- [ ] **No Options API mixing** — pick one style per component. Composition API is the future.

## 4. Components

- [ ] **`defineProps`, `defineEmits`, `defineExpose`** — compiler macros. No imports needed. Type-safe: `defineProps<{ items: Item[] }>()`. Props are explicit contracts — no prop drilling past 2 levels (compose or use `provide`/`inject`).
- [ ] **`provide`/`inject`** — for deeply nested component trees. Not a replacement for Pinia — use for component-level dependency injection.
- [ ] **`useTemplateRef` (Vue 3.5+)** — typed template refs. `const input = useTemplateRef<HTMLInputElement>('myInput')`. Replaces string refs.
- [ ] **`<Suspense>`** — async component loading with fallback. Still experimental but stable in 3.5+.
- [ ] **`<KeepAlive>`** — cache component instances. Preserve form state across tab switches.
- [ ] **`v-memo`** — skip re-render when deps unchanged. For heavy lists: `<div v-for="item in list" :key="item.id" v-memo="[item.id, item.selected]">`.

## 5. State Management (Pinia)

- [ ] **Pinia** — official state management. `defineStore('name', () => { ... })`. Setup stores (composition-style) over options stores.
- [ ] **URL as state** — `useRoute().query` + `useRouter().push`. Filters, pagination, search in URL params.
- [ ] **No Vuex** — deprecated. Pinia is the replacement.
- [ ] **Form state** — `vee-validate` + `zod` (§11). Never in a global store.
- [ ] **No duplicated server state** — Pinia never caches what TanStack Query already owns (§6).

## 6. Data Fetching & Server State

- [ ] **TanStack Query (Vue Query)** — for all server state. `useQuery`, `useMutation`. Every async read goes through a query, every async write through a mutation.
- [ ] **Query key conventions** — Namespaced, hierarchical keys: `['users', userId, 'posts']`. Cache-friendly and granular.
- [ ] **No `watchEffect` + `fetch` for server data** — That's reimplementing TanStack Query badly (no cache, no dedup, no retry, race conditions). Wrap it in a composable (§12) if you need a nicer API.

## 7. Real-Time & Live Data (Client)

- [ ] **SSE vs WebSocket** — SSE for one-way server→client streams (notifications, activity feeds, LLM tokens). WebSocket (Socket.IO, Pusher, Ably) only for bidirectional (chat, collaboration, presence). Default to SSE — simpler, works over HTTP/2, auto-reconnects natively. Server-side implementation: [[nuxt]] §Nitro Server Routes, or your backend.
- [ ] **Events write into the server-state cache** — On message: TanStack Query Vue `queryClient.setQueryData(key, merge)` or `invalidateQueries(key)` — or in Nuxt, update `useState` / call `useAsyncData`'s `refresh()` (→ [[nuxt]]). Never a parallel `ref()` copy of live data — one source of truth.
- [ ] **Reconnection with backoff + resume** — Exponential backoff with jitter. `Last-Event-ID` (SSE) or resume token (WS) so a reconnect doesn't duplicate or drop messages. Browser `EventSource` auto-reconnects; custom WS clients don't.
- [ ] **Ordering & dedup** — Sequence numbers on server events, client dedupes by event ID. Chatty streams: batch/throttle reactive UI updates (backpressure).
- [ ] **Optimistic + server-authoritative** — Optimistic UI for the user's own actions, reconciled on server ack. Server wins on conflict.
- [ ] **Auth over the channel** — Cookie-authenticated SSE/WS. Never a token in the query string (leaks into logs).
- [ ] **Cleanup in `onUnmounted` / `onScopeDispose`** — Close the socket, remove listeners, call `AbortController.abort()`. Composables register teardown with `onScopeDispose` so cleanup works in any effect scope, not just components. No zombie connections after route changes.
- [ ] **Scale awareness** — WS doesn't scale on serverless (Vercel/Netlify functions) — use SSE, Pusher/Ably, or a dedicated WS server. Per-connection server cost; pub/sub fan-out via Valkey/Redis; fallback to polling when proxies block WS.

## 8. Routing (Vue Router 4 — SPA Path)

- [ ] **`createRouter` with `createWebHistory`** — HTML5 history mode. No hash unless legacy support needed.
- [ ] **Route meta** — `meta: { requiresAuth: true, title: 'Dashboard' }`. Navigation guards check `route.meta`.
- [ ] **`beforeEach` guard** — auth check, redirect to login. `router.beforeEach((to, from, next) => { ... })`.
- [ ] **Lazy-loaded routes** — `component: () => import('./views/Heavy.vue')`. Automatic code splitting.
- [ ] **Nested routes** — `children: [...]` with `<router-view>` in parent. Persistent layouts.
- [ ] **Scroll behavior** — `scrollBehavior(to, from, savedPosition)`. Restore scroll on back navigation.
- [ ] **In a meta-framework, routing is the framework's job** — File-based routing (`pages/`), layouts, middleware → [[nuxt]].

## 9. Styling

- [ ] **UnoCSS** or **Tailwind CSS** — UnoCSS is faster, smaller, Vue-native. Tailwind has larger ecosystem. Pick one.
- [ ] **`<style scoped>`** — component-scoped styles. No leakage. `:deep()` for child component styling.
- [ ] **CSS Modules** — `<style module>` for programmatic access. `$style.container`.
- [ ] **Component library** — PrimeVue (most complete), shadcn-vue (Radix port), or Naive UI (Tree-shakable, great TS). Don't build modals/dropdowns from scratch.
- [ ] **Dark mode** — `useDark()` from VueUse. Toggle `dark` class on `<html>`. Tailwind `dark:` prefix.

## 10. Performance

- [ ] **`shallowRef` / `shallowReactive`** — only top-level reactivity. For large data structures where nested tracking isn't needed.
- [ ] **Virtual scrolling** — `@tanstack/vue-virtual` or `vue-virtual-scroller`. Lists > 100 items.
- [ ] **Image optimization (SPA path)** — `vite-plugin-image-optimizer`. Lazy loading: `loading="lazy"`. In Nuxt: `@nuxt/image` → [[nuxt]].
- [ ] **LCP < 2.5s** — `vite-plugin-vue-inspector` to find render bottlenecks.
- [ ] **Route/component code splitting** — lazy routes (§8) + `defineAsyncComponent` for heavy below-the-fold components (charts, editors).

## 11. Forms

- [ ] **vee-validate + zod** — `useForm({ validationSchema: toFormValidator(schema) })`. Field-level errors. `ErrorMessage` component or `errorMessage` from `useField`. (FormKit is the batteries-included alternative — schema-driven forms with built-in validation and a11y.)
- [ ] **Server-side validation too** — Zod schema on both ends if possible. Client = UX, server = security (where that happens: [[nuxt]] §Nitro Server Routes or your API).
- [ ] **Form states handled** — Idle, submitting (disable + spinner), success (redirect/toast), error (inline fields + form-level).

## 12. Composables (Reusable Logic)

- [ ] **Data-fetching composable** — wrapped TanStack Query (§6) or custom composable with loading/error/data states. In Nuxt, `useFetch`/`useAsyncData` are built in → [[nuxt]].
- [ ] **VueUse** — `useStorage`, `useDark`, `useToggle`, `useDebounceFn`, `useThrottleFn`. Don't write your own.
- [ ] **Composable naming** — `use*` prefix. Auto-imported by `unplugin-auto-import` (in Nuxt: auto-import caveats → [[nuxt]]).

## 13. Testing

- [ ] **Vitest** — fast, Vite-native. `@vue/test-utils` and/or Vue Testing Library (`@testing-library/vue`) for component mounting.
- [ ] **Component tests** — `mount(Component, { props, slots })`. `wrapper.find()`, `wrapper.emitted()`. Test behavior, not implementation.
- [ ] **MSW (Mock Service Worker)** — API mocking at network level. Components test against realistic responses.
- [ ] **Playwright** — E2E for critical flows. Also visual regression with `toHaveScreenshot()`.
- [ ] **Query by role/text, not structure** — prefer `findByText`/`findByRole` (Testing Library). Avoid `findComponent` (couples tests to implementation structure).

## 14. Accessibility

- [ ] Semantic HTML — `<button>` for actions, `<nav>` for nav. Vue templates are HTML-first — use it.
- [ ] `v-bind` for ARIA — `:aria-expanded="isOpen"`, `:aria-label="'Close ' + title"`.
- [ ] Focus management — `nextTick(() => ref.value?.focus())` after v-if reveals content.
- [ ] Heading hierarchy — one `<h1>`, nested `<h2>` → `<h3>`.
- [ ] Screen reader tested at least once — VoiceOver or NVDA.

## 15. Security (Frontend-Specific)

- [ ] No `v-html` without sanitization — use `DOMPurify.sanitize(userContent)`.
- [ ] No secrets in `VITE_*` env vars — these ship to the browser (Nuxt runtime config prefixes → [[nuxt]]).
- [ ] Auth tokens in HTTP-only cookies, not localStorage.
- [ ] CSP configured → [[03 API Security]].

## 16. Build & Deploy (Vite SPA Path)

- [ ] **Production build tested locally** — `vite build && vite preview` before shipping. No `vue-tsc` type errors, no warnings.
- [ ] **Environment variables** — Only `VITE_*` reach the client; documented which are required/optional. No secret ever prefixed.
- [ ] **SPA fallback routing** — Server/CDN configured to serve `index.html` for all routes (else deep links 404).
- [ ] **Bundle analysis** — `rollup-plugin-visualizer`. Budgets in CI (initial JS < 200KB gzipped). Catch accidentally-large deps.
- [ ] **CI/CD** — Lint → type-check (`vue-tsc`) → unit test → build → deploy preview → E2E → promote.
- [ ] **With a meta-framework, build/deploy is the framework's job** — Nitro presets, `.output`, SSR caching → [[nuxt]].

## 17. Error Handling & Observability

- [ ] **App-level error handler** — `app.config.errorHandler` logs and reports uncaught errors. Not silent failures.
- [ ] **`onErrorCaptured` boundaries** — Component-level capture with fallback UI + retry, not a white screen. Wrap risky features.
- [ ] **Sentry (client)** — `@sentry/vue` with source maps uploaded (not public). Breadcrumbs + user context. `beforeSend` PII scrubbing (§19).
- [ ] **Async state errors rendered** — Every fetch/mutation has loading, empty, error, and loaded UI states (§6).

## 18. AI/LLM Integration (Client)

- [ ] **Vercel AI SDK Vue** — `@ai-sdk/vue` `useChat` composable for chat UIs. Streaming server side (`server/api/chat.ts`) → [[nuxt]].
- [ ] **Never expose provider keys** — `VITE_*` / `NUXT_PUBLIC_*` ship to the browser. All LLM calls go through server routes or your backend. Keys live in server-only env (`NUXT_SECRET_*` / `.env` server-side → [[nuxt]]).
- [ ] **Streaming UX** — SSE from server route, `useChat` parses the stream. Typing indicator, partial markdown, stop button, regenerate + edit messages.
- [ ] **Markdown rendering** — `markdown-it` or `marked` + DOMPurify sanitization. Never `v-html` raw model output.
- [ ] **AI state with Pinia** — chat history, pending status, and streamed messages in a Pinia store when multiple components share them. `$state`-style local refs for single-view chats.
- [ ] **Non-chat AI calls** — TanStack Query (Vue Query) mutations with loading/error states. Cache identical prompts (response dedup). Debounce expensive AI calls.
- [ ] **Graceful degradation** — Error state with retry, cached fallback, "AI can be wrong" disclaimers where user-facing. Rate-limit UX on 429.

## 19. Data Privacy & Compliance (Frontend-Specific)

- [ ] **Error monitoring scrubbing** — Sentry `beforeSend` strips PII (emails, tokens, form values) from error payloads.
- [ ] **Cookie consent** — GDPR/CCPA banner before analytics fire. Load Plausible/Umami/PostHog only after opt-in.
- [ ] **PII minimization** — Don't store user data in localStorage/IndexedDB unnecessarily. Mask sensitive data in UI previews.
- [ ] **Third-party script inventory** — Audit what loads, what it collects, where it's sent (EU/US). Remove dead scripts. (In Nuxt: `@nuxt/scripts` → [[nuxt]].)
- [ ] **Data retention UI** — "Delete my data" / "Export my data" flows calling backend erasure/export endpoints.
- [ ] **Privacy policy & terms** — Up-to-date, linked in footer. Cover collection, retention, rights (access/erasure/portability).
- [ ] **Do Not Track / GPC** — Respect `navigator.doNotTrack` and Global Privacy Control where feasible.

---

## Quick Sanity Check Before Launch

- [ ] No console errors in production build
- [ ] `vite build` succeeds — no type errors, no warnings
- [ ] Lighthouse ≥ 90 on mobile
- [ ] Forms submit with Enter key
- [ ] Back button works (no redirect loops; SPA fallback configured)
- [ ] Tested on actual mobile device
- [ ] All `<img>` have `alt`, all `<a>` have `href` or `@click` with keyboard support
- [ ] No `v-if` on `<Transition>` root — use `v-show` or wrap in inner element
- [ ] Meta-framework items → [[nuxt]] Quick Sanity Check

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
| 1 | Project Setup (Vite SPA) | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 2 | Architecture & Code Organization | 🟡 | 🟡 | ✅ | ✅ | ✅ + boundaries | ✅ + enforced CI | ✅ + formal review |
| 3 | Composition API & Script Setup | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 4 | Components | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 5 | State Management (Pinia) | 🟡 | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ |
| 6 | Data Fetching & Server State | 🟡 fetch basics | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ |
| 7 | Real-Time & Live Data | ❌ | 🟡 if used | 🟡 if used | ✅ if used | ✅ + scale | ✅ + load testing | ✅ + HA/failover |
| 8 | Routing (Vue Router 4 SPA) | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 9 | Styling | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ + design system |
| 10 | Performance | ❌ | 🟡 basic CWV | ✅ | ✅ + budgets | ✅ + profiling | ✅ + SLO | ✅ + capacity |
| 11 | Forms | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ + audit trail |
| 12 | Composables | ❌ | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ |
| 13 | Testing | ❌ maybe smoke | 🟡 unit | ✅ + component | ✅ + E2E | ✅ + visual reg | ✅ + a11y in CI | ✅ + formal verification |
| 14 | Accessibility | ❌ | 🟡 basics | ✅ | ✅ WCAG AA | ✅ + audits | ✅ + WCAG AA certified | ✅ + legal/regulatory |
| 15 | Security (Frontend) | 🟡 no secrets | 🟡 essentials | ✅ | ✅ + CSP | ✅ + pentest | ✅ + hardened | ✅ + formal audit |
| 16 | Build & Deploy (Vite SPA) | ❌ | 🟡 basic build | ✅ + CI | ✅ + previews | ✅ + canary + flags | ✅ + full pipeline | ✅ + signed artifacts |
| 17 | Error Handling & Observability | ❌ | 🟡 error handler | ✅ + Sentry | ✅ + RUM | ✅ + dashboards | ✅ + SLO/alerting | ✅ + full stack |
| 18 | AI/LLM Integration (Client) | 🟡 if AI is the POC | 🟡 | 🟡 if used | ✅ if used | ✅ | ✅ + guardrails | ✅ + audit trail |
| 19 | Data Privacy & Compliance | ❌ | ❌ | 🟡 minimal | ✅ consent + PII | ✅ + DPA | ✅ full compliance | ✅ + regulatory framework |

---

## Sources

- Vue 3 Docs — https://vuejs.org/
- General companion: [[web]] · Meta-framework layer: [[nuxt]] · Launch gate: [[Frontend Launch]]
- Split 2026-09-14 from `vue-js.md`: library concerns here, Nuxt concerns in [[nuxt]].
- SEO & Metadata, SSR/hybrid rendering, and Nitro server concerns moved to [[nuxt]].
