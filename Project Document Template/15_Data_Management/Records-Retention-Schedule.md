---
document_type: Records Retention Schedule
version: "1.0"
status: Draft
author: "[Data Governance Officer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [records-retention, compliance, dmbok]
standard_ref:
  - DMBOK v2 — Document & Content Management
---

# Records Retention Schedule

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines retention periods for all business records — balancing legal requirements, business needs, and storage costs.

## 2. Retention Schedule

| Record Type | Category | Retention | Legal Basis | Disposal Method |
|------------|---------|----------|------------|----------------|
| [Customer contracts] | [Legal] | [7 years after termination] | [Contract law] | [Secure deletion] |
| [Financial records] | [Financial] | [7 years] | [Tax law] | [Secure deletion] |
| [Tax records] | [Financial] | [7 years] | [Tax law] | [Secure deletion] |
| [Audit logs] | [Compliance] | [3 years] | [Internal policy] | [Automated cleanup] |
| [Employee records] | [HR] | [7 years after departure] | [Labor law] | [Secure deletion] |
| [Email] | [Communication] | [3 years] | [Internal policy] | [Automated cleanup] |
| [Request records] | [Business] | [7 years] | [Business need] | [Archive then delete] |
| [Transaction records] | [Financial] | [7 years] | [Tax law] | [Archive then delete] |
| [System logs] | [Technical] | [90 days] | [Internal policy] | [Automated cleanup] |
| [Backups] | [Technical] | [30 days] | [Internal policy] | [Automated rotation] |

## 3. Retention by Classification

| Classification | Default Retention | Maximum Retention | Approval for Extension |
|---------------|------------------|------------------|----------------------|
| 🔴 L1 Restricted | [7 years] | [10 years] | [DGO + Legal] |
| 🟡 L2 Confidential | [5 years] | [7 years] | [DGO] |
| 🟢 L3 Internal | [3 years] | [5 years] | [Data Steward] |
| ⚪ L4 Public | [1 year] | [3 years] | [Data Steward] |

## 4. Disposal Procedures

| Step | Action | Verification | Documentation |
|------|--------|-------------|--------------|
| 1 | [Confirm retention period expired] | [Date check] | [Log entry] |
| 2 | [Verify no legal hold] | [Legal check] | [Confirmation] |
| 3 | [Verify no active litigation] | [Legal check] | [Confirmation] |
| 4 | [Delete from primary storage] | [Deletion command] | [Deletion log] |
| 5 | [Delete from backups] | [Backup rotation] | [Rotation log] |
| 6 | [Document disposal] | [Disposal certificate] | [Retention log] |

## 5. Legal Hold Override

| Trigger | Action | Owner | Duration |
|---------|--------|-------|---------|
| [Litigation hold] | [Suspend all disposal] | [Legal] | [Until released] |
| [Regulatory investigation] | [Suspend affected disposal] | [Legal] | [Until released] |
| [Audit] | [Suspend affected disposal] | [DGO] | [Until audit complete] |

## 6. Monitoring

| Metric | Threshold | Alert |
|--------|----------|-------|
| [Records past retention] | [0] | [Monthly report] |
| [Disposal compliance] | [100%] | [Quarterly review] |
| [Legal holds active] | [Monitor] | [Monthly report] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Data-Retention-Archival-Policy]] | Overall retention policy |
| [[ECM-Strategy]] | Content management |
| [[Regulatory-Compliance-Register]] | Legal requirements |

---

> **Template Standard:** Based on DMBOK v2
> **Usage:** Retention is *not* "keep forever." Keep what you must, delete what you must. Legal holds override everything.
