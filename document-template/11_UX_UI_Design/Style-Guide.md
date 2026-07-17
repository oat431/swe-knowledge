---
document_type: Style Guide
version: "1.0"
status: Draft
author: "[UX Designer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [style-guide, visual-design, branding]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
---

# Style Guide

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> The style guide defines visual standards — colors, typography, spacing, and branding rules that ensure visual consistency.

## 2. Brand Identity

| Element | Specification |
|---------|--------------|
| [Logo] | [Primary: Full color / Secondary: Monochrome] |
| [Logo Minimum Size] | [32px height] |
| [Logo Clear Space] | [1x logo height on all sides] |
| [Tagline] | [Optional tagline placement] |

## 3. Color System

### 3.1 Primary Colors

| Color | Hex | RGB | Usage |
|-------|-----|-----|-------|
| 🔵 Primary | [#2196F3] | [33, 150, 243] | [CTAs, links, active states] |
| 🔵 Primary Light | [#64B5F6] | [100, 181, 246] | [Hover states, highlights] |
| 🔵 Primary Dark | [#1976D2] | [25, 118, 210] | [Active/pressed states] |

### 3.2 Semantic Colors

| Color | Hex | Usage |
|-------|-----|-------|
| 🟢 Success | [#4CAF50] | [Approved, success messages] |
| 🟠 Warning | [#FF9800] | [Pending, warnings] |
| 🔴 Error | [#f44336] | [Rejected, errors, critical] |
| 🔵 Info | [#2196F3] | [Information, tips] |

### 3.3 Neutral Colors

| Color | Hex | Usage |
|-------|-----|-------|
| ⬛ Text Primary | [#212121] | [Body text] |
| ⬛ Text Secondary | [#757575] | [Labels, captions] |
| ⬛ Text Disabled | [#BDBDBD] | [Disabled text] |
| ⬜ Background | [#F5F5F5] | [Page background] |
| ⬜ Surface | [#FFFFFF] | [Cards, panels] |
| ⬛ Border | [#E0E0E0] | [Dividers, borders] |

### 3.4 Accessibility — Color Contrast

| Combination | Ratio | WCAG | Status |
|------------|-------|------|--------|
| [Primary on White] | [4.5:1] | [AA ✅] | ✅ |
| [Text Primary on White] | [15:1] | [AAA ✅] | ✅ |
| [Text Secondary on White] | [4.6:1] | [AA ✅] | ✅ |
| [White on Primary] | [4.5:1] | [AA ✅] | ✅ |
| [Error on White] | [5.2:1] | [AA ✅] | ✅ |

## 4. Typography

### 4.1 Type Scale

| Level | Font | Size | Weight | Line Height | Usage |
|-------|------|------|--------|------------|-------|
| [Display] | [Inter] | [48px] | [700] | [56px] | [Hero sections] |
| [H1] | [Inter] | [32px] | [700] | [40px] | [Page titles] |
| [H2] | [Inter] | [24px] | [600] | [32px] | [Section titles] |
| [H3] | [Inter] | [18px] | [600] | [24px] | [Card titles] |
| [Body] | [Inter] | [16px] | [400] | [24px] | [Body text] |
| [Body Small] | [Inter] | [14px] | [400] | [20px] | [Secondary text] |
| [Caption] | [Inter] | [12px] | [400] | [16px] | [Labels, metadata] |
| [Button] | [Inter] | [14px] | [600] | [20px] | [Button text] |

### 4.2 Typography Rules

| Rule | Specification |
|------|--------------|
| [Maximum line length] | [80 characters] |
| [Paragraph spacing] | [16px after] |
| [Heading spacing] | [24px before, 16px after] |
| [Font stack] | ['Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif] |

## 5. Spacing System

| Token | Value | Usage |
|-------|-------|-------|
| [4px] | [xs] | [Icon gaps, tight spacing] |
| [8px] | [sm] | [Form field gaps, compact] |
| [16px] | [md] | [Default — section padding] |
| [24px] | [lg] | [Card padding, relaxed] |
| [32px] | [xl] | [Page margins] |
| [48px] | [2xl] | [Section separation] |
| [64px] | [3xl] | [Page section gaps] |

## 6. Iconography

| Aspect | Specification |
|--------|--------------|
| [Icon Set] | [Lucide Icons / Phosphor Icons] |
| [Size — Small] | [16px] |
| [Size — Default] | [20px] |
| [Size — Large] | [24px] |
| [Color] | [Inherit from text color] |
| [Stroke Width] | [2px] |

## 7. Border & Radius

| Element | Border | Radius |
|---------|--------|--------|
| [Card] | [1px solid #E0E0E0] | [8px] |
| [Button] | [None / 1px solid] | [6px] |
| [Input] | [1px solid #E0E0E0] | [4px] |
| [Badge] | [None] | [12px (pill)] |
| [Modal] | [None] | [12px] |
| [Avatar] | [None] | [50% (circle)] |

## 8. Elevation (Shadows)

| Level | Shadow | Usage |
|-------|--------|-------|
| [Level 0] | [none] | [Flat elements] |
| [Level 1] | [0 1px 3px rgba(0,0,0,0.12)] | [Cards, inputs] |
| [Level 2] | [0 4px 6px rgba(0,0,0,0.16)] | [Dropdowns, popovers] |
| [Level 3] | [0 10px 20px rgba(0,0,0,0.19)] | [Modals, dialogs] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Design-System]] | System this guide is part of |
| [[Component-Library]] | Components using these styles |
| [[UI-Mockups]] | Mockups following this guide |

---

> **Template Standard:** Based on ISO 9241-210
> **Usage:** The style guide is the *visual contract*. Every design decision should reference this guide. If it's not in the guide, it's not approved.
