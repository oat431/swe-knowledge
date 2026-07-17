---
tags:
- hci
- computing
- foundations
- accessibility
---

# 03 Accessibility Basics

Accessibility (a11y) means designing software that works for everyone — including people with visual, auditory, motor, or cognitive disabilities. It's not optional. It's an engineering requirement, often a legal one, and it makes software better for ALL users.

---

## Why Developers Must Care

| Reason | Reality |
|--------|--------|
| **15% of world population has a disability** | ~1 billion people. Your users are among them. |
| **Legal requirement** | ADA (US), EAA (EU), DDA (UK/AU). Lawsuits are real. |
| **Better for everyone** | Curb cuts help wheelchairs AND strollers AND luggage. Captions help deaf AND noisy environments. |
| **SEO benefit** | Accessible sites rank better (semantic HTML, alt text, structure). |

---

## WCAG (Web Content Accessibility Guidelines)

The standard. Four principles — **POUR:**

| Principle | Meaning | Example |
|-----------|---------|--------|
| **Perceivable** | Can users perceive the content? | Alt text on images, captions on video, sufficient color contrast |
| **Operable** | Can users operate the interface? | Keyboard navigation, no time limits, no seizure-triggering content |
| **Understandable** | Can users understand the content? | Clear language, predictable navigation, error guidance |
| **Robust** | Does it work with assistive tech? | Semantic HTML, ARIA labels, valid markup |

**Conformance levels:** A (minimum) → AA (standard target) → AAA (highest)

---

## Top Developer Mistakes

| Mistake | Fix |
|---------|-----|
| Images without `alt` text | Always add descriptive alt (or `alt=""` for decorative) |
| `<div>` with click handler instead of `<button>` | Use semantic HTML — it's keyboard/screen-reader accessible for free |
| Low color contrast | WCAG AA: 4.5:1 for text, 3:1 for large text. Use contrast checkers. |
| Form fields without `<label>` | Associate every input with a label (`for="id"` or wrapping) |
| Focus not visible | Never `outline: none` without a replacement. Focus indicators are essential. |
| Content only conveyed by color | Add icons, text, or patterns. Color-blind users can't distinguish red/green alone. |
| Auto-playing media | Never auto-play with sound. Provide pause/stop controls. |

---

## Quick Wins (5 minutes each)

1. Run `axe DevTools` or Lighthouse accessibility audit on your page
2. Try navigating your app with keyboard only (Tab, Enter, Escape)
3. Check color contrast with WebAIM Contrast Checker
4. Add `alt` text to every meaningful image
5. Use `<button>`, `<a>`, `<nav>`, `<main>`, `<header>` — not `<div>` for everything

---

## Testing Tools

| Tool | What It Does |
|------|-------------|
| **axe DevTools** | Browser extension, automated a11y audit |
| **Lighthouse** | Chrome built-in, a11y score + recommendations |
| **NVDA / VoiceOver** | Screen readers — test like your blind users |
| **WebAIM Contrast Checker** | Check text/background contrast ratios |
| **Stark (Figma plugin)** | Design-time contrast and simulation |

---

## Sources

- WCAG 2.2 — https://www.w3.org/WAI/WCAG22/quickref/
- WebAIM — https://webaim.org/
- a11yproject.com — https://www.a11yproject.com/
