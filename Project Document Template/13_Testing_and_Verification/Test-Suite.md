---
document_type: Test Suite
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
    ROOT[Test Suites] --> SMOKE[Smoke Suite<br>10 tests]
    ROOT --> CRITICAL[Critical Path<br>25 tests]
    ROOT --> REGRESSION[Regression Suite<br>73 tests]
    ROOT --> MODULE[Module Suites]
    
    MODULE --> M1[Request Module<br>25 tests]
    MODULE --> M2[Processing Module<br>18 tests]
    MODULE --> M3[Auth Module<br>12 tests]
    MODULE --> M4[Notification Module<br>8 tests]
    MODULE --> M5[Reporting Module<br>10 tests]

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
| **Purpose** | [Verify critical paths after deployment] |
| **Size** | [10 tests] |
| **Execution Time** | [< 5 minutes] |
| **Frequency** | [Every deployment] |
| **Automation** | [100%] |

| # | Test Case | Module | Priority |
|---|----------|--------|---------|
| 1 | [TC-001: Submit valid request] | [Request] | 🔴 |
| 2 | [TC-003: Auto-approve eligible] | [Processing] | 🔴 |
| 3 | [TC-010: Login success] | [Auth] | 🔴 |
| 4 | [TC-015: View request detail] | [Request] | 🔴 |
| 5 | [TC-020: Approve request] | [Processing] | 🔴 |

### 3.2 Critical Path Suite

| Field | Detail |
|-------|--------|
| **Purpose** | [Verify end-to-end business flows] |
| **Size** | [25 tests] |
| **Execution Time** | [< 30 minutes] |
| **Frequency** | [Every PR to main] |
| **Automation** | [100%] |

### 3.3 Regression Suite

| Field | Detail |
|-------|--------|
| **Purpose** | [Verify no regressions in existing functionality] |
| **Size** | [73 tests] |
| **Execution Time** | [< 2 hours] |
| **Frequency** | [Nightly, pre-release] |
| **Automation** | [80%] |

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
| [Smoke] | [10] | [10] | [0] | [0] | [100%] |
| [Critical Path] | [25] | [24] | [1] | [0] | [96%] |
| [Regression] | [73] | [69] | [4] | [0] | [95%] |

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
