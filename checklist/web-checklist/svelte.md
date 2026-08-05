# Svelte Launch Checklist

> Tick every box before a Svelte 5 app hits production. Framework companion to [[Frontend Launch]]. Svelte 5 introduced runes — reactive state without `$:` or stores.
> Last updated: 2026-08-05

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

## AI/LLM Integration

- [ ] **Vercel AI SDK Svelte** — `@ai-sdk/svelte` `useChat` for chat UIs. SvelteKit `+server.ts` route streams with `streamText` + `toDataStreamResponse()`.
- [ ] **Never expose provider keys** — `$env/static/public` ships to the browser. LLM keys live in `$env/static/private` / `$env/dynamic/private` only, accessed in `+server.ts` or `+page.server.ts`.
- [ ] **Rune-based AI state** — `$state()` for messages and streaming status, `$derived()` for computed UI state, `$effect()` for scroll-to-bottom side effects. `$state.snapshot()` for persistence.
- [ ] **Streaming UX** — SSE from server route, `useChat` parses the stream. Typing indicator, partial markdown, stop button, regenerate + edit messages.
- [ ] **Markdown rendering** — `marked` or `mdsvex` + DOMPurify. Never bare `{@html}` on model output — always `{@html DOMPurify.sanitize(content)}`.
- [ ] **Non-chat AI calls** — TanStack Query Svelte `createMutation` for classify/extract/summarize. Cache identical prompts (response dedup).
- [ ] **Graceful degradation** — Error state with retry, cached fallback, "AI can be wrong" disclaimers where user-facing. Rate-limit UX on 429.

## Data Privacy & Compliance (Frontend-Specific)

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
| 2 | Runes (Svelte 5 Reactivity) | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 3 | State Management | 🟡 | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ |
| 4 | SvelteKit Routing & Data Loading | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 5 | Components | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 6 | Styling | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ + design system |
| 7 | Forms | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ + audit trail |
| 8 | Performance | ❌ | 🟡 basic CWV | ✅ | ✅ + budgets | ✅ + profiling | ✅ + SLO | ✅ + capacity |
| 9 | Routing | 🟡 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 10 | Testing | ❌ maybe smoke | 🟡 unit | ✅ + component | ✅ + E2E | ✅ + visual reg | ✅ + a11y in CI | ✅ + formal verification |
| 11 | Accessibility | ❌ | 🟡 basics | ✅ | ✅ WCAG AA | ✅ + audits | ✅ + WCAG AA certified | ✅ + legal/regulatory |
| 12 | Security (Frontend) | 🟡 no secrets | 🟡 essentials | ✅ | ✅ + CSP | ✅ + pentest | ✅ + hardened | ✅ + formal audit |
| 13 | SvelteKit Adapters | ❌ | 🟡 adapter-auto | ✅ + node | ✅ + platform | ✅ + edge | ✅ + multi-region | ✅ + signed artifacts |
| 14 | AI/LLM Integration | 🟡 if AI is the POC | 🟡 | 🟡 if used | ✅ if used | ✅ | ✅ + guardrails | ✅ + audit trail |
| 15 | Data Privacy & Compliance | ❌ | ❌ | 🟡 minimal | ✅ consent + PII | ✅ + DPA | ✅ full compliance | ✅ + regulatory framework |

---

## Sources

- Svelte 5 Docs — https://svelte.dev/
- SvelteKit Docs — https://kit.svelte.dev/
- `[[Frontend Launch]]` — general frontend checklist (tick first)
