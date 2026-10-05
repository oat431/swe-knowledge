---
title: Data Security, Privacy, and Compliance
role: Solutions and Enterprise Architect
capability_area: Enterprise Data Architecture
topic: Data Security, Privacy, and Compliance
status: complete
created: 2026-10-05
updated: 2026-10-05
tags:
  - career-path
  - enterprise-architect
  - data-security
  - data-privacy
  - compliance
  - data-classification
---

# Data Security, Privacy, and Compliance

> **Core skill:** Designing protection into data architecture — classification first, then encryption, access, masking, retention, and privacy mechanisms — so regulatory obligations are structural properties, not retrofit projects.

## Why This Matters

Regulators, customers, and attackers all converge on the same question: does the enterprise know where its sensitive data is and who can reach it? An architecture that cannot answer that question fails audits, amplifies breaches, and turns every privacy request into a forensic expedition. The difference between a contained incident and a crisis is usually not the control that failed but the map that was missing.

The architect's role is not to interpret law — that is the legal team's function — but to convert stated obligations into structural mechanisms. Classification is the foundation: without it, protection is applied uniformly (expensive and still wrong) or sporadically (cheap and wrong). With it, every downstream decision — encryption, access, masking, residency, retention — has a criterion to apply.

Privacy adds a temporal dimension most technical controls ignore: data has a purpose, a consent basis, a permitted lifespan, and subjects with rights over it. Designing for erasure, correction, portability, and residency after the fact is one of the most expensive forms of rework in enterprise architecture. The senior architect brings these requirements forward into the data model, the flow design, and the platform zones — where they are cheap.

## Data Classification

| Class | Definition | Handling Rules |
|-------|------------|----------------|
| **Public** | Approved for open release | No special controls; integrity matters |
| **Internal** | Business use; no external release | Access by role; standard protection |
| **Confidential** | Commercially sensitive | Need-to-know access; encryption; no shadow copies |
| **Restricted** | Regulated personal or highly sensitive | Strict access with purpose; masking by default; enhanced audit |

Classification must be attached to data assets in the catalog and enforced through access design. A classification scheme that lives only in a policy PDF changes nothing.

## Protection Mechanisms

| Mechanism | Protects Against | Design Notes |
|-----------|------------------|--------------|
| Encryption at rest | Storage compromise; media theft | Key management is the real work; per-zone keys |
| Encryption in transit | Interception; network exposure | Default for all internal traffic, not only external |
| Tokenization | Correlation without exposure | Irreversible surrogate for the highest classes |
| Masking and pseudonymization | Over-broad visibility | Dynamic at query time; static for lower environments |
| Row and column access controls | Need-to-know enforcement | Policies expressed as data, not code branches |
| Key and secret management | Credential sprawl | Central custody; rotation; separation of duties |
| Data loss prevention | Exfiltration paths | Outbound monitoring; egress control at boundaries |

## Privacy Principles into Architecture

| Principle | Architectural Mechanism |
|-----------|-------------------------|
| Purpose limitation | Purpose tags on datasets; access grants tied to stated purposes |
| Data minimization | Field-level design reviews; no collection without a use |
| Consent and lawful basis | Consent state as first-class mastered data, distributed by contract |
| Individual rights | Locate-by-subject capability; erasure and correction flows designed in |
| Residency and transfer | Placement policies per zone; transfer gateways with logs |
| Transparency | Lineage from collection to use that can answer "where did this go" |

The single most useful architectural investment for privacy is answering "where is all the data about this person?" — across operational stores, analytics copies, archives, and vendor exports. Enterprises that can answer it treat privacy as operations; those that cannot treat it as crisis management.

## Compliance Landscape

| Regime | Focus | Architectural Implications |
|--------|-------|----------------------------|
| GDPR-style laws | Personal data protection; rights of subjects | Lawful basis; erasure; residency; breach timelines |
| Consumer privacy laws | Disclosure; opt-out; sale restrictions | Consent state; purpose tracking; data inventories |
| Industry standards | Payment, health, financial controls | Segmentation; audit trails; access certification |
| Financial controls | Integrity of reporting | Immutable ledgers; segregation of duties; evidence retention |
| Sector directives | Critical infrastructure; safety | Availability design; incident reporting; supply chain controls |

Regimes vary by jurisdiction and change over time; the design principle is constant: mechanisms that can satisfy a stricter regime should be the default, and regime-specific behavior should be configuration, not rearchitecture.

## Retention and Disposal

| Concept | Rule | Mechanism |
|---------|------|-----------|
| Retention schedule | Each data class has a stated lifespan per purpose | Policy attributes on dataset metadata |
| Legal hold | Deletion suspends for named matters | Hold flags checked before disposal |
| Defensible deletion | Deletion is provable and complete across copies | Propagation of deletion through flows and archives |
| Archive retrievability | Archived data remains findable and restorable within promise | Indexed archives; tested restoration |
| Disposal evidence | The organization can show what was deleted and when | Disposal logs retained as compliance records |

Deletion is easy to promise and hard to deliver across copies, caches, backups, and partner exports. Deletion propagation belongs in the flow architecture, not in a quarterly cleanup project.

## Access Architecture

| Model | Use | Trade-Off |
|-------|-----|-----------|
| Role-based access | Stable functions with predictable needs | Role explosion without curation |
| Attribute-based access | Fine-grained, context-dependent rules | Complexity; needs mature policy tooling |
| Purpose-based access | Privacy-sensitive data with stated uses | Requires purpose capture and audit |
| Break-glass access | Emergencies and incident response | Strong logging and post-hoc review mandatory |
| Access review cadence | All of the above | Unowned grants accumulate without periodic certification |

## The Protection Lifecycle

```mermaid
flowchart LR
    CLASSIFY["Classify data by sensitivity"] --> PROTECT["Apply protection - encryption, masking, tokens"]
    PROTECT --> ACCESS["Control access by role and purpose"]
    ACCESS --> MONITOR["Monitor, log, and audit usage"]
    MONITOR --> RETAIN["Retain and dispose per policy"]
```

## Practical Applications

### Data Protection Checklist

- [ ] Every catalogued dataset carries a classification and a purpose
- [ ] Encryption and key management are designed per class, not per system
- [ ] Access follows role and purpose, with periodic certification built in
- [ ] Erasure and correction can propagate through every copy and export
- [ ] Retention schedules attach to datasets, with holds and disposal evidence
- [ ] Children and derived data are covered — not just primary records

### Classification and Handling Matrix

```markdown
## Classification and Handling

| Aspect | Internal | Confidential | Restricted |
|--------|----------|--------------|------------|
| Access basis | Role | Need to know | Purpose plus role |
| At rest | Standard encryption | Dedicated keys | Tokenization where feasible |
| In non-production | Masked | Synthetic preferred | Prohibited or tokenized |
| Retention | Per business need | Per schedule | Per regulation, minimal |
| Audit | Standard logs | Enhanced logging | Full access audit |
| Sharing | Business units | Contracted partners only | Prohibited without approval |
```

## Common Pitfalls

| Pitfall | Why It Is a Problem | Better Approach |
|---------|---------------------|-----------------|
| **Security bolted on** | Retrofits cost multiples and still leave gaps | Classify and design controls at the model and flow stage |
| **Classification once** | Data sensitivity changes as it combines and ages | Re-review on schema change and periodic recertification |
| **Erasure optimism** | Copies, backups, and exports make deletion hard | Design deletion propagation into flows; test it |
| **One control for all** | Uniform protection is expensive and still under-protects the sensitive | Controls scaled to class and purpose |
| **Privacy as legal checkbox** | Rights requests become manual archaeology | Locate-by-subject and rights flows as platform capabilities |
| **Neglecting derived data** | Aggregates and models can re-expose individuals | Apply privacy review to derived and trained artifacts |

## Success Indicators

- Access reviews happen and result in actual grant removals
- Subject rights requests are fulfilled from tooling within regulatory timelines
- Deletion runs are provable across stores, caches, and exports
- New projects classify data before design approval as a matter of course
- Audit findings trend down because evidence is generated, not assembled

## Related Topics

- [[03_Data_Governance_and_Quality]]: shared policy and stewardship machinery
- [[07_Data_Lifecycle_and_Value_Realization]]: retention and disposal across the lifecycle
- [[05_Security_Risk_and_Compliance/00_overview|Security Risk and Compliance]]: the enterprise security and risk frame
- [[career-path/06_Software_Architect/05_Security_Architecture/00_overview|Security Architecture (Architect)]]: solution-level security architecture practice

## Summary

Data security, privacy, and compliance become tractable when they are structural: classification attached to real assets, protection scaled to class, access governed by role and purpose, rights and residency designed into flows, and retention with provable disposal. The senior architect converts legal obligations into mechanisms at the point where they are cheapest — model, zone, and flow design — and treats competence as the ability to answer, quickly and completely, where any piece of sensitive data is and who can reach it.
