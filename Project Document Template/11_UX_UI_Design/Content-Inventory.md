---
document_type: Content Inventory
version: "1.0"
status: Draft
author: "[UX Designer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [content-inventory, content-audit, ux]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
  - Kristina Halvorson — Content Strategy for the Web
---

# Content Inventory

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> A content inventory catalogs all content in the product — pages, messages, labels, help text — ensuring nothing is missing, duplicated, or outdated.

## 2. Content Types

| Type | Count | Owner | Status |
|------|-------|-------|--------|
| [Pages] | [26] | [UX] | ✅ Inventoried |
| [Form Labels] | [45] | [UX] | ✅ Inventoried |
| [Error Messages] | [23] | [UX] | ✅ Inventoried |
| [Help Text] | [18] | [UX] | ✅ Inventoried |
| [Notifications] | [12] | [UX] | ✅ Inventoried |
| [Status Labels] | [8] | [UX] | ✅ Inventoried |
| [Button Labels] | [15] | [UX] | ✅ Inventoried |
| **Total** | **[147]** | | |

## 3. Page Content Inventory

| # | Page | Title | Description | Status | Owner |
|---|------|-------|-------------|--------|-------|
| 1 | [Home] | [Welcome — Project Name] | [Landing page with login] | ✅ | [UX] |
| 2 | [Dashboard] | [Dashboard — Project Name] | [Customer overview] | ✅ | [UX] |
| 3 | [New Request] | [New Request — Project Name] | [Multi-step form] | ✅ | [UX] |
| 4 | [My Requests] | [My Requests — Project Name] | [Request list] | ✅ | [UX] |
| 5 | [Request Detail] | [Request #XXX — Project Name] | [Request details] | ✅ | [UX] |
| 6 | [Admin Dashboard] | [Admin Dashboard — Project Name] | [Operations overview] | ✅ | [UX] |
| 7 | [Work Queue] | [Work Queue — Project Name] | [Staff request list] | ✅ | [UX] |
| 8 | [Reports] | [Reports — Project Name] | [Analytics and reports] | ✅ | [UX] |

## 4. Form Label Inventory

| # | Form | Field Label | Helper Text | Error Message | Status |
|---|------|------------|-------------|--------------|--------|
| 1 | [New Request] | [Request Type] | [Select the type of request] | [Request type is required] | ✅ |
| 2 | [New Request] | [Amount] | [Enter amount in USD] | [Amount must be greater than 0] | ✅ |
| 3 | [New Request] | [Description] | [Describe your request] | [Description is required] | ✅ |
| 4 | [New Request] | [Documents] | [Upload supporting files] | [File must be under 10MB] | ✅ |
| 5 | [Login] | [Email] | — | [Please enter a valid email] | ✅ |
| 6 | [Login] | [Password] | — | [Password is required] | ✅ |

## 5. Error Message Inventory

| # | Context | Message | Tone | Status |
|---|---------|---------|------|--------|
| 1 | [Network error] | [Unable to connect. Please check your internet connection.] | [Helpful] | ✅ |
| 2 | [Server error] | [Something went wrong. Please try again.] | [Neutral] | ✅ |
| 3 | [Not found] | [The page you're looking for doesn't exist.] | [Helpful] | ✅ |
| 4 | [Unauthorized] | [Your session has expired. Please log in again.] | [Helpful] | ✅ |
| 5 | [Validation — required] | [{Field} is required.] | [Direct] | ✅ |
| 6 | [Validation — format] | [Please enter a valid {format}.] | [Helpful] | ✅ |

## 6. Notification Content Inventory

| # | Trigger | Channel | Subject/Title | Body | Status |
|---|---------|---------|--------------|------|--------|
| 1 | [Request submitted] | [Email] | [Request #XXX Received] | [Your request has been received...] | ✅ |
| 2 | [Request approved] | [Email + SMS] | [Request #XXX Approved] | [Your request has been approved...] | ✅ |
| 3 | [Request rejected] | [Email] | [Request #XXX Not Approved] | [Your request was not approved...] | ✅ |
| 4 | [Status changed] | [Email] | [Request #XXX Status Update] | [Your request status has changed...] | ✅ |
| 5 | [SLA warning] | [Slack] | [SLA Warning] | [Request #XXX approaching SLA...] | ✅ |

## 7. Content Quality Checklist

| # | Check | Status |
|---|-------|--------|
| 1 | [All pages have unique, descriptive titles] | ✅ |
| 2 | [All form fields have labels] | ✅ |
| 3 | [All form fields have error messages] | ✅ |
| 4 | [Error messages are helpful, not technical] | ✅ |
| 5 | [Notifications are concise and actionable] | ✅ |
| 6 | [Content follows brand voice] | ✅ |
| 7 | [No placeholder text (Lorem Ipsum) in production] | ✅ |
| 8 | [Content is accessible (readable, clear)] | ✅ |

## 8. Content Gaps

| # | Gap | Priority | Owner | Status |
|---|-----|---------|-------|--------|
| 1 | [Help documentation missing] | 🟡 | [UX] | ⬜ Planned |
| 2 | [Onboarding tooltips missing] | 🟡 | [UX] | ⬜ Planned |
| 3 | [Keyboard shortcut guide missing] | 🟢 | [UX] | ⬜ Planned |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Information Architecture (IA)]] | Content structure |
| [[Style Guide]] | Content standards |
| [[Brand Guidelines]] | Voice and tone |

---

> **Template Standard:** Based on ISO 9241-210, Kristina Halvorson
> **Usage:** You can't manage what you can't measure. The content inventory is the *foundation* of content strategy. Review it quarterly — content rots.
