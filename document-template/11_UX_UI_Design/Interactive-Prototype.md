---
document_type: Interactive Prototype
version: "1.0"
status: Draft
author: "[UX Designer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [prototype, interactive, ux, usability-testing]
standard_ref:
  - ISO 9241-210 — Human-Centred Design
---

# Interactive Prototype

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]
>
> ⚠️ **Note:** Interactive prototypes are built in Figma/Adobe XD. This document *specifies* the prototype — the actual prototype lives in the design tool.

## 1. Purpose

> Interactive prototypes simulate the final product — allowing usability testing before development begins.

## 2. Prototype Specification

| Field | Detail |
|-------|--------|
| [Tool] | [Figma / Adobe XD] |
| [Prototype URL] | [https://figma.com/proto/xxx] |
| [Version] | [v1.0] |
| [Fidelity] | [High — realistic content, interactions] |
| [Tested With] | [X participants] |

## 3. Prototype Screens

| # | Screen | Interactions | Linked To |
|---|--------|------------|----------|
| 1 | [Login] | [Enter credentials → Submit → Dashboard] | [Screen 2] |
| 2 | [Customer Dashboard] | [Click New Request → Form] | [Screen 3] |
| 3 | [Request Form — Step 1] | [Fill fields → Next → Step 2] | [Screen 4] |
| 4 | [Request Form — Step 2] | [Fill fields → Next → Step 3] | [Screen 5] |
| 5 | [Request Form — Step 3] | [Upload docs → Next → Review] | [Screen 6] |
| 6 | [Request Review] | [Edit → Submit → Confirmation] | [Screen 7] |
| 7 | [Confirmation] | [View Status → Status Page] | [Screen 8] |
| 8 | [Status Page] | [View timeline → Back to Dashboard] | [Screen 2] |

## 4. Interaction Specifications

| Interaction | Trigger | Response | Animation |
|------------|---------|---------|-----------|
| [Form Navigation] | [Click Next/Back] | [Slide to next/prev step] | [Slide left/right, 300ms] |
| [Form Validation] | [Blur / Submit] | [Show error messages] | [Shake field, 200ms] |
| [Document Upload] | [Drag & drop / Click] | [Show upload progress] | [Progress bar, 1s] |
| [Status Update] | [Click status badge] | [Expand timeline] | [Expand down, 300ms] |
| [Confirmation] | [Submit request] | [Show success page] | [Fade in, 500ms] |
| [Navigation] | [Click nav item] | [Navigate to page] | [Instant transition] |

## 5. Prototype Flows for Testing

| Flow | Steps | Success Criteria | Time Estimate |
|------|-------|-----------------|--------------|
| [Submit Request] | [Login → Form → Upload → Submit] | [Complete without errors] | [5-10 min] |
| [Track Status] | [Login → My Requests → Detail → Status] | [Find current status] | [2-3 min] |
| [Process Request] | [Login → Queue → Review → Approve] | [Complete approval] | [3-5 min] |

## 6. Usability Test Plan (for Prototype)

| Aspect | Detail |
|--------|--------|
| [Participants] | [5-8 per persona] |
| [Tasks] | [3-5 key tasks from flows above] |
| [Metrics] | [Success rate, time on task, error rate, satisfaction] |
| [Method] | [Moderated think-aloud, remote or in-person] |
| [Recording] | [Screen + audio recording] |

## 7. Prototype Limitations

| Limitation | Workaround |
|-----------|-----------|
| [No real data] | [Use realistic sample data] |
| [No real backend] | [Simulate responses with hotspots] |
| [No real auth] | [Skip login or use dummy flow] |
| [No real file upload] | [Simulate with placeholder] |
| [Performance not realistic] | [Note in test script] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Wireframes-Mid-fi]] | Wireframes the prototype implements |
| [[UI-Mockups]] | Visual design in prototype |
| [[Usability-Test-Plan]] | Testing the prototype |

---

> **Template Standard:** Based on ISO 9241-210
> **Usage:** Prototypes are for *testing* — not for showing stakeholders (use mockups for that). Test early, test often, iterate fast.
