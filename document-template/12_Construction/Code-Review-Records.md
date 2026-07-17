---
document_type: Code Review Records
version: "1.0"
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

> Records of code reviews — findings, patterns, and metrics that improve code quality over time.

## 2. Code Review Process

```mermaid
flowchart TD
    PR[Pull Request<br>Created] --> AUTO[Automated Checks]
    AUTO --> CHECK{Pass?}
    CHECK -->|No| FIX_AUTO[Fix Issues]
    FIX_AUTO --> PR
    CHECK -->|Yes| REVIEW[Peer Review]
    REVIEW --> FINDINGS{Findings?}
    FINDINGS -->|Yes| FIX[Author Fixes]
    FIX --> REVIEW
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
| [PR Size] | [< 400 lines changed] |
| [Reviewers] | [Minimum 1 approval] |
| [Response Time] | [< 24 hours] |
| [Tests Required] | [Yes — for all changes] |
| [Documentation] | [Required for public APIs] |

## 4. Review Checklist

| # | Check | Category |
|---|-------|---------|
| 1 | [Code follows [[Coding-Standards]]] | Style |
| 2 | [No hardcoded secrets or credentials] | Security |
| 3 | [Error handling is comprehensive] | Reliability |
| 4 | [Tests cover happy path + edge cases] | Testing |
| 5 | [No unnecessary complexity] | Maintainability |
| 6 | [Performance implications considered] | Performance |
| 7 | [Security implications considered] | Security |
| 8 | [Documentation updated if needed] | Documentation |

## 5. Review Metrics

| Metric | Target | Current | Status |
|--------|--------|---------|--------|
| [PRs reviewed per week] | [> 10] | [X] | 🟢🟡🔴 |
| [Avg review time] | [< 24h] | [Xh] | 🟢🟡🔴 |
| [Findings per PR] | [< 5] | [X] | 🟢🟡🔴 |
| [Critical findings] | [0] | [X] | 🟢🟡🔴 |
| [Review coverage] | [100%] | [X%] | 🟢🟡🔴 |

## 6. Common Findings

| Finding | Frequency | Prevention |
|---------|----------|-----------|
| [Missing error handling] | [High] | [Checklist item #3] |
| [No tests for edge cases] | [Medium] | [Checklist item #4] |
| [Hardcoded values] | [Medium] | [Extract to config] |
| [Inconsistent naming] | [Low] | [[[Coding-Standards]]] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Coding-Standards]] | Standards being enforced |
| [[Design-Review-Records]] | Design-level reviews |
| [[README-Developer-Guide]] | Contribution guidelines |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC 20246
> **Usage:** Code review is *quality gate #1*. Every PR gets reviewed. Track metrics to improve the process.
