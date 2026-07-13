---
document_type: Privacy Impact Assessment
version: "1.0"
status: Draft
author: "[Data Governance Officer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Confidential"
tags: [pia, privacy, gdpr, dmbok]
standard_ref:
  - DMBOK v2 — Data Security
  - GDPR — Article 35
---

# Privacy Impact Assessment (PIA)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Assesses privacy risks of data processing activities — required by GDPR Article 35 for high-risk processing.

## 2. PIA Trigger Criteria

| Trigger | Applicable | Action |
|---------|-----------|--------|
| [New system processing PII] | ✅ | [Full PIA required] |
| [Significant change to processing] | [If applicable] | [Update PIA] |
| [New data sharing agreement] | [If applicable] | [PIA for sharing] |
| [High-risk processing] | ✅ | [Full PIA required] |

## 3. Processing Description

| Field | Detail |
|-------|--------|
| [Processing Activity] | [Customer request management] |
| [Data Controller] | [Organization Name] |
| [Data Processor] | [Organization Name (internal)] |
| [Purpose] | [Process customer requests for service] |
| [Legal Basis] | [Consent + Contract performance] |
| [Data Subjects] | [Customers, Staff] |
| [Data Categories] | [Name, email, phone, address, request details] |
| [Retention Period] | [7 years after account closure] |

## 4. Data Mapping

| Data Element | Source | Purpose | Legal Basis | Retention | Classification |
|-------------|--------|---------|-----------|----------|---------------|
| [Name] | [Web form] | [Identification] | [Consent] | [7 years] | 🔴 L1 |
| [Email] | [Web form] | [Communication] | [Consent] | [7 years] | 🔴 L1 |
| [Phone] | [Web form] | [Communication] | [Consent] | [7 years] | 🔴 L1 |
| [Address] | [Web form] | [Service delivery] | [Contract] | [7 years] | 🔴 L1 |
| [Request details] | [Web form] | [Service processing] | [Contract] | [7 years] | 🟡 L2 |

## 5. Privacy Risks

| # | Risk | Likelihood | Impact | Risk Level | Mitigation |
|---|------|-----------|--------|-----------|-----------|
| 1 | [Unauthorized access to PII] | [Low] | [High] | 🟡 Medium | [MFA, RBAC, audit logging] |
| 2 | [Data breach via API] | [Low] | [High] | 🟡 Medium | [Input validation, rate limiting] |
| 3 | [Excessive data collection] | [Medium] | [Medium] | 🟡 Medium | [Data minimization review] |
| 4 | [Data retention beyond necessity] | [Low] | [Medium] | 🟢 Low | [Automated retention enforcement] |
| 5 | [Cross-border transfer] | [Low] | [High] | 🟡 Medium | [SCCs, adequacy assessment] |

## 6. Privacy Controls

| Control | Implementation | Status |
|---------|---------------|--------|
| [Consent management] | [Consent at registration, withdrawal option] | ✅ |
| [Data minimization] | [Collect only necessary fields] | ✅ |
| [Purpose limitation] | [Use data only for stated purpose] | ✅ |
| [Storage limitation] | [Automated retention enforcement] | ✅ |
| [Data subject rights] | [Access, rectification, erasure APIs] | ✅ |
| [Data portability] | [Standard format export] | ✅ |
| [Breach notification] | [72-hour notification process] | ✅ |
| [DPO appointment] | [DPO designated] | ✅ |

## 7. Data Subject Rights

| Right | Implementation | Response Time |
|-------|---------------|-------------|
| [Right of Access] | [Data export API] | [30 days] |
| [Right to Rectification] | [Profile update API] | [30 days] |
| [Right to Erasure] | [Data deletion process] | [30 days] |
| [Right to Restriction] | [Processing flag] | [30 days] |
| [Right to Portability] | [Standard format export] | [30 days] |
| [Right to Object] | [Opt-out mechanisms] | [30 days] |

## 8. PIA Outcome

| Field | Detail |
|-------|--------|
| [Residual Risk] | [Low — controls in place] |
| [DPO Recommendation] | [Proceed with processing] |
| [Approval] | [DGO + DPO sign-off] |
| [Review Date] | [YYYY-MM-DD] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Data-Masking-Anonymization-Rules]] | Privacy controls |
| [[Data-Access-Control-Policy]] | Access controls |
| [[Regulatory-Compliance-Register]] | Compliance tracking |

---

> **Template Standard:** Based on DMBOK v2, GDPR Article 35
> **Usage:** PIA is *required* before processing high-risk PII. Document everything. If in doubt, do a PIA.
