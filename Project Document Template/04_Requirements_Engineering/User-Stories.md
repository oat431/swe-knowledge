---
document_type: User Stories
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
ba_owner: "[Business Analyst Name]"
po_owner: "[Product Owner]"
classification: "Internal / Confidential"
tags: [user-stories, agile, backlog, swebok]
standard_ref:
  - SWEBOK v4 — Requirements
  - ISO/IEC/IEEE 29148 — Requirements Engineering
---

# User Stories

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | In Backlog]
> **Last Updated:** [YYYY-MM-DD]

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Business Analyst | [Name / Role] |
| Product Owner | [Name / Role] |

### Revision History

| Version | Date | Author | Change Description |
|---------|------|--------|--------------------|
| 0.1 | [YYYY-MM-DD] | [Name] | Initial draft |
| 1.0 | [YYYY-MM-DD] | [Name] | Approved version |

---

## 1. Purpose

> This document captures requirements as user stories in the format: **"As a [role], I want [feature], so that [benefit]."** Each story includes acceptance criteria and estimation.

## 2. User Story Standards

### 2.1 INVEST Criteria

| Criterion | Description | Check |
|-----------|-------------|-------|
| **I**ndependent | [Story can be developed independently] | ✅ |
| **N**egotiable | [Details can be discussed and refined] | ✅ |
| **V**aluable | [Delivers value to a user or business] | ✅ |
| **E**stimable | [Team can estimate effort] | ✅ |
| **S**mall | [Fits within a sprint] | ✅ |
| **T**estable | [Has clear acceptance criteria] | ✅ |

### 2.2 Story Format

```markdown
**As a** [role/persona]
**I want** [feature/capability]
**So that** [benefit/value]

**Acceptance Criteria:**
Given [precondition]
When [action]
Then [outcome]

**Story Points:** [1, 2, 3, 5, 8, 13]
**Priority:** [🔴 Must Have / 🟡 Should Have / 🟢 Could Have]
**Epic:** [Parent epic]
**Sprint:** [Target sprint]
```

## 3. Epic Overview

| Epic ID | Epic Name | Stories | Total Points | Sprint |
|---------|-----------|---------|-------------|--------|
| E-01 | [Customer Portal] | [6] | [21] | [Sprint 1-3] |
| E-02 | [Request Processing] | [8] | [34] | [Sprint 2-4] |
| E-03 | [Notifications] | [4] | [13] | [Sprint 3-4] |
| E-04 | [Reporting & Dashboard] | [5] | [18] | [Sprint 4-5] |
| E-05 | [Administration] | [4] | [13] | [Sprint 2-3] |

## 4. User Stories

### Epic E-01: Customer Portal

#### US-001: Submit Request Online

**As a** customer
**I want** to submit my request online via a web portal
**So that** I don't have to visit an office or send paper forms

**Acceptance Criteria:**
- **AC-1:** Given I am logged in to the portal, When I fill all required fields and click Submit, Then my request is created with a unique reference number
- **AC-2:** Given I have a saved draft, When I log in, Then I see my draft and can continue filling it
- **AC-3:** Given I submit a request, When submission is successful, Then I receive a confirmation email within 5 minutes

**Story Points:** 5
**Priority:** 🔴 Must Have
**Epic:** E-01
**Sprint:** Sprint 1
**Status:** Draft

---

#### US-002: View Request Status

**As a** customer
**I want** to see the real-time status of my request
**So that** I don't have to call to check progress

**Acceptance Criteria:**
- **AC-1:** Given I have an active request, When I navigate to My Requests, Then I see all my requests with current status
- **AC-2:** Given my request status changes, When the change occurs, Then I receive an email notification within 5 minutes
- **AC-3:** Given I click on a request, When the detail page loads, Then I see a full timeline of all status changes

**Story Points:** 3
**Priority:** 🔴 Must Have
**Epic:** E-01
**Sprint:** Sprint 1
**Status:** Draft

---

#### US-003: Upload Supporting Documents

**As a** customer
**I want** to upload supporting documents with my request
**So that** I can provide required evidence without visiting an office

**Acceptance Criteria:**
- **AC-1:** Given I am filling a request form, When I click Upload, Then I can select files (PDF, JPG, PNG) up to 10MB each
- **AC-2:** Given I upload a file, When the upload completes, Then the file appears in my upload list with name and size
- **AC-3:** Given I upload more than 5 files, When I try to upload the 6th, Then an error message: "Maximum 5 files allowed"

**Story Points:** 3
**Priority:** 🔴 Must Have
**Epic:** E-01
**Sprint:** Sprint 2
**Status:** Draft

---

### Epic E-02: Request Processing

#### US-101: Auto-Validate Request

**As a** operations staff member
**I want** requests to be automatically validated against business rules
**So that** I only review complete and valid requests

**Acceptance Criteria:**
- **AC-1:** Given a request is submitted with missing required fields, When validation runs, Then the request is rejected with specific field-level errors
- **AC-2:** Given a request is submitted with all valid data, When validation passes, Then the request proceeds to classification

**Story Points:** 5
**Priority:** 🔴 Must Have
**Epic:** E-02
**Sprint:** Sprint 2
**Status:** Draft

---

#### US-102: Auto-Route Request

**As a** operations staff member
**I want** requests to be automatically classified and routed to the correct queue
**So that** I receive requests matching my expertise

**Acceptance Criteria:**
- **AC-1:** Given a validated request, When classification rules run, Then the request is assigned to the correct queue
- **AC-2:** Given the queue has capacity, When a request arrives, Then it is assigned to the next available staff member

**Story Points:** 5
**Priority:** 🔴 Must Have
**Epic:** E-02
**Sprint:** Sprint 2
**Status:** Draft

---

#### US-103: Auto-Approve Request

**As a** operations staff member
**I want** eligible requests to be automatically approved
**So that** I can focus on complex cases that need human judgment

**Acceptance Criteria:**
- **AC-1:** Given a request meets ALL auto-approval criteria, When it enters the approval stage, Then it is approved within 1 minute
- **AC-2:** Given a VIP customer submits ≤$25K, When it enters the approval stage, Then it is auto-approved
- **AC-3:** Given a request does NOT meet auto-approval criteria, When it enters the approval stage, Then it is routed to manual review

**Story Points:** 8
**Priority:** 🔴 Must Have
**Epic:** E-02
**Sprint:** Sprint 3
**Status:** Draft

---

### Epic E-03: Notifications

#### US-201: Email Notifications

**As a** stakeholder (customer, staff, manager)
**I want** to receive email notifications at each status change
**So that** I am always informed without checking the system

**Acceptance Criteria:**
- **AC-1:** Given a request status changes, When the change is saved, Then an email is sent to the relevant stakeholders within 5 minutes
- **AC-2:** Given the email is sent, When the recipient opens it, Then the email contains request reference, new status, and a link to view details

**Story Points:** 3
**Priority:** 🔴 Must Have
**Epic:** E-03
**Sprint:** Sprint 3
**Status:** Draft

---

### Epic E-04: Reporting & Dashboard

#### US-301: Operational Dashboard

**As a** manager
**I want** to see a real-time operational dashboard
**So that** I can monitor team performance and identify bottlenecks

**Acceptance Criteria:**
- **AC-1:** Given I am logged in as a manager, When I open the dashboard, Then I see: requests today, average processing time, queue depth, SLA compliance %
- **AC-2:** Given the dashboard is open, When new data arrives, Then the dashboard auto-refreshes every 30 seconds
- **AC-3:** Given I click on a metric, When the drill-down loads, Then I see the underlying data with filters

**Story Points:** 8
**Priority:** 🟡 Should Have
**Epic:** E-04
**Sprint:** Sprint 4
**Status:** Draft

---

## 5. Story Estimation Summary

| Epic | Stories | Total Points | Sprint Allocation |
|------|---------|-------------|------------------|
| E-01 Customer Portal | [6] | [21] | Sprint 1-3 |
| E-02 Request Processing | [8] | [34] | Sprint 2-4 |
| E-03 Notifications | [4] | [13] | Sprint 3-4 |
| E-04 Reporting | [5] | [18] | Sprint 4-5 |
| E-05 Administration | [4] | [13] | Sprint 2-3 |
| **Total** | **[27]** | **[99]** | |

## 6. Story Map

| | Sprint 1 | Sprint 2 | Sprint 3 | Sprint 4 | Sprint 5 |
|--|---------|---------|---------|---------|---------|
| **E-01 Portal** | US-001, US-002 | US-003, US-004 | US-005, US-006 | | |
| **E-02 Processing** | | US-101, US-102 | US-103, US-104 | US-105, US-106 | |
| **E-03 Notifications** | | | US-201, US-202 | US-203, US-204 | |
| **E-04 Reporting** | | | | US-301, US-302 | US-303, US-304, US-305 |
| **E-05 Admin** | | US-401, US-402 | US-403, US-404 | | |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[SRS]] | User stories elaborate SRS requirements |
| [[Acceptance Criteria]] | ACs defined per user story |
| [[Requirements Traceability Matrix]] | Stories traced to objectives |
| [[Requirements Architecture]] | Stories organized by epic |
| [[Product Backlog]] | Stories managed in backlog |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC/IEEE 29148
> **Usage:** User stories are the *Agile format* for requirements. Each story must have acceptance criteria, story points, and priority. Stories are refined in backlog grooming sessions and delivered in sprints.
