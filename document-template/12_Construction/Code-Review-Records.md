---
document_type: Code Review Records
version: "2.0"
status: Active
author: "[Technical Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [code-review, pull-request, swebok, iso-20246]
standard_ref:
  - SWEBOK v4 — Construction
  - ISO/IEC 20246 — Work Product Reviews
---

# Code Review Records

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Records of code reviews — findings, patterns, and metrics that improve code quality over time. Code review is **quality gate #1**. Every PR gets reviewed. Track metrics to improve the process.

## 2. Code Review Process

```mermaid
flowchart TD
    PR[Pull Request Created] --> AUTO[Automated Checks]
    AUTO --> CHECK{All Pass?}
    CHECK -->|No| FIX_AUTO[Fix: lint, test, build]
    FIX_AUTO --> PR
    CHECK -->|Yes| REVIEW[Peer Review]
    REVIEW --> FINDINGS{Findings?}
    FINDINGS -->|Yes| DISCUSS[Discuss & Fix]
    DISCUSS --> REVIEW
    FINDINGS -->|No| APPROVE[Approve]
    APPROVE --> MERGE[Merge]

    style PR fill:#2196F3,color:#fff
    style AUTO fill:#FF9800,color:#fff
    style REVIEW fill:#9C27B0,color:#fff
    style APPROVE fill:#4CAF50,color:#fff
    style MERGE fill:#4CAF50,color:#fff
```

## 3. Review Standards

| Aspect | Standard |
|--------|---------|
| PR Size | < 400 lines changed (smaller = faster, better review) |
| Reviewers | Minimum 1 approval |
| Response Time | < 24 hours |
| Tests Required | For all feature/fix changes |
| Documentation Required | For config changes and public API changes |
| Automated Checks | Must pass: lint, test, build |
| Commit Hygiene | Squash-merge to main, clean messages per [[Commit-Messages-Changelog]] |

## 4. Review Checklist (Stack-Agnostic)

| # | Check | Category |
|---|-------|---------|
| 1 | Code follows [[Coding-Standards]] for the target language | Style |
| 2 | No hardcoded secrets, credentials, or tokens | Security |
| 3 | Error handling is comprehensive — not swallowing exceptions | Reliability |
| 4 | Tests cover happy path + edge cases + error paths | Testing |
| 5 | No dead code, commented-out blocks, or TODO without ticket reference | Maintainability |
| 6 | Performance implications considered (N+1 queries, large allocations) | Performance |
| 7 | API changes are reflected in the API spec (if applicable) | Documentation |
| 8 | Configuration changes are documented with rationale | Documentation |
| 9 | No breaking changes without `BREAKING CHANGE:` footer or version bump | Compatibility |
| 10 | Dependencies are pinned and audited (no `*` or `latest` versions) | Security |

## 5. Review Metrics

| Metric | Target | Current |
|--------|--------|---------|
| PRs reviewed per sprint | > 5 | [X] |
| Avg time to first review | < 24h | [X]h |
| Findings per PR | < 5 | [X] |
| Critical/blocking findings | 0 | [X] |
| Review coverage (% of PRs reviewed) | 100% | [X]% |
| Rework rate (PRs needing > 2 rounds) | < 20% | [X]% |

## 6. Severity Legend

| Level | Meaning | Example |
|:---:|---------|---------|
| 🔴 | **Critical** — blocks merge | Security vulnerability, data loss risk, breaking change without notice |
| 🟡 | **Important** — should fix before merge | Missing tests, unclear error handling, missing docs |
| 🟢 | **Nit** — nice to have, non-blocking | Variable naming preference, minor style, optional refactor |

## 7. Common Findings Across Projects

| Finding | Frequency | Prevention |
|---------|:---:|-----------|
| Missing error handling | High | Checklist #3 |
| No tests for edge cases | Medium | Checklist #4 |
| Hardcoded configuration values | Medium | Extract to env vars / config files |
| Inconsistent naming with standards | Low | Checklist #1 + linter |
| Missing documentation for new API endpoints | Medium | Checklist #7 |
| Large PRs (> 500 lines) | Medium | Encourage smaller, incremental PRs |

## 8. Review Record Template

Capture significant reviews for traceability:

```markdown
### Review #001 — [Feature / Fix Summary]

| Field | Detail |
|-------|--------|
| **PR** | [#N](link) |
| **Author** | [Name] |
| **Reviewer** | [Name] |
| **Date** | [YYYY-MM-DD] |
| **Service** | [service-name] |
| **Type** | [feat / fix / refactor / chore] |
| **Lines Changed** | [+XX / -YY] |

**Findings:**

| # | Severity | Category | Description | Resolution |
|---|:---:|---------|-------------|-----------|
| 1 | 🔴 | Security | Hardcoded JWT secret in config | Extracted to env var |
| 2 | 🟡 | Testing | Missing test for empty-registry edge case | Added test |
| 3 | 🟢 | Style | Variable name too short | Renamed |

**Outcome:** [✅ Approved / 🔄 Changes Requested]

**Lessons Learned:** [One takeaway to prevent recurrence across the team]
```

## 9. Example — Real Review Record

From a real Eureka Server implementation:

```markdown
### Review #001 — Initial Implementation

| Field | Detail |
|-------|--------|
| **PR** | N/A (direct push, solo developer) |
| **Author** | Dev Persona |
| **Reviewer** | User |
| **Date** | 2026-07-23 |
| **Service** | flowero-discover |
| **Type** | feat — initial Eureka server |
| **Lines Changed** | ~12 files created |

**Findings:**

| # | Severity | Category | Description | Resolution |
|---|:---:|---------|-------------|-----------|
| 1 | 🟢 | Style | TestRestTemplate not available in Boot 4.1 — switched to plain RestTemplate | Fixed in same session |
| 2 | 🟢 | Testing | Self-registration test checked wrong app name | Fixed in same session |

**Outcome:** ✅ Approved

**Lessons Learned:**
- Spring Boot 4.1's TestRestTemplate may need spring-boot-starter-web in test scope. Plain RestTemplate works with Eureka Server's embedded Tomcat.
- Eureka uppercases spring.application.name: "flowero-discover" → "FLOWERO-DISCOVER"
```

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Coding-Standards]] | Standards being enforced |
| [[Commit-Messages-Changelog]] | Commit format for PRs |
| [[README-Developer-Guide]] | Developer onboarding |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC 20246
> **Usage:** Code review is **quality gate #1**. Every PR gets reviewed. Use the stack-agnostic checklist — it works for Java, TypeScript, and Go equally. The review record template captures significant decisions for traceability. Track metrics to spot systemic issues (e.g., "we keep missing error handling" → add to linter rules).
