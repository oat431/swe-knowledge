---
document_type: MBSE Models (SysML)
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
architect: "[Solution Architect]"
classification: "Internal / Confidential"
tags: [mbse, sysml, model-based, sebok, iso-24641]
standard_ref:
  - SEBoK v2 — System Architecture
  - ISO/IEC/IEEE 24641 — Model-Based Systems Engineering
---

# MBSE Models (SysML)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This document captures Model-Based Systems Engineering (MBSE) artifacts using SysML notation. MBSE uses models (not just documents) as the primary means of capturing system requirements, design, and analysis.

## 2. MBSE Approach

| Aspect | Approach |
|--------|---------|
| [Modeling Language] | [SysML (OMG Systems Modeling Language)] |
| [Modeling Tool] | [Draw.io / PlantUML / Cameo] |
| [Model Scope] | [Requirements, structure, behavior, parametrics] |
| [Model Purpose] | [Communication, analysis, verification] |

## 3. SysML Diagram Types Applied

| Diagram Type | Purpose | Applied | Status |
|-------------|---------|---------|--------|
| [Requirements Diagram] | [Requirements structure and relationships] | ✅ | Complete |
| [Block Definition Diagram (BDD)] | [System structure — blocks, components] | ✅ | Complete |
| [Internal Block Diagram (IBD)] | [Internal structure — ports, flows] | ✅ | Complete |
| [Activity Diagram] | [Behavioral — process flows] | ✅ | Complete |
| [Sequence Diagram] | [Behavioral — interactions] | ✅ | Complete |
| [State Machine Diagram] | [Behavioral — state transitions] | ✅ | Complete |
| [Use Case Diagram] | [Functional — actor-goal modeling] | ✅ | Complete |
| [Parametric Diagram] | [Analysis — constraints and equations] | ⬜ | Not Started |

## 4. Requirements Diagram

```mermaid
flowchart TD
    BIZ[Business<br>Requirements] --> SYS[System<br>Requirements]
    SYS --> SW[Software<br>Requirements]
    SW --> TEST[Test<br>Cases]

    BIZ --> BR01[BR-XX: Business<br>Requirement]
    BIZ --> BR02[BR-XX: Business<br>Requirement]
    SYS --> SYRS01[SYRS-XXX: System<br>Requirement]
    SYS --> SYRS02[SYRS-XXX: System<br>Requirement]
    SW --> FR01[FR-XXX: Software<br>Requirement]
    SW --> FR02[FR-XXX: Software<br>Requirement]

    BR01 -.->|satisfies| SYRS01
    BR02 -.->|satisfies| SYRS02
    SYRS01 -.->|refines| FR01
    SYRS02 -.->|refines| FR02

    style BIZ fill:#4CAF50,color:#fff
    style SYS fill:#2196F3,color:#fff
    style SW fill:#FF9800,color:#fff
    style TEST fill:#9C27B0,color:#fff
```

## 5. Block Definition Diagram (BDD)

```mermaid
flowchart TD
    subgraph System["System"]
        BLOCK1[Block 1]
        BLOCK2[Block 2]
        BLOCK3[Block 3]
        BLOCK4[Block 4]
        BLOCK5[Block 5]
        EXT[External Systems]
    end

    subgraph Services_Detail["Sub-Blocks"]
        SUB1[Sub-block 1]
        SUB2[Sub-block 2]
        SUB3[Sub-block 3]
        SUB4[Sub-block 4]
    end

    BLOCK4 --> SUB1
    BLOCK4 --> SUB2
    BLOCK4 --> SUB3
    BLOCK4 --> SUB4

    style System fill:#1a237e,color:#fff
    style Services_Detail fill:#2196F3,color:#fff
```

## 6. Sequence Diagram: [Scenario Name]

```mermaid
sequenceDiagram
    actor Actor
    participant ComponentA
    participant ComponentB
    participant ComponentC
    participant ComponentD
    participant DB
    participant ComponentE

    Actor->>ComponentA: [Action]
    ComponentA->>ComponentB: [Request]
    ComponentB->>ComponentB: [Process]
    ComponentB->>ComponentC: [Call]
    ComponentC->>DB: [Persist]
    ComponentC-->>ComponentB: [Response]
    ComponentB-->>ComponentA: [Response]
    ComponentA-->>Actor: [Result]

    ComponentC->>ComponentD: Event: [EventName]
    ComponentD->>ComponentD: [Process]
    ComponentD->>ComponentD: [Process]
    ComponentD->>DB: [Update]
    ComponentD->>ComponentE: Event: [EventName]
    ComponentE->>Actor: [Notification]
```

## 7. State Machine Diagram: [Entity] Lifecycle

```mermaid
stateDiagram-v2
    [*] --> State1: [Trigger]
    State1 --> State2: [Trigger]
    State2 --> State3: [Trigger]
    State3 --> State4: [Guard condition met]
    State3 --> State5: [Guard condition failed]
    State5 --> State1: [Recovery action]
    State4 --> State6: [Trigger]
    State6 --> State7: [Trigger]
    State7 --> State8: [Trigger]
    State8 --> [*]
    State6 --> [*]
```

## 8. Activity Diagram: [Decision Logic Name]

```mermaid
flowchart TD
    START([Activity Start]) --> C1{Condition<br>1?}
    C1 -->|Yes| C2{Condition<br>2?}
    C1 -->|No| C3{Condition<br>3?}
    C2 -->|Yes| C4{Condition<br>4?}
    C2 -->|No| MANUAL[Exception Path]
    C3 -->|Yes| C5{Condition<br>5?}
    C3 -->|No| MANUAL
    C5 -->|Yes| APPROVE[Success Path]
    C5 -->|No| MANUAL
    C4 -->|Yes| APPROVE
    C4 -->|No| MANUAL

    style START fill:#4CAF50,color:#fff
    style APPROVE fill:#4CAF50,color:#fff
    style MANUAL fill:#FF9800,color:#fff
```

## 9. Model Traceability

| Model Element | Requirement | Design | Test |
|--------------|-----------|--------|------|
| [Model Element] | [FR-XXX] | [Design Element] | [TC-XXX] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[System-Architecture-Description]] | Architecture modeled here |
| [[Software-Requirements-Specification]] | Requirements modeled here |
| [[Functional-Architecture]] | Functions modeled here |
| [[Architecture-Views-4-1]] | Views using MBSE models |

---

> **Template Standard:** Based on SEBoK v2, ISO/IEC/IEEE 24641
> **Usage:** MBSE models are *executable* and *analyzable* — unlike documents. Use them for complex systems where visual models reduce ambiguity. Keep models in sync with requirements and code.