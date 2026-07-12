---
document_type: Regression Test Suite
version: "1.0"
status: Active
author: "[QA Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [regression-testing, test-suite, swebok]
standard_ref:
  - SWEBOK v4 — Testing
---

# Regression Test Suite

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Regression testing verifies that new changes don't break existing functionality.

## 2. Regression Strategy

| Scope | When | Tests | Duration |
|-------|------|-------|---------|
| [Smoke] | [Every deployment] | [10] | [< 5 min] |
| [Targeted] | [Affected modules] | [Variable] | [< 30 min] |
| [Full Regression] | [Pre-release] | [73] | [< 2 hours] |

## 3. Regression Suite Composition

| Category | Tests | Automation | Priority |
|---------|-------|-----------|---------|
| [Critical Paths] | [10] | [100%] | 🔴 Always run |
| [Core Features] | [25] | [100%] | 🔴 Always run |
| [Edge Cases] | [20] | [80%] | 🟡 Pre-release |
| [UI/UX] | [18] | [60%] | 🟡 Pre-release |
| **Total** | **[73]** | **[80%]** | |

## 4. Regression Triggers

| Trigger | Suite | Automation |
|---------|-------|-----------|
| [PR to main] | [Critical Path] | [GitHub Actions] |
| [Merge to develop] | [Affected modules] | [GitHub Actions] |
| [Nightly] | [Full Regression] | [Scheduled] |
| [Pre-release] | [Full Regression] | [Manual trigger] |
| [Hotfix] | [Smoke + Critical Path] | [GitHub Actions] |

## 5. Regression Execution

```mermaid
flowchart TD
    CHANGE[Code Change] --> DETECT[Detect Affected<br>Modules]
    DETECT --> SELECT[Select Regression<br>Scope]
    SELECT --> EXEC[Execute Tests]
    EXEC --> PASS{All Pass?}
    PASS -->|Yes| MERGE[Allow Merge]
    PASS -->|No| BLOCK[Block Merge]
    BLOCK --> FIX[Fix Defects]
    FIX --> EXEC

    style CHANGE fill:#2196F3,color:#fff
    style MERGE fill:#4CAF50,color:#fff
    style BLOCK fill:#f44336,color:#fff
```

## 6. Regression Metrics

| Metric | Target | Current | Status |
|--------|--------|---------|--------|
| [Regression pass rate] | [≥ 95%] | [X%] | 🟢🟡🔴 |
| [Regression execution time] | [< 2 hours] | [X min] | 🟢🟡🔴 |
| [Regression coverage] | [100% critical paths] | [X%] | 🟢🟡🔴 |
| [Defects caught by regression] | [> 50%] | [X%] | 🟢🟡🔴 |

## 7. Flaky Test Management

| Test | Issue | Frequency | Action |
|------|-------|----------|--------|
| [TC-XXX] | [Timing issue] | [10%] | [Quarantine + fix] |
| [TC-YYY] | [External dependency] | [5%] | [Mock dependency] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Test-Suite]] | Suite organization |
| [[Test-Scripts-Automated]] | Automation scripts |
| [[Build-Scripts]] | CI/CD integration |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Regression testing is *insurance*. Run it often. Fix flaky tests immediately — they hide real failures.
