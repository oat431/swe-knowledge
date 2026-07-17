---
document_type: Usability Test Plan
version: "1.0"
status: Draft
author: "[UX Researcher]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [usability-testing, ux-research, iso-9241]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
  - ISO 20282 — Usability of Consumer Products
---

# Usability Test Plan

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This plan defines how usability testing will be conducted — objectives, participants, tasks, metrics, and logistics.

## 2. Test Overview

| Field | Detail |
|-------|--------|
| [Test Type] | [Moderated, think-aloud] |
| [Format] | [Remote (video call) + In-person] |
| [Duration] | [45-60 minutes per session] |
| [Participants] | [5-8 per persona (15-24 total)] |
| [Prototype] | [Figma interactive prototype] |
| [Test Dates] | [YYYY-MM-DD to YYYY-MM-DD] |

## 3. Research Objectives

| # | Objective | Metric | Target |
|---|----------|--------|--------|
| 1 | [Can users submit a request without help?] | [Success rate] | [≥ 90%] |
| 2 | [Can users find their request status?] | [Success rate] | [≥ 95%] |
| 3 | [Can staff process requests efficiently?] | [Time on task] | [< 3 min] |
| 4 | [Is the system easy to learn?] | [SUS score] | [≥ 68] |
| 5 | [Are error messages helpful?] | [Error recovery rate] | [≥ 80%] |

## 4. Participant Recruitment

| Segment | Count | Criteria | Source |
|---------|-------|---------|-------|
| [Customers] | [8] | [Submit requests regularly, mixed tech savviness] | [User panel, social media] |
| [Operations Staff] | [4] | [Process requests daily, 1+ years experience] | [Internal team] |
| [Managers] | [3] | [Monitor performance, generate reports] | [Internal team] |

### Screener Questions

| # | Question | Criteria |
|---|---------|---------|
| 1 | [How often do you submit requests?] | [At least monthly] |
| 2 | [What device do you primarily use?] | [Mix of mobile + desktop] |
| 3 | [How would you rate your tech comfort?] | [Mix of levels] |

## 5. Test Tasks

### Task 1: Submit a New Request

| Field | Detail |
|-------|--------|
| [Scenario] | [You need to submit a new request for $5,000 for equipment purchase.] |
| [Start Screen] | [Customer Dashboard] |
| [Success Criteria] | [Complete submission and see confirmation] |
| [Time Limit] | [10 minutes] |
| [Observations] | [Navigation, form completion, error handling] |

### Task 2: Check Request Status

| Field | Detail |
|-------|--------|
| [Scenario] | [You submitted a request yesterday. Check its current status.] |
| [Start Screen] | [Customer Dashboard] |
| [Success Criteria] | [Find and view current status] |
| [Time Limit] | [3 minutes] |
| [Observations] | [Navigation, information findability] |

### Task 3: Process a Request (Staff)

| Field | Detail |
|-------|--------|
| [Scenario] | [A new request has arrived in your queue. Review and approve it.] |
| [Start Screen] | [Admin Dashboard] |
| [Success Criteria] | [Approve request and add comment] |
| [Time Limit] | [5 minutes] |
| [Observations] | [Queue navigation, review process, action completion] |

### Task 4: Find a Specific Request (Staff)

| Field | Detail |
|-------|--------|
| [Scenario] | [A customer called about request REQ-2026-042. Find it.] |
| [Start Screen] | [Admin Dashboard] |
| [Success Criteria] | [Find request by ID] |
| [Time Limit] | [2 minutes] |
| [Observations] | [Search functionality, filter usage] |

### Task 5: Generate a Report (Manager)

| Field | Detail |
|-------|--------|
| [Scenario] | [Generate a report showing this week's processing metrics.] |
| [Start Screen] | [Manager Dashboard] |
| [Success Criteria] | [View and export report] |
| [Time Limit] | [3 minutes] |
| [Observations] | [Dashboard navigation, export process] |

## 6. Metrics

| Metric | Measurement | Target |
|--------|-----------|--------|
| [Task Success Rate] | [Binary: complete/incomplete] | [≥ 90%] |
| [Time on Task] | [Seconds from start to completion] | [Within time limit] |
| [Error Rate] | [Number of errors per task] | [< 2 per task] |
| [Error Recovery] | [Self-recovered / needed help] | [≥ 80% self-recovered] |
| [SUS Score] | [System Usability Scale questionnaire] | [≥ 68 (above average)] |
| [Satisfaction] | [Post-task satisfaction (1-5)] | [≥ 4.0] |

## 7. Test Script

### Introduction (5 min)

> "Thank you for participating. Today we're testing a new system design — we're testing the design, not you. There are no wrong answers.

> I'll give you tasks to complete. Please think aloud — tell me what you're looking for, what you expect, and what's confusing.

> The session will be recorded for analysis. Do you have any questions?"

### Tasks (30-40 min)

> [Read each task scenario. Observe. Don't help unless stuck for > 2 minutes.]

### Post-Test Questions (10 min)

| # | Question | Scale |
|---|---------|-------|
| 1 | [How easy was the system to use?] | [1-5] |
| 2 | [How easy was it to find what you needed?] | [1-5] |
| 3 | [How confident were you in your actions?] | [1-5] |
| 4 | [What was the most frustrating part?] | [Open] |
| 5 | [What would you improve?] | [Open] |

### SUS Questionnaire (5 min)

| # | Statement | Strongly Disagree → Strongly Agree |
|---|----------|-----------------------------------|
| 1 | [I think I would like to use this system frequently] | [1 2 3 4 5] |
| 2 | [I found the system unnecessarily complex] | [1 2 3 4 5] |
| 3 | [I thought the system was easy to use] | [1 2 3 4 5] |
| 4 | [I think I would need technical support] | [1 2 3 4 5] |
| 5 | [I found the functions well integrated] | [1 2 3 4 5] |

## 8. Logistics

| Aspect | Detail |
|--------|--------|
| [Location] | [Conference room / Zoom] |
| [Equipment] | [Laptop, screen recorder, camera] |
| [Recording] | [Screen + audio + facial expressions] |
| [Facilitator] | [UX Researcher] |
| [Observer] | [UX Designer, Product Manager] |
| [Consent] | [Written consent form] |
| [Incentive] | [$50 gift card per participant] |

## 9. Analysis & Reporting

| Aspect | Timeline |
|--------|---------|
| [Analysis] | [Within 3 days of last session] |
| [Report] | [Within 1 week of last session] |
| [Prioritization] | [With product team] |
| [Design Iteration] | [Within 2 weeks] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Interactive-Prototype]] | Prototype being tested |
| [[User-Personas]] | Participants matching personas |
| [[Accessibility-Audit]] | Accessibility testing complement |

---

> **Template Standard:** Based on ISO 9241-210, ISO 20282
> **Usage:** Test with 5-8 participants per segment — you'll find 80% of usability issues. Don't wait for perfect designs — test early, iterate fast.
