---
document_type: Definition of Done (DoD)
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
po_owner: "[Product Owner]"
scrum_master: "[Scrum Master]"
classification: "Internal / Confidential"
tags: [dod, definition-of-done, quality-gate, scrum, agile, swebok]
standard_ref:
  - SWEBOK v4 — Quality Assurance
  - Scrum Guide (Schwaber & Sutherland, 2020)
  - Agile Practice Guide (PMI, 2017)
---

# Definition of Done (DoD)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | Baselined]
> **Last Updated:** [YYYY-MM-DD]

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Product Owner | [Name / Role] |
| Tech Lead | [Name / Role] |

### Approvals

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Product Owner | | | |
| Tech Lead | | | |
| QA Lead | | | |

---

## 1. Purpose

> This document defines the "Definition of Done" (DoD) for the project. The DoD is a shared understanding of what it means for work to be complete. It ensures transparency, quality, and consistency across all deliverables.

---

## 2. Definition of Done — User Story

A user story is **Done** when:

| # | Criteria | ✓ |
|---|----------|---|
| 1 | Code is written and follows [[Coding-Standards]] | ☐ |
| 2 | Unit tests written and passing (≥80% coverage) | ☐ |
| 3 | Integration tests passing | ☐ |
| 4 | Code reviewed and approved by ≥1 peer | ☐ |
| 5 | No critical or high-severity defects | ☐ |
| 6 | API specification updated (if applicable) | ☐ |
| 7 | Database schema changes documented (if applicable) | ☐ |
| 8 | README updated (if setup/usage changed) | ☐ |
| 9 | Acceptance criteria verified | ☐ |
| 10 | Product Owner accepted | ☐ |

---

## 3. Definition of Done — Sprint

A sprint is **Done** when:

| # | Criteria | ✓ |
|---|----------|---|
| 1 | All selected stories meet DoD | ☐ |
| 2 | Sprint backlog items completed or re-prioritized | ☐ |
| 3 | Regression tests passing | ☐ |
| 4 | No blocking defects in main branch | ☐ |
| 5 | Sprint review completed with stakeholders | ☐ |
| 6 | Sprint retrospective completed | ☐ |
| 7 | Release notes updated (if releasing) | ☐ |

---

## 4. Definition of Done — Release

A release is **Done** when:

| # | Criteria | ✓ |
|---|----------|---|
| 1 | All stories in release scope meet DoD | ☐ |
| 2 | Full regression test suite passing | ☐ |
| 3 | Performance tests passing (if applicable) | ☐ |
| 4 | Security tests passing | ☐ |
| 5 | Release notes finalized | ☐ |
| 6 | Deployment plan reviewed and approved | ☐ |
| 7 | CI/CD pipeline green | ☐ |
| 8 | Database migrations tested | ☐ |
| 9 | Rollback plan documented | ☐ |
| 10 | Stakeholders notified | ☐ |

---

## 5. Definition of Done — Bug Fix

A bug fix is **Done** when:

| # | Criteria | ✓ |
|---|----------|---|
| 1 | Root cause identified and documented | ☐ |
| 2 | Fix implemented and code reviewed | ☐ |
| 3 | Unit test added to prevent regression | ☐ |
| 4 | All existing tests passing | ☐ |
| 5 | Verified in staging environment | ☐ |
| 6 | Defect report updated with resolution | ☐ |

---

## 6. Quality Gates

### Code Quality

| Metric | Threshold | Tool |
|--------|-----------|------|
| Unit test coverage | ≥80% | [Jest / Pytest / JUnit] |
| Code review approval | ≥1 reviewer | [GitHub / GitLab] |
| Linting errors | 0 | [ESLint / Pylint] |
| Critical vulnerabilities | 0 | [Snyk / Dependabot] |

### Performance

| Metric | Threshold | Tool |
|--------|-----------|------|
| API response time | <500ms (p95) | [k6 / Artillery] |
| Page load time | <3s | [Lighthouse] |
| Error rate | <1% | [Sentry / Datadog] |

---

## 7. Exceptions

> Exceptions to the DoD require explicit approval from the Product Owner and Tech Lead. All exceptions must be documented with:
> - Reason for exception
> - Risk assessment
> - Remediation plan
> - Target date for completion

| Date | Story/Release | Exception | Approved By | Remediation Date |
|------|---------------|-----------|-------------|------------------|
| [YYYY-MM-DD] | [Story #] | [Description] | [Name] | [YYYY-MM-DD] |

---

## 8. Evolution

> The DoD should be reviewed and updated at each sprint retrospective. As the team matures, the DoD should become more rigorous.

| Version | Date | Changes | Approved By |
|---------|------|---------|-------------|
| 1.0 | [YYYY-MM-DD] | Initial DoD | [Name] |
| 1.1 | [YYYY-MM-DD] | [Description] | [Name] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Acceptance-Criteria]] | Per-story acceptance criteria |
| [[Test-Plan]] | Testing strategy |
| [[Coding-Standards]] | Code quality standards |
| [[Test-Cases]] | Test implementation |

---

> **Template Standard:** Based on Scrum Guide and Agile Practice Guide
> **Usage:** The DoD is a living document. Review and update it regularly as the team evolves.
