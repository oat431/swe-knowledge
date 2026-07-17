---
document_type: Accessibility Audit
version: "1.0"
status: Draft
author: "[UX Designer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [accessibility, wcag, a11y, audit]
standard_ref:
  - WCAG 2.1 Level AA
  - ISO 40500 — WCAG 2.0
  - Section 508
---

# Accessibility Audit

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document audits the product against WCAG 2.1 Level AA — ensuring the system is usable by people with disabilities.

## 2. Accessibility Target

| Standard | Level | Status |
|---------|-------|--------|
| [WCAG 2.1] | [AA] | [Target] |
| [Section 508] | [Full] | [Target] |
| [ADA Compliance] | [Full] | [Target] |

## 3. Audit Results

### 3.1 Perceivable

| # | WCAG Criterion | Level | Requirement | Status | Notes |
|---|---------------|-------|-------------|--------|-------|
| 1.1.1 | [Non-text Content] | [A] | [All images have alt text] | ✅ | [All images have descriptive alt] |
| 1.2.1 | [Audio/Video] | [A] | [Captions for video] | N/A | [No video content] |
| 1.3.1 | [Info and Relationships] | [A] | [Semantic HTML] | ✅ | [Proper heading hierarchy] |
| 1.3.2 | [Meaningful Sequence] | [A] | [Logical reading order] | ✅ | [DOM order matches visual] |
| 1.3.3 | [Sensory Characteristics] | [A] | [No color-only instructions] | ✅ | [Icons + text used] |
| 1.4.1 | [Use of Color] | [A] | [Color not sole indicator] | ✅ | [Icons + text + color] |
| 1.4.3 | [Contrast Minimum] | [AA] | [4.5:1 text contrast] | ✅ | [All text meets ratio] |
| 1.4.4 | [Resize Text] | [AA] | [Text resizable to 200%] | ✅ | [Responsive layout] |
| 1.4.11 | [Non-text Contrast] | [AA] | [3:1 UI component contrast] | ✅ | [Buttons, inputs meet ratio] |

### 3.2 Operable

| # | WCAG Criterion | Level | Requirement | Status | Notes |
|---|---------------|-------|-------------|--------|-------|
| 2.1.1 | [Keyboard] | [A] | [All functions via keyboard] | ✅ | [Tab navigation works] |
| 2.1.2 | [No Keyboard Trap] | [A] | [Can navigate away] | ✅ | [Escape closes modals] |
| 2.2.1 | [Timing Adjustable] | [A] | [Session timeout adjustable] | ✅ | [30 min, extendable] |
| 2.3.1 | [Three Flashes] | [A] | [No flashing > 3/sec] | ✅ | [No flashing content] |
| 2.4.1 | [Bypass Blocks] | [A] | [Skip navigation link] | ✅ | [Skip to main content] |
| 2.4.2 | [Page Titled] | [A] | [Descriptive page titles] | ✅ | [Unique per page] |
| 2.4.3 | [Focus Order] | [A] | [Logical focus order] | ✅ | [Follows visual order] |
| 2.4.4 | [Link Purpose] | [A] | [Links make sense alone] | ✅ | [Descriptive link text] |
| 2.4.6 | [Headings and Labels] | [AA] | [Descriptive headings] | ✅ | [Clear hierarchy] |
| 2.4.7 | [Focus Visible] | [AA] | [Focus indicator visible] | ✅ | [Blue outline] |
| 2.5.1 | [Pointer Gestures] | [A] | [Single pointer alternative] | ✅ | [No complex gestures] |

### 3.3 Understandable

| # | WCAG Criterion | Level | Requirement | Status | Notes |
|---|---------------|-------|-------------|--------|-------|
| 3.1.1 | [Language of Page] | [A] | [Page language declared] | ✅ | [lang="en"] |
| 3.2.1 | [On Focus] | [A] | [No context change on focus] | ✅ | [No auto-navigation] |
| 3.2.2 | [On Input] | [A] | [No unexpected changes] | ✅ | [Explicit submit buttons] |
| 3.3.1 | [Error Identification] | [A] | [Errors identified] | ✅ | [Inline error messages] |
| 3.3.2 | [Labels or Instructions] | [A] | [Form fields labeled] | ✅ | [All fields have labels] |
| 3.3.3 | [Error Suggestion] | [AA] | [Error correction suggested] | ✅ | [Helpful error messages] |
| 3.3.4 | [Error Prevention] | [AA] | [Confirm submissions] | ✅ | [Review page before submit] |

### 3.4 Robust

| # | WCAG Criterion | Level | Requirement | Status | Notes |
|---|---------------|-------|-------------|--------|-------|
| 4.1.1 | [Parsing] | [A] | [Valid HTML] | ✅ | [Validated HTML] |
| 4.1.2 | [Name, Role, Value] | [A] | [ARIA labels] | ✅ | [Proper ARIA attributes] |
| 4.1.3 | [Status Messages] | [AA] | [ARIA live regions] | ✅ | [Toast notifications announced] |

## 4. Audit Summary

| Category | Passed | Failed | N/A | Score |
|---------|--------|--------|-----|-------|
| [Perceivable] | [9] | [0] | [1] | [100%] |
| [Operable] | [11] | [0] | [0] | [100%] |
| [Understandable] | [7] | [0] | [0] | [100%] |
| [Robust] | [3] | [0] | [0] | [100%] |
| **Total** | **30** | **0** | **1** | **100%** |

## 5. Accessibility Testing Tools

| Tool | Purpose | When |
|------|---------|------|
| [axe DevTools] | [Automated accessibility testing] | [Every build] |
| [Lighthouse] | [Accessibility score] | [Every sprint] |
| [WAVE] | [Visual accessibility checker] | [Design review] |
| [Screen Reader (NVDA)] | [Manual screen reader testing] | [Every release] |
| [Keyboard Only] | [Manual keyboard testing] | [Every sprint] |

## 6. Remediation Plan

| # | Issue | Severity | Owner | Status |
|---|-------|---------|-------|--------|
| 1 | [None — all criteria passed] | — | — | ✅ |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Style-Guide]] | Color contrast requirements |
| [[Component-Library]] | Accessible component specs |
| [[Usability-Test-Plan]] | Accessibility testing plan |

---

> **Template Standard:** Based on WCAG 2.1 Level AA, ISO 40500
> **Usage:** Accessibility is not optional — it's a legal requirement in many jurisdictions and the right thing to do. Test with real assistive technologies, not just automated tools.
