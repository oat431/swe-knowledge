---
document_type: Assumption Log
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
pm_owner: "[Project Manager]"
ba_owner: "[Business Analyst Name]"
classification: "Internal / Confidential"
tags: [assumption-log, assumptions, constraints, pmbok, iso-21502]
standard_ref:
  - PMBOK v8 — Planning
  - ISO 21502 — Project Management Guidance
---

# Assumption Log

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Project Manager | [Name / Role] |

---

## 1. Purpose

> This log tracks all assumptions and constraints throughout the project lifecycle. Assumptions are factors believed to be true for planning purposes. If an assumption proves false, it may impact scope, schedule, cost, or quality.

## 2. Assumption Log

| ID | Category | Assumption | Source | Impact if Invalid | Probability of Being Invalid | Impact Level | Validation Method | Validated | Status | Owner | Review Date |
|----|----------|-----------|--------|-------------------|--------------------------|-------------|-------------------|----------|--------|-------|-------------|
| ASM-001 | Technical | [ERP API remains available and stable] | [IT Architect] | [Integration rework — +2 weeks, +$10K] | 🟡 Medium | 🔴 High | [Vendor confirmation] | ⏳ Pending | Open | [Tech Lead] | [YYYY-MM-DD] |
| ASM-002 | Resource | [Key staff available as per resource plan] | [PM] | [Schedule delays — +1-2 weeks per resource] | 🟡 Medium | 🔴 High | [Manager confirmation] | ✅ Validated | Closed | [PM] | [YYYY-MM-DD] |
| ASM-003 | Regulatory | [No major regulatory changes during project] | [Compliance] | [Scope change, rework] | 🟢 Low | 🟡 Medium | [Regulatory monitoring] | ⏳ Pending | Open | [Compliance] | [YYYY-MM-DD] |
| ASM-004 | Data | [Data quality sufficient for migration] | [Data Analyst] | [Extended cleansing — +2 weeks, +$5K] | 🟡 Medium | 🟡 Medium | [Data profiling report] | ⏳ Pending | Open | [Data Architect] | [YYYY-MM-DD] |
| ASM-005 | Vendor | [Vendor delivers on committed timeline] | [PM] | [Schedule dependency — delays cascade] | 🟡 Medium | 🟡 Medium | [Milestone tracking] | ⏳ Pending | Open | [PM] | [YYYY-MM-DD] |
| ASM-006 | Budget | [Budget approved as proposed] | [Sponsor] | [Scope reduction or project deferral] | 🟢 Low | 🔴 High | [Finance Committee] | ⏳ Pending | Open | [Sponsor] | [YYYY-MM-DD] |
| ASM-007 | Technical | [Cloud provider maintains current SLA] | [IT] | [Availability impact] | 🟢 Low | 🟡 Medium | [SLA review] | ✅ Validated | Closed | [IT Lead] | [YYYY-MM-DD] |
| ASM-008 | Organizational | [No major organizational restructuring] | [HR] | [Stakeholder changes, delays] | 🟢 Low | 🟡 Medium | [HR monitoring] | ⏳ Pending | Open | [PM] | [YYYY-MM-DD] |

## 3. Constraint Log

| ID | Category | Constraint | Source | Impact | Mitigation | Owner |
|----|----------|-----------|--------|--------|-----------|-------|
| CON-001 | Technical | [Must use existing cloud provider] | [Org policy] | [Platform limitation] | [Use cloud-native services] | [Tech Lead] |
| CON-002 | Legal | [Data sovereignty — in-country only] | [Local law] | [Hosting constraint] | [Local cloud region] | [IT Lead] |
| CON-003 | Technical | [Must integrate with existing ERP] | [Business decision] | [API dependency] | [Early POC, fallback plan] | [Tech Lead] |
| CON-004 | Financial | [Budget cap $500K] | [Business case] | [Scope limitation] | [MoSCoW prioritization] | [PM] |
| CON-005 | Time | [Go-live by YYYY-MM-DD] | [Regulatory] | [Schedule pressure] | [Phased approach] | [PM] |
| CON-006 | Resource | [No new headcount] | [HR policy] | [Capacity constraint] | [Vendor augmentation, training] | [PM] |

## 4. Assumption Status Summary

| Status | Count | Percentage |
|--------|-------|-----------|
| ✅ Validated (Confirmed True) | [X] | [Y%] |
| ⏳ Pending (Awaiting Validation) | [X] | [Y%] |
| ❌ Invalid (Confirmed False) | [X] | [Y%] |
| ⚠️ At Risk (Likely False) | [X] | [Y%] |
| **Total** | **[X]** | **100%** |

## 5. Invalidated Assumptions (Impact Log)

| ID | Assumption | Date Invalidated | Impact | Action Taken | Owner |
|----|-----------|-----------------|--------|-------------|-------|
| [If any assumptions were proven false, record the impact and corrective action here] |

## 6. Assumption Review Schedule

| Review | Frequency | Participants | Purpose |
|--------|-----------|-------------|---------|
| [Assumption Review] | [Bi-weekly] | [PM, BA, Tech Lead] | [Validate pending assumptions, identify at-risk] |
| [Risk-Assumption Link] | [Monthly] | [PM, Risk Owner] | [Ensure invalidated assumptions create risk entries] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Risk-Analysis-Results]] | Invalidated assumptions become risks |
| [[Project-Scope-Statement]] | Assumptions underlying scope |
| [[Business-Case]] | Assumptions underlying the business case |
| [[Change-Log]] | Assumption changes may trigger scope changes |

---

> **Template Standard:** Based on PMBOK v8, ISO 21502
> **Usage:** Assumptions are *not* facts — they are beliefs that need validation. Review this log regularly. When an assumption is invalidated, immediately assess impact and create a risk entry if needed.
