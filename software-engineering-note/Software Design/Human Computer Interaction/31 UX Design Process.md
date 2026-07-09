---
tags: [ux-design, wireframes, prototypes, information-architecture, user-flows, ux-ui-design]
---

# 01 — UX Design

> *Source: Information Architecture by Rosenfeld & Morville, Lean UX by Gothelf & Seiden, Nielsen Norman Group*

## Purpose

UX Design translates user research into structured, usable interfaces. It focuses on **how the product works** — information architecture, user flows, wireframes, and prototypes — before any visual design begins.

## Information Architecture (IA)

### What It Is
The structural design of shared information environments — organizing, labeling, and structuring content so users can find what they need.

### IA Principles
- **Organization** — Group related content logically (alphabetical, chronological, topical, audience-based)
- **Labeling** — Clear, consistent names for categories and navigation
- **Navigation** — How users move through the product (menus, breadcrumbs, search)
- **Search** — When navigation isn't enough, users need to find content directly

### IA Deliverables
- **Sitemap** — Hierarchical map of all pages/sections
- **Content inventory** — Spreadsheet listing all content with metadata
- **Navigation model** — Structure of menus, tabs, and links
- **Taxonomy** — Classification system for content (tags, categories)

### IA Methods
- **Card sorting** — Users group content into categories (open/closed)
- **Tree testing** — Users find items in a proposed hierarchy (without seeing the UI)
- **First-click testing** — Where users click first to find something
- **Tools:** OptimalSort, Treejack, Miro

## User Flows

### What They Are
Visual diagrams showing the steps a user takes to complete a task or achieve a goal.

### Components
- **Entry point** — Where the user starts (homepage, email, ad)
- **Decision points** — Where the user makes choices
- **Actions** — What the user does (click, type, select)
- **Screens** — What the user sees at each step
- **Success/failure paths** — Happy path and error handling

### Flow Types
- **Task flows** — Single task (e.g., "Sign up for an account")
- **User flows** — Multiple paths through a feature (e.g., "Onboarding")
- **Screen flows** — Visual map of every screen and connection

### User Flow Example
```
Entry: Landing Page
  ↓
[Sign Up Button]
  ↓
Registration Form
  ↓
[Submit] → Validation Error? → Show Error → Fix & Resubmit
  ↓
Email Verification
  ↓
[Click Link]
  ↓
Welcome Screen → Dashboard
```

## Wireframes

### What They Are
Low-fidelity sketches of a page layout — showing structure, content placement, and functionality without visual design.

### Wireframe Types
- **Sketches** — Hand-drawn, quick, disposable
- **Low-fidelity** — Basic boxes, lines, placeholder text (Balsamiq, wireframe.cc)
- **Mid-fidelity** — More detail, real content, grayscale (Figma, Sketch)
- **High-fidelity** — Close to final design, real content, interactions (Figma, Adobe XD)

### What to Include
- Page layout and structure
- Content hierarchy (what's most important)
- Navigation placement
- Key interactive elements (buttons, forms, menus)
- Content labels (not final copy)

### What NOT to Include
- Final visual design (colors, fonts, images)
- Real content (use placeholder text)
- Micro-interactions (save for prototypes)
- Brand elements

### Wireframe Best Practices
- Start with sketches before digital tools
- Use grayscale — color distracts from structure
- Label everything clearly
- Include annotations explaining behavior
- Create multiple variations before committing

## Prototypes

### What They Are
Interactive simulations of the final product — clickable wireframes or designs that allow users to experience the flow.

### Prototype Fidelity
| Level | Description | Use Case |
|---|---|---|
| **Paper** | Hand-drawn screens, flip through manually | Early concept validation |
| **Low-fi** | Clickable wireframes, basic interactions | User flow testing |
| **Mid-fi** | Realistic interactions, some visual design | Usability testing |
| **High-fi** | Near-final design, animations, real content | Stakeholder presentations, final testing |

### Prototyping Tools
- **Figma** — Industry standard, collaborative, browser-based
- **Sketch** — Mac-only, strong plugin ecosystem
- **Adobe XD** — Integrated with Adobe Creative Suite
- **InVision** — Legacy, still used in some organizations
- **Framer** — Advanced interactions, code-based
- **Axure** — Complex logic, conditional interactions

### Prototype Best Practices
- Prototype only what you need to test
- Use real content where possible
- Include error states and edge cases
- Test with 5 users per iteration
- Document what the prototype demonstrates

## Interaction Design

### Principles
- **Feedback** — Every action should have a visible response
- **Consistency** — Similar actions should work the same way
- **Affordance** — Elements should look like what they do
- **Constraints** — Limit options to prevent errors
- **Visibility** — Important elements should be visible
- **Mapping** — Controls should relate logically to their effects

### Micro-interactions
Small, focused interactions that provide feedback:
- Button hover states
- Loading spinners
- Success/error messages
- Form validation feedback
- Toggle switches
- Pull-to-refresh

## Anti-Patterns

| Anti-Pattern | What It Looks Like | Fix |
|---|---|---|
| **Skipping wireframes** | Going straight to high-fidelity design | Always sketch first, even if rough |
| **Over-prototyping** | Building a prototype that's almost the final product | Prototype only what you need to test |
| **Ignoring IA** | Poor navigation, users can't find content | Card sort and tree test before design |
| **Design by committee** | Too many stakeholders, no clear decisions | Assign one design lead, use research to decide |
| **No user flows** | Screens designed in isolation | Map the complete user journey first |

## Related

- [[00_User_Research_Methods]] — Research that informs UX design decisions
- [[02_UI_Design]] — Visual design applied to wireframes
- [[03_Usability_Testing_and_AB_Testing]] — Testing prototypes with users
- [[04_UX_UI_Essential_Documents]] — UX deliverables checklist
