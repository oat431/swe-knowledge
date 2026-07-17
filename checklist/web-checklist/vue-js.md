# Vue Launch Checklist

> Tick every box before a Vue 3 app hits production. For Nuxt-specific items, see the Nuxt section. Framework companion to [[Frontend Launch]].

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

## Sources

- Vue 3 Docs — https://vuejs.org/
- `[[Frontend Launch]]` — general frontend checklist (tick first)
- Nuxt 3 Docs — https://nuxt.com/
