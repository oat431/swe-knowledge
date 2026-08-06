---
title: "Privacy Engineering"
note_type: capability-topic
capability_area: data-security-and-privacy
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
  - "[[01_Data_Classification_and_Sensitivity]]"
tags:
  - career-path
  - data-engineering
  - privacy
  - GDPR
  - CCPA
---

# Privacy Engineering

> Embedding privacy obligations into data pipelines and systems so compliance is automatic, not manual.

## Why This Is a Senior Skill

Mid-level engineers implement privacy features when given specifications. Senior engineers translate legal requirements into engineering constraints, design pipelines that support data subject rights at scale, and build systems where compliance is structural rather than procedural.

The senior challenge is making privacy enforceable in code so that a pipeline failure does not accidentally violate a regulation.

## Core Frameworks

### Privacy Regulation to Engineering Mapping

| Regulation | Key Requirement | Engineering Implementation |
|------------|----------------|---------------------------|
| GDPR Article 5 | Data minimization | Collect only fields with documented purpose |
| GDPR Article 6 | Lawful basis tracking | Tag each data source with its legal basis |
| GDPR Article 17 | Right to erasure | Build subject-level deletion across all stores |
| GDPR Article 20 | Data portability | Export subject data in machine-readable format |
| GDPR Article 25 | Privacy by design | Privacy checks in schema review and pipeline design |
| CCPA | Right to know | Query all systems for a subject's data footprint |
| CCPA | Right to delete | Same as erasure with California-specific scope |
| CCPA | Opt-out of sale | Consent flag checked at every data-sharing boundary |

### Data Minimization Decision Matrix

| Question | If Yes | If No |
|----------|--------|-------|
| Is this field needed for the stated purpose? | Include with documented purpose | Do not collect |
| Can we use a less granular version? | Use aggregated or truncated form | Use full precision |
| Do we need to retain it beyond the purpose period? | Set retention policy and TTL | Auto-delete after purpose fulfilled |
| Can we pseudonymize before storage? | Store pseudonymized form with separate mapping | Store with full encryption and access control |

### Consent Management Architecture

| Component | Responsibility | Failure Mode |
|-----------|---------------|--------------|
| Consent store | Records what consent was given, when, by whom | Consent lost: must assume no consent |
| Consent propagation | Pushes consent state to all downstream systems | Lag: data processed before consent update arrives |
| Consent audit trail | Logs every consent change with timestamp | Audit gap: cannot prove consent at point in time |
| Consent enforcement | Checks consent before processing | Bypass: pipeline ignores consent flag |
| Withdrawal handling | Processes consent withdrawal within legal timeframe | Delay: continued processing after withdrawal |

## In Practice

**Build erasure as a pipeline, not a script.** The right to erasure means deleting a subject's data from every system: operational database, data warehouse, data lake, backup, and ML training sets. A manual script misses systems. A pipeline with a manifest of all subject-data locations ensures completeness.

**Tag data with its legal basis.** Every column should have metadata recording why it was collected: consent, contract performance, legal obligation, or legitimate interest. When a legal basis expires or consent is withdrawn, the tag triggers automated review.

**Design for portability from the start.** Data portability means exporting all of a subject's data in a structured format. If your data is scattered across 20 systems with different schemas, this becomes a multi-week project. Design a subject-data index that maps subject ID to all data locations.

**Treat consent as a first-class data entity.** Consent is not a boolean flag on a user record. It has scope: what processing, what data, what duration. Model consent with granularity so that withdrawing consent for marketing does not affect consent for service delivery.

**Test privacy like you test correctness.** Write integration tests that verify: erasure removes data from all stores within the legal timeframe, consent withdrawal stops processing within 72 hours, data export includes all systems in the manifest.

## Practical Exercise

Design a right-to-erasure pipeline for a system you work with:
1. Identify all systems that store subject-level data
2. For each system, define the erasure method: hard delete, anonymize, or pseudonymize
3. Design the orchestration: how does the erasure request flow across systems?
4. Handle the edge cases: what about data in backups, aggregated analytics, and ML training sets?
5. Define verification: how do you prove erasure was complete?
6. Set a target SLA: erasure complete within how many days?

## Knowledge Connections

- [[05_Data_Security]]: DMBOK privacy and security intersection
- [[CyBOK v1 - Overview]]: privacy knowledge area
- [[01_Data_Classification_and_Sensitivity]]: classification identifies privacy-relevant data
- [[05_Audit_and_Lineage_for_Compliance]]: proving privacy compliance to regulators
- [[06_Secure_Data_Sharing]]: privacy obligations at sharing boundaries

## Common Pitfalls

- Treating consent as a boolean: consent has scope, duration, and granularity
- Building erasure as a one-off script: erasure must be a repeatable pipeline with a system manifest
- Forgetting ML training data: right to erasure includes removing a subject's data from training sets
- Privacy reviews only at launch: ongoing processing requires ongoing consent verification

## Key Takeaways

- Privacy is an engineering concern, not just a legal one: regulations map to specific system capabilities
- Right to erasure requires a system manifest: you cannot delete what you cannot find
- Consent is a first-class data entity with scope, duration, and granularity
- Data minimization is a design-time decision, not a cleanup activity
- Privacy compliance needs automated tests, not just manual audits
- Senior engineers translate legal language into pipeline constraints that are enforceable in code
