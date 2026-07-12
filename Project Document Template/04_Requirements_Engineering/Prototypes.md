---
document_type: Prototypes (Low/High Fidelity)
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
ba_owner: "[Business Analyst Name]"
designer: "[UI/UX Designer]"
classification: "Internal / Confidential"
tags: [prototypes, wireframes, mockups, ux, swebok]
standard_ref:
  - SWEBOK v4 — Requirements
  - ISO 9241-210 — Human-Centred Design
---

# Prototypes (Low/High Fidelity)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Business Analyst | [Name / Role] |
| UI/UX Designer | [Name / Role] |

### Revision History

| Version | Date | Author | Change Description |
|---------|------|--------|--------------------|
| 0.1 | [YYYY-MM-DD] | [Name] | Low-fi wireframes |
| 0.2 | [YYYY-MM-DD] | [Name] | Mid-fi wireframes |
| 1.0 | [YYYY-MM-DD] | [Name] | High-fi prototypes |

---

## 1. Purpose

> This document catalogs all prototypes created during requirements elicitation and validation. Prototypes help stakeholders visualize requirements, validate assumptions, and reduce misunderstandings before development begins.

## 2. Prototype Types

| Type | Fidelity | Tool | Purpose | When to Use |
|------|---------|------|---------|-------------|
| **Sketch** | Very Low | Paper / Whiteboard | [Quick idea exploration] | [Brainstorming, early concept] |
| **Low-Fi Wireframe** | Low | Balsamiq / Sketch | [Layout and content structure] | [Requirements validation] |
| **Mid-Fi Wireframe** | Medium | Figma / Sketch | [Interaction and flow] | [User testing, stakeholder review] |
| **High-Fi Mockup** | High | Figma / Adobe XD | [Visual design and branding] | [Design approval, development handoff] |
| **Interactive Prototype** | High | Figma / InVision | [Clickable flow for user testing] | [UAT preparation, user validation] |

## 3. Prototype Inventory

| # | Screen/Page | Fidelity | Tool | Version | Status | Related Requirements |
|---|------------|----------|------|---------|--------|---------------------|
| P-01 | [Customer Portal — Home] | Low-Fi → High-Fi | [Figma] | v2.0 | ✅ Approved | FR-001 |
| P-02 | [Request Form] | Low-Fi → High-Fi | [Figma] | v3.0 | ✅ Approved | FR-001, FR-002, FR-003 |
| P-03 | [My Requests — List] | Low-Fi → Mid-Fi | [Figma] | v1.0 | ⚠️ In Review | FR-006 |
| P-04 | [Request Detail — Timeline] | Low-Fi | [Figma] | v1.0 | ⬜ Draft | FR-006, FR-007 |
| P-05 | [Admin Portal — Work Queue] | Low-Fi | [Balsamiq] | v1.0 | ⬜ Draft | FR-101, FR-102 |
| P-06 | [Admin Portal — Request Review] | Low-Fi | [Balsamiq] | v1.0 | ⬜ Draft | FR-104, FR-106 |
| P-07 | [Dashboard — Overview] | Low-Fi | [Balsamiq] | v1.0 | ⬜ Draft | FR-301, FR-305 |
| P-08 | [Login / Registration] | High-Fi | [Figma] | v1.0 | ✅ Approved | SEC-001 |

## 4. Prototype Specifications

### 4.1 Low-Fi Wireframe: Request Form (P-02)

**Purpose:** Validate form layout, field order, and validation behavior with stakeholders.

**Key Elements:**

| Element | Description | Interaction | Validation |
|---------|-------------|------------|-----------|
| [Request Type dropdown] | [Select request type] | [Changes form fields dynamically] | [Required] |
| [Personal Info section] | [Name, email, phone] | [Auto-fill if logged in] | [Format validation] |
| [Request Details section] | [Description, amount, priority] | [Amount triggers business rules] | [Required, range check] |
| [Document Upload] | [File upload area] | [Drag & drop or click] | [File type, size limit] |
| [Submit Button] | [Primary action] | [Triggers final validation] | [Disabled until valid] |
| [Save Draft Button] | [Secondary action] | [Saves current state] | [Always available] |

**Stakeholder Feedback:**

| # | Feedback | Source | Action Taken |
|---|---------|--------|-------------|
| 1 | ["Amount field should auto-format with commas"] | [Ops Manager] | [Added auto-formatting] |
| 2 | ["Need a progress indicator for multi-step form"] | [End User] | [Added step indicator] |
| 3 | ["Upload should show file size limit"] | [CS Lead] | [Added "Max 10MB per file" text] |

### 4.2 High-Fi Mockup: Customer Portal Home (P-01)

**Purpose:** Final visual design for development handoff.

**Design Specifications:**

| Element | Specification |
|---------|--------------|
| [Layout] | [Responsive — desktop: 12-col grid, tablet: 8-col, mobile: 4-col] |
| [Typography] | [Headings: Inter Bold 24/20/16, Body: Inter Regular 16/14] |
| [Colors] | [Primary: #2196F3, Secondary: #4CAF50, Error: #f44336, Background: #FAFAFA] |
| [Spacing] | [8px grid system, 16px padding, 24px section gaps] |
| [Components] | [Buttons, cards, forms per design system] |

**Accessibility Notes:**

| Check | Status | Notes |
|-------|--------|-------|
| [Color contrast ≥4.5:1] | ✅ | [Verified with tool] |
| [Focus indicators visible] | ✅ | [Blue outline, 2px] |
| [ARIA labels on icons] | ✅ | [All icons labeled] |
| [Keyboard navigation] | ✅ | [Tab order logical] |

## 5. Interactive Prototype

### 5.1 Prototype Link

| Prototype | Tool | Link | Version | Status |
|-----------|------|------|---------|--------|
| [Customer Portal Flow] | [Figma] | [URL] | v2.0 | ✅ User Tested |
| [Admin Portal Flow] | [Figma] | [URL] | v1.0 | ⬜ Not Started |

### 5.2 User Test Results

| Test | Date | Participants | Tasks | Success Rate | Issues Found |
|------|------|-------------|-------|-------------|-------------|
| [Customer Portal — Submit Request] | [YYYY-MM-DD] | [5 users] | [3 tasks] | [85%] | [2 — button placement unclear] |
| [Customer Portal — Track Status] | [YYYY-MM-DD] | [5 users] | [2 tasks] | [95%] | [0] |
| [Admin Portal — Process Request] | [YYYY-MM-DD] | [3 staff] | [3 tasks] | [70%] | [3 — workflow confusing] |

### 5.3 Usability Issues from Prototyping

| Issue ID | Screen | Issue | Severity | Source | Resolution |
|----------|--------|-------|----------|--------|-----------|
| UI-001 | [Request Form] | [Submit button not visible without scrolling] | 🟡 Medium | [User test] | [Moved button to sticky footer] |
| UI-002 | [Work Queue] | [Filter options unclear] | 🟡 Medium | [Staff feedback] | [Added filter labels and tooltips] |
| UI-003 | [Request Detail] | [Timeline hard to read] | 🟢 Low | [User test] | [Redesigned with visual timeline] |

## 6. Prototype Review Record

| Review | Date | Participants | Prototype | Outcome | Sign-Off |
|--------|------|-------------|-----------|---------|----------|
| [Review 1 — Lo-fi] | [YYYY-MM-DD] | [BA, Ops Manager, End Users] | [P-01, P-02] | [Approved with modifications] | ✅ |
| [Review 2 — Mid-fi] | [YYYY-MM-DD] | [BA, Designer, End Users] | [P-02, P-03] | [Approved] | ✅ |
| [Review 3 — Hi-fi] | [YYYY-MM-DD] | [BA, Designer, Sponsor, End Users] | [P-01, P-02, P-08] | [Approved] | ✅ |
| [Review 4 — Interactive] | [YYYY-MM-DD] | [All stakeholders] | [Customer Portal Flow] | [User testing complete] | ✅ |

## 7. Design-to-Development Handoff

| Screen | Design File | Specs | Assets | Dev Notes | Status |
|--------|------------|-------|--------|-----------|--------|
| [P-01 Home] | [Figma link] | [Specs link] | [Assets folder] | [Responsive, lazy load] | ✅ Handed Off |
| [P-02 Form] | [Figma link] | [Specs link] | [Assets folder] | [Multi-step, validation] | ✅ Handed Off |
| [P-03 My Requests] | [Figma link] | [Specs link] | [Assets folder] | [Pagination, filters] | ⬜ Pending |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Software-Requirements-Specification]] | Requirements these prototypes visualize |
| [[User-Stories]] | Stories validated via prototypes |
| [[Acceptance-Criteria]] | ACs tested via interactive prototypes |
| [[Design-System]] | Component library used in high-fi prototypes |
| [[Usability-Test-Report]] | Formal usability test results |

---

> **Template Standard:** Based on SWEBOK v4, ISO 9241-210
> **Usage:** Prototypes are *requirements artifacts*, not just design artifacts. They validate requirements with stakeholders before development. Document feedback from every review — it's valuable requirements data. Link prototypes to requirements for traceability.
