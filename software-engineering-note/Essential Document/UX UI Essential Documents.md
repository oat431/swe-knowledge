---
tags: [essential-documents, ux, ui, design, deliverables, ux-ui-design]
---

# UX/UI — Essential Documents by Phase

> **Source:** **UI Design — Industry Standards**
> Organized by design phases: Research → UX Design → UI Design → Testing → Handoff
>
> ⚠️ **This is a document-only extract.** For the full design process, see:
> `F:\projects\orlita_md\software-engineering-note\Software Design\Human Computer Interaction\`

**Priority Legend:**

| Icon | Level | Meaning |
|---|---|---|
| 🔴 | **Must Have** | Essential for all projects; skipping creates significant risk |
| 🟡 | **Nice to Have** | Recommended for medium+ complexity |
| 🟢 | **Optional** | Situational — depends on project size and domain |

---

## 1. Research Phase
> **Owner:** UX Researcher / Product Manager

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **User Personas** | Fictional characters representing user segments with goals, behaviors, frustrations | 🔴 Must Have | ISO 9241-210 |
| **User Interview Script** | Structured questions for user interviews | 🔴 Must Have | ISO 9241-210 |
| **Survey Questionnaire** | Structured questions for quantitative research | 🟡 Nice to Have | — |
| **Journey Map** | Visual timeline of user experience with pain points and opportunities | 🔴 Must Have | ISO 9241-210 |
| **Empathy Map** | What users think, feel, say, do | 🟡 Nice to Have | — |
| **Competitive Analysis** | Analysis of competitor products' UX | 🟡 Nice to Have | — |
| **User Research Report** | Summary of findings from interviews, surveys, observations | 🔴 Must Have | ISO 9241-210 |
| **Contextual Inquiry Notes** | Observations from shadowing users | 🟢 Optional | ISO 9241-210 |
| **Diary Study Results** | Long-term user behavior data | 🟢 Optional | — |

---

## 2. UX Design Phase
> **Owner:** UX Designer / Information Architect

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Information Architecture (IA)** | Content structure, hierarchy, navigation model | 🔴 Must Have | ISO 9241-110 |
| **Sitemap** | Visual map of all pages/sections | 🔴 Must Have | — |
| **User Flows** | Step-by-step paths users take to complete tasks | 🔴 Must Have | ISO 9241-210 |
| **Wireframes (Low-fi)** | Basic page layouts showing structure and content placement | 🔴 Must Have | — |
| **Wireframes (Mid-fi)** | More detailed wireframes with real content | 🟡 Nice to Have | — |
| **Interactive Prototype** | Clickable wireframes for user testing | 🔴 Must Have | ISO 9241-210 |
| **Content Inventory** | Spreadsheet of all content with metadata | 🟡 Nice to Have | — |
| **Card Sort Results** | How users grouped content | 🟡 Nice to Have | — |
| **Tree Test Results** | How users navigated the proposed structure | 🟡 Nice to Have | — |

---

## 3. UI Design Phase
> **Owner:** UI Designer / Visual Designer

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Style Guide** | Typography, colors, spacing, icons | 🔴 Must Have | — |
| **UI Mockups** | High-fidelity screen designs | 🔴 Must Have | — |
| **Component Library** | Reusable UI components with specs | 🔴 Must Have | — |
| **Design System** | Complete system: tokens, components, patterns, guidelines | 🟡 Nice to Have | — |
| **Responsive Specifications** | Desktop, tablet, mobile layouts | 🔴 Must Have | ISO 9241-112 |
| **State Variations** | Hover, active, disabled, error, loading states | 🔴 Must Have | — |
| **Icon Library** | Custom or selected icon set | 🟡 Nice to Have | — |
| **Brand Guidelines** | Logo usage, color palette, typography | 🟡 Nice to Have | — |
| **Accessibility Audit** | WCAG compliance check | 🔴 Must Have | ISO/IEC 40500 (WCAG 2.0) |

---

## 4. Testing & Iteration Phase
> **Owner:** UX Researcher / QA

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Usability Test Plan** | Tasks, participants, metrics, schedule | 🔴 Must Have | ISO 9241-11 |
| **Usability Test Report** | Findings, severity ratings, recommendations | 🔴 Must Have | ISO 9241-11 |
| **A/B Test Plan** | Hypothesis, variants, metrics, sample size | 🟡 Nice to Have | — |
| **A/B Test Results** | Winner, statistical significance, impact | 🟡 Nice to Have | — |
| **Analytics Dashboard** | Key metrics: conversion, engagement, retention | 🔴 Must Have | — |
| **Heatmap Report** | Where users click, scroll, focus | 🟡 Nice to Have | — |
| **Session Recording Review** | Analysis of user behavior recordings | 🟢 Optional | — |
| **NPS/CSAT Survey** | User satisfaction measurement | 🟡 Nice to Have | — |
| **Iteration Log** | Changes made based on testing, with rationale | 🟡 Nice to Have | — |

---

## 5. Handoff & Implementation
> **Owner:** UI Designer / Frontend Developer

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Design Specifications** | Measurements, colors, typography for developers | 🔴 Must Have | — |
| **Asset Export Package** | Icons, images, logos in required formats | 🔴 Must Have | — |
| **Interaction Specifications** | Animations, transitions, micro-interactions | 🟡 Nice to Have | — |
| **Design Tokens** | CSS variables for colors, spacing, typography | 🟡 Nice to Have | — |
| **Responsive Behavior Spec** | How layouts adapt across breakpoints | 🔴 Must Have | ISO 9241-112 |
| **Error State Specifications** | All error states and recovery flows | 🔴 Must Have | — |
| **Empty State Designs** | What users see when there's no data | 🟡 Nice to Have | — |

---

## Project Size Adaptation

| Profile | Research | UX Design | UI Design | Testing | Total |
|---|---|---|---|---|---|
| 🚀 **Startup/MVP** | 2–3 docs | 3–4 docs | 2–3 docs | 1–2 docs | ~10–12 |
| 🏢 **Medium Product** | 4–5 docs | 5–6 docs | 4–5 docs | 3–4 docs | ~16–20 |
| 🏗️ **Enterprise** | 6+ docs | 7+ docs | 6+ docs | 5+ docs | ~24+ |

---

## Related

- [[30 User Research Methods]] — Research methods
- [[31 UX Design Process]] — UX design methods
- [[32 UI Design Process]] — UI design methods
- [[33 Usability Testing and AB Testing]] — Testing methods
