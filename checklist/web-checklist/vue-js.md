# Vue Launch Checklist

> Tick every box before a Vue 3 app hits production. For Nuxt-specific items, see the Nuxt section. Framework companion to [[Frontend Launch]].
> Last updated: 2026-08-05

---

## Project Setup

- [ ] **Vite + Vue 3.5+** — `create-vue` (official scaffold). Composition API + `<script setup>` by default.
- [ ] **TypeScript strict** — `tsconfig.json`: `strict: true`. `vue-tsc` for type checking `.vue` files (slower but safer).
- [ ] **pnpm** — strict, fast, disk-efficient. Lockfile committed.
- [ ] **ESLint + Prettier** or **Biome** — Biome is gaining. `@vue/eslint-config-typescript`. Pre-commit: `lint-staged`.
- [ ] **Vue DevTools** — browser extension + Vite plugin (`vite-plugin-vue-devtools`). Browser extension is legacy now.

---

## Composition API & Script Setup

- [ ] **`<script setup lang="ts">`** — default. No `setup()` function boilerplate.
- [ ] **`ref` vs `reactive`** — `ref()` for primitives and single values. `reactive()` only for objects with known shape (rare — `ref` with object works fine).
- [ ] **`computed`** — derived state. Never mutate a computed. Use writable computed for v-model support.
- [ ] **`watch` vs `watchEffect`** — `watch` for explicit deps. `watchEffect` for auto-tracking. `watchEffect` runs immediately.
- [ ] **`defineProps`, `defineEmits`, `defineExpose`** — compiler macros. No imports needed. Type-safe: `defineProps<{ items: Item[] }>()`.
- [ ] **`provide`/`inject`** — for deeply nested component trees. Not a replacement for Pinia — use for component-level dependency injection.
- [ ] **`useTemplateRef` (Vue 3.5+)** — typed template refs. `const input = useTemplateRef<HTMLInputElement>('myInput')`. Replaces string refs.
- [ ] **No Options API mixing** — pick one style per component. Composition API is the future.

---

## State Management

- [ ] **Pinia** — official state management. `defineStore('name', () => { ... })`. Setup stores (composition-style) over options stores.
- [ ] **TanStack Query (Vue Query)** — for all server state. `useQuery`, `useMutation`. Namespaced keys: `['users', userId]`.
- [ ] **URL as state** — `useRoute().query` + `useRouter().push`. Filters, pagination, search in URL params.
- [ ] **No Vuex** — deprecated. Pinia is the replacement.
- [ ] **Form state** — `vee-validate` + `zod`. `useForm({ validationSchema: toFormValidator(zodSchema) })`. Field-level error messages.

---

## Routing (Vue Router 4)

- [ ] **`createRouter` with `createWebHistory`** — HTML5 history mode. No hash unless legacy support needed.
- [ ] **Route meta** — `meta: { requiresAuth: true, title: 'Dashboard' }`. Navigation guards check `route.meta`.
- [ ] **`beforeEach` guard** — auth check, redirect to login. `router.beforeEach((to, from, next) => { ... })`.
- [ ] **Lazy-loaded routes** — `component: () => import('./views/Heavy.vue')`. Automatic code splitting.
- [ ] **Nested routes** — `children: [...]` with `<router-view>` in parent. Persistent layouts.
- [ ] **Scroll behavior** — `scrollBehavior(to, from, savedPosition)`. Restore scroll on back navigation.

---

## Styling

- [ ] **UnoCSS** or **Tailwind CSS** — UnoCSS is faster, smaller, Vue-native. Tailwind has larger ecosystem. Pick one.
- [ ] **`<style scoped>`** — component-scoped styles. No leakage. `:deep()` for child component styling.
- [ ] **CSS Modules** — `<style module>` for programmatic access. `$style.container`.
- [ ] **Component library** — PrimeVue (most complete), shadcn-vue (Radix port), or Naive UI (Tree-shakable, great TS). Don't build modals/dropdowns from scratch.
- [ ] **Dark mode** — `useDark()` from VueUse. Toggle `dark` class on `<html>`. Tailwind `dark:` prefix.

---

## Performance

- [ ] **`<Suspense>`** — async component loading with fallback. Still experimental but stable in 3.5+.
- [ ] **`<KeepAlive>`** — cache component instances. Preserve form state across tab switches.
- [ ] **`v-memo`** — skip re-render when deps unchanged. For heavy lists: `<div v-for="item in list" :key="item.id" v-memo="[item.id, item.selected]">`.
- [ ] **`shallowRef` / `shallowReactive`** — only top-level reactivity. For large data structures where nested tracking isn't needed.
- [ ] **Virtual scrolling** — `@tanstack/vue-virtual` or `vue-virtual-scroller`. Lists > 100 items.
- [ ] **Image optimization** — `@nuxt/image` (Nuxt) or `vite-plugin-image-optimizer`. Lazy loading: `loading="lazy"`.
- [ ] **LCP < 2.5s** — `vite-plugin-vue-inspector` to find render bottlenecks.

---

## Forms

- [ ] **vee-validate + zod** — `useForm({ validationSchema: toFormValidator(schema) })`. Field-level errors. `ErrorMessage` component or `errorMessage` from `useField`.
- [ ] **Server-side validation too** — Zod schema on both ends if possible. Client = UX, server = security.
- [ ] **Form states handled** — Idle, submitting (disable + spinner), success (redirect/toast), error (inline fields + form-level).

---

## Composables (Reusable Logic)

- [ ] **`useFetch` / `useAsyncData`** — wrapped TanStack Query or custom composable with loading/error/data states.
- [ ] **VueUse** — `useStorage`, `useDark`, `useToggle`, `useDebounceFn`, `useThrottleFn`. Don't write your own.
- [ ] **Composable naming** — `use*` prefix. Auto-imported by `unplugin-auto-import`.

---

## Testing

- [ ] **Vitest** — fast, Vite-native. `@vue/test-utils` for component mounting.
- [ ] **Component tests** — `mount(Component, { props, slots })`. `wrapper.find()`, `wrapper.emitted()`. Test behavior, not implementation.
- [ ] **MSW (Mock Service Worker)** — API mocking at network level. Components test against realistic responses.
- [ ] **Playwright** — E2E for critical flows. Also visual regression with `toHaveScreenshot()`.
- [ ] **`@vue/test-utils` best practices** — prefer `findByText`/`findByRole`. Avoid `findComponent` (couples tests to structure).

---

## Accessibility

- [ ] Semantic HTML — `<button>` for actions, `<nav>` for nav. Vue templates are HTML-first — use it.
- [ ] `v-bind` for ARIA — `:aria-expanded="isOpen"`, `:aria-label="'Close ' + title"`.
- [ ] Focus management — `nextTick(() => ref.value?.focus())` after v-if reveals content.
- [ ] Heading hierarchy — one `<h1>`, nested `<h2>` → `<h3>`.
- [ ] Screen reader tested at least once — VoiceOver or NVDA.

---

## Security

- [ ] No `v-html` without sanitization — use `DOMPurify.sanitize(userContent)`.
- [ ] No secrets in `VITE_*` env vars — these ship to the browser.
- [ ] Auth tokens in HTTP-only cookies, not localStorage.
- [ ] CSP configured → [[03 API Security]].

---

## Nuxt 3 (SSR / Full-Stack)

- [ ] **Nuxt modules** — `@nuxt/image`, `@nuxt/fonts`, `@nuxt/scripts`, `nuxt-security`. Don't DIY.
- [ ] **`useFetch` / `useAsyncData`** — built-in. Auto-deduped, cached, SSR-safe. Replaces manual `fetch` + `useState`.
- [ ] **Server routes** — `server/api/` directory. `defineEventHandler`. Good for lightweight APIs, proxies, webhooks.
- [ ] **`nuxt.config.ts`** — `app.head` for global meta, `routeRules` for per-route caching/ISR, `nitro.prerender` for static.
- [ ] **Hybrid rendering** — `routeRules: { '/blog/**': { swr: 3600 }, '/admin/**': { ssr: false } }`. Per-route rendering strategy.
- [ ] **`useHead` composable** — per-page meta. `<Title>`, `<Meta>`, `<Link>`, `<Script>` components for declarative head.

## AI/LLM Integration

- [ ] **Vercel AI SDK Vue** — `@ai-sdk/vue` `useChat` composable for chat UIs. Server route (`server/api/chat.ts` in Nuxt) streams with `streamText` + `toDataStreamResponse()`.
- [ ] **Never expose provider keys** — `VITE_*` / `NUXT_PUBLIC_*` ship to the browser. All LLM calls go through server routes or your backend. Keys live in server-only env (`NUXT_SECRET_*` / `.env` server-side).
- [ ] **Streaming UX** — SSE from server route, `useChat` parses the stream. Typing indicator, partial markdown, stop button, regenerate + edit messages.
- [ ] **Markdown rendering** — `markdown-it` or `marked` + DOMPurify sanitization. Never `v-html` raw model output.
- [ ] **AI state with Pinia** — chat history, pending status, and streamed messages in a Pinia store when multiple components share them. `$state`-style local refs for single-view chats.
- [ ] **Non-chat AI calls** — TanStack Query (Vue Query) mutations with loading/error states. Cache identical prompts (response dedup). Debounce expensive AI calls.
- [ ] **Graceful degradation** — Error state with retry, cached fallback, "AI can be wrong" disclaimers where user-facing. Rate-limit UX on 429.

## Data Privacy & Compliance (Frontend-Specific)

- [ ] **Error monitoring scrubbing** — Sentry `beforeSend` strips PII (emails, tokens, form values) from error payloads.
- [ ] **Cookie consent** — GDPR/CCPA banner before analytics fire. Load Plausible/Umami/PostHog only after opt-in.
- [ ] **PII minimization** — Don't store user data in localStorage/IndexedDB unnecessarily. Mask sensitive data in UI previews.
- [ ] **Third-party script inventory** — `@nuxt/scripts` audit: what loads, what it collects, where it's sent (EU/US). Remove dead scripts.
- [ ] **Data retention UI** — "Delete my data" / "Export my data" flows calling backend erasure/export endpoints.
- [ ] **Privacy policy & terms** — Up-to-date, linked in footer. Cover collection, retention, rights (access/erasure/portability).
- [ ] **Do Not Track / GPC** — Respect `navigator.doNotTrack` and Global Privacy Control where feasible.

---

## Quick Sanity Check

- [ ] No console errors in production build
- [ ] `vite build` succeeds — no type errors, no warnings
- [ ] Lighthouse ≥ 90 on mobile
- [ ] Forms submit with Enter key
- [ ] Back button works (no redirect loops)
- [ ] Tested on actual mobile device
- [ ] All `<img>` have `alt`, all `<a>` have `href` or `@click` with keyboard support
- [ ] No `v-if` on `<Transition>` root — use `v-show` or wrap in inner element


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
| 2 | Composition API & Script Setup | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 3 | State Management | 🟡 | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ |
| 4 | Routing (Vue Router 4) | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 5 | Styling | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ + design system |
| 6 | Performance | ❌ | 🟡 basic CWV | ✅ | ✅ + budgets | ✅ + profiling | ✅ + SLO | ✅ + capacity |
| 7 | Forms | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ + audit trail |
| 8 | Composables | ❌ | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ |
| 9 | Testing | ❌ maybe smoke | 🟡 unit | ✅ + component | ✅ + E2E | ✅ + visual reg | ✅ + a11y in CI | ✅ + formal verification |
| 10 | Accessibility | ❌ | 🟡 basics | ✅ | ✅ WCAG AA | ✅ + audits | ✅ + WCAG AA certified | ✅ + legal/regulatory |
| 11 | Security (Frontend) | 🟡 no secrets | 🟡 essentials | ✅ | ✅ + CSP | ✅ + pentest | ✅ + hardened | ✅ + formal audit |
| 12 | Nuxt 3 (SSR / Full-Stack) | ❌ SPA only | 🟡 | ✅ if SSR | ✅ | ✅ | ✅ | ✅ |
| 13 | AI/LLM Integration | 🟡 if AI is the POC | 🟡 | 🟡 if used | ✅ if used | ✅ | ✅ + guardrails | ✅ + audit trail |
| 14 | Data Privacy & Compliance | ❌ | ❌ | 🟡 minimal | ✅ consent + PII | ✅ + DPA | ✅ full compliance | ✅ + regulatory framework |

---

## Sources

- Vue 3 Docs — https://vuejs.org/
- `[[Frontend Launch]]` — general frontend checklist (tick first)
- Nuxt 3 Docs — https://nuxt.com/
