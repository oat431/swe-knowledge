---
document_type: Risk Assessment Report (Security)
version: "1.0"
status: Draft
author: "[Security Officer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Confidential"
tags: [risk-assessment, security-risk, cyberok, iso-27005]
standard_ref:
  - CyBOK v1 — Security Risk Management
  - ISO/IEC 27005 — Information Security Risk Management
---

# Risk Assessment Report (Security)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Systematic identification and evaluation of security risks to the system and its data.

## 2. Risk Assessment Methodology

| Aspect | Approach |
|--------|---------|
| [Method] | [OCTAVE / NIST SP 800-30] |
| [Scope] | [All project assets, systems, and data] |
| [Risk Scale] | [5×5 matrix (Likelihood × Impact)] |
| [Assessment Date] | [YYYY-MM-DD] |
| [Next Review] | [YYYY-MM-DD] |

## 3. Asset Inventory

| Asset | Type | Classification | Value | Owner |
|-------|------|---------------|-------|-------|
| [Customer PII] | [Data] | 🔴 Restricted | [Critical] | [Data Owner] |
| [Financial Data] | [Data] | 🔴 Restricted | [Critical] | [Data Owner] |
| [Application Code] | [Software] | 🟡 Confidential | [High] | [Tech Lead] |
| [Credentials/Keys] | [Data] | 🔴 Restricted | [Critical] | [Security Officer] |
| [Infrastructure] | [System] | 🟡 Confidential | [High] | [DevOps] |
| [Logs/Audit Trail] | [Data] | 🟡 Internal | [Medium] | [DevOps] |

## 4. Threat Landscape

| Threat Actor | Motivation | Capability | Target |
|-------------|-----------|-----------|--------|
| [External Attacker] | [Financial gain] | [High] | [Customer data, credentials] |
| [Insider Threat] | [Various] | [Medium] | [Sensitive data, systems] |
| [Competitor] | [Espionage] | [Medium] | [Business data, IP] |
| [Automated Attack] | [Opportunistic] | [Low-Medium] | [Any vulnerable system] |

## 5. Risk Register

| ID | Threat | Vulnerability | Asset | Likelihood | Impact | Risk Level | Treatment |
|----|--------|-------------|-------|-----------|--------|-----------|-----------|
| [SR-001] | [SQL Injection] | [Input validation gap] | [Application] | [3] | [5] | 🔴 High | [Mitigate] |
| [SR-002] | [Credential Theft] | [Weak passwords] | [User accounts] | [4] | [5] | 🔴 High | [Mitigate] |
| [SR-003] | [Data Breach] | [Insufficient encryption] | [Customer data] | [2] | [5] | 🟡 Medium | [Mitigate] |
| [SR-004] | [DDoS Attack] | [No rate limiting] | [API] | [3] | [4] | 🟡 Medium | [Mitigate] |
| [SR-005] | [Insider Threat] | [Excessive access] | [All systems] | [2] | [4] | 🟡 Medium | [Mitigate] |
| [SR-006] | [Dependency Vulnerability] | [Outdated packages] | [Application] | [4] | [3] | 🟡 Medium | [Mitigate] |
| [SR-007] | [Session Hijacking] | [Insecure session mgmt] | [User sessions] | [2] | [3] | 🟢 Low | [Accept] |
| [SR-008] | [Physical Access] | [Cloud — N/A] | [Infrastructure] | [1] | [5] | 🟢 Low | [Transfer] |

## 6. Risk Heat Map

| Likelihood \ Impact | Negligible (1) | Minor (2) | Moderate (3) | Major (4) | Critical (5) |
|-------------------|---------------|-----------|-------------|-----------|-------------|
| **Very Likely (5)** | 🟡 | 🟡 | 🟠 | 🔴 | 🔴 |
| **Likely (4)** | 🟢 | 🟡 | 🟠 SR-006 | 🔴 SR-002 | 🔴 |
| **Possible (3)** | 🟢 | 🟡 | 🟡 SR-004 | 🟠 SR-004 | 🔴 SR-001 |
| **Unlikely (2)** | 🟢 | 🟢 | 🟢 SR-007 | 🟡 SR-005 | 🟡 SR-003 |
| **Rare (1)** | 🟢 | 🟢 | 🟢 | 🟢 | 🟢 SR-008 |

## 7. Risk Summary

| Risk Level | Count | Percentage |
|-----------|-------|-----------|
| 🔴 High | [2] | [25%] |
| 🟡 Medium | [4] | [50%] |
| 🟢 Low | [2] | [25%] |
| **Total** | **[8]** | **100%** |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Risk-Treatment-Plan]] | Treatment for identified risks |
| [[ISMS-Documentation]] | ISMS framework |
| [[Threat-Model]] | Detailed threat analysis |

---

> **Template Standard:** Based on CyBOK v1, ISO/IEC 27005
> **Usage:** Risk assessment is *not* a one-time activity. Review annually, after major changes, or after incidents.
