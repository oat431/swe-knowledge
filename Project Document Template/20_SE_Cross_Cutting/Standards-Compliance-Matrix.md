---
document_type: Standards Compliance Matrix
version: "1.0"
status: Draft
author: "[Systems Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [standards, compliance, matrix, sebok]
standard_ref:
  - SEBoK v2 — Systems Engineering Management
---

# Standards Compliance Matrix

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Maps project compliance to applicable standards — demonstrating conformance and identifying gaps.

## 2. Applicable Standards

| Standard | Description | Applicability | Status |
|---------|-----------|-------------|--------|
| [ISO/IEC/IEEE 15288] | [System Life Cycle] | [Required] | ✅ Compliant |
| [ISO/IEC/IEEE 12207] | [Software Life Cycle] | [Required] | ✅ Compliant |
| [ISO/IEC/IEEE 29148] | [Requirements Engineering] | [Required] | ✅ Compliant |
| [ISO/IEC/IEEE 42010] | [Architecture Description] | [Required] | ✅ Compliant |
| [ISO 9001] | [Quality Management] | [Required] | ✅ Compliant |
| [ISO/IEC 27001] | [Information Security] | [Required] | ✅ Compliant |
| [ISO 31000] | [Risk Management] | [Required] | ✅ Compliant |

## 3. Compliance Matrix

### ISO/IEC/IEEE 15288 — System Life Cycle

| Clause | Requirement | Implementation | Evidence | Status |
|--------|-----------|---------------|---------|--------|
| [6.1] | [Stakeholder requirements] | [[Stakeholder-Needs-Document]] | [Document] | ✅ |
| [6.2] | [Requirements analysis] | [[System-Requirements-Specification]] | [Document] | ✅ |
| [6.3] | [Architecture] | [[System-Architecture-Description]] | [Document] | ✅ |
| [6.4] | [Design] | [[High-Level-Design]] | [Document] | ✅ |
| [6.5] | [Implementation] | [[Implementation-Plan]] | [Document] | ✅ |
| [6.6] | [Integration] | [[Integration-Plan-Reports]] | [Document] | ✅ |
| [6.7] | [Verification] | [[Verification-Reports]] | [Document] | ✅ |
| [6.8] | [Transition] | [[Transition-Plan]] | [Document] | ✅ |
| [6.9] | [Validation] | [[Validation-Reports]] | [Document] | ✅ |
| [6.10] | [Operation] | [[Operations-Manual-Runbook]] | [Document] | ✅ |
| [6.11] | [Maintenance] | [[Maintenance-Plan]] | [Document] | ✅ |
| [6.12] | [Disposal] | [[System-Disposal-Retirement-Plan]] | [Document] | ✅ |

### ISO/IEC/IEEE 29148 — Requirements Engineering

| Clause | Requirement | Implementation | Evidence | Status |
|--------|-----------|---------------|---------|--------|
| [5.1] | [Requirements processes] | [[SEMP]] | [Document] | ✅ |
| [5.2] | [Stakeholder requirements] | [[Stakeholder-Needs-Document]] | [Document] | ✅ |
| [5.3] | [System requirements] | [[System-Requirements-Specification]] | [Document] | ✅ |
| [5.4] | [Requirements traceability] | [[Requirements-Traceability-Matrix]] | [Document] | ✅ |

## 4. Compliance Summary

| Standard | Clauses | Compliant | Partial | Non-Compliant | Score |
|---------|---------|----------|---------|-------------|-------|
| [ISO/IEC/IEEE 15288] | [12] | [12] | [0] | [0] | [100%] |
| [ISO/IEC/IEEE 12207] | [10] | [10] | [0] | [0] | [100%] |
| [ISO/IEC/IEEE 29148] | [4] | [4] | [0] | [0] | [100%] |
| [ISO/IEC/IEEE 42010] | [3] | [3] | [0] | [0] | [100%] |
| **Overall** | **[29]** | **[29]** | **[0]** | **[0]** | **[100%]** |

## 5. Compliance Gaps

| # | Gap | Standard | Risk | Remediation | Status |
|---|-----|----------|------|-----------|--------|
| 1 | [None currently] | — | — | — | — |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[SEMP]] | SE management |
| [[Compliance-Assessment-Report]] | Security compliance |
| [[Quality-Management-Plan]] | Quality compliance |

---

> **Template Standard:** Based on SEBoK v2
> **Usage:** Compliance is *evidence-based*. The matrix shows what's required, what's done, and what's missing.
