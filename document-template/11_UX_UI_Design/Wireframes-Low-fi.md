---
document_type: Wireframes (Low-fi)
version: "1.0"
status: Draft
author: "[UX Designer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [wireframes, low-fidelity, ux, layout]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
---

# Wireframes (Low-fidelity)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]
>
> ⚠️ **Note:** Low-fi wireframes are *structural blueprints* — they show layout and hierarchy without visual design. For pixel-perfect mockups, see [[UI-Mockups]] in Figma.

## 1. Purpose

> Low-fi wireframes define page structure, content placement, and interaction patterns — without color, typography, or imagery.

## 2. Wireframe Index

| # | Page | Persona | Device | Status |
|---|------|---------|--------|--------|
| WF-001 | [Customer Dashboard] | [Sarah] | [Mobile + Desktop] | ✅ Approved |
| WF-002 | [Request Form — Step 1] | [Sarah] | [Mobile + Desktop] | ✅ Approved |
| WF-003 | [Request Form — Step 2] | [Sarah] | [Mobile + Desktop] | ✅ Approved |
| WF-004 | [Request Detail] | [Sarah] | [Mobile + Desktop] | ✅ Approved |
| WF-005 | [Admin Work Queue] | [James] | [Desktop] | ✅ Approved |
| WF-006 | [Admin Request Detail] | [James] | [Desktop] | ✅ Approved |
| WF-007 | [Manager Dashboard] | [Linda] | [Desktop] | ✅ Approved |

## 3. Wireframe Specifications

### WF-001: Customer Dashboard

```
┌─────────────────────────────────────────────────────────────┐
│  HEADER                                                      │
│  ┌──────┐  ┌──────────────────────────┐  ┌──────┐          │
│  │ Logo │  │  Nav: Home | New | My Req │  │ User │          │
│  └──────┘  └──────────────────────────┘  └──────┘          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  WELCOME BANNER                                      │    │
│  │  "Welcome back, Sarah"                               │    │
│  │  [New Request] button                                │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                  │
│  │ KPI Card │  │ KPI Card │  │ KPI Card │                  │
│  │ Active: 3│  │ Pending:1│  │ Done: 12 │                  │
│  └──────────┘  └──────────┘  └──────────┘                  │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  RECENT REQUESTS                                      │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │ REQ-001 | Standard | $5,000 | Submitted     │    │    │
│  │  ├─────────────────────────────────────────────┤    │    │
│  │  │ REQ-002 | VIP      | $15,000| Approved      │    │    │
│  │  ├─────────────────────────────────────────────┤    │    │
│  │  │ REQ-003 | Standard | $2,000 | Draft         │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  │  [View All Requests]                                 │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
├─────────────────────────────────────────────────────────────┤
│  FOOTER                                                      │
│  Help | Privacy | Terms                                      │
└─────────────────────────────────────────────────────────────┘
```

**Layout Specifications:**

| Element | Position | Size | Priority |
|---------|---------|------|---------|
| [Header] | [Top, full width] | [60px height] | 🔴 Fixed |
| [Welcome Banner] | [Below header] | [Full width, 120px] | 🔴 Primary CTA |
| [KPI Cards] | [Row, 3 columns] | [Equal width, 80px height] | 🔴 Key metrics |
| [Recent Requests] | [Below KPIs] | [Full width, scrollable] | 🔴 Core content |
| [Footer] | [Bottom] | [40px] | 🟢 Secondary |

### WF-002: Request Form — Step 1 (Personal Info)

```
┌─────────────────────────────────────────────────────────────┐
│  HEADER                                                      │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  PROGRESS BAR                                        │    │
│  │  [●] Step 1  [○] Step 2  [○] Step 3  [○] Review    │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  STEP 1: PERSONAL INFORMATION                        │    │
│  │                                                       │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │ Full Name *                                   │    │    │
│  │  │ [___________________________________]        │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  │                                                       │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │ Email *                                       │    │    │
│  │  │ [___________________________________]        │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  │                                                       │    │
│  │  ┌────────────────────┐  ┌────────────────────┐    │    │
│  │  │ Phone               │  │ Address             │    │    │
│  │  │ [_______________]   │  │ [_______________]   │    │    │
│  │  └────────────────────┘  └────────────────────┘    │    │
│  │                                                       │    │
│  │  [Save Draft]                    [Next Step →]       │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
├─────────────────────────────────────────────────────────────┤
│  FOOTER                                                      │
└─────────────────────────────────────────────────────────────┘
```

**Mobile Layout:**

```
┌─────────────────────┐
│ HEADER              │
├─────────────────────┤
│ Progress: ● ○ ○ ○  │
├─────────────────────┤
│ Step 1: Personal    │
│                     │
│ Full Name *         │
│ [_______________]   │
│                     │
│ Email *             │
│ [_______________]   │
│                     │
│ Phone               │
│ [_______________]   │
│                     │
│ Address             │
│ [_______________]   │
│                     │
│ [Save Draft]        │
│ [Next Step →]       │
├─────────────────────┤
│ FOOTER              │
└─────────────────────┘
```

### WF-005: Admin Work Queue (James)

```
┌─────────────────────────────────────────────────────────────────────┐
│  HEADER                                                              │
│  ┌──────┐  ┌─────────────────────────────────────┐  ┌──────┐       │
│  │ Logo │  │ Nav: Dashboard|Queue|Search|Reports  │  │ User │       │
│  └──────┘  └─────────────────────────────────────┘  └──────┘       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌────────────────────────────────────────────────────────────┐     │
│  │  FILTERS BAR                                                │     │
│  │  [Status ▼] [Type ▼] [Priority ▼] [Date Range] [Search...] │     │
│  └────────────────────────────────────────────────────────────┘     │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────┐     │
│  │  QUEUE TABLE                                                │     │
│  │  ┌────┬──────┬───────┬────────┬───────┬──────┬──────────┐ │     │
│  │  │ ID │ Type │Amount │Customer│Status │ SLA  │ Actions  │ │     │
│  │  ├────┼──────┼───────┼────────┼───────┼──────┼──────────┤ │     │
│  │  │001 │ VIP  │$15,000│ Sarah  │Queue  │ ⚠️ 2h│ [👁️][✅] │ │     │
│  │  ├────┼──────┼───────┼────────┼───────┼──────┼──────────┤ │     │
│  │  │002 │ Corp │$50,000│ Bob    │Queue  │ ✅ 8h│ [👁️][✅] │ │     │
│  │  ├────┼──────┼───────┼────────┼───────┼──────┼──────────┤ │     │
│  │  │003 │ Std  │$3,000 │ Alice  │Review │ ✅ 4h│ [👁️][✅] │ │     │
│  │  └────┴──────┴───────┴────────┴───────┴──────┴──────────┘ │     │
│  │                                                              │     │
│  │  [< Prev]  Page 1 of 5  [Next >]                           │     │
│  └────────────────────────────────────────────────────────────┘     │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────┐     │
│  │  SUMMARY BAR                                                │     │
│  │  Queue: 23 | Today: 8 | SLA Risk: 2 | Avg Time: 2.5h      │     │
│  └────────────────────────────────────────────────────────────┘     │
│                                                                      │
├─────────────────────────────────────────────────────────────────────┤
│  FOOTER                                                              │
└─────────────────────────────────────────────────────────────────────┘
```

## 4. Wireframe Annotations

| # | Element | Behavior | Notes |
|---|---------|----------|-------|
| 1 | [Progress Bar] | [Click step to navigate back] | [Save current data before navigating] |
| 2 | [KPI Cards] | [Click to filter requests] | [Link to filtered list] |
| 3 | [Queue Table Rows] | [Click row to open detail] | [Eye icon for quick preview] |
| 4 | [Filter Bar] | [Real-time filtering] | [Debounce search input] |
| 5 | [Save Draft] | [Auto-save every 30s] | [Show last saved timestamp] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Wireframes-Mid-fi]] | Higher fidelity version |
| [[UI-Mockups]] | Visual design (Figma) |
| [[User-Flows]] | Flows these wireframes support |

---

> **Template Standard:** Based on ISO 9241-210
> **Usage:** Low-fi wireframes are for *layout exploration* — quick to create, easy to change. Use them to validate structure before investing in visual design.
