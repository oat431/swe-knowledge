---
document_type: Digital Forensics Report
version: "1.0"
status: Draft
author: "[Security Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Confidential"
tags: [digital-forensics, incident-investigation, cyberok]
standard_ref:
  - CyBOK v1 — Digital Forensics
---

# Digital Forensics Report

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Not Applicable]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Post-incident forensic analysis and evidence preservation — understanding what happened, how, and what was affected.

## 2. Forensic Readiness

| Aspect | Status | Implementation |
|--------|--------|---------------|
| [Logging enabled] | ✅ | [All actions logged, centralized] |
| [Log retention] | ✅ | [1 year for security logs] |
| [Evidence preservation] | ✅ | [Secure storage, chain of custody] |
| [Forensic tools available] | ✅ | [Volatility, Autopsy, Wireshark] |
| [Trained personnel] | ✅ | [Security team trained] |

## 3. Forensic Report Template

### FOR-XXX: [Incident Title]

| Field | Detail |
|-------|--------|
| **Report ID** | [FOR-XXX] |
| **Related Incident** | [INC-XXX] |
| **Analyst** | [Name] |
| **Date** | [YYYY-MM-DD] |
| **Scope** | [Systems examined] |

### Timeline of Events

| Timestamp | Event | Source | Evidence |
|-----------|-------|--------|---------|
| [YYYY-MM-DD HH:MM] | [Event description] | [Log source] | [Evidence ID] |
| [YYYY-MM-DD HH:MM] | [Event description] | [Log source] | [Evidence ID] |

### Evidence Collected

| ID | Type | Source | Collection Date | Hash | Chain of Custody |
|----|------|--------|----------------|------|-----------------|
| [E-001] | [System logs] | [SIEM] | [YYYY-MM-DD] | [SHA256: abc123] | [Documented] |
| [E-002] | [Network capture] | [VPC Flow Logs] | [YYYY-MM-DD] | [SHA256: def456] | [Documented] |
| [E-003] | [Memory dump] | [Affected server] | [YYYY-MM-DD] | [SHA256: ghi789] | [Documented] |

### Findings

| # | Finding | Evidence | Impact |
|---|--------|---------|--------|
| 1 | [Finding description] | [Evidence ID] | [Impact] |
| 2 | [Finding description] | [Evidence ID] | [Impact] |

### Root Cause

> [What caused the incident based on forensic analysis]

### Recommendations

| # | Recommendation | Priority |
|---|---------------|---------|
| 1 | [Recommendation] | 🔴🟡🟢 |

## 4. Chain of Custody Log

| Evidence ID | Action | By | Date | Location |
|------------|--------|-----|------|---------|
| [E-001] | [Collected] | [Name] | [YYYY-MM-DD] | [Secure storage] |
| [E-001] | [Analyzed] | [Name] | [YYYY-MM-DD] | [Forensic workstation] |
| [E-001] | [Stored] | [Name] | [YYYY-MM-DD] | [Archive] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Incident-Response-Plan]] | Incident handling |
| [[Incident-Management-Process]] | Operational incidents |

---

> **Template Standard:** Based on CyBOK v1
> **Usage:** Forensics requires *evidence integrity*. Document chain of custody. Never modify original evidence. Work on copies.
