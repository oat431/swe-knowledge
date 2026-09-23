---
document_type: Stakeholder Needs Document
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
systems_engineer: "[SE Name]"
ba_owner: "[BA Name]"
classification: "Internal / Confidential"
tags: [stakeholder-needs, expectations, measures-of-effectiveness, sebok, iso-15288]
standard_ref:
  - SEBoK v2 — Concept Definition
  - ISO/IEC/IEEE 15288 — System Life Cycle Processes (§6.4.1)
  - ISO/IEC/IEEE 29148 — Requirements Engineering
---

# Stakeholder Needs Document

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | Archived]
> **Last Updated:** [YYYY-MM-DD]

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Systems Engineer | [Name / Role] |
| Business Analyst | [Name / Role] |

### Revision History

| Version | Date | Author | Change Description |
|---------|------|--------|--------------------|
| 0.1 | [YYYY-MM-DD] | [Name] | Initial draft |
| 1.0 | [YYYY-MM-DD] | [Name] | Approved version |

### Approvals

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Project Sponsor | | | |
| Systems Engineer | | | |
| Operations Director | | | |

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Stakeholder Identification](#2-stakeholder-identification)
3. [Stakeholder Needs](#3-stakeholder-needs)
4. [Measures of Effectiveness](#4-measures-of-effectiveness)
5. [Needs Prioritization](#5-needs-prioritization)
6. [Needs Traceability](#6-needs-traceability)
7. [Constraints & Assumptions](#7-constraints--assumptions)

---

## 1. Executive Summary

| Field | Detail |
|-------|--------|
| Total Stakeholder Groups | [X] |
| Total Needs Identified | [X] |
| Critical Needs | [X] |
| Source | Stakeholder interviews, workshops, observation |
| Baseline Date | [YYYY-MM-DD] |

---

## 2. Stakeholder Identification

### 2.1 Stakeholder Categories

| Category | Stakeholder Groups | Role in System | Primary Concerns |
|----------|-------------------|---------------|-----------------|
| **Primary Users** | [Stakeholder groups] | [Role in system] | [Primary concerns] |
| **Secondary Users** | [Stakeholder groups] | [Role in system] | [Primary concerns] |
| **Governance** | [Stakeholder groups] | [Role in system] | [Primary concerns] |
| **External** | [Stakeholder groups] | [Role in system] | [Primary concerns] |
| **Maintenance** | [Stakeholder groups] | [Role in system] | [Primary concerns] |

### 2.2 Stakeholder Register

| ID | Stakeholder Group | Representatives | Contact | Engagement Level |
|----|------------------|----------------|---------|-----------------|
| SH-[XX] | [Stakeholder group] | [Representative roles] | [Contact info] | [Engagement level] |

---

## 3. Stakeholder Needs

### 3.1 Needs Register

| Need ID | Stakeholder Group | Need | Rationale | Priority | Source |
|---------|------------------|------|-----------|----------|--------|
| SN-[XX] | [Stakeholder group] | [Need statement] | [Rationale] | [Priority] | [Workshop / Interview / Survey] |
| SN-[XX] | | | | | |

### 3.2 Needs by Stakeholder Group

#### [Stakeholder Group]

| Need ID | Need | Priority | MoE |
|---------|------|---------|-----|
| SN-[XX] | [Need statement] | [Priority] | MOE-[XX] |

#### [Stakeholder Group]

| Need ID | Need | Priority | MoE |
|---------|------|---------|-----|
| SN-[XX] | [Need statement] | [Priority] | MOE-[XX] |

---

## 4. Measures of Effectiveness

| MOE ID | Measure | Description | Unit | Threshold | Objective | Linked Needs |
|--------|---------|-------------|------|-----------|-----------|-------------|
| MOE-[XX] | [Measure name] | [Description] | [Unit] | [Threshold] | [Objective] | SN-[XX] |

---

## 5. Needs Prioritization

### 5.1 Prioritization Matrix

| Need ID | Need | Business Value | Urgency | Feasibility | Score | Priority |
|---------|------|---------------|---------|-------------|-------|----------|
| SN-[XX] | [Need statement] | [X] | [X] | [X] | **[X.X]** | [Priority] |

### 5.2 Needs Conflict Resolution

| Conflict | Needs | Resolution | Rationale |
|----------|-------|-----------|-----------|
| [e.g., Speed vs Security] | SN-[XX], SN-[XX] | [Resolution] | [Rationale] |
| [e.g., Simplicity vs Features] | SN-[XX], SN-[XX] | [Resolution] | [Rationale] |

---

## 6. Needs Traceability

### 6.1 Traceability Matrix

| Need ID | Mission Objective | System Requirement | Test Case |
|---------|------------------|-------------------|-----------|
| SN-[XX] | MO-[XX] [Objective name] | SYRS-[XXX] | TC-[XXX] |

---

## 7. Constraints & Assumptions

### 7.1 Constraints

| ID | Constraint | Type | Impact |
|----|-----------|------|--------|
| CON-[XX] | [Constraint description] | [Technical / Legal / Financial / Time] | [Impact] |

### 7.2 Assumptions

| ID | Assumption | Impact if Invalid |
|----|-----------|-------------------|
| ASM-[XX] | [Assumption statement] | [Impact if invalid] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Mission-Analysis-Report]] | Mission context for these needs |
| [[Concept-of-Operations]] | Operational scenarios address these needs |
| [[System-Requirements-Specification]] | Needs are elaborated into system requirements |
| [[Stakeholder-Register]] | Detailed stakeholder information |
| [[Stakeholder-Engagement-Approach]] | How stakeholders were engaged |

---

> **Template Standard:** Based on SEBoK v2 (Concept Definition), ISO/IEC/IEEE 15288 (§6.4.1), ISO/IEC/IEEE 29148
> **Usage:** This document captures *what stakeholders need*, not *how* the system will meet those needs. The "how" comes later in [[Concept-of-Operations]] and [[System-Requirements-Specification]].
