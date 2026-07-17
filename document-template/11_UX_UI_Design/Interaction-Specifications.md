---
document_type: Interaction Specifications
version: "1.0"
status: Draft
author: "[UX Designer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [interaction-design, micro-interactions, ux]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
  - Dan Saffer — Microinteractions
---

# Interaction Specifications

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines micro-interactions — the small moments where user and design interact. Trigger → Rules → Feedback → Loops.

## 2. Interaction Catalog

| # | Interaction | Trigger | Feedback | Duration | Easing |
|---|-----------|---------|---------|---------|--------|
| I-001 | [Button Hover] | [Mouse enter] | [Darken 10%] | [150ms] | [ease-out] |
| I-002 | [Button Click] | [Click/tap] | [Scale 0.98] | [100ms] | [ease-in-out] |
| I-003 | [Input Focus] | [Click/tab] | [Blue border, shadow] | [200ms] | [ease-out] |
| I-004 | [Input Error] | [Validation fail] | [Red border, shake] | [300ms] | [ease-out] |
| I-005 | [Toast Appear] | [Action complete] | [Slide in from right] | [300ms] | [ease-out] |
| I-006 | [Toast Dismiss] | [Auto 5s / click X] | [Fade out] | [200ms] | [ease-in] |
| I-007 | [Modal Open] | [Trigger click] | [Fade in + scale up] | [200ms] | [ease-out] |
| I-008 | [Modal Close] | [Close click / Esc] | [Fade out + scale down] | [150ms] | [ease-in] |
| I-009 | [Dropdown Open] | [Trigger click] | [Slide down + fade in] | [200ms] | [ease-out] |
| I-010 | [Dropdown Close] | [Item select / click out] | [Slide up + fade out] | [150ms] | [ease-in] |
| I-011 | [Tab Switch] | [Tab click] | [Slide indicator] | [200ms] | [ease-in-out] |
| I-012 | [Skeleton Loading] | [Data fetch] | [Shimmer animation] | [1.5s loop] | [linear] |
| I-013 | [Success Check] | [Action success] | [Draw checkmark] | [400ms] | [ease-out] |
| I-014 | [Page Transition] | [Navigation] | [Crossfade] | [300ms] | [ease-in-out] |

## 3. Interaction Details

### I-004: Input Error Shake

```
Trigger: Validation fails on form submit
Animation: Horizontal shake
  - Move right 10px (50ms)
  - Move left 10px (50ms)
  - Move right 6px (50ms)
  - Move left 6px (50ms)
  - Return to center (100ms)
Total: 300ms
Easing: ease-out
```

### I-005: Toast Notification

```
Trigger: Action completes (success/error/info)
Entry: Slide in from right (300ms, ease-out)
  - Start: translateX(100%), opacity(0)
  - End: translateX(0), opacity(1)
Display: 5 seconds
Exit: Fade out + slide right (200ms, ease-in)
  - Start: translateX(0), opacity(1)
  - End: translateX(100%), opacity(0)
```

### I-013: Success Checkmark

```
Trigger: Action succeeds (submit, save, approve)
Animation: Draw checkmark SVG path
  - Duration: 400ms
  - Easing: ease-out
  - Stroke dasharray animation
Then: Scale bounce (1.0 → 1.1 → 1.0, 200ms)
```

## 4. Haptic Feedback (Mobile)

| Interaction | Haptic | When |
|------------|--------|------|
| [Button tap] | [Light] | [Every tap] |
| [Success] | [Medium] | [Action complete] |
| [Error] | [Heavy] | [Validation fail] |
| [Delete confirmation] | [Heavy] | [Destructive action] |

## 5. Sound Design (Optional)

| Event | Sound | Volume | When |
|-------|-------|--------|------|
| [Success] | [Subtle chime] | [20%] | [Action complete] |
| [Error] | [Subtle buzz] | [20%] | [Error occurs] |
| [Notification] | [Soft ping] | [30%] | [New notification] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Design-Specifications]] | Technical specs |
| [[Component-Library]] | Component interactions |
| [[State-Variations]] | State changes |

---

> **Template Standard:** Based on ISO 9241-210, Dan Saffer
> **Usage:** Micro-interactions are the *polish* that makes a product feel premium. Keep them fast (<300ms), purposeful, and consistent.
