---
title: "Audit and Lineage for Compliance"
note_type: capability-topic
capability_area: data-security-and-privacy
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - audit
  - data-lineage
  - compliance
---

# Audit and Lineage for Compliance

> Recording who accessed what data, when, and why, in a tamper-evident format that satisfies regulatory auditors.

## Why This Is a Senior Skill

Mid-level engineers add logging to pipelines. Senior engineers design audit systems that are complete enough for regulators, performant enough for production, and queryable enough for compliance teams to self-serve without engineering support.

The senior challenge is building audit trails that prove compliance without becoming a storage or performance bottleneck.

## Core Frameworks

### Audit Log Design Dimensions

| Dimension | Design Choice | Trade-off |
|-----------|--------------|-----------|
| Completeness | Log every access vs sample | Complete is expensive but audit-proof |
| Granularity | Row-level vs table-level vs query-level | Finer granularity increases storage 10-100x |
| Immutability | Append-only with hash chain vs mutable | Immutable prevents tampering but cannot correct errors |
| Retention | 7 years vs regulatory minimum vs indefinite | Longer retention increases cost and breach surface |
| Queryability | Raw logs vs indexed audit store | Indexed enables self-service but adds infrastructure |
| Tamper evidence | Hash chain, signed entries, blockchain anchor | Adds complexity but provides cryptographic proof |

### Lineage Types for Compliance

| Lineage Type | What It Tracks | Compliance Use |
|--------------|---------------|----------------|
| Table-level | Which tables feed which | Impact analysis, data sovereignty |
| Column-level | Which columns flow where | PII tracking, minimization proof |
| Transformation | What operations were applied | Reproducibility, fairness audit |
| Access | Who read or wrote what | Breach investigation, access review |
| Consent | Which legal basis applies | GDPR Article 6 compliance |
| Retention | How long data was kept | Storage limitation proof |

### Compliance Reporting Matrix

| Report | Audience | Frequency | Data Source |
|--------|----------|-----------|-------------|
| Access summary | Security team | Weekly | Audit log aggregation |
| PII inventory | DPO, regulators | Quarterly | Classification metadata and lineage |
| Data subject requests | Legal, DPO | On demand | Erasure and export pipeline logs |
| Retention compliance | Compliance team | Monthly | TTL metadata and deletion logs |
| Cross-border transfers | DPO, regulators | Quarterly | Lineage with geographic metadata |
| Breach notification | Regulators, affected subjects | Within 72 hours | Audit log, access anomaly detection |

## In Practice

**Build audit as a sidecar, not inline.** Writing audit logs in the critical path adds latency. Use change data capture or query-log extraction to populate an audit store asynchronously. The audit store can lag by seconds but must never lose entries.

**Make lineage column-level.** Table-level lineage tells you that customer data flows to the analytics warehouse. Column-level lineage tells you that the email column specifically flows there, which is what the DPO needs for a data subject access request.

**Design for the auditor's question.** Regulators ask: "Can you show me every time employee X accessed patient records in the last 12 months?" Your audit system should answer this in under 60 seconds without engineering involvement. Index on actor, resource, and timestamp.

**Separate audit storage from operational storage.** Audit logs are high-volume and append-heavy. Storing them in the same database as operational data creates contention and risk. Use a dedicated audit store with its own retention and access policies.

**Automate compliance reports.** If the compliance team needs an engineer to run a query every quarter, the system is not mature enough. Build dashboards and scheduled reports that the compliance team can access directly.

## Practical Exercise

Design an audit and lineage system for a healthcare analytics platform:
1. Define what events must be logged: reads, writes, schema changes, access grants
2. Choose a storage architecture: separate audit database, what engine, what retention
3. Design column-level lineage tracking for one pipeline: source columns to destination columns
4. Write the query that answers the regulator's question: "Show all access to patient records by user X in date range Y"
5. Design one automated compliance report: what it shows, who sees it, how often

## Knowledge Connections

- [[05_Data_Security]]: DMBOK audit and security monitoring
- [[01_Data_Classification_and_Sensitivity]]: classification metadata feeds audit reports
- [[03_Access_Control_for_Data]]: access events are the primary audit source
- [[04_Privacy_Engineering]]: lineage proves privacy compliance
- [[06_Secure_Data_Sharing]]: audit extends to sharing boundaries

## Common Pitfalls

- Audit logs in the same database as operational data: contention and risk of accidental deletion
- Table-level lineage only: regulators need column-level tracking for PII subject access requests
- Logging without indexing: a billion audit rows are useless if the compliance team cannot query them in seconds
- Treating lineage as a documentation exercise: lineage must be automated from pipeline metadata, not manually maintained

## Key Takeaways

- Audit logs must be complete, immutable, and queryable: incomplete logs fail audits, mutable logs fail trust tests
- Column-level lineage is the minimum for privacy compliance: table-level is insufficient for PII tracking
- Audit is a sidecar concern: extract from query logs or CDC, do not add latency to the critical path
- Design for the auditor's question: compliance teams should self-serve without engineering support
- Separate audit storage from operational storage to avoid contention and enable independent retention
- Automated compliance reports are the maturity signal: manual report generation means the system is not ready
