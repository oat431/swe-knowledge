---
document_type: Design Specifications
version: "1.0"
status: Draft
author: "[UX Designer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [design-specifications, handoff, dev-specs]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
---

# Design Specifications

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Design specifications provide developers with exact measurements, values, and behaviors needed to implement designs accurately.

## 2. Specification Links

| Resource | Tool | URL |
|---------|------|-----|
| [Figma Project] | [Figma] | [https://figma.com/file/xxx] |
| [Inspect Mode] | [Figma] | [Enable in Figma for exact specs] |
| [Design Tokens] | [JSON/CSS] | [See [[Design-Tokens]]] |
| [Storybook] | [Storybook] | [Link] |

## 3. Spacing Specifications

| Element | Top | Right | Bottom | Left |
|---------|-----|-------|--------|------|
| [Page Container] | [32px] | [24px] | [32px] | [24px] |
| [Section] | [24px] | [0] | [24px] | [0] |
| [Card] | [16px] | [16px] | [16px] | [16px] |
| [Form Field Group] | [0] | [0] | [16px] | [0] |
| [Button Group] | [16px] | [0] | [0] | [0] |
| [Table Cell] | [12px] | [16px] | [12px] | [16px] |

## 4. Size Specifications

| Element | Width | Height | Notes |
|---------|-------|--------|-------|
| [Button — Default] | [Auto] | [40px] | [Min-width: 120px] |
| [Button — Small] | [Auto] | [32px] | [Min-width: 80px] |
| [Input — Default] | [100%] | [40px] | [Full width of container] |
| [Avatar — Small] | [32px] | [32px] | [Circle] |
| [Avatar — Default] | [40px] | [40px] | [Circle] |
| [Icon — Small] | [16px] | [16px] | — |
| [Icon — Default] | [20px] | [20px] | — |
| [Badge] | [Auto] | [24px] | [Padding: 4px 8px] |

## 5. Animation Specifications

| Animation | Duration | Easing | Property |
|-----------|---------|--------|---------|
| [Button Hover] | [150ms] | [ease-out] | [background-color] |
| [Modal Open] | [200ms] | [ease-out] | [opacity, transform] |
| [Modal Close] | [150ms] | [ease-in] | [opacity, transform] |
| [Toast Enter] | [300ms] | [ease-out] | [transform: translateX] |
| [Toast Exit] | [200ms] | [ease-in] | [opacity, transform] |
| [Page Transition] | [300ms] | [ease-in-out] | [opacity] |
| [Skeleton Shimmer] | [1.5s] | [linear] | [background-position] |

## 6. Z-Index Scale

| Layer | Z-Index | Usage |
|-------|---------|-------|
| [Base] | [0] | [Default content] |
| [Dropdown] | [100] | [Dropdowns, popovers] |
| [Sticky] | [200] | [Sticky headers, nav] |
| [Overlay] | [300] | [Backdrop overlays] |
| [Modal] | [400] | [Modals, dialogs] |
| [Toast] | [500] | [Toast notifications] |
| [Tooltip] | [600] | [Tooltips] |

## 7. Responsive Specifications

| Breakpoint | Container Max | Columns | Gutter | Margin |
|-----------|--------------|---------|--------|--------|
| [Mobile < 768px] | [100%] | [4] | [16px] | [16px] |
| [Tablet 768-1199px] | [100%] | [8] | [24px] | [24px] |
| [Desktop ≥ 1200px] | [1200px] | [12] | [24px] | [Auto] |

## 8. Handoff Checklist

| # | Item | Status |
|---|------|--------|
| 1 | [All screens exported from Figma] | ☐ |
| 2 | [Design tokens exported as CSS/JSON] | ☐ |
| 3 | [Assets exported as SVG/PNG] | ☐ |
| 4 | [Inspect mode enabled for all screens] | ☐ |
| 5 | [Interaction specs documented] | ☐ |
| 6 | [Responsive variants created] | ☐ |
| 7 | [All states designed (default/hover/active/disabled/error)] | ☐ |
| 8 | [Accessibility specs included] | ☐ |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Style-Guide]] | Visual standards |
| [[Design-Tokens]] | Token values |
| [[Component-Library]] | Component specs |

---

> **Template Standard:** Based on ISO 9241-210
> **Usage:** Use Figma's inspect mode for pixel-perfect specs. This document supplements Figma with animation, z-index, and responsive specs that Figma doesn't show.
