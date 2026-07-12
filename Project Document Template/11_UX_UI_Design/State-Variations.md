---
document_type: State Variations
version: "1.0"
status: Draft
author: "[UX Designer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [state-variations, ui-states, ux]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
---

# State Variations

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document defines all UI states — every component and page must be designed for each applicable state. Missing states = bugs.

## 2. Global States

| State | Visual | When | Behavior |
|-------|--------|------|---------|
| [Default] | [Standard appearance] | [Normal] | [Interactive] |
| [Hover] | [Subtle highlight] | [Mouse over] | [Cursor change] |
| [Active/Pressed] | [Darker shade] | [Click/tap] | [Pressed feedback] |
| [Focus] | [Blue outline] | [Keyboard navigation] | [Tab-accessible] |
| [Disabled] | [50% opacity, gray] | [Not available] | [Not interactive] |
| [Loading] | [Spinner/skeleton] | [Data loading] | [Not interactive] |
| [Error] | [Red border + message] | [Validation failed] | [Needs correction] |
| [Empty] | [Illustration + message] | [No data] | [Guidance to add data] |

## 3. Page States

### 3.1 List Page States

| State | Visual | Content | CTA |
|-------|--------|---------|-----|
| [Loading] | [Skeleton cards/rows] | [Placeholder shapes] | [None] |
| [Empty] | [Illustration + "No requests yet"] | [Help text] | [Create First Request] |
| [With Data] | [Full list/table] | [Actual data] | [Pagination] |
| [Error] | [Error illustration + message] | [Retry message] | [Try Again] |
| [Filtered — No Results] | [No results illustration] | ["No requests match filters"] | [Clear Filters] |

### 3.2 Detail Page States

| State | Visual | Content | CTA |
|-------|--------|---------|-----|
| [Loading] | [Skeleton detail] | [Placeholder shapes] | [None] |
| [Found] | [Full detail] | [Actual data] | [Actions] |
| [Not Found] | [404 illustration] | ["Request not found"] | [Back to List] |
| [Error] | [Error illustration] | [Error message] | [Try Again] |
| [Unauthorized] | [Lock illustration] | ["You don't have access"] | [Request Access] |

## 4. Component States

### 4.1 Button States

| State | Visual | Cursor | Behavior |
|-------|--------|--------|---------|
| [Default] | [Standard color] | [Pointer] | [Clickable] |
| [Hover] | [Darken 10%] | [Pointer] | [Visual feedback] |
| [Active] | [Darken 20%] | [Pointer] | [Pressed feedback] |
| [Disabled] | [50% opacity] | [Not-allowed] | [Not clickable] |
| [Loading] | [Spinner replaces text] | [Wait] | [Not clickable] |

### 4.2 Input States

| State | Visual | Behavior |
|-------|--------|---------|
| [Empty] | [Placeholder text] | [Ready for input] |
| [Focus] | [Blue border] | [Receives input] |
| [Filled] | [Text present] | [Editable] |
| [Error] | [Red border + message] | [Needs correction] |
| [Disabled] | [Gray bg] | [Not editable] |

### 4.3 Card States

| State | Visual | Behavior |
|-------|--------|---------|
| [Default] | [Standard card] | [Clickable if interactive] |
| [Hover] | [Shadow increase] | [Visual feedback] |
| [Selected] | [Blue border] | [Indicates selection] |
| [Disabled] | [50% opacity] | [Not interactive] |

## 5. Status Badge States

| Status | Color | Icon | Text |
|--------|-------|------|------|
| [Draft] | [Gray] | [📝] | [Draft] |
| [Submitted] | [Blue] | [📤] | [Submitted] |
| [Validating] | [Blue pulse] | [⏳] | [Validating] |
| [Under Review] | [Orange] | [👁️] | [Under Review] |
| [Approved] | [Green] | [✅] | [Approved] |
| [Rejected] | [Red] | [❌] | [Rejected] |
| [Escalated] | [Orange pulse] | [⬆️] | [Escalated] |

## 6. Empty State Templates

| Page | Illustration | Message | CTA |
|------|-------------|---------|-----|
| [My Requests — Empty] | [Inbox illustration] | ["You haven't submitted any requests yet"] | [Submit Your First Request] |
| [Work Queue — Empty] | [Checkmark illustration] | ["All caught up! No requests in queue"] | [View Completed] |
| [Search — No Results] | [Search illustration] | ["No results found for '{query}'"] | [Clear Search] |
| [Documents — Empty] | [Folder illustration] | ["No documents uploaded yet"] | [Upload Document] |

## 7. Loading State Patterns

| Pattern | Usage | Duration |
|---------|-------|---------|
| [Skeleton Screen] | [Page-level loading] | [> 1 second] |
| [Spinner] | [Button/action loading] | [< 3 seconds] |
| [Progress Bar] | [File upload, long operations] | [Known duration] |
| [Shimmer Effect] | [Content loading] | [> 1 second] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[UI Mockups]] | Visual designs for each state |
| [[Component Library]] | Component state specifications |
| [[Accessibility Audit]] | Accessible state requirements |

---

> **Template Standard:** Based on ISO 9241-210
> **Usage:** Design *every* state before development. The question isn't "what does it look like with data?" — it's "what does it look like with no data, wrong data, loading data, and error data?"
