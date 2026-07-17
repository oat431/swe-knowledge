# Svelte Launch Checklist

> Tick every box before a Svelte 5 app hits production. Framework companion to [[Frontend Launch]]. Svelte 5 introduced runes — reactive state without `$:` or stores.

---

## Project Setup

- [ ] **SvelteKit** — `npx sv create`. SvelteKit is to Svelte what Next.js is to React. Use it for everything except embeddable widgets.
- [ ] **TypeScript** — `lang="ts"` in `<script>` blocks. `svelte.config.js`: `vitePlugin: { inspector: true }` for devtools.
- [ ] **pnpm** — fast, strict. Lockfile committed.
- [ ] **ESLint + Prettier** — `@sveltejs/eslint-config`. `svelte-check` for type checking `.svelte` files.
- [ ] **Svelte 5 runes mode** — `compilerOptions: { runes: true }` in config. Runes are the new reactivity model. No `$:`, no `$store`, use `$state`, `$derived`, `$effect`.

---

## Runes (Svelte 5 Reactivity)

- [ ] **`$state()`** — reactive variable. `let count = $state(0)`. Any reassignment triggers reactivity. Works in `.svelte`, `.svelte.ts`, `.svelte.js`.
- [ ] **`$derived()`** — computed value. `let double = $derived(count * 2)`. Auto-tracks deps. Lazy — only recomputed when read.
- [ ] **`$effect()`** — side effects. `$effect(() => console.log(count))`. Runs after DOM updates. Auto-cleanup on destroy. `$effect.pre` for pre-DOM effects.
- [ ] **`$props()`** — component props. `let { name, count = 0 } = $props()`. Replaces `export let`. Default values, rest props (`...rest`).
- [ ] **`$bindable()`** — two-way binding prop. `let { value = $bindable(0) } = $props()`. Parent can `<Component bind:value={myVal} />`.
- [ ] **`$inspect()`** — debug reactive values. `$inspect(count)`. Logs whenever value changes. Stripped in production builds.
- [ ] **`$state.snapshot()`** — plain object copy of reactive state. For serialization, logging, non-reactive consumers.

---

## State Management

- [ ] **Runes for local state** — `$state()` in components, `$state()` in `.svelte.ts` modules. No store library needed for most cases.
- [ ] **Shared state** — export `$state` from `.svelte.ts` files. Import anywhere. Truly reactive cross-component state without stores or context APIs.
- [ ] **Svelte stores (legacy)** — `writable`, `readable`, `derived`. Still work but runes are the future. `$store` auto-subscribe syntax still valid.
- [ ] **TanStack Query (Svelte)** — for server state. `createQuery`, `createMutation`. Caching, refetch, optimistic updates.
- [ ] **URL state** — SvelteKit `$page.url.searchParams`. Built-in. No library needed.

---

## SvelteKit — Routing & Data Loading

- [ ] **File-based routing** — `src/routes/` directory. `+page.svelte` for UI, `+page.ts` for data loading, `+layout.svelte` for persistent layouts.
- [ ] **`load` functions** — `+page.ts`: `export async function load({ params, fetch, url }) { return { user: await fetchUser(params.id) } }`. Data flows to `+page.svelte` via `let { data } = $props()`.
- [ ] **Universal vs server loads** — `+page.ts` = universal (runs on server AND client for navigation). `+page.server.ts` = server-only (secrets, DB access).
- [ ] **Form actions** — `+page.server.ts`: `export const actions = { default: async ({ request }) => { ... } }`. Form `method="POST" action="?/actionName"`. Progressive enhancement — works without JS.
- [ ] **`enhance`** — SvelteKit's form enhancer. `use:enhance={({ formElement, formData, action, result }) => ...}`. Ajaxy forms without writing fetch calls.
- [ ] **Error handling** — `+error.svelte` per route segment. `throw error(404, 'Not found')` in load. `throw redirect(302, '/login')`.
- [ ] **Hooks** — `src/hooks.server.ts`: `handle()`, `handleFetch()`, `handleError()`. Auth guards, header injection, error logging.

---

## Components

- [ ] **Template syntax** — `{#if}`, `{#each items as item (item.id)}`, `{#await promise}`, `{#snippet name()}...{/snippet}`. No JSX — HTML-first.
- [ ] **`{@render}`** — render snippets or components. `{@render children()}`. Replaces `<slot>` (Svelte 4).
- [ ] **`{@html}`** — raw HTML. Must sanitize. `{@html DOMPurify.sanitize(content)}`. Never bare `{@html userInput}`.
- [ ] **Event handlers** — `onclick={handler}` (lowercase). Modifiers: `onclick|preventDefault={handler}`, `onclick|once={handler}`.
- [ ] **Styling** — `<style>` in `.svelte` files is component-scoped by default. Zero config. `:global()` for escaping.
- [ ] **`bind:` directive** — `bind:value={name}` for inputs. `bind:this={element}` for DOM refs. `bind:open={isOpen}` on `<dialog>`.

---

## Styling

- [ ] **Tailwind CSS** — works natively. `@tailwind base/components/utilities` in `app.css`. Or UnoCSS (lighter, tree-shakable).
- [ ] **Component library** — Melt UI (headless, accessible primitives) + shadcn-svelte (styled). Or Skeleton UI (Tailwind-native). Don't build from scratch.
- [ ] **Dark mode** — `tailwind.config`: `darkMode: 'class'`. Toggle `dark` class on `<html>`. Or use SvelteKit's `handle` hook to read `prefers-color-scheme`.

---

## Forms

- [ ] **SvelteKit form actions** — no client-side form library needed for basic cases. `export const actions` + `use:enhance`.
- [ ] **Superforms** — `sveltekit-superforms` + `zod`. `const form = await superValidate(request, zodSchema)`. Client: `const { form, enhance, errors } = superForm(data.form)`. Field-level errors built-in.
- [ ] **Progressive enhancement** — forms work without JavaScript. `use:enhance` adds AJAX + validation on top.
- [ ] **Form states** — Superforms provides `$submitting`, `$errors`, `$message`, `$tainted`. Every state is tracked and accessible in template.

---

## Performance

- [ ] **No virtual DOM** — Svelte compiles to direct DOM manipulation. This is the default. No extra configuration needed.
- [ ] **Build-time optimization** — dead code elimination at compile time. Unused CSS purged. Reactive declarations optimized to minimal DOM updates. All automatic — no plugins required.
- [ ] **Code splitting** — route-level splitting is automatic in SvelteKit. `import()` for component-level splitting.
- [ ] **`{#key}`** — force re-render when expression changes. `{#key item.id}<ExpensiveComponent />{/key}`.
- [ ] **Image optimization** — `@sveltejs/enhanced-img`. `<enhanced:img src="./pic.jpg" alt="..." />`. Auto resizes, generates srcset, lazy-loads.
- [ ] **LCP < 2.5s** — Svelte's compile-time approach typically produces smaller bundles than React/Vue frameworks by default. Verify with Lighthouse.

---

## Routing

- [ ] SvelteKit file-based routing — `+page.svelte`, `+layout.svelte`. Nested layouts preserved across navigations
- [ ] Dynamic routes — `[id]` folders. Typed params in `load` functions
- [ ] Navigation guards — `hooks.server.ts` for auth checks
- [ ] Error boundaries per route — `+error.svelte`
- [ ] 404 — `+error.svelte` in root with `$page.status === 404`

---

## Testing

- [ ] **Vitest** — fast, Vite-native, works with Svelte. `@sveltejs/vite-plugin-svelte`.
- [ ] **`@testing-library/svelte`** — `render(Component, { props })`. `screen.getByRole()`, `screen.getByText()`. Test behavior, not structure.
- [ ] **Playwright** — E2E. SvelteKit has first-class support. `page.goto('/login')`, `page.fill()`, `page.click()`.
- [ ] **`svelte-check`** — type-check `.svelte` files. Runs in CI. `svelte-check --tsconfig ./tsconfig.json`.

---

## Accessibility

- [ ] Semantic HTML — `{#if}`/`{#each}` encourage native HTML structures. `<button>` for actions, `<a>` for navigation.
- [ ] Svelte accessibility warnings — compiler warns about missing `alt`, unlabeled inputs, positive tabindex, missing `lang`. Fix all warnings — they're free a11y audits.
- [ ] Focus management — `bind:this={element}` + `element.focus()` after conditional renders.

---

## Security

- [ ] No `{@html}` without DOMPurify — `{@html DOMPurify.sanitize(userContent)}`.
- [ ] No secrets in `$env/static/public` — these are inlined at build time. `$env/static/private` for server-only.
- [ ] CSP — SvelteKit outputs static HTML + minimal JS. Works well with strict CSPs (no `unsafe-inline` needed once `enhanced:img` generates proper `srcset` without inline styles).

---

## SvelteKit Adapters

- [ ] **Adapter choice** — `adapter-auto` (detects environment). `adapter-node` (self-hosted Node). `adapter-vercel`, `adapter-cloudflare`, `adapter-netlify` (serverless edge). `adapter-static` (SPA or fully static).
- [ ] **Edge deployment** — `adapter-cloudflare-workers` or `adapter-vercel`. SvelteKit is edge-ready. SSR at the edge with minimal cold start.

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

## Sources

- Svelte 5 Docs — https://svelte.dev/
- SvelteKit Docs — https://kit.svelte.dev/
- `[[Frontend Launch]]` — general frontend checklist (tick first)
