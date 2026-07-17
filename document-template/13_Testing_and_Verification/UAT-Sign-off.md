---
document_type: UAT Sign-off
version: "1.0"
status: Draft
author: "[Business Analyst]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [uat, user-acceptance, sign-off, swebok]
standard_ref:
  - SWEBOK v4 — Testing
---

# UAT Sign-off

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Formal sign-off from business stakeholders confirming the system meets business requirements and is ready for production deployment.

## 2. UAT Summary

| Field | Detail |
|-------|--------|
| [UAT Period] | [YYYY-MM-DD to YYYY-MM-DD] |
| [UAT Environment] | [uat.project.com] |
| [Participants] | [Names + roles] |
| [Scenarios Tested] | [X of Y] |
| [Overall Result] | ✅ Pass / ⚠️ Conditional Pass / ❌ Fail |

## 3. UAT Scenarios

| # | Scenario | Business Owner | Result | Notes |
|---|---------|---------------|--------|-------|
| 1 | [Customer submits request] | [Sarah (Customer)] | ✅ Pass | [Works as expected] |
| 2 | [Operations processes request] | [James (Staff)] | ✅ Pass | [Efficient workflow] |
| 3 | [Manager views dashboard] | [Linda (Manager)] | ⚠️ Minor | [Date filter needs fix] |
| 4 | [Report generation] | [Linda (Manager)] | ✅ Pass | [Meets requirements] |
| 5 | [Mobile request submission] | [Sarah (Customer)] | ✅ Pass | [Mobile-friendly] |

## 4. Business Acceptance

| # | Requirement | Verified | Accepted | Notes |
|---|-------------|---------|---------|-------|
| 1 | [Online request submission] | ✅ | ✅ | [Meets BR-01] |
| 2 | [Auto-validation] | ✅ | ✅ | [Meets BR-02] |
| 3 | [Real-time status tracking] | ✅ | ✅ | [Meets BR-03] |
| 4 | [Email notifications] | ✅ | ✅ | [Meets BR-05] |
| 5 | [Dashboard reporting] | ⚠️ | ✅ | [Minor date filter issue] |

## 5. Outstanding Issues

| # | Issue | Severity | Workaround | Accepted |
|---|-------|---------|-----------|---------|
| 1 | [Date filter on dashboard] | 🟢 Low | [Manual date entry] | ✅ Accepted |

## 6. Formal Sign-off

> We, the undersigned, confirm that the system has been tested during UAT and meets the business requirements for production deployment.

| Role | Name | Decision | Signature | Date |
|------|------|---------|-----------|------|
| Business Owner | | ✅ Accept | | |
| Operations Manager | | ✅ Accept | | |
| IT Director | | ✅ Accept | | |
| Project Sponsor | | ✅ Accept | | |

## 7. Conditions

| # | Condition | Owner | Due Date |
|---|----------|-------|---------|
| 1 | [Fix date filter in next sprint] | [Dev Team] | [YYYY-MM-DD] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Test-Completion-Report]] | Test completion |
| [[Test-Report]] | Test results |
| [[Verified-Deliverables]] | Deliverable verification |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** UAT sign-off is *business approval*. Without it, don't deploy. Document any conditions — they become the next sprint's backlog.
