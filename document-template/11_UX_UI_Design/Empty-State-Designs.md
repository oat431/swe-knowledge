---
document_type: Empty State Designs
version: "1.0"
status: Draft
author: "[UX Designer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [empty-states, ux, illustrations]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
---

# Empty State Designs

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Empty states are what users see when there's no data — first-time use, no results, or cleared content. Design them to guide, not frustrate.

## 2. Empty State Types

| Type | When | Goal |
|------|------|------|
| [First Use] | [User has never used this feature] | [Guide to first action] |
| [No Results] | [Search/filter returns nothing] | [Help refine search] |
| [Cleared] | [User completed all items] | [Celebrate completion] |
| [Error] | [Data failed to load] | [Provide recovery] |

## 3. Empty State Specifications

### 3.1 My Requests — First Use

```
┌─────────────────────────────────────────────────────────────┐
│                                                              │
│                    [Inbox Illustration]                      │
│                                                              │
│              No requests yet                                 │
│                                                              │
│    Submit your first request to get started.                 │
│    It only takes a few minutes.                              │
│                                                              │
│              [Submit Your First Request]                     │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

| Element | Specification |
|---------|--------------|
| [Illustration] | [Friendly inbox illustration, 200px] |
| [Title] | [H2, 24px, bold] |
| [Description] | [Body, 16px, secondary color, 2 lines max] |
| [CTA] | [Primary button] |

### 3.2 Work Queue — All Caught Up

```
┌─────────────────────────────────────────────────────────────┐
│                                                              │
│                    [Checkmark Illustration]                  │
│                                                              │
│              All caught up!                                  │
│                                                              │
│    No requests in your queue right now.                      │
│    Great work!                                               │
│                                                              │
│              [View Completed Requests]                       │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### 3.3 Search — No Results

```
┌─────────────────────────────────────────────────────────────┐
│                                                              │
│                    [Search Illustration]                     │
│                                                              │
│              No results found                               │
│                                                              │
│    We couldn't find anything matching "{query}".             │
│    Try adjusting your search or filters.                     │
│                                                              │
│              [Clear Filters]  [Try Different Search]         │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### 3.4 Documents — Empty

```
┌─────────────────────────────────────────────────────────────┐
│                                                              │
│                    [Folder Illustration]                     │
│                                                              │
│              No documents yet                               │
│                                                              │
│    Upload supporting documents for your request.             │
│                                                              │
│              [Upload Document]                               │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### 3.5 Notifications — Empty

```
┌─────────────────────────────────────────────────────────────┐
│                                                              │
│                    [Bell Illustration]                       │
│                                                              │
│              You're all caught up                            │
│                                                              │
│    No new notifications.                                     │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## 4. Empty State Guidelines

| Rule | Example |
|------|---------|
| [Use friendly, helpful tone] | ✅ "No requests yet" ❌ "Empty" |
| [Include illustration] | [Visual breaks up whitespace] |
| [Provide clear next action] | ✅ "Submit Your First Request" ❌ No CTA |
| [Keep text brief] | [2-3 lines max] |
| [Match brand voice] | [Professional, helpful] |

## 5. Illustration Specifications

| Illustration | Size | Format | Color |
|-------------|------|--------|-------|
| [Inbox — Empty] | [200px] | [SVG] | [Primary blue] |
| [Checkmark — Complete] | [200px] | [SVG] | [Success green] |
| [Search — No Results] | [200px] | [SVG] | [Neutral gray] |
| [Folder — Empty] | [200px] | [SVG] | [Primary blue] |
| [Bell — No Notifications] | [200px] | [SVG] | [Neutral gray] |
| [Error — Something Wrong] | [200px] | [SVG] | [Error red] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[State-Variations]] | Empty states as part of overall states |
| [[Asset-Export-Package]] | Illustration exports |
| [[Style-Guide]] | Visual standards |

---

> **Template Standard:** Based on ISO 9241-210
> **Usage:** Empty states are *opportunities* — they guide users to their first action. Don't leave them blank. A well-designed empty state converts better than a blank screen.
