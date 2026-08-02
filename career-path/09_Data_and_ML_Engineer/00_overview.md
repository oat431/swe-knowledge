---
title: "Data and Machine Learning Engineer"
note_type: career-path-overview
career_family: specialist-engineering
level: senior-specialist
entry_from:
  - "[[career-path/02_Senior_Software_Engineer/00_overview|Senior Software Engineer]]"
next_paths:
  - "[[career-path/03_Staff_Engineer/00_overview|Staff Engineer]]"
  - "[[career-path/06_Software_Architect/00_overview|Software Architect]]"
  - "[[career-path/11_Engineering_Manager/00_overview|Engineering Manager]]"
source_frameworks:
  - "[[DMBoK v2 - Overview]]"
  - "[[SWEBOK v4 - Overview]]"
  - "[[CyBOK v1 - Overview]]"
tags:
  - career-path
  - data-engineering
  - machine-learning
  - specialist-engineering
---

# Data and Machine Learning Engineer

> **Positioning:** A specialist engineering path focused on building reliable data platforms, analytical systems, and machine-learning capabilities.

## What This Path Is

Data engineering focuses on making data available, trustworthy, governed, and useful. Machine learning engineering adds the responsibility of building, deploying, monitoring, and improving models as part of production systems.

These are related but distinct specializations. A data engineer usually emphasizes data pipelines, storage, integration, quality, and governance. An ML engineer usually emphasizes features, model serving, evaluation, experimentation, and ML operations. Both require strong software engineering.

## Primary Outcomes

- Reliable data pipelines and platforms
- Data that is discoverable, governed, secure, and fit for purpose
- Reproducible analytical and machine-learning workflows
- Models that are monitored after deployment
- Clear ownership of data quality and model behavior
- Efficient systems that balance latency, accuracy, cost, and maintainability

## Capability Areas

| Capability | Specialist behavior | Existing vault anchor |
|---|---|---|
| Data architecture | Designs data storage, movement, ownership, and lifecycle | [[02_Data_Architecture]] |
| Data modeling | Chooses models appropriate for operational and analytical use | [[03_Data_Modeling_and_Design]] |
| Data integration | Builds reliable ETL, ELT, streaming, and interface contracts | [[06_Data_Integration_and_Interoperability]] |
| Data quality | Defines quality dimensions, rules, profiling, and monitoring | [[11_Data_Quality]] |
| Data security | Protects sensitive data and supports privacy obligations | [[05_Data_Security]] |
| ML lifecycle | Handles training, evaluation, serving, monitoring, and retraining | [[computing-foundation-note/Artificial_Intelligence/AI Overview]] |
| Production engineering | Operates distributed systems and automation safely | [[software-engineering-note/06_Software_Engineering_Operations/Software Engineering Operations Overview]] |

## Typical Progression

```mermaid
flowchart LR
    SENIOR["Senior Software Engineer"] --> DATA["Build reliable data systems"]
    DATA --> PLATFORM["Own data or ML platform"]
    PLATFORM --> SYSTEMS["Lead data-intensive system design"]
    SYSTEMS --> STRATEGY["Shape organization-wide data and AI direction"]
```

## Signals for Moving Forward

- You are interested in data correctness and lifecycle, not only application behavior.
- You can reason about schema evolution, lineage, privacy, and operational failure.
- You are comfortable with statistics, experimentation, or model evaluation.
- You can explain the limits of data and models to non-specialists.
- You can make data or ML capabilities usable by other teams.

## Evidence to Build

- Data architecture blueprint
- Data model and data contract
- ETL or streaming pipeline design
- Data quality scorecard
- Data lineage documentation
- ML model card and evaluation report
- ML monitoring and rollback plan
- Data governance or privacy assessment

## Nearby Paths

- [[career-path/06_Software_Architect/00_overview|Software Architect]]: broader system structure
- [[career-path/07_SRE_and_Platform_Engineer/00_overview|SRE and Platform Engineer]]: platform reliability and operations
- [[career-path/08_Security_Engineer/00_overview|Security Engineer]]: data and model protection
- [[career-path/03_Staff_Engineer/00_overview|Staff Engineer]]: cross-team technical influence
- [[career-path/14_Product_Manager/00_overview|Product Manager]]: data-informed product decisions

## Suggested Future Note Route

1. [[DMBoK v2 - Overview]]
2. [[02_Data_Architecture]]
3. [[03_Data_Modeling_and_Design]]
4. [[06_Data_Integration_and_Interoperability]]
5. [[10_Metadata_Management]]
6. [[11_Data_Quality]]
7. [[05_Data_Security]]
8. [[computing-foundation-note/Artificial_Intelligence/AI Overview]]
9. [[computing-foundation-note/Artificial_Intelligence/09_AI_SE_Intersection]]

## Sources

- [[DMBoK v2 - Overview]]
- [[SWEBOK v4 - Overview]]
- [[CyBOK v1 - Overview]]

## Related

- [[00_Career_Path_Overview]]
- [[career-path/02_Senior_Software_Engineer/00_overview|Senior Software Engineer]]
- [[career-path/07_SRE_and_Platform_Engineer/00_overview|SRE and Platform Engineer]]
- [[career-path/08_Security_Engineer/00_overview|Security Engineer]]
