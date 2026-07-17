---
tags:
- quality-attributes
- safety
- security
- resilience
- systems-engineering
- sebok
---

# 18 — SE and Quality Attributes

> *Source: SEBoK v2, Part 6 — Systems Engineering and Quality Attributes*

## Purpose

This chapter summarizes the **Systems Engineering and Quality Attributes** Knowledge Area (KA) from Part 6 of the SEBoK. Every system has properties that are largely a function of the system as a whole rather than just its constituent parts — properties such as security, reliability, safety, resistance to electromagnetic interference, usability, and resilience. Alternatively called **quality attributes**, **non-functional requirements**, **-ilities**, or **specialties**, these properties are often interdependent rather than orthogonal; trade-offs among them are a core responsibility of the systems engineer. The term **specialty engineering** refers to the collective engineering of all quality attributes of a system.

---

## Framework for Viewing Quality Attributes

### Loss-Driven Systems Engineering (LDSE)

A unifying framework views many quality attributes through the **lens of loss**. Loss-driven systems engineering (LDSE) is the value-adding unification of specialty areas that address potential losses associated with developing and using systems — including resilience, safety, security, operational risk, environmental protection, quality, and availability.

**Commonality among loss-driven areas**: They share assets, loss types, adversity types, coping strategies, and aspects of the system/environment under consideration. Synergies include shared loss scenarios, requirements, modeling/analysis techniques, architecture/design solutions, and risk management.

**Core LDSE Principles** (Winstead 2020):
- SE minimizes hazards; controls hazards that cannot be avoided
- SE uses proven processes, solutions, methods, and materials when they achieve intended trustworthiness
- The human within the system should be enabled to prevent, minimize, and recover from loss
- SE should strive for the simplest solutions
- SE produces evolvable systems likely to maintain or improve loss-driven properties through change
- Actions should trace to the entity responsible
- Any critical task should be possible to perform in more than one way

**LDSE Design Techniques**: Anomaly detection, commensurate protection/response, continuous protection, defense-in-depth, distributed privilege, diversity, domain separation, least functionality/persistence/privilege/sharing, loss margins, mediated access, protective defaults/failure/recovery, and redundancy.

### Integration of Specialty Engineering

Specialty engineering must be integrated throughout the project life cycle. The systems engineering process ensures participation of specialty disciplines at key decision points. Design alternatives pass through three conceptual filters: (1) traditional design considerations (structural, electronics, thermodynamics), (2) specialty discipline evaluation (safety, affordability, RAM, human factors, producibility), and (3) facility/equipment/procedural/personnel integration to produce the final requirements.

---

## Human Systems Integration

**Human Systems Integration (HSI)** is "the management and technical discipline of planning, enabling, coordinating, and optimizing all human-related considerations during system design, development, test, production, use, and disposal" (SAE 2019). Initiated by the U.S. DoD as part of the "total system approach" to acquisition, HSI aims to optimize total system performance — hardware, software, and human.

### Seven HSI Domains (DoD)

1. **Manpower** — Determining the most efficient and cost-effective mix of manpower and contract support
2. **Personnel** — Selecting appropriate cognitive, physical, and social capabilities for operators and maintainers
3. **Training** — Developing efficient options for individual, collective, and joint training
4. **Human Factors Engineering (HFE)** — Designing human-machine interfaces consistent with user physical, cognitive, and sensory abilities
5. **Safety & Occupational Health** — Minimizing risks of illness, disability, injury, and death
6. **Force Protection & Survivability** — Protecting individuals from direct threat events and accidents
7. **Habitability** — Establishing living and working conditions that sustain morale, safety, health, and comfort

### Key Practices

- HSI activities must begin early in system development and continue throughout
- The human systems integrator is a member of the systems engineering team, not a domain specialist
- HSI should not be confused with HFE — HFE is a domain within HSI; HSI is about mutual integration of technology, organizations, and people
- Key tools: C3TRACE, CHIEF, HARPS, HSIF, IMPRINT
- Pitfall: Relegating HSI to a "specialty engineering" team deprives the integrator of sufficient scope

---

## Manufacturability and Producibility

**Manufacturability** is the ease of manufacture; **producibility** also encompasses other dimensions of the production task including packaging and shipping. Both attributes can be improved by incorporating proper design decisions that consider the entire system life cycle.

The machines and processes used to build a system must be architected and designed — manufacturing equipment and processes can sometimes cost more than the system being built. Manufacturability and producibility can be a discriminator between competing system solution concepts and must be considered early, as well as during design maturation.

Key references include Design for Manufacturability (Boothroyd, Dewhurst, and Knight) and *The Art of Systems Architecting* (Maier and Rechtin).

---

## System Affordability

Affordability is the ability to fund desired investment — solutions are affordable if they can be deployed in sufficient quantity to meet mission needs within the likely available budget. **Design for affordability** is the practice of considering affordability as a design characteristic or constraint.

### Key Principles

- Cost and schedule estimation must be coupled with technical SE trade-off analyses
- Modularization of architecture around the most frequent sources of change (Parnas 1979) is a key SE principle for affordability
- Extend the focus from life cycle costs to **total ownership costs**, encompassing costs of failures
- Different stakeholders have different utility functions — resolving multi-criteria decision analysis (MCDA) problems is a central challenge
- Methods needed: new trade-off analyses between cost, schedule, effectiveness, and resilience; methods to adjust priorities to meet budgets; more affordable development processes

---

## System Hardware Assurance

**System hardware assurance** is a set of system security engineering activities to quantify and increase confidence that electronics function as intended and only as intended throughout their life cycle, and to manage identified risks.

### Scope

- Focuses on electronic components (integrated circuits/chips) and their interconnections with software and firmware
- Addresses weaknesses (flaws, bugs, errors) and vulnerabilities (exploitable weaknesses)
- Concerns: adversarial exploitation, subversion of functionality, counterfeit production, loss of technological advantage

### Life Cycle Concerns

Hardware assurance should be applied from architecture and design through manufacturing, testing, deployment, sustainment, and disposal. The "bake-in" principle applies — risks created early are challenging to address later. The value of the component and the cost of mitigating risks increase at each stage.

### Risk Areas

Three basic operational risk areas: (1) failure to meet quality standards, (2) maliciously tainted goods, (3) counterfeit hardware. Counterfeits may be refurbished, re-marked, overproduced, or substandard production parts.

### Quantification Methods

- **FMEA** (Failure Mode and Effects Analysis) — semi-quantitative, adapted from quality/reliability engineering
- **Game theoretic analysis** — models of conflict and cooperation between intelligent decision-makers
- **SAE AS6171** — confidence intervals for detecting counterfeit microelectronics
- **Distributed ledger technology (DLT)** — emerging for tamper resistance, provenance, and traceability

### Risk Management

Risk management seeks to decrease weakness-based attack surfaces while improving confidence in exploitation resistance. **Mitigation-in-depth** — multiple coordinated mitigations — is essential. A **dynamic risk profile** tracks and addresses risks throughout the complete hardware and system life cycles.

---

## System Reliability, Availability, and Maintainability (RAM)

### Definitions

- **Reliability**: Probability of a product performing its intended function under stated conditions without failure for a given period of time
- **Maintainability**: Probability that a given maintenance action can be performed within a stated time interval under stated conditions using stated procedures and resources. Two categories: serviceability (ease of scheduled servicing) and repairability (ease of restoring service after failure)
- **Availability**: Probability that a repairable system or system element is operational at a given point in time. Depends on both reliability and maintainability

### Development Life Cycle Processes (GEIA 2008)

1. **Understanding user requirements and constraints** — Eliciting RAM requirements conditioned on operating environments
2. **Design for reliability** — Redundancy, diversity, built-in testing, advanced diagnostics, modularity, use of higher-strength materials, quality components, thermal/EMI analyses
3. **Production for reliability** — Repeatability, uniformity, unambiguous specifications, configuration management
4. **Monitoring during operation and use** — FRACAS (Failure Reporting and Corrective Action System), tracking operating time and outage duration

### Key Analysis Models

- **Reliability Block Diagrams (RBD)** — Graphical representation of reliability dependence; directed, acyclic graph showing paths to success
- **Fault Trees** — Graphical representation of failure modes using AND, OR, NOT, K-of-N gates; show paths to failure
- **Failure Mode Effects and Criticality Analysis (FMECA)** — Table listing failure modes, likelihood, and effects; criticality = consequence × likelihood
- **Probability distributions**: Exponential, Weibull, log-normal, generalized gamma for life data; discrete (Bernoulli, Binomial, Poisson) for expected failure counts

### Testing Types

Reliability life tests, accelerated life tests (ALT), HALT/HASS, parts screening/burn-in, stability tests, reliability growth tests (TAAF), failure/recovery tests, and maintainability tests.

### Availability Classifications

- **Inherent availability** — only corrective maintenance downtime counts
- **Achieved availability** — both corrective and preventive maintenance downtime counts
- **Operational availability** — all sources of downtime including logistical and administrative

### Data Challenges

RAM data is often censored, biased, observational, and missing covariates. Small sample sizes from expensive testing produce imprecise estimates. Sophisticated strategies are needed — proper prior planning for data collection (FRACAS design) prevents poor performance.

---

## System Resilience and Resilience Modeling

### System Resilience

**Resilience** is "the ability to provide required capability when facing adversity" (INCOSE RSWG). It encompasses both **proactive** (actions before adversity) and **reactive** (actions after adversity) perspectives.

#### Fundamental Objectives

1. **Avoid adversity** — eliminate or reduce exposure to stress
2. **Withstand adversity** — resist capability degradation when stressed
3. **Recover from adversity** — replenish lost capability after degradation

#### Means Objectives (Layer 2 Taxonomy)

Adapt, anticipate, constrain, continue, degrade gracefully, disaggregate, evade, evolve, fortify, manage complexity, minimize adversity, minimize faults, monitor, preserve integrity, prevent, reduce vulnerability, repair, replace, tolerate, and understand.

#### Architecture/Design/Operational Techniques (Layer 3)

Absorption, anomaly detection, backward/forward recovery, boundary enforcement, buffering, coordinated defense (defense-in-depth), deception, distributed privilege, distribution, diversification, domain separation, drift correction, dynamic positioning, effect tolerance, error recovery, fail soft, fault tolerance, human participation, least functionality/persistence/privilege/sharing, loose coupling, loss margins, mediated access, modularity, neutral state, protective defaults/failure/recovery, realignment, redundancy (functional and physical), repairability, replacement, safe state, segmentation, shielding, substantiated integrity, substitution, unpredictability, and virtualization.

#### Resilience Requirements Pattern

```
The <system, mode(t), state(t)> encountering <adversity(t), source, type>, 
which imposes <stress(t), metric, units, value(t)> thus affecting delivery 
of <capability(t), metric, units> during <scenario timeframe> and under 
<scenario constraints>, shall achieve <resilience target(t)> for 
<resilience metric, units, determination method>
```

#### Metrics

Common metrics include: time duration of failure/recovery, ratio of performance recovery to loss, speed of recovery, performance before/after disruption, maximum outage period/depth, expected value of capability, threat resiliency, and expected availability of required capability. The single most effective metric: **expected availability of the required capability**.

#### Affordable Resilience

Balancing cost and resilience over the system life cycle, considering both risks from adversities and opportunities in future environments. Prioritization via MAUT (Multi-attribute Utility Theory) or AHP (Analytical Hierarchy Process).

### Resilience Modeling

Resilience modeling is an emerging topic in digital engineering, MBSE, and AI/ML. No single methodology is universally accepted.

#### Approaches

- **Resilience Contracts (RCs)** — Extension of Contract-Based Design using Partially Observable Markov Decision Processes (POMDPs) to handle uncertainty and unpredictability
- **System Dynamics** — Captures behavior over time; uses causal loop diagrams, archetypes, and quantitative models (first-order linear differential equations); supports participatory model building and interactive "what-if" experimentation
- **Constraint Theory** — Checks whether complex models are internally consistent; identifies over-constrained and under-constrained computational sets that can lead to incorrect predictions

---

## System Safety

**System safety** is "the application of engineering and management principles, criteria, and techniques to achieve acceptable risk, within the constraints of operational effectiveness and suitability, time, and cost, throughout all phases of the system life cycle" (MIL-STD-882E).

### Core Concepts

- **Hazard**: A real or potential condition that could lead to a mishap resulting in death, injury, occupational illness, damage to or loss of equipment or property, or damage to the environment
- **Risk**: A combination of the severity of the mishap and the probability that the mishap will occur
- **Goal**: Reduce or eliminate severity and probability of hazards; minimize risk where hazards cannot be eliminated

### Eight System Safety Activities (AFI 91-202)

1. Document the system safety approach
2. Identify and analyze hazards over the system life cycle
3. Assess risk (severity × probability)
4. Identify and assess potential risk mitigation measures
5. Implement measures to reduce risks to acceptable levels
6. Verify risk reduction
7. Accept risks by appropriate authorities
8. Track hazards and risks throughout the system life cycle

### Key Relationships

System safety is closely tied to RAM — they share concerns about failure modes, severity, criticality, hazard identification, risk, and mitigation. Safety Working Groups (SWG) and Management Safety Review Boards (MSRB) coordinate safety across integrated product teams.

---

## System Security

**Systems Security** is about engineering for intended and authorized system behavior and outcomes despite anticipated and unanticipated adversity — threats, attacks, hazards, disruptions, and exposures.

### Three Essential Characteristics

1. Enable required system capability delivery despite intentional and unintentional adversity
2. Enforce constraints to ensure only desired behaviors and outcomes
3. Enforce constraints based on rules defining only authorized interactions and operations

### Core Principles

- Security must be engineered from inception — "Unless security is engineered into a system from its inception, there is little chance that it can be made secure by retrofit" (Anderson 1972)
- A system should be **as secure as reasonably practical (ASARP)** while meeting minimum stakeholder expectations
- **Commensurate trustworthiness**: trustworthy to a level commensurate with the most significant adverse effect resulting from loss or failure
- Focus on a system's **trustworthiness** rather than singularly on risk

### Asset Classes and Loss

| Class | Loss Protection Criteria |
|---|---|
| Material Resources & Infrastructure | Not stolen, damaged, or destroyed; function as intended |
| System Capability | Meets performance expectations; delivers only authorized capability |
| Human Resources | Not injured, suffer illness, or killed |
| Intellectual Property | Not stolen, corrupted, destroyed, or reverse-engineered |
| Data and Information | No unauthorized alteration, exfiltration, infiltration, or destruction |
| Derivative Non-Tangible (image, reputation, trust) | Protected by ensuring adequate protection of other asset classes |

### Loss Control Objectives

- **Prevent the loss from occurring**: Remove events/conditions, or tolerate them without adverse effect
- **Limit the extent of loss**: Limit dispersion, duration, capacity, or volume; use loss recovery and loss delay

### Systems Thinking for Security

Security should shift from a tactics/forensics problem (threat defense, incident response) to a systems thinking approach focused on assuring the system performs and protects stakeholder assets. Quality evidence from verification and validation feeds **assurance arguments** that merit trustworthiness.

### Enterprise and Discipline Relationships

Systems security shares commonality with safety, quality management, RAM, survivability, operational risk management, and resilience — overlapping concerns with assets, losses, adversities, requirements, and engineering analyses (e.g., STPA — System Theoretic Process Analysis).

---

## Related Chapters

- [[00_Introduction_to_SEBoK]] — Overview and structure of the SEBoK
- [[04_Systems_Models_and_Approach]] — Modeling and the systems approach
- [[05_Life_Cycles_and_Processes]] — Life cycle processes that integrate quality attributes
- [[06_Technical_Management_Processes]] — Risk management, decision management, and technical planning
