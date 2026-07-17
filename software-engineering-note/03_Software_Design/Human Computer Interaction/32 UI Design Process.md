---
tags: [ui-design, mockups, design-systems, visual-design, component-library, ux-ui-design]
---

# 02 — UI Design

> *Source: Atomic Design by Brad Frost, Refactoring UI by Wathan & Schoger, Material Design, Apple HIG*

## Purpose

UI Design focuses on **how the product looks** — visual design, component systems, and the aesthetic layer that makes interfaces beautiful, accessible, and brand-consistent.

## Visual Design Principles

### Hierarchy
- **Size** — Larger elements draw attention first
- **Color** — Bright/dark colors stand out against muted backgrounds
- **Contrast** — High-contrast elements are noticed first
- **Spacing** — White space creates separation and emphasis
- **Position** — Top-left (in LTR cultures) gets the most attention

### Typography
- **Limit fonts** — 2–3 fonts maximum (one for headings, one for body)
- **Scale** — Use a consistent type scale (e.g., 12, 14, 16, 20, 24, 32, 48)
- **Line height** — 1.4–1.6 for body text, 1.1–1.3 for headings
- **Line length** — 50–75 characters per line for readability
- **Contrast** — Text should have 4.5:1 contrast ratio (WCAG AA)

### Color
- **Limit palette** — 3–5 colors maximum
- **Primary** — Brand color, used for key actions
- **Secondary** — Supporting color, used for accents
- **Neutral** — Grays for text, borders, backgrounds
- **Semantic** — Green (success), red (error), yellow (warning), blue (info)
- **Accessibility** — Ensure sufficient contrast for all text

### Spacing & Layout
- **Grid system** — 8px or 4px grid for consistent spacing
- **Whitespace** — Don't fill every pixel; breathing room improves readability
- **Alignment** — Align elements to a grid; avoid random placement
- **Proximity** — Group related elements; separate unrelated ones

## UI Mockups

### What They Are
High-fidelity visual designs showing the final appearance of the interface — colors, typography, imagery, and brand elements.

### Mockup Deliverables
- **Screen designs** — Every unique screen in the product
- **Responsive variants** — Desktop, tablet, mobile layouts
- **State variations** — Default, hover, active, disabled, error, loading, empty
- **Component specs** — Measurements, colors, typography for developers

### Mockup Best Practices
- Design at 1x scale (actual pixel dimensions)
- Use real content, not Lorem Ipsum
- Show all states (hover, active, disabled, error)
- Include responsive breakpoints
- Annotate interactions and transitions

## Design Systems

### What They Are
A collection of reusable components, guided by clear standards, that can be assembled to build any number of applications.

### Design System Components

#### Tokens (Foundations)
- **Colors** — Primary, secondary, neutral, semantic palettes
- **Typography** — Font families, sizes, weights, line heights
- **Spacing** — Grid, margins, padding, gaps
- **Shadows** — Elevation levels
- **Border radius** — Corner rounding values
- **Icons** — Icon library and usage guidelines

#### Components
- **Buttons** — Primary, secondary, tertiary, ghost, icon-only
- **Forms** — Input fields, selects, checkboxes, radios, toggles
- **Navigation** — Menus, tabs, breadcrumbs, pagination
- **Data display** — Tables, cards, lists, badges
- **Feedback** — Alerts, toasts, modals, tooltips
- **Layout** — Grid, container, sidebar, header, footer

#### Patterns
- **Login/signup** — Authentication flows
- **Search** — Search bar, filters, results
- **Empty states** — What users see when there's no data
- **Error states** — How errors are displayed and resolved
- **Loading states** — Skeletons, spinners, progress bars

### Design System Tools
- **Figma** — Design + component library
- **Storybook** — Component documentation for developers
- **Zeroheight** — Design system documentation
- **Tokens Studio** — Design token management

### Design System Benefits
- **Consistency** — Same components everywhere
- **Efficiency** — Don't redesign the same thing twice
- **Scalability** — New features use existing components
- **Developer handoff** — Clear specs, reusable code
- **Accessibility** — Built-in accessibility standards

## Accessibility (a11y)

### WCAG Principles
- **Perceivable** — Content must be presentable in ways all users can perceive
- **Operable** — UI components must be operable by all users
- **Understandable** — Content and UI must be understandable
- **Robust** — Content must be robust enough for assistive technologies

### Key Accessibility Requirements
- **Color contrast** — 4.5:1 for normal text, 3:1 for large text (WCAG AA)
- **Keyboard navigation** — All interactive elements accessible via keyboard
- **Screen reader support** — Proper ARIA labels, semantic HTML
- **Focus indicators** — Visible focus states for keyboard users
- **Alt text** — Descriptive text for images
- **Form labels** — Every input has a visible label
- **Error identification** — Errors clearly identified and described

### Accessibility Testing Tools
- **axe** — Browser extension for automated testing
- **WAVE** — Web accessibility evaluation tool
- **Lighthouse** — Google's accessibility audit
- **Screen readers** — NVDA (Windows), VoiceOver (Mac), TalkBack (Android)

## Responsive Design

### Breakpoints
| Device | Width | Layout |
|---|---|---|
| Mobile | 320–767px | Single column, stacked |
| Tablet | 768–1023px | Two columns, collapsible |
| Desktop | 1024–1439px | Full layout |
| Large desktop | 1440px+ | Max-width container |

### Responsive Strategies
- **Mobile-first** — Design for mobile, then enhance for larger screens
- **Progressive disclosure** — Show more content as screen size increases
- **Adaptive layouts** — Different layouts for different breakpoints
- **Fluid typography** — Font sizes scale with viewport

## Anti-Patterns

| Anti-Pattern | What It Looks Like | Fix |
|---|---|---|
| **Design without system** | Every screen looks different | Create a design system first |
| **Ignoring accessibility** | Low contrast, no keyboard nav | Test with axe, follow WCAG |
| **Pixel-perfect obsession** | Spending hours on pixels that don't matter | Focus on components and patterns |
| **No responsive design** | Only designing for desktop | Always design mobile-first |
| **Decoration over function** | Pretty but unusable | UX before UI — function over form |

## Related

- [[30 User Research Methods]] — Research that informs visual design decisions
- [[31 UX Design Process]] — Wireframes and prototypes that UI builds upon
- [[33 Usability Testing and AB Testing]] — Testing visual designs with users
- [[UX UI Essential Documents]] — UI deliverables checklist
