---
title: "Data Classification and Sensitivity"
note_type: capability-topic
capability_area: data-security-and-privacy
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - data-classification
  - data-security
---

# Data Classification and Sensitivity

> Categorizing data by sensitivity level so that protection measures match the risk of exposure.

## Why This Is a Senior Skill

Mid-level engineers tag columns as "sensitive" when told. Senior engineers design classification frameworks that scale across hundreds of tables, automate discovery of unclassified sensitive data, and negotiate handling rules with legal and compliance teams.

The hard part is not labeling a Social Security number. It is deciding whether a combination of non-sensitive fields creates a re-identification risk, or whether derived analytics output inherits the sensitivity of its inputs.

## Core Frameworks

### Sensitivity Tier Model

| Tier | Examples | Handling Rules | Access Scope |
|------|----------|---------------|--------------|
| Public | Product catalog, public docs | Standard encryption | Anyone |
| Internal | Employee directory, internal metrics | Encrypted at rest, RBAC | All employees |
| Confidential | Financial records, contracts | Field-level encryption, column masking | Need-to-know teams |
| Restricted | PII, PHI, payment data | Tokenized, row-level security, audit log | Named individuals |
| Secret | Encryption keys, credentials | Hardware security module, split knowledge | Security team only |

### Classification Decision Matrix

| Data Attribute | Tier Impact | Rationale |
|----------------|------------|-----------|
| Directly identifies a person | Restricted | PII regulation applies |
| Indirectly identifies when combined | Confidential to Restricted | Re-identification risk |
| Financial transaction detail | Restricted | PCI-DSS, SOX |
| Health information | Restricted | HIPAA |
| Aggregated and anonymized | Internal | Lower risk if k-anonymity holds |
| Model weights or training data | Confidential | IP and potential data leakage |

### Automated Discovery Signals

| Signal | What It Detects | Tool Approach |
|--------|----------------|---------------|
| Regex patterns | SSN, email, phone, card numbers | Column-level scanning |
| Statistical uniqueness | Quasi-identifiers | k-anonymity measurement |
| Metadata tags | Already-classified sources | Catalog propagation |
| NLP on column names | Implied sensitivity | ML classifier on schema |
| Data flow tracing | Derived sensitive data | Lineage-aware propagation |

## In Practice

**Start with a schema scan.** Run regex-based discovery on all production tables. Expect 30-40% false positives on column names alone. Combine with sample-data scanning for higher confidence.

**Propagate through lineage.** If a Restricted column feeds an aggregation, the output may still be Restricted if the aggregation is not sufficiently anonymized. Senior engineers trace sensitivity through transformations, not just at rest.

**Negotiate handling rules.** Legal says "encrypt everything." Engineering says "field-level encryption adds 40% latency to this dashboard." The senior engineer finds the middle ground: tokenization for the dashboard query path, full encryption for the source of record.

**Reclassify periodically.** Data sensitivity changes. A public dataset becomes Confidential when combined with a newly acquired dataset. Schedule quarterly reviews and trigger reclassification on schema changes.

## Practical Exercise

Choose 5 tables in a system you work with. For each:
1. List every column and assign a sensitivity tier
2. Identify any quasi-identifiers: columns that are not sensitive alone but become sensitive in combination
3. Write handling rules for each tier: who can see it, how it is stored, how it is transmitted
4. Trace one sensitive column through its lineage: where does it originate, what transformations touch it, where does it land

Document your classification in a markdown table and share it with your team for review.

## Knowledge Connections

- [[05_Data_Security]]: DMBOK security fundamentals
- [[02_Encryption_and_Tokenization]]: protection mechanisms driven by classification
- [[03_Access_Control_for_Data]]: access rules derived from tiers
- [[04_Privacy_Engineering]]: regulatory drivers for Restricted tier
- [[05_Audit_and_Lineage_for_Compliance]]: proving classification is enforced

## Common Pitfalls

- Classifying all data as Restricted: this makes every query expensive and every access request a bottleneck
- Ignoring derived data sensitivity: aggregation of non-sensitive fields can create a re-identification risk
- One-time classification projects: data changes, sensitivity changes, classification must be continuous
- Relying solely on column names for discovery: "user_notes" could contain PII or could be harmless text

## Key Takeaways

- Classification is the foundation: every other security control depends on knowing what needs protection
- Automated discovery catches 60-70% of sensitive data; human review handles the rest
- Sensitivity propagates through lineage: derived data inherits the classification of its inputs unless explicitly de-identified
- Quasi-identifiers are the hidden risk: non-sensitive columns that enable re-identification when combined
- Classification must be a living process, not a one-time project
- Senior engineers negotiate handling rules with legal and engineering trade-offs, not just compliance checklists
