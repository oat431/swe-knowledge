---
document_type: Incident / Problem Reports
version: "1.0"
status: Active
author: "[Technical Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [incident-reports, problem-reports, root-cause, sebok]
standard_ref:
  - SEBoK v2 — Operations
  - ITIL — Problem Management
---

# Incident / Problem Reports

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Records incidents and problems — what happened, why, and how to prevent recurrence. Incidents are *what* happened; problems are *why*.

## 2. Incident vs Problem

| Aspect | Incident | Problem |
|--------|---------|---------|
| **Definition** | [Unplanned interruption] | [Root cause of incidents] |
| **Focus** | [Restore service] | [Find root cause] |
| **When** | [During incident] | [After incident resolved] |
| **Output** | [Workaround] | [Permanent fix] |

## 3. Incident Report Template

| Field | Value |
|-------|-------|
| **Incident ID** | [INC-XXX] |
| **Date** | [YYYY-MM-DD HH:MM] |
| **Severity** | [SEV-1 / SEV-2 / SEV-3 / SEV-4] |
| **Duration** | [X hours Y minutes] |
| **Impact** | [Users affected, services impacted] |
| **Status** | [Resolved / Under investigation] |

### Timeline

| Time | Event |
|------|-------|
| [HH:MM] | [Incident detected] |
| [HH:MM] | [On-call engineer acknowledged] |
| [HH:MM] | [Root cause identified] |
| [HH:MM] | [Fix implemented] |
| [HH:MM] | [Service restored] |
| [HH:MM] | [Incident closed] |

### Root Cause

> [Clear description of what caused the incident]

### Resolution

> [What was done to fix it]

### Prevention

| # | Action | Owner | Due Date | Status |
|---|--------|-------|---------|--------|
| 1 | [Prevention action 1] | [Name] | [Date] | ⬜ |
| 2 | [Prevention action 2] | [Name] | [Date] | ⬜ |

## 4. Problem Report Template

| Field | Value |
|-------|-------|
| **Problem ID** | [PRB-XXX] |
| **Related Incidents** | [INC-XXX, INC-YYY] |
| **Status** | [Open / Under Investigation / Known Error / Resolved] |
| **Root Cause** | [Description] |
| **Workaround** | [Temporary solution] |
| **Permanent Fix** | [Planned solution] |
| **Fix Version** | [vX.Y.Z] |

## 5. Incident Register

| ID | Date | Severity | Duration | Impact | Root Cause | Status |
|----|------|---------|---------|--------|-----------|--------|
| [INC-001] | [YYYY-MM-DD] | [SEV-2] | [45 min] | [Cannot submit requests] | [Database connection pool] | ✅ Resolved |
| [INC-002] | [YYYY-MM-DD] | [SEV-3] | [2 hours] | [Slow dashboard] | [Missing index] | ✅ Resolved |
| [INC-003] | [YYYY-MM-DD] | [SEV-1] | [15 min] | [Site down] | [Deployment error] | ✅ Resolved |

## 6. Problem Register

| ID | Related Incidents | Root Cause | Workaround | Permanent Fix | Status |
|----|------------------|-----------|-----------|--------------|--------|
| [PRB-001] | [INC-001] | [Connection pool too small] | [Restart pods] | [Increase pool size] | ✅ Resolved |
| [PRB-002] | [INC-002] | [Missing database index] | [Manual query] | [Add index] | ✅ Resolved |
| [PRB-003] | [INC-003] | [Deployment script error] | [Manual rollback] | [Fix script] | ✅ Resolved |

## 7. Metrics

| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| [Incidents this month] | [X] | [< 5] | 🟢🟡🔴 |
| [MTTR] | [X min] | [< 60 min] | 🟢🟡🔴 |
| [Incidents by severity] | [SEV-1: X, SEV-2: X, SEV-3: X] | — | — |
| [Repeat incidents] | [X] | [0] | 🟢🟡🔴 |
| [Open problems] | [X] | [< 5] | 🟢🟡🔴 |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Incident-Management-Process]] | Incident handling process |
| [[Maintenance-Log-Change-History]] | Change history |
| [[Defect-Report]] | Software defects |

---

> **Template Standard:** Based on SEBoK v2, ITIL
> **Usage:** Every incident gets a report. Every recurring incident gets a problem investigation. Fix the root cause, not the symptom.
