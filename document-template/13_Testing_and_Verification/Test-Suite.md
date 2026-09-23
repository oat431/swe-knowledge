---
document_type: Test Suite
schema_version: 2
canonical_name: Test Suite
doc_form: light
applicability: universal
min_project_tier: 4  # Small-Prod
minimum_form: Lightweight markdown doc (frontmatter + required sections only)
version: "1.0"
status: Active
author: "[QA Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [test-suite, test-organization, swebok]
standard_ref:
  - SWEBOK v4 — Testing
---

# Test Suite

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Organizes test cases into logical suites — by module, type, or execution priority.

## 2. Suite Structure

```mermaid
flowchart TD
    ROOT[Test Suites] --> SMOKE["Smoke Suite<br>[N] tests"]
    ROOT --> CRITICAL["Critical Path<br>[N] tests"]
    ROOT --> REGRESSION["Regression Suite<br>[N] tests"]
    ROOT --> MODULE[Module Suites]
    
    MODULE --> M1["[Module] Suite<br>[N] tests"]
    MODULE --> M2["[Module] Suite<br>[N] tests"]
    MODULE --> M3["[Module] Suite<br>[N] tests"]

    style ROOT fill:#1a237e,color:#fff
    style SMOKE fill:#f44336,color:#fff
    style CRITICAL fill:#FF9800,color:#fff
    style REGRESSION fill:#2196F3,color:#fff
    style MODULE fill:#4CAF50,color:#fff
```

## 3. Suite Definitions

### 3.1 Smoke Suite

| Field | Detail |
|-------|--------|
| **Purpose** | [Purpose] |
| **Size** | [N tests] |
| **Execution Time** | [Time] |
| **Frequency** | [Frequency] |
| **Automation** | [X]% |

| # | Test Case | Module | Priority |
|---|----------|--------|---------|
| 1 | [TC-XXX: Test case title] | [Module Name] | [Priority] |

### 3.2 Critical Path Suite

| Field | Detail |
|-------|--------|
| **Purpose** | [Purpose] |
| **Size** | [N tests] |
| **Execution Time** | [Time] |
| **Frequency** | [Frequency] |
| **Automation** | [X]% |

### 3.3 Regression Suite

| Field | Detail |
|-------|--------|
| **Purpose** | [Purpose] |
| **Size** | [N tests] |
| **Execution Time** | [Time] |
| **Frequency** | [Frequency] |
| **Automation** | [X]% |

## 4. Suite Execution Matrix

| Suite | Commit | PR | Nightly | Pre-Release | Post-Deploy |
|-------|--------|-----|---------|------------|------------|
| [Smoke] | ❌ | ❌ | ❌ | ✅ | ✅ |
| [Critical Path] | ❌ | ✅ | ✅ | ✅ | ✅ |
| [Regression] | ❌ | ❌ | ✅ | ✅ | ❌ |
| [Module] | ✅ (changed) | ✅ | ✅ | ✅ | ❌ |

## 5. Suite Metrics

| Suite | Tests | Pass | Fail | Skip | Pass Rate |
|-------|-------|------|------|------|----------|
| [Smoke] | [N] | [N] | [N] | [N] | [X]% |
| [Critical Path] | [N] | [N] | [N] | [N] | [X]% |
| [Regression] | [N] | [N] | [N] | [N] | [X]% |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Test-Cases]] | Cases organized into suites |
| [[Test-Scripts-Automated]] | Automated execution |
| [[Regression-Test-Suite]] | Regression-specific suite |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Suites are *execution units*. Run the right suite at the right time — smoke for deploys, regression for releases.
