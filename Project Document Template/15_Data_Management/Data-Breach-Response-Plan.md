---
document_type: Data Breach Response Plan
version: "1.0"
status: Draft
author: "[Security Officer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Confidential"
tags: [data-breach, incident-response, gdpr, dmbok]
standard_ref:
  - DMBOK v2 — Data Security
  - GDPR — Article 33, 34
---

# Data Breach Response Plan

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines procedures for responding to data breaches involving personal or sensitive data — detection, containment, notification, and remediation.

## 2. Breach Definition

> A data breach is any incident resulting in unauthorized access, disclosure, alteration, or destruction of personal or sensitive data.

## 3. Breach Severity

| Severity | Definition | Examples | Notification |
|---------|-----------|---------|------------|
| 🔴 **Critical** | [Large-scale PII exposure] | [Database dump, ransomware] | [Authority + Data subjects] |
| 🟠 **High** | [Limited PII exposure] | [Unauthorized access, email leak] | [Authority] |
| 🟡 **Medium** | [Potential PII exposure] | [Suspicious activity, misconfiguration] | [Internal only] |
| 🟢 **Low** | [No PII exposure] | [System compromise, no data accessed] | [Internal only] |

## 4. Response Timeline

| Phase | Action | Timeline | Owner |
|-------|--------|---------|-------|
| [Detection] | [Identify and confirm breach] | [< 1 hour] | [Security Team] |
| [Containment] | [Stop ongoing breach] | [< 4 hours] | [IT Operations] |
| [Assessment] | [Determine scope and impact] | [< 24 hours] | [Security + DGO] |
| [Authority Notification] | [Notify supervisory authority] | [< 72 hours] | [DPO] |
| [Data Subject Notification] | [Notify affected individuals] | [Without undue delay] | [DPO + Communications] |
| [Remediation] | [Fix root cause] | [< 30 days] | [Security + Dev] |
| [Post-Mortem] | [Lessons learned] | [< 7 days] | [Incident Commander] |

## 5. GDPR Notification Requirements

### 5.1 Authority Notification (Article 33)

| Field | Requirement |
|-------|-----------|
| [Timeline] | [72 hours from awareness] |
| [Recipient] | [Supervisory authority (DPA)] |
| [Content] | [Nature of breach, categories and approximate number of data subjects, likely consequences, measures taken or proposed] |
| [Exception] | [Not required if unlikely to result in risk to individuals] |

### 5.2 Data Subject Notification (Article 34)

| Field | Requirement |
|-------|-----------|
| [Timeline] | [Without undue delay] |
| [Recipient] | [Affected data subjects] |
| [Content] | [Nature of breach, DPO contact, likely consequences, measures taken] |
| [Exception] | [Not required if data encrypted, or if disproportionate effort (public communication)] |

## 6. Breach Response Checklist

| # | Action | Owner | Status |
|---|--------|-------|--------|
| 1 | [Confirm breach occurred] | [Security] | ☐ |
| 2 | [Contain the breach] | [IT Operations] | ☐ |
| 3 | [Preserve evidence] | [Security] | ☐ |
| 4 | [Assess scope and impact] | [Security + DGO] | ☐ |
| 5 | [Identify affected data subjects] | [DGO] | ☐ |
| 6 | [Notify DPO] | [Security] | ☐ |
| 7 | [Notify supervisory authority (if required)] | [DPO] | ☐ |
| 8 | [Notify affected data subjects (if required)] | [DPO + Communications] | ☐ |
| 9 | [Document the breach] | [Security] | ☐ |
| 10 | [Remediate root cause] | [Security + Dev] | ☐ |
| 11 | [Conduct post-mortem] | [Incident Commander] | ☐ |
| 12 | [Update controls] | [Security] | ☐ |

## 7. Communication Templates

### Authority Notification

> **Subject:** Data Breach Notification — [Project Name]
>
> **Date of breach:** [YYYY-MM-DD]
> **Date of awareness:** [YYYY-MM-DD]
> **Nature of breach:** [Description]
> **Categories of data subjects:** [Customers, Staff]
> **Approximate number:** [X]
> **Categories of data:** [PII, financial]
> **Likely consequences:** [Description]
> **Measures taken:** [Description]
> **DPO contact:** [Name, Email, Phone]

## 8. Breach Register

| ID | Date | Severity | Data Subjects | Data Types | Authority Notified | Status |
|----|------|---------|--------------|-----------|-------------------|--------|
| [BR-001] | [YYYY-MM-DD] | [None yet] | — | — | — | — |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Incident-Response-Plan]] | Overall incident response |
| [[Data-Access-Control-Policy]] | Access controls |
| [[Privacy-Impact-Assessment]] | Privacy assessment |

---

> **Template Standard:** Based on DMBOK v2, GDPR Article 33, 34
> **Usage:** 72 hours is the *hard deadline*. If you miss it, you're non-compliant. Practice this plan.
