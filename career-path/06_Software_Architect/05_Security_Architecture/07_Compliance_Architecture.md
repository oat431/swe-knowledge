---
title: Compliance Architecture
role: Software Architect
capability_area: Security Architecture
topic: Compliance Architecture
status: complete
created: 2026-08-22
updated: 2026-08-22
tags:
  - career-path
  - software-architect
  - compliance
  - data-residency
  - audit-trail
---

# Compliance Architecture

> **Core skill:** The architect translates regulatory constraints — data residency, encryption standards, retention, audit trails, access review — into structural design decisions, and maintains a compliance matrix mapping every regulation to the architecture control that satisfies it.

## Why This Matters

Compliance requirements arrive as legal text, but they land as architecture constraints: where data may live, how it must be protected, how long it must be kept, who may touch it, and what must be provable afterward. An architect who treats compliance as a documentation exercise discovers late that the system's structure contradicts a regulation — and structure is the slowest thing in the system to change.

The compliance matrix is the architect's translation tool: regulation to obligation to architecture control to evidence. Once built, it makes audits a lookup instead of a scramble and makes new requirements cheap to assess — which rows change, which controls move, which views need updating. It also makes gaps visible while they are still cheap: a regulation with no control is a finding waiting to happen.

The central design question is when compliance drives the architecture versus when it constrains it. Some requirements genuinely shape the structure — residency forces region topology, deletion rights force data inventory and propagation design, segregation of duties forces workflow structure. Others merely constrain mechanism choices within a structure that is already sound. Telling the two apart prevents under-design against the first and gold-plating against the second.

## Regulatory Constraints as Architecture Drivers

| Constraint | Typical Requirement | Architectural Response | Affected View |
|------------|---------------------|------------------------|---------------|
| **Data residency** | Personal data stays within a jurisdiction | Region-scoped storage and processing; routing rules at boundaries | Deployment, data flow |
| **Encryption standards** | Approved algorithms at rest and in transit; managed keys | Key management as an architecture component with custody rules | Data, deployment |
| **Retention and deletion** | Keep no longer than the stated purpose; deletion enforceable | Lifecycle designed into flows, including backups and analytics copies | Data architecture |
| **Audit trails** | Who did what, when, with tamper evidence | Audit component spanning every boundary that touches regulated data | Operational, security |
| **Access review** | Least privilege demonstrable; periodic recertification | Role and permission records with review workflow | Identity |
| **Breach notification** | Detect and report within a fixed window | Observability and notification paths designed for the window | Operational |
| **Segregation of duties** | No single actor completes a sensitive process alone | Separation of privilege at workflow level | Identity, process |

## The Compliance Matrix

```markdown
## Compliance Matrix — <system> — <regulation set>

| Regulation | Obligation | Architecture Control | Evidence | Owner | Status |
|------------|-----------|----------------------|----------|-------|--------|
| GDPR Art. 17 | Right to erasure on request | Deletion jobs propagate across stores and caches; inventory names every copy | Deletion audit log with per-store confirmation | Data team | Implemented |
| GDPR Art. 5 | Storage limitation | Retention policies per dataset enforced by lifecycle rules | Retention schedule; lifecycle rule export | Data team | Implemented |
| PCI DSS 3.4 | Cardholder data encrypted at rest | Envelope encryption with managed key custody | Key management configuration; access log | Platform | Implemented |
| SOC 2 CC6.2 | Access recertification | Quarterly access review workflow on identity store | Review records with reviewer sign-off | Security | In progress |
```

## Compliance in the Data Architecture View

Compliance constraints become visible when they are drawn onto the data view rather than kept in a spreadsheet.

```mermaid
flowchart LR
    SOURCE["Classified data source"] --> RESIDENCY["Residency boundary restricts regions"]
    RESIDENCY --> STORE["Storage with encryption and retention rules"]
    STORE --> AUDIT["Access and change audit trail"]
    AUDIT --> REVIEW["Periodic access review and recertification"]
```

## When Compliance Drives vs Constrains

| Situation | What It Is | Example | Architect's Move |
|-----------|------------|---------|------------------|
| The requirement cannot be satisfied within the current structure | Compliance drives architecture | Residency demanded from a single-region design | Change the structure: region topology, routing, per-region keys |
| The requirement is satisfiable within the structure by a mechanism choice | Compliance constrains implementation | An approved encryption standard replacing a weaker default | Select the mechanism; record it; no structural change |
| The requirement is ambiguous or contested | Neither yet | Where logging for audit must physically reside | Clarify with legal and compliance; keep a decision record of the interpretation |

The distinction is practical, not semantic: it tells reviewers which changes need architecture review and which are implementation decisions. A structure-changing compliance requirement that is treated as a constraint becomes a violated regulation.

## Practical Applications

### Compliance Architecture Checklist

- [ ] Every applicable regulation is mapped to obligations and to the architecture controls that satisfy them
- [ ] Residency and retention constraints are drawn on the data architecture view
- [ ] Audit trails over regulated data carry tamper evidence and named coverage
- [ ] Key management is designed as an architecture component with custody and rotation rules
- [ ] Every compliance matrix row has an owner, evidence, and a review date

### Regulatory Impact Assessment

```markdown
## Regulatory Impact Assessment — <new regulation or change>

| Question | Answer |
|----------|--------|
| Which obligations are new or changed? | <list> |
| Which architecture controls already satisfy them? | <matrix rows> |
| Which structures must change, if any? | <views affected> |
| What evidence will demonstrate compliance? | <artifacts> |
| What is the deadline and the owning role? | <date and owner> |
```

## Common Pitfalls

| Pitfall | Why It Is a Problem | Better Approach |
|---------|---------------------|-----------------|
| **Compliance as a documentation exercise** | Rows are filled in, but the structure still violates the regulation | Every control in the matrix names a structural mechanism, not an intention |
| **Residency by policy only** | A policy states the rule; replication quietly moves data across regions | Enforce residency in the topology; verify from infrastructure, not text |
| **Deletion without propagation** | Primary store deletes; caches, backups, and analytics keep copies | Design deletion as a flow: inventory, propagation, per-store confirmation |
| **Audit trails as log side effects** | Logs rotate, get edited, or miss admin paths; they cannot serve as evidence | Audit trail as a designed component with coverage and integrity guarantees |
| **Gold-plating** | The strictest interpretation applied everywhere; cost and complexity without requirement | Match control strength to the obligation; record the interpretation |
| **One compliance owner** | A specialist holds all context; the architecture never internalizes the constraints | Matrix rows have architecture owners; compliance reviews the matrix |

## Success Indicators

- An audit produces artifacts by lookup, not by archaeology
- A new regulation is impact-assessed in days using the matrix
- Deletion requests complete with per-store evidence within the stated window
- Key management and custody are reviewed as architecture components
- Residency is enforced by structure and verified from deployment reality

## Related Topics

- [[01_Secure_by_Design_Principles]]
- [[05_Identity_and_Access_Architecture]]
- [[06_Security_Decisions_and_ADRs]]
- [[04_Architecture_Evaluation_and_Trade_Offs/00_overview|Architecture Evaluation and Trade Offs]]

## Summary

Compliance architecture translates regulatory text into structural constraints: residency, encryption, retention, auditability, access review, and segregation of duties become decisions in the deployment, data, identity, and operations views. The compliance matrix — regulation to obligation to control to evidence — keeps the translation honest and makes audits a lookup. The architect's judgment is in distinguishing the requirements that change the structure from those that only constrain it, and in recording the interpretation either way.
