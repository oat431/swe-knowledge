---
document_type: Responsive Specifications
version: "1.0"
status: Draft
author: "[UX Designer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [responsive, breakpoints, mobile-first, ux]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
---

# Responsive Specifications

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document defines responsive design breakpoints, layout rules, and component behavior across devices.

## 2. Breakpoints

| Breakpoint | Width | Target Device | Grid Columns | Gutter |
|-----------|-------|--------------|-------------|--------|
| [Mobile] | [< 768px] | [Phones] | [4] | [16px] |
| [Tablet] | [768px - 1199px] | [Tablets, small laptops] | [8] | [24px] |
| [Desktop] | [≥ 1200px] | [Desktops, large laptops] | [12] | [24px] |

## 3. Layout Rules

### 3.1 Grid System

| Breakpoint | Columns | Max Width | Margin |
|-----------|---------|----------|--------|
| [Mobile] | [4] | [100%] | [16px] |
| [Tablet] | [8] | [100%] | [24px] |
| [Desktop] | [12] | [1200px] | [Auto (centered)] |

### 3.2 Layout Patterns

| Pattern | Mobile | Tablet | Desktop |
|---------|--------|--------|---------|
| [Single Column] | [Full width] | [Full width] | [8 columns, centered] |
| [Two Column] | [Stacked] | [Side by side] | [Side by side] |
| [Three Column] | [Stacked] | [2 + 1] | [Side by side] |
| [Sidebar + Content] | [Hidden sidebar] | [Collapsed sidebar] | [Expanded sidebar] |
| [Dashboard Grid] | [1 column] | [2 columns] | [3-4 columns] |

## 4. Component Responsive Behavior

### 4.1 Navigation

| Breakpoint | Behavior |
|-----------|---------|
| [Mobile] | [Hamburger menu, slide-out drawer] |
| [Tablet] | [Collapsed nav, icon + text] |
| [Desktop] | [Full horizontal nav] |

### 4.2 Data Table

| Breakpoint | Behavior |
|-----------|---------|
| [Mobile] | [Card view — each row becomes a card] |
| [Tablet] | [Horizontal scroll, sticky first column] |
| [Desktop] | [Full table, all columns visible] |

### 4.3 Forms

| Breakpoint | Behavior |
|-----------|---------|
| [Mobile] | [Single column, full-width inputs] |
| [Tablet] | [Two columns where appropriate] |
| [Desktop] | [Two-three columns, side-by-side fields] |

### 4.4 Dashboard

| Breakpoint | Behavior |
|-----------|---------|
| [Mobile] | [Stacked cards, swipeable] |
| [Tablet] | [2-column grid] |
| [Desktop] | [3-4 column grid] |

## 5. Touch Targets

| Element | Minimum Size | Recommended |
|---------|-------------|------------|
| [Buttons] | [44px × 44px] | [48px × 48px] |
| [Links] | [44px height] | [48px height] |
| [Checkboxes] | [44px × 44px] | [48px × 48px] |
| [Radio buttons] | [44px × 44px] | [48px × 48px] |
| [Icons] | [44px × 44px] | [48px × 48px] |

## 6. Responsive Images

| Image Type | Mobile | Tablet | Desktop |
|-----------|--------|--------|---------|
| [Hero] | [100vw] | [100vw] | [1200px max] |
| [Card] | [100% width] | [100% width] | [Fixed width] |
| [Avatar] | [40px] | [40px] | [40px] |
| [Thumbnail] | [80px] | [100px] | [120px] |

## 7. Responsive Testing Checklist

| # | Test | Mobile | Tablet | Desktop | Status |
|---|------|--------|--------|---------|--------|
| 1 | [Navigation works] | ☐ | ☐ | ☐ | ⬜ |
| 2 | [Forms are usable] | ☐ | ☐ | ☐ | ⬜ |
| 3 | [Tables are readable] | ☐ | ☐ | ☐ | ⬜ |
| 4 | [Touch targets ≥ 44px] | ☐ | ☐ | ☐ | ⬜ |
| 5 | [Text is readable] | ☐ | ☐ | ☐ | ⬜ |
| 6 | [Images scale properly] | ☐ | ☐ | ☐ | ⬜ |
| 7 | [No horizontal scroll] | ☐ | ☐ | ☐ | ⬜ |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Style-Guide]] | Visual standards |
| [[UI-Mockups]] | Mockups at each breakpoint |
| [[Component-Library]] | Component responsive behavior |

---

> **Template Standard:** Based on ISO 9241-210
> **Usage:** Design mobile-first — start with the smallest screen and enhance. Test on real devices, not just browser resize.
