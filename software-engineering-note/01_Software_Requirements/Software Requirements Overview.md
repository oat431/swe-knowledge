---
tags: [software-requirements, overview, requirements-engineering, swebok]
---

# Software Requirements — Overview

> **Source:** *Software Requirements* 3rd Edition by Karl Wiegers (Microsoft Press)

## What Is This?

This vault covers **software requirements engineering** — the process of eliciting, documenting, validating, and managing requirements. It fills SWEBOK's Software Requirements KA with practical, industry-tested techniques from Karl Wiegers' classic reference.

## Files

| File | Topics | Source |
|---|---|---|
| [[01_Requirements_Fundamentals]] | Essential requirements, levels/types, customer perspective, good practices, BA role | Ch 1–4 |
| [[02_Business_and_User_Requirements]] | Vision/scope, user classes, personas, product champions | Ch 5–6 |
| [[03_Requirements_Elicitation]] | Interviews, workshops, focus groups, observation, questionnaires, interface analysis | Ch 7 |
| [[04_Use_Cases_and_Business_Rules]] | Use cases, user stories, business rules taxonomy, facts/constraints/inferences | Ch 8–9 |
| [[05_Documenting_Requirements]] | SRS, template, labeling, writing excellent requirements | Ch 10–11 |
| [[06_Requirements_Modeling]] | DFD, swimlane, state-transition, dialog maps, decision tables, UML, data dictionary | Ch 12–13 |
| [[07_Quality_and_Prototyping]] | Quality attributes, constraints, Planguage, prototyping techniques | Ch 14–15 |
| [[08_Prioritization_Validation_and_Reuse]] | MoSCoW, pairwise, $100, inspection, acceptance criteria, requirements reuse | Ch 16–19 |
| [[09_Agile_and_Project_Types]] | Agile, enhancement, packaged, outsourced, BPA, analytics, embedded | Ch 20–26 |
| [[10_Requirements_Management]] | Baseline, version control, change control, CCB, impact analysis, traceability | Ch 27–29 |
| [[11_Tools_Process_Improvement_and_Risk]] | Requirements tools, process improvement, risk management | Ch 30–32 |

## How These Topics Relate

```mermaid
flowchart TD
    FUND["Fundamentals"] --> BIZ["Business & User Req"]
    BIZ --> ELIC["Elicitation"]
    ELIC --> UC["Use Cases & Rules"]
    UC --> DOC["Documenting"]
    DOC --> MODEL["Modeling"]
    DOC --> QUAL["Quality & Prototyping"]
    MODEL --> PRIOR["Prioritization & Validation"]
    QUAL --> PRIOR
    PRIOR --> AGILE["Agile & Project Types"]
    PRIOR --> MGMT["Requirements Management"]
    MGMT --> TOOLS["Tools & Process Improvement"]
    AGILE --> MGMT
```

## Reading Paths

| Your Goal | Start Here |
|---|---|
| **Requirements basics** | [[01_Requirements_Fundamentals]] → [[02_Business_and_User_Requirements]] → [[03_Requirements_Elicitation]] |
| **Writing requirements** | [[05_Documenting_Requirements]] → [[08_Prioritization_Validation_and_Reuse]] |
| **Modeling** | [[06_Requirements_Modeling]] → [[04_Use_Cases_and_Business_Rules]] |
| **Agile requirements** | [[09_Agile_and_Project_Types]] → [[10_Requirements_Management]] |
| **Change management** | [[10_Requirements_Management]] → [[11_Tools_Process_Improvement_and_Risk]] |
| **Full requirements process** | [[01_Requirements_Fundamentals]] through [[11_Tools_Process_Improvement_and_Risk]] |

## Related

- [[../02_Software_Architecture/Software Architecture Overview|Software Architecture]] — Design decisions from requirements
- [[../05_Software_Testing/Software Testing Overview|Software Testing]] — Test cases from requirements
- [[../10_Software_Engineering_Process/Software Methodology - Overview|Software Methodology]] — Agile and process models
