---
document_type: Responsive Behavior Spec
version: "1.0"
status: Draft
author: "[UX Designer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [responsive, behavior, breakpoints, ux]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
---

# Responsive Behavior Spec

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Detailed responsive behavior specifications — how each component and layout adapts across breakpoints.

## 2. Breakpoints

| Name | Min | Max | Target |
|------|-----|-----|--------|
| [Mobile] | [0] | [767px] | [Phones] |
| [Tablet] | [768px] | [1199px] | [Tablets] |
| [Desktop] | [1200px] | [∞] | [Desktops] |

## 3. Navigation Behavior

| Breakpoint | Behavior |
|-----------|---------|
| [Mobile] | [Hamburger menu → slide-out drawer from left] |
| [Tablet] | [Collapsed nav → icon + label, hamburger for overflow] |
| [Desktop] | [Full horizontal nav bar] |

## 4. Form Behavior

| Breakpoint | Layout | Input Width | Label Position |
|-----------|--------|-----------|---------------|
| [Mobile] | [Single column] | [100%] | [Above input] |
| [Tablet] | [Two columns] | [50%] | [Above input] |
| [Desktop] | [Two columns] | [50%] | [Above input] |

## 5. Table Behavior

| Breakpoint | Behavior |
|-----------|---------|
| [Mobile] | [Card view — each row becomes a card with label:value pairs] |
| [Tablet] | [Horizontal scroll, sticky first column + actions column] |
| [Desktop] | [Full table, all columns visible] |

## 6. Dashboard Behavior

| Breakpoint | Grid | Card Size |
|-----------|------|----------|
| [Mobile] | [1 column, stacked] | [Full width] |
| [Tablet] | [2 columns] | [50% width] |
| [Desktop] | [3-4 columns] | [25-33% width] |

## 7. Modal Behavior

| Breakpoint | Behavior |
|-----------|---------|
| [Mobile] | [Full-screen sheet from bottom] |
| [Tablet] | [Centered, 80% width, max 600px] |
| [Desktop] | [Centered, 50% width, max 800px] |

## 8. Image Behavior

| Image Type | Mobile | Tablet | Desktop |
|-----------|--------|--------|---------|
| [Hero] | [Full width, cropped] | [Full width] | [Max 1200px] |
| [Card Image] | [Full width] | [Full width] | [Fixed width] |
| [Thumbnail] | [60px] | [80px] | [100px] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Responsive Specifications]] | Breakpoint overview |
| [[Component Library]] | Component responsive behavior |
| [[UI Mockups]] | Mockups at each breakpoint |

---

> **Template Standard:** Based on ISO 9241-210
> **Usage:** This spec answers "What happens when the screen shrinks?" for every component. Test on real devices, not just browser resize.
