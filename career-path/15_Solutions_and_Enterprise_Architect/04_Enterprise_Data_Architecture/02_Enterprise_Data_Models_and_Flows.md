---
title: Enterprise Data Models and Flows
role: Solutions and Enterprise Architect
capability_area: Enterprise Data Architecture
topic: Enterprise Data Models and Flows
status: complete
created: 2026-10-05
updated: 2026-10-05
tags:
  - career-path
  - enterprise-architect
  - data-modeling
  - master-data
  - data-flows
---

# Enterprise Data Models and Flows

> **Core skill:** Maintaining the conceptual enterprise data model — the core entities and relationships every system shares — and the cross-system flows that move them, so the organization argues about data in one vocabulary.

## Why This Matters

Every enterprise already has a data model; the question is whether it is written down. Without a shared one, the same concept carries five names across five systems, "customer" means something different in billing and in marketing, and every integration project begins by negotiating reality from scratch. The conceptual enterprise model — a few dozen entities, their definitions, and their relationships — is the cheapest interoperability asset an enterprise can own.

Below the conceptual level sit the logical models of domains and the physical schemas of systems. The senior architect's discipline is matching ambition to level: keep the conceptual model small, stable, and owned; let domains maintain logical models; never attempt to unify physical schemas across the enterprise. Enterprises that try to standardize physical models everywhere spend years and produce a model nobody uses.

The model's counterpart is the flow view: where each entity is created, mastered, replicated, and consumed. Entities and flows together answer the two questions that recur forever — what does this data mean, and where does it live — and they turn master data management from a tool purchase into an architectural decision about which system is the authority for what.

## Levels of Data Modeling

| Level | Audience | Content | Change Frequency |
|-------|----------|---------|------------------|
| **Conceptual** | Business and architecture | Core entities, definitions, relationships | Rarely; on strategy or scope shifts |
| **Logical** | Domain architects and analysts | Attributes, keys, normalization, domains | Per domain change cycle |
| **Physical** | Engineers | Tables, types, indexes, partitioning | Per system release |

The enterprise model lives at the conceptual level. Its power is proportional to its parsimony: a hundred entities nobody reads is worse than thirty that every project references.

## Core Enterprise Entities

| Entity Pattern | Meaning | Examples | Typical Authority |
|----------------|---------|----------|-------------------|
| **Party** | A person or organization the enterprise deals with | Customer, prospect, supplier, employee | CRM or identity hub |
| **Product** | Something offered or managed | Goods, services, bundles, plans | PIM or ERP |
| **Agreement** | A commitment between parties | Contract, order, subscription, policy | Contract or order systems |
| **Account** | A financial or service relationship | Billing account, credit facility, login | Billing or ERP |
| **Asset** | Something owned or operated | Equipment, property, fleet, licenses | EAM or asset systems |
| **Location** | A place or address | Site, warehouse, region, delivery point | Location services or ERP |
| **Transaction** | A recorded event of economic or operational meaning | Sale, payment, shipment, claim | Ledger or transactional systems |
| **Reference** | Shared codes and classifications | Currency, country, industry code | Reference data hub |

The party pattern is the enterprise workhorse: most organizations discover that customer, supplier, and employee data are the same party with different roles, and that insight alone resolves years of duplicated identity debates.

## Master Data, Reference Data, and Transactional Data

| Data Class | Definition | Examples | Management Approach |
|------------|------------|----------|---------------------|
| Master data | Core entities shared across processes | Customer, product, employee, supplier | Governance plus a mastering model |
| Reference data | Codes and classifications used across systems | Country, currency, cost center | Central definition; wide distribution |
| Transactional data | Records of events and interactions | Orders, payments, tickets | Owned by the process system; referenced by others |

Master data management models, in rising order of centralization:

| MDM Style | Mechanism | Trade-Off |
|-----------|-----------|-----------|
| Registry | Index of keys across sources; no data consolidation | Lightest; no single truth, only reference |
| Consolidation | Copies merged for reporting and insight | Read-only central view; sources remain authoritative |
| Coexistence | Hub masters some attributes; sources others | Complex but politically realistic |
| Centralized | Hub is the sole authoring point | Cleanest data; heaviest process change |

Choose the lightest style the governance reality can sustain. Centralized MDM imposed on unwilling business units becomes a registry in practice — after the budget is spent.

## Systems of Record, Reference, and Insight

| Role | Definition | Consequence |
|------|------------|-------------|
| System of record | Authoritative creator and editor of an entity | Owns quality and definition of that data |
| System of reference | Read-only consumer that others trust for lookup | Syncs from record; never competes on truth |
| System of insight | Analytical consumer; adds no operational truth | Never feeds corrections back without a governed path |

The classic enterprise pathology is a system of insight quietly becoming a system of record — a reporting database whose corrections actually drive operations. Naming each system's role, and enforcing it, prevents the shadow governance that follows.

## Cross-System Flows

| Flow Pattern | When It Applies | Design Rule |
|--------------|-----------------|-------------|
| Master distribution | One authority, many consumers | Publish on change with versioned contracts |
| Reference propagation | Codes used everywhere | Central definition; cache-tolerant distribution |
| Transactional handoff | Process spans systems | Explicit ownership of state transitions per system |
| Analytical replication | Operational data feeds insight | One-way; latency stated; no write-back without governance |

Every flow needs five facts: source authority, target, pattern, latency expectation, and failure behavior. A flow without a stated authority is a future reconciliation incident.

## The Model and Flow View

```mermaid
flowchart LR
    SOURCES["Source systems - systems of entry"] --> RESOLVE["Identity resolution and golden record"]
    RESOLVE --> MASTER["Master and reference data hub"]
    MASTER --> DISTRIBUTE["Governed distribution"]
    DISTRIBUTE --> CONSUMERS["Consumers - operations, analytics, partners"]
```

## Practical Applications

### Enterprise Model Checklist

- [ ] A conceptual model exists with thirty to sixty entities, defined in business language
- [ ] Every entity names its authority system and its owning domain
- [ ] Master data is managed with a stated style — registry, consolidation, coexistence, or central
- [ ] Each system's role is declared: record, reference, or insight
- [ ] Cross-system flows are registered with authority, latency, and failure behavior
- [ ] New projects adopt model entities rather than defining private variants

### Entity Definition Template

```markdown
# Entity: [Name]

## Definition
[One paragraph in business language; what it is and is not]

## Relationships
- [Related entities and cardinality]

## Authority
- System of record: [system]
- Domain owner: [name]

## Key Attributes
- [Defining attributes; identifier strategy]

## Flows
- [Where this entity is created, replicated, consumed]

## Variants to Reconcile
- [Known local synonyms across systems]
```

## Common Pitfalls

| Pitfall | Why It Is a Problem | Better Approach |
|---------|---------------------|-----------------|
| **Physical-first modeling** | Diving into schemas before shared concepts guarantees rework | Fix the conceptual model first; derive logical models per domain |
| **Model without owners** | A model nobody maintains diverges from reality within months | Assign entity owners and review changes through them |
| **Entity name drift** | Customer, client, and party become three truths | Publish a glossary; reconcile variants as part of the model |
| **MDM as a tool project** | Software is bought before mastering decisions are made | Decide style, authority, and ownership — then choose tooling |
| **No identifier strategy** | Matching records becomes impossible across systems | Define keys and resolution rules as architecture |
| **Insight systems becoming record** | Shadow authority creates divergent truth | Declare roles per system; govern any write-back path |

## Success Indicators

- Integration projects begin from the enterprise model instead of negotiating terms
- The glossary is cited in design reviews and business discussions alike
- Master data issues are routed to a named authority, not a working group
- New entities are added through the model's review, with flow registrations attached
- Reconciliation effort declines measurably as authorities are respected

## Related Topics

- [[01_Data_as_an_Enterprise_Asset]]: the ownership structure the model assumes
- [[04_Data_Integration_and_Interoperability]]: how flows are carried between systems
- [[career-path/06_Software_Architect/06_Data_Architecture/00_overview|Data Architecture (Architect)]]: solution-level modeling that this model frames
- [[03_Enterprise_Architecture_Practice/00_overview|Enterprise Architecture Practice]]: the wider landscape these entities live in

## Summary

The enterprise data model is a small, stable set of core entities and relationships defined in business language, paired with a flow view that names each entity's authority and each system's role — record, reference, or insight. The senior architect keeps the model conceptual, delegates logical and physical detail to domains, chooses the lightest master data style the organization can sustain, and treats identifier strategy and flow registration as architecture rather than project detail.
