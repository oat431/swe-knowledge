# React Launch Checklist (v2)

> Tick every box before a React/Next.js app hits production. For deep reference, see [[react-js]].
> Assumes Next.js App Router + TypeScript. For Vite/SPA: skip Next-specific items.

---

## Rendering

- [ ] Rendering model chosen: Server Components by default. `"use client"` only for interactivity → [[react-js]]
- [ ] `loading.tsx` per route, `<Suspense>` per component — no white screens
- [ ] `error.tsx` per route, global `<ErrorBoundary>` — no white screens on crash
- [ ] Static pages use `export const revalidate` or `generateStaticParams` — not SSR'd every request

---

## Data Fetching

- [ ] TanStack Query for all async data (or SWR for simpler needs) → [[react-js]]
- [ ] `staleTime` tuned per query type (short for real-time, long for static)
- [ ] Optimistic updates on mutations — UI responds instantly, rolls back on error
- [ ] Prefetch on hover/focus for common navigation paths
- [ ] Server Components fetch data — no `useEffect` + `fetch` for initial page data

---

## State

- [ ] Zustand or Jotai for client state — no Redux unless you have a specific, justified need
- [ ] URL params for filters, pagination, search — shareable, back-button works
- [ ] React Hook Form + Zod for forms — uncontrolled by default, schema validation
- [ ] No Context for frequent-update state (re-renders all consumers)

---

## Routing

- [ ] Next.js App Router — file-based routing. `layout.tsx` for persistent layouts
- [ ] Loading UI per route — `loading.tsx`, `<Suspense>` for component-level
- [ ] Error boundaries per route — `error.tsx`
- [ ] `middleware.ts` for auth guards, redirects, locale detection
- [ ] Dynamic routes with typed params — `/user/[id]` → `params.id`

---

## Performance

- [ ] `next/image` with `sizes`, `priority` on LCP image, `placeholder="blur"`
- [ ] `next/font` with `subset`, `display: swap`, self-hosted fonts
- [ ] `next/dynamic` for heavy components below the fold → [[react-js]]
- [ ] `@tanstack/react-virtual` for lists > 50 items in viewport
- [ ] Debounce search inputs — not `onChange` → API call
- [ ] Bundle analyzed — no accidentally-large deps, no duplicated libraries
- [ ] LCP < 2.5s, INP < 200ms, CLS < 0.1 on mobile

---

## Styling

- [ ] Tailwind CSS + `tailwind-merge`/`clsx` for conditional classes
- [ ] shadcn/ui or Radix UI primitives — don't build modals/dropdowns from scratch
- [ ] Design tokens as CSS custom properties — consistent colors, spacing, radii
- [ ] Dark mode via Tailwind `dark:` prefix — tested on both themes

---

## Accessibility

- [ ] Semantic HTML — `<button>` for actions, `<nav>` for nav, `<main>` for content
- [ ] Logical heading hierarchy — one `<h1>`, nested `<h2>` → `<h3>`
- [ ] All images have meaningful `alt` text
- [ ] Color contrast ≥ WCAG AA (4.5:1 text, 3:1 large text)
- [ ] Keyboard navigation works — Tab order logical, focus indicators visible
- [ ] Screen reader tested at least once (VoiceOver or NVDA)

---

## Security

- [ ] No `dangerouslySetInnerHTML` without DOMPurify
- [ ] No secrets in `NEXT_PUBLIC_*` / `VITE_*` — these ship to the browser
- [ ] Auth tokens in HTTP-only cookies, not localStorage
- [ ] CSP configured — no `unsafe-inline`, no `unsafe-eval` → [[03 API Security]]
- [ ] `npm audit` / `pnpm audit` in CI — zero critical/high CVEs

---

## Testing

- [ ] Vitest + React Testing Library for component tests
- [ ] MSW for API mocking — components test against realistic responses
- [ ] Playwright for critical user flows (login → navigate → create → delete)
- [ ] `jest-axe` or `@axe-core/playwright` — accessibility violations caught in CI
- [ ] Tested on: Chrome, Firefox, Safari, mobile Safari/Chrome on actual device

---

## SEO & Meta

- [ ] `<title>` unique and descriptive on every page
- [ ] `<meta name="description">` on every page
- [ ] Open Graph tags: `og:title`, `og:image`, `og:description`
- [ ] `robots.txt` and `sitemap.xml` present
- [ ] Canonical URLs on pages with duplicate content

---

## Deployment

- [ ] Build tested locally — `next build && next start`
- [ ] Preview deployment per PR — Vercel/Netlify/Cloudflare Pages
- [ ] Static assets cached with content hash (`/_next/static/*` → immutable)
- [ ] Environment variables documented (which are required, which are optional)
- [ ] 404 page custom — not the framework default
- [ ] Error monitoring: Sentry, LogRocket, or equivalent

---

## Quick Sanity Check

- [ ] No console errors in production build
- [ ] Lighthouse ≥ 90 on mobile (Performance, Accessibility, Best Practices)
- [ ] Forms submit with Enter key
- [ ] Back button works (no redirect loops)
- [ ] Tested on actual mobile device, not just DevTools responsive mode
- [ ] Feature flags in place for risky changes (LaunchDarkly or simple flags)

---

## Sources

- Deep reference: [[react-js]] (original checklist with detailed explanations)
- `[[Frontend Launch]]` — general frontend checklist (tick first)
