---
document_type: UI Mockups
version: "1.0"
status: Draft
author: "[UX Designer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [mockups, visual-design, ui, figma]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
---

# UI Mockups

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]
>
> ⚠️ **Note:** UI mockups are pixel-perfect visual designs created in Figma/Sketch. This document *specifies and links* the mockups — the actual designs live in the design tool.

## 1. Purpose

> UI mockups define the final visual design — colors, typography, spacing, imagery, and branding. They're the design team's deliverable to development.

## 2. Design Tool Links

| Resource | URL | Version |
|---------|-----|---------|
| [Figma Project] | [https://figma.com/file/xxx] | [v1.0] |
| [Design System] | [https://figma.com/file/xxx] | [v1.0] |
| [Component Library] | [https://figma.com/file/xxx] | [v1.0] |
| [Prototype] | [https://figma.com/proto/xxx] | [v1.0] |

## 3. Mockup Inventory

| # | Screen | Device | Status | Figma Link |
|---|--------|--------|--------|-----------|
| 1 | [Login] | [Desktop + Mobile] | ✅ Done | [Link] |
| 2 | [Customer Dashboard] | [Desktop + Mobile] | ✅ Done | [Link] |
| 3 | [Request Form — All Steps] | [Desktop + Mobile] | ✅ Done | [Link] |
| 4 | [Request Detail — Customer] | [Desktop + Mobile] | ✅ Done | [Link] |
| 5 | [Status Tracker] | [Desktop + Mobile] | ✅ Done | [Link] |
| 6 | [Admin Dashboard] | [Desktop] | ✅ Done | [Link] |
| 7 | [Work Queue] | [Desktop] | ✅ Done | [Link] |
| 8 | [Request Detail — Admin] | [Desktop] | ✅ Done | [Link] |
| 9 | [Review Panel] | [Desktop] | ✅ Done | [Link] |
| 10 | [Reports] | [Desktop] | ✅ Done | [Link] |
| 11 | [Settings] | [Desktop] | 🔄 In Progress | [Link] |
| 12 | [Error Pages (404, 500)] | [Desktop + Mobile] | ⬜ Not Started | — |
| 13 | [Empty States] | [Desktop + Mobile] | ⬜ Not Started | — |
| 14 | [Loading States] | [Desktop + Mobile] | ⬜ Not Started | — |

## 4. Visual Specifications

### 4.1 Color Palette

| Role | Color | Hex | Usage |
|------|-------|-----|-------|
| [Primary] | [Blue] | [#2196F3] | [CTAs, links, active states] |
| [Secondary] | [Teal] | [#009688] | [Secondary actions, accents] |
| [Success] | [Green] | [#4CAF50] | [Success messages, approved] |
| [Warning] | [Orange] | [#FF9800] | [Warnings, pending states] |
| [Error] | [Red] | [#f44336] | [Errors, rejected, critical] |
| [Background] | [Light Gray] | [#F5F5F5] | [Page background] |
| [Surface] | [White] | [#FFFFFF] | [Cards, panels] |
| [Text Primary] | [Dark Gray] | [#212121] | [Body text] |
| [Text Secondary] | [Medium Gray] | [#757575] | [Labels, captions] |

### 4.2 Typography

| Element | Font | Size | Weight | Line Height |
|---------|------|------|--------|------------|
| [H1 — Page Title] | [Inter] | [32px] | [700] | [40px] |
| [H2 — Section Title] | [Inter] | [24px] | [600] | [32px] |
| [H3 — Card Title] | [Inter] | [18px] | [600] | [24px] |
| [Body] | [Inter] | [14px] | [400] | [20px] |
| [Small] | [Inter] | [12px] | [400] | [16px] |
| [Button] | [Inter] | [14px] | [600] | [20px] |
| [Caption] | [Inter] | [12px] | [400] | [16px] |

### 4.3 Spacing

| Token | Value | Usage |
|-------|-------|-------|
| [xs] | [4px] | [Tight spacing — icon gaps] |
| [sm] | [8px] | [Compact spacing — form fields] |
| [md] | [16px] | [Default spacing — sections] |
| [lg] | [24px] | [Relaxed spacing — between cards] |
| [xl] | [32px] | [Loose spacing — page margins] |
| [2xl] | [48px] | [Section separation] |

### 4.4 Shadows

| Level | Shadow | Usage |
|-------|--------|-------|
| [None] | [none] | [Flat elements] |
| [Low] | [0 1px 3px rgba(0,0,0,0.12)] | [Cards, inputs] |
| [Medium] | [0 4px 6px rgba(0,0,0,0.16)] | [Dropdowns, popovers] |
| [High] | [0 10px 20px rgba(0,0,0,0.19)] | [Modals, dialogs] |

## 5. Mockup Review Checklist

| # | Check | Status |
|---|-------|--------|
| 1 | [All screens match style guide] | ✅❌ |
| 2 | [Responsive at all breakpoints] | ✅❌ |
| 3 | [All states designed (default, hover, active, disabled, error)] | ✅❌ |
| 4 | [Empty states designed] | ✅❌ |
| 5 | [Loading states designed] | ✅❌ |
| 6 | [Error states designed] | ✅❌ |
| 7 | [Accessibility — color contrast AA] | ✅❌ |
| 8 | [Accessibility — focus states visible] | ✅❌ |
| 9 | [Design tokens match code variables] | ✅❌ |
| 10 | [Developer handoff notes complete] | ✅❌ |

## 6. Handoff to Development

| Aspect | Specification |
|--------|--------------|
| [Design Tokens] | [Export as CSS variables / JSON] |
| [Assets] | [Export as SVG/PNG from Figma] |
| [Spacing] | [Use 4px grid system] |
| [Responsive] | [3 breakpoints: 768px, 1024px, 1200px] |
| [Animations] | [Specified in interaction panel] |
| [Redlines] | [Figma inspect mode] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Style-Guide]] | Visual standards |
| [[Component-Library]] | Reusable components |
| [[Design-System]] | Complete design system |

---

> **Template Standard:** Based on ISO 9241-210
> **Usage:** Mockups are the *visual contract* between design and development. Use Figma's inspect mode for exact specs. Don't let developers guess — specify everything.
