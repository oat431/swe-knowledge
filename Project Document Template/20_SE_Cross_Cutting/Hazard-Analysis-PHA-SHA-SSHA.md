---
document_type: Hazard Analysis (PHA, SHA, SSHA)
version: "1.0"
status: Draft
author: "[Safety Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Confidential"
tags: [hazard-analysis, pha, sha, ssha, sebok, mil-std-882]
standard_ref:
  - SEBoK v2 — System Safety
  - MIL-STD-882 — System Safety
---

# Hazard Analysis (PHA, SHA, SSHA)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Systematic identification and analysis of hazards at system, subsystem, and component levels.

## 2. Analysis Types

| Type | Scope | Timing | Standard |
|------|-------|--------|---------|
| [PHA — Preliminary Hazard Analysis] | [System-level] | [Early concept] | [MIL-STD-882] |
| [SHA — System Hazard Analysis] | [Full system] | [Design phase] | [MIL-STD-882] |
| [SSHA — Subsystem Hazard Analysis] | [Subsystems] | [Detailed design] | [MIL-STD-882] |

## 3. Preliminary Hazard Analysis (PHA)

| ID | Hazard | Cause | Effect | Severity | Probability | Risk Level | Mitigation |
|----|--------|-------|--------|---------|-----------|-----------|-----------|
| [PHA-001] | [Data loss] | [System failure, human error] | [Business impact] | 🔴 Critical | [Low] | 🟡 Medium | [Backups, replication] |
| [PHA-002] | [Unauthorized access] | [Weak authentication] | [Data breach] | 🔴 Critical | [Medium] | 🟠 High | [MFA, RBAC] |
| [PHA-003] | [System unavailability] | [Infrastructure failure] | [Service disruption] | 🟠 High | [Low] | 🟡 Medium | [HA, failover] |
| [PHA-004] | [Data corruption] | [Software defect] | [Incorrect data] | 🟠 High | [Low] | 🟡 Medium | [Validation, checksums] |
| [PHA-005] | [Performance degradation] | [Load spike] | [Slow response] | 🟡 Medium | [Medium] | 🟡 Medium | [Auto-scaling, monitoring] |

## 4. System Hazard Analysis (SHA)

| ID | Hazard | Subsystem | Cause | Effect | Risk Level | Mitigation |
|----|--------|----------|-------|--------|-----------|-----------|
| [SHA-001] | [Authentication bypass] | [Auth Service] | [Vulnerability] | [Unauthorized access] | 🟠 High | [Security testing, MFA] |
| [SHA-002] | [Database failure] | [Database] | [Hardware/software] | [Data unavailability] | 🟡 Medium | [HA, backups] |
| [SHA-003] | [API abuse] | [API Gateway] | [Malicious traffic] | [Service disruption] | 🟡 Medium | [Rate limiting, WAF] |
| [SHA-004] | [Data leakage] | [All] | [Logging PII] | [Privacy violation] | 🟠 High | [PII masking] |

## 5. Subsystem Hazard Analysis (SSHA)

| ID | Hazard | Component | Cause | Effect | Risk Level | Mitigation |
|----|--------|----------|-------|--------|-----------|-----------|
| [SSHA-001] | [SQL injection] | [API Layer] | [Input validation gap] | [Data breach] | 🟠 High | [Parameterized queries] |
| [SSHA-002] | [XSS attack] | [Web UI] | [Output encoding gap] | [Session hijacking] | 🟡 Medium | [Output encoding, CSP] |
| [SSHA-003] | [Session fixation] | [Auth Service] | [Session management flaw] | [Account takeover] | 🟡 Medium | [Session regeneration] |
| [SSHA-004] | [CSRF attack] | [Web UI] | [Missing CSRF token] | [Unauthorized actions] | 🟡 Medium | [CSRF tokens] |

## 6. Risk Assessment Matrix

| Severity \ Probability | Frequent | Probable | Occasional | Remote | Improbable |
|----------------------|---------|---------|-----------|--------|-----------|
| **Catastrophic** | 🔴 | 🔴 | 🔴 | 🟠 | 🟡 |
| **Critical** | 🔴 | 🔴 | 🟠 | 🟡 | 🟢 |
| **Marginal** | 🔴 | 🟠 | 🟡 | 🟢 | 🟢 |
| **Negligible** | 🟠 | 🟡 | 🟢 | 🟢 | 🟢 |

## 7. Hazard Summary

| Severity | Count | Mitigated | Residual Acceptable |
|---------|-------|----------|-------------------|
| 🔴 Critical | [3] | [3] | ✅ |
| 🟠 High | [4] | [4] | ✅ |
| 🟡 Medium | [5] | [5] | ✅ |
| 🟢 Low | [0] | — | — |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[System-Safety-Plan]] | Safety framework |
| [[Risk-Register]] | Risk management |
| [[Security-Requirements-Specification]] | Security requirements |

---

> **Template Standard:** Based on SEBoK v2, MIL-STD-882
> **Usage:** Hazard analysis is *proactive*. Identify hazards before they become incidents. Analyze at all levels.
