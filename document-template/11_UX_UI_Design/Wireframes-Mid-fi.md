---
document_type: Wireframes (Mid-fi)
version: "1.0"
status: Draft
author: "[UX Designer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [wireframes, mid-fidelity, ux, layout]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
---

# Wireframes (Mid-fidelity)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]
>
> ⚠️ **Note:** Mid-fi wireframes add more detail than low-fi — realistic content, actual spacing, interaction patterns. For final visual design, see [[UI-Mockups]] in Figma.

## 1. Purpose

> Mid-fi wireframes bridge low-fi sketches and high-fi mockups — showing realistic content, actual data patterns, and interaction details.

## 2. Key Differences from Low-fi

| Aspect | Low-fi | Mid-fi |
|--------|--------|--------|
| [Content] | [Placeholder text] | [Realistic content] |
| [Spacing] | [Approximate] | [Actual grid/padding] |
| [Interactions] | [Noted in annotations] | [Shown in states] |
| [Typography] | [Uniform] | [Hierarchy shown] |
| [Icons] | [None] | [Actual icons] |
| [Color] | [None] | [Grayscale + 1 accent] |

## 3. Mid-fi Wireframe Specifications

### Request Detail Page — Desktop

```
┌─────────────────────────────────────────────────────────────────────┐
│  HEADER                                          [🔔] [👤 Sarah ▼] │
├─────────────────────────────────────────────────────────────────────┤
│  ← Back to My Requests                                              │
│                                                                      │
│  ┌─────────────────────────────────────────────────────────────┐    │
│  │  REQ-2026-001           ● Submitted           Priority: 🟡  │    │
│  │  Standard Request       Amount: $5,000.00                   │    │
│  └─────────────────────────────────────────────────────────────┘    │
│                                                                      │
│  ┌──────────────────────────────────┐  ┌────────────────────────┐   │
│  │  REQUEST INFORMATION (70%)       │  │  STATUS (30%)          │   │
│  │                                   │  │                        │   │
│  │  Type: Standard                   │  │  ┌─ ● Submitted       │   │
│  │  Amount: $5,000.00                │  │  │   Jul 12, 2:30 PM  │   │
│  │  Submitted: Jul 12, 2026          │  │  │                    │   │
│  │                                   │  │  ├─ ● Validating      │   │
│  │  Description:                     │  │  │   Jul 12, 2:31 PM  │   │
│  │  "Request for equipment           │  │  │                    │   │
│  │   purchase for new office"        │  │  ├─ ○ Classified      │   │
│  │                                   │  │  │   Pending...       │   │
│  │  ─────────────────────────────   │  │  │                    │   │
│  │                                   │  │  └─ ○ Completed       │   │
│  │  DOCUMENTS (2)                    │  │      Pending...       │   │
│  │  ┌─────────────┐ ┌─────────────┐ │  │                        │   │
│  │  │ 📄 invoice  │ │ 📄 quote    │ │  │  ──────────────────── │   │
│  │  │ .pdf  2.1MB │ │ .pdf  1.5MB │ │  │                        │   │
│  │  │ [Preview]   │ │ [Preview]   │ │  │  Assigned: Auto        │   │
│  │  └─────────────┘ └─────────────┘ │  │  SLA: 24h remaining   │   │
│  │                                   │  │                        │   │
│  └──────────────────────────────────┘  └────────────────────────┘   │
│                                                                      │
├─────────────────────────────────────────────────────────────────────┤
│  FOOTER                                                              │
└─────────────────────────────────────────────────────────────────────┘
```

### Admin Review Panel — Desktop

```
┌─────────────────────────────────────────────────────────────────────┐
│  Review: REQ-2026-001                               [← Back to Queue]│
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌─────────────────────────────────────────────────────────────┐    │
│  │  ACTION BAR                                                 │    │
│  │  [✅ Approve]  [❌ Reject]  [⬆️ Escalate]  [💬 Comment]    │    │
│  └─────────────────────────────────────────────────────────────┘    │
│                                                                      │
│  ┌──────────────────────────┐  ┌──────────────────────────────┐    │
│  │  REQUEST DETAILS (60%)    │  │  DOCUMENT VIEWER (40%)       │    │
│  │                           │  │                              │    │
│  │  Customer: Sarah Chen     │  │  ┌────────────────────────┐ │    │
│  │  Type: Standard           │  │  │                        │ │    │
│  │  Amount: $5,000.00        │  │  │   📄 invoice.pdf       │ │    │
│  │  Submitted: Jul 12        │  │  │                        │ │    │
│  │                           │  │  │   [Document Preview]   │ │    │
│  │  Description:             │  │  │                        │ │    │
│  │  "Equipment purchase..."  │  │  │                        │ │    │
│  │                           │  │  └────────────────────────┘ │    │
│  │  Business Rules:          │  │                              │    │
│  │  ✅ Amount ≤ $10K         │  │  [◀ Prev]  1 of 2  [Next ▶]│    │
│  │  ✅ No duplicate          │  │                              │    │
│  │  ✅ Risk score: 0.15      │  │  ────────────────────────  │    │
│  │  ✅ Auto-approve eligible │  │  QUICK ACTIONS              │    │
│  │                           │  │  [✅ Approve] [❌ Reject]   │    │
│  │  ─────────────────────   │  │  [⬆️ Escalate]              │    │
│  │                           │  │                              │    │
│  │  HISTORY                  │  │  ────────────────────────  │    │
│  │  Jul 12 2:30 - Submitted  │  │  ADD COMMENT                │    │
│  │  Jul 12 2:31 - Validating │  │  [________________________]│    │
│  │                           │  │  [Submit Comment]           │    │
│  └──────────────────────────┘  └──────────────────────────────┘    │
│                                                                      │
├─────────────────────────────────────────────────────────────────────┤
│  FOOTER                                                              │
└─────────────────────────────────────────────────────────────────────┘
```

## 4. Responsive Breakpoints

| Breakpoint | Width | Layout Change |
|-----------|-------|--------------|
| [Desktop] | [≥1200px] | [Side-by-side: details + documents] |
| [Tablet] | [768-1199px] | [Stacked: details above documents] |
| [Mobile] | [<768px] | [Single column, tab navigation] |

## 5. Interaction Specifications

| Element | Interaction | Feedback |
|---------|-----------|---------|
| [Approve Button] | [Click → Confirmation dialog] | [Modal: "Are you sure?"] |
| [Document Preview] | [Click → Inline preview] | [Expand document in panel] |
| [Status Timeline] | [Hover → Detail tooltip] | [Show timestamp + actor] |
| [Comment Submit] | [Click → Add to history] | [Comment appears in timeline] |
| [Filter Dropdowns] | [Select → Real-time filter] | [Table updates immediately] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Wireframes-Low-fi]] | Low-fi version |
| [[UI-Mockups]] | Visual design (Figma) |
| [[Component-Library]] | Components used |

---

> **Template Standard:** Based on ISO 9241-210
> **Usage:** Mid-fi wireframes are for *interaction validation* — test with users before investing in visual design. They're detailed enough for usability testing.
