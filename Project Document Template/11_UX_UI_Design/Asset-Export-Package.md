---
document_type: Asset Export Package
version: "1.0"
status: Draft
author: "[UX Designer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [assets, export, icons, images, figma]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
---

# Asset Export Package

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document catalogs all design assets — icons, images, illustrations, logos — with export specifications for development.

## 2. Asset Inventory

| # | Asset | Type | Format | Sizes | Status |
|---|-------|------|--------|-------|--------|
| 1 | [Logo — Primary] | [Logo] | [SVG, PNG] | [32, 64, 128, 256px] | ✅ |
| 2 | [Logo — Monochrome] | [Logo] | [SVG, PNG] | [32, 64, 128, 256px] | ✅ |
| 3 | [Favicon] | [Icon] | [ICO, PNG] | [16, 32, 48px] | ✅ |
| 4 | [App Icon] | [Icon] | [PNG] | [192, 512px] | ✅ |
| 5 | [Illustration — Empty State] | [Illustration] | [SVG] | [1x] | ✅ |
| 6 | [Illustration — Error] | [Illustration] | [SVG] | [1x] | ✅ |
| 7 | [Illustration — Success] | [Illustration] | [SVG] | [1x] | ✅ |
| 8 | [Icon Set — Lucide] | [Icons] | [SVG] | [16, 20, 24px] | ✅ |

## 3. Export Specifications

### 3.1 Icons

| Specification | Value |
|--------------|-------|
| [Format] | [SVG (primary), PNG (fallback)] |
| [Sizes] | [16px, 20px, 24px] |
| [Color] | [currentColor (inherits from CSS)] |
| [Stroke Width] | [2px] |
| [Optimization] | [SVGO optimized] |

### 3.2 Illustrations

| Specification | Value |
|--------------|-------|
| [Format] | [SVG (primary)] |
| [Max Width] | [400px] |
| [Color] | [CSS variables for theming] |
| [Optimization] | [SVGO optimized] |

### 3.3 Logos

| Specification | Value |
|--------------|-------|
| [Format] | [SVG (primary), PNG (fallback)] |
| [Sizes] | [32px, 64px, 128px, 256px] |
| [Padding] | [1x logo height clear space] |
| [Variants] | [Full color, Monochrome, White] |

## 4. Asset Naming Convention

| Type | Pattern | Example |
|------|---------|---------|
| [Icon] | [icon-{name}-{size}] | [icon-arrow-right-20.svg] |
| [Illustration] | [illust-{name}] | [illust-empty-state.svg] |
| [Logo] | [logo-{variant}-{size}] | [logo-primary-128.png] |
| [Image] | [img-{name}-{size}] | [img-hero-banner-1200.jpg] |

## 5. Asset Delivery

| Method | Tool | URL |
|--------|------|-----|
| [Figma Export] | [Figma] | [Export panel → SVG/PNG] |
| [Asset Pipeline] | [CI/CD] | [GitHub Action] |
| [CDN] | [CloudFront] | [/assets/] |

## 6. Asset Checklist

| # | Asset | Design | Exported | Optimized | In Code | Status |
|---|-------|--------|---------|-----------|---------|--------|
| 1 | [Logo] | ✅ | ☐ | ☐ | ☐ | ⬜ |
| 2 | [Icons] | ✅ | ☐ | ☐ | ☐ | ⬜ |
| 3 | [Illustrations] | ✅ | ☐ | ☐ | ☐ | ⬜ |
| 4 | [Images] | ✅ | ☐ | ☐ | ☐ | ⬜ |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Style-Guide]] | Visual standards |
| [[Component-Library]] | Components using assets |
| [[Design-Specifications]] | Export specs |

---

> **Template Standard:** Based on ISO 9241-210
> **Usage:** Export assets as SVG whenever possible — they scale, are small, and support CSS theming. Use PNG only for complex images.
