---
tags: [overview, systems-engineering, sebok, essential-documents]
---

# SEBoK — Essential Documents by Phase

> **Source:** [[SEBoK v2 - Overview|SEBoK v2 — Systems Engineering Body of Knowledge (BKCASE, 2025)]]
> Organized by the systems engineering life cycle: Concept → Architecture → Realization → Operations → Maintenance
> Plus technical management and cross-cutting documents
>
> ⚠️ **This is a document-only extract.** For the full body of knowledge (fundamentals, systems science, thinking, standards, case studies), see:
> `F:\projects\orlita_md\software-engineering-note\Body of Knowledge\System Engineer BOK\`

**Priority Legend:**

| Icon | Level            | Meaning                                                           |     |
| ---- | ---------------- | ----------------------------------------------------------------- | --- |
| 🔴   | **Must Have**    | Essential for virtually all systems                               |     |
| 🟡   | **Nice to Have** | Recommended for medium+ complexity                                |     |
| 🟢   | **Optional**     | Situational — depends on domain, criticality, or regulatory needs |     |

---

## 1. Concept Definition
> **Owner:** Systems Engineer / Sponsor

| Document                                     | Description                                                                                | Priority        | ISO/IEEE Reference                              |
| -------------------------------------------- | ------------------------------------------------------------------------------------------ | --------------- | ----------------------------------------------- |
| **Business or Mission Analysis Report**      | Problem/opportunity definition, mission needs, operational context, capability gaps        | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.2)                     |
| **Stakeholder Needs Document**               | Elicited stakeholder expectations, operational scenarios, measures of effectiveness (MoEs) | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.1), ISO/IEC/IEEE 29148 |
| **System Requirements Specification (SyRS)** | Functional and non-functional requirements, performance parameters, interface requirements | 🔴 Must Have    | ISO/IEC/IEEE 29148, ISO/IEC/IEEE 15288 (§6.4.3) |
| **Requirements Traceability Matrix (RTM)**   | Forward/backward traceability: need → requirement → architecture → verification            | 🔴 Must Have    | ISO/IEC/IEEE 29148                              |
| **Concept of Operations (ConOps)**           | How the system will be used: users, tasks, operational scenarios, environment              | 🔴 Must Have    | ISO/IEC/IEEE 29148, ISO/IEC/IEEE 15288 (§6.4.1) |
| **Stakeholder Register**                     | All identified stakeholders, their concerns, influence, and engagement approach            | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.1)                     |
| **Market Analysis / Technology Assessment**  | Technology readiness, COTS availability, make-or-buy assessment                            | 🟡 Nice to Have | ISO/IEC/IEEE 15288 (§6.4.2)                     |
| **Feasibility Study**                        | Technical, economic, and schedule feasibility of proposed solutions                        | 🟡 Nice to Have | ISO/IEC/IEEE 15288 (§6.4.2)                     |

---

## 2. System Architecture & Design
> **Owner:** Systems Architect

| Document                                | Description                                                                                | Priority        | ISO/IEEE Reference                              |
| --------------------------------------- | ------------------------------------------------------------------------------------------ | --------------- | ----------------------------------------------- |
| **Functional Architecture**             | Functional decomposition: functions, flows, allocation to components                       | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.4), ISO/IEC/IEEE 42010 |
| **Logical Architecture**                | Abstract system structure: components, interfaces, data/control flows                      | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.4), ISO/IEC/IEEE 42010 |
| **Physical Architecture**               | Physical realization: subsystems, assemblies, connections, installation                    | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.4), ISO/IEC/IEEE 42010 |
| **Interface Control Document (ICD)**    | Interface specifications: mechanical, electrical, data, software, human                    | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.4)                     |
| **System Architecture Description**     | Views and viewpoints per ISO/IEC/IEEE 42010: functional, physical, data, behavioral        | 🔴 Must Have    | ISO/IEC/IEEE 42010                              |
| **Trade Study Reports**                 | Alternative evaluation: criteria, weighting, scoring, sensitivity analysis, recommendation | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.4)                     |
| **Architecture Decision Records (ADR)** | Rationale for each significant architectural decision with alternatives considered         | 🟡 Nice to Have | ISO/IEC/IEEE 42010                              |
| **System Bill of Materials (BOM)**      | All physical and software components with quantities and specifications                    | 🟡 Nice to Have | ISO/IEC/IEEE 15288 (§6.4.5)                     |
| **MBSE Models** (SysML)                 | Requirements diagrams, block definition, internal block, activity, sequence, state machine | 🟡 Nice to Have | ISO/IEC/IEEE 24641                              |
| **Digital Twin Specification**          | Virtual representation requirements for simulation and monitoring                          | 🟢 Optional     | ISO/IEC/IEEE 24641                              |

---

## 3. System Realization
> **Owner:** Systems Engineer / Developer

| Document                     | Description                                                                      | Priority        | ISO/IEEE Reference          |
| ---------------------------- | -------------------------------------------------------------------------------- | --------------- | --------------------------- |
| **Implementation Plan**      | Build/buy/code strategy, work packages, resource allocation                      | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.5) |
| **Integration Plan**         | Assembly sequence, integration order, interface verification, test environment   | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.6) |
| **Integration Test Reports** | Results of subsystem and system-level integration testing                        | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.6) |
| **Verification Plan**        | How each requirement will be verified: inspection, analysis, demonstration, test | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.7) |
| **Verification Reports**     | Results against each requirement: pass/fail, evidence, non-conformances          | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.7) |
| **Validation Plan**          | How stakeholder needs and ConOps will be validated in operational environment    | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.8) |
| **Validation Report**        | Operational testing results, stakeholder acceptance, MoE achievement             | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.8) |
| **Transition Plan**          | System handover: installation, training, data migration, operational readiness   | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.9) |
| **As-Built Documentation**   | Actual configuration, deviations from design, manufacturing records              | 🟡 Nice to Have | ISO/IEC/IEEE 15288 (§6.4.5) |

---

## 4. Operations & Maintenance
> **Owner:** Ops Engineer / SRE

| Document                              | Description                                                                    | Priority        | ISO/IEEE Reference                                 |
| ------------------------------------- | ------------------------------------------------------------------------------ | --------------- | -------------------------------------------------- |
| **Operations Manual / Runbook**       | Routine procedures: startup, shutdown, monitoring, emergency response          | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.10)                       |
| **Maintenance Plan**                  | Preventive, corrective, adaptive, perfective maintenance strategy and schedule | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.11)                       |
| **Logistics Plan**                    | Supply chain, spares, tools, training, facilities for system support           | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.11)                       |
| **Service Level Agreement (SLA)**     | Availability, performance, response-time commitments for operational systems   | 🔴 Must Have    | ISO/IEC 20000-1                               |
| **Incident / Problem Reports**        | Fault documentation, root cause analysis, corrective action                    | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.11), ISO/IEC 20000-1 |
| **Change Requests (CR/ECR)**          | Proposed modifications with impact analysis (technical, cost, schedule, risk)  | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.11)                       |
| **Capability Upgrade Plan**           | Modernization roadmap: technology refresh, obsolescence management             | 🟡 Nice to Have | ISO/IEC/IEEE 15288 (§6.4.11)                       |
| **System Disposal / Retirement Plan** | Decommissioning, data archival, environmental compliance, material recovery    | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.4.12)                       |

---

## 5. Technical Management
> **Owner:** Systems Engineer / PM

### Planning & Control
> **Owner:** PM / Systems Engineer

| Document                                       | Description                                                                     | Priority     | ISO/IEEE Reference                                |
| ---------------------------------------------- | ------------------------------------------------------------------------------- | ------------ | ------------------------------------------------- |
| **Systems Engineering Management Plan (SEMP)** | How SE will be performed: organization, processes, schedule, resources, metrics | 🔴 Must Have | ISO/IEC/IEEE 15288 (§6.3.1), ISO/IEC/IEEE 24748-1 |
| **Project Plan / Schedule**                    | SE activities sequenced: reviews, analyses, integration events, decision gates  | 🔴 Must Have | ISO/IEC/IEEE 15288 (§6.3.1)                       |
| **Work Breakdown Structure (WBS)**             | Hierarchical decomposition of system work scope                                 | 🔴 Must Have | ISO/IEC/IEEE 15288 (§6.3.1), ISO 21511            |
| **Decision Records**                           | Structured documentation of key decisions, alternatives, rationale, approvals   | 🔴 Must Have | ISO/IEC/IEEE 15288 (§6.3.2)                       |

### Risk Management
> **Owner:** Systems Engineer

| Document                 | Description                                                                    | Priority        | ISO/IEEE Reference                     |
| ------------------------ | ------------------------------------------------------------------------------ | --------------- | -------------------------------------- |
| **Risk Management Plan** | Risk process, organization, thresholds, reporting                              | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.3.3), ISO 31000 |
| **Risk Register**        | Identified technical risks: probability, impact, mitigation, ownership, status | 🔴 Must Have    | ISO 31000, ISO/IEC/IEEE 15288 (§6.3.3) |
| **Risk Burn-Down Chart** | Risk exposure trending over time                                               | 🟡 Nice to Have | ISO 31000                              |

### Configuration Management
> **Owner:** Config Manager

| Document                                    | Description                                                                             | Priority        | ISO/IEEE Reference                     |
| ------------------------------------------- | --------------------------------------------------------------------------------------- | --------------- | -------------------------------------- |
| **Configuration Management Plan**           | How CIs are identified, controlled, audited, reported                                   | 🔴 Must Have    | ISO 10007, ISO/IEC/IEEE 15288 (§6.3.4) |
| **Configuration Baselines**                 | Formally approved CI versions at life cycle milestones (functional, allocated, product) | 🔴 Must Have    | ISO 10007, ISO/IEC/IEEE 15288 (§6.3.4) |
| **Configuration Status Accounting Reports** | CI status, change traffic, metrics                                                      | 🟡 Nice to Have | ISO 10007                              |

### Technical Reviews & Audits
> **Owner:** QA / V&V Engineer

| Document                                                | Description                                                  | Priority     | ISO/IEEE Reference                     |
| ------------------------------------------------------- | ------------------------------------------------------------ | ------------ | -------------------------------------- |
| **Technical Review Records** (SRR, PDR, CDR, TRR, etc.) | Review findings, action items, entry/exit criteria, sign-off | 🔴 Must Have | ISO/IEC/IEEE 15288 (§6.3.5)            |
| **Audit Reports** (FCA, PCA)                            | Functional/physical configuration audit results              | 🔴 Must Have | ISO 10007, ISO/IEC/IEEE 15288 (§6.3.5) |

### Measurement
> **Owner:** Systems Engineer

| Document                                  | Description                                                                     | Priority        | ISO/IEEE Reference                              |
| ----------------------------------------- | ------------------------------------------------------------------------------- | --------------- | ----------------------------------------------- |
| **Technical Performance Measures (TPMs)** | Quantitative metrics tracking critical technical parameters                     | 🔴 Must Have    | ISO/IEC/IEEE 15939, ISO/IEC/IEEE 15288 (§6.3.6) |
| **Measurement Plan**                      | What to measure, how, when, thresholds, reporting                               | 🟡 Nice to Have | ISO/IEC/IEEE 15939                              |
| **SE Performance Dashboard**              | Leading and lagging indicators: requirement volatility, rework rate, TPM trends | 🟡 Nice to Have | ISO/IEC/IEEE 15939                              |

---

## 6. Cross-Cutting Documents
> **Owner:** Systems Engineer

### Quality
> **Owner:** QA Lead

| Document                      | Description                                                            | Priority     | ISO/IEEE Reference                               |
| ----------------------------- | ---------------------------------------------------------------------- | ------------ | ------------------------------------------------ |
| **Quality Management Plan**   | Quality policies, objectives, assurance, control activities            | 🔴 Must Have | ISO 9001, ISO 10006, ISO/IEC/IEEE 15288 (§6.3.5) |
| **Quality Assurance Reports** | Process compliance audits, non-conformance reports, corrective actions | 🔴 Must Have | ISO 9001                                         |

### Safety & Security
> **Owner:** Security Engineer

| Document                                    | Description                                                               | Priority        | ISO/IEEE Reference                |
| ------------------------------------------- | ------------------------------------------------------------------------- | --------------- | --------------------------------- |
| **System Safety Plan / Safety Case**        | Hazard identification, risk assessment, safety requirements, verification | 🔴 Must Have    | IEC 61508, ISO/IEC/IEEE 15288     |
| **Hazard Analysis** (PHA, SHA, SSHA, O&SHA) | Systematic hazard identification at each system level                     | 🔴 Must Have    | IEC 61508, ISO 26262 (automotive) |
| **Security Plan**                           | Threat analysis, security requirements, architecture, verification        | 🔴 Must Have    | ISO/IEC 27001:2022                     |
| **FMEA / FMECA**                            | Failure Mode, Effects, and Criticality Analysis                           | 🟡 Nice to Have | IEC 60812                         |
| **Fault Tree Analysis (FTA)**               | Top-down deductive failure analysis for safety-critical systems           | 🟡 Nice to Have | IEC 61025                         |

### Standards Compliance
> **Owner:** Systems Engineer

| Document                        | Description                                                  | Priority        | ISO/IEEE Reference           |
| ------------------------------- | ------------------------------------------------------------ | --------------- | ---------------------------- |
| **Standards Compliance Matrix** | Mapping of system requirements to applicable standards       | 🔴 Must Have    | ISO/IEC/IEEE 15288 (§6.3.5)  |
| **Tailoring Justification**     | Rationale for adapting standard processes to project context | 🟡 Nice to Have | ISO/IEC/IEEE 15288 (Annex A) |

### Domain-Specific (apply as relevant)
> **Owner:** Varies by domain

| Document                           | Description                                                                      | Priority                          | ISO/IEEE Reference                      |
| ---------------------------------- | -------------------------------------------------------------------------------- | --------------------------------- | --------------------------------------- |
| **Software Development Plan**      | For software-intensive systems: SW life cycle, methods, tools, standards         | 🔴 Must Have (SW)                 | ISO/IEC/IEEE 12207                      |
| **Software Assurance Plan**        | Software safety, reliability, and verification requirements                      | 🔴 Must Have (safety-critical SW) | DO-178C (avionics), IEC 62304 (medical) |
| **Medical Device File**            | Design history, risk management, clinical evaluation per regulatory requirements | 🔴 Must Have (medical)            | ISO 13485, IEC 62304                    |
| **Security Accreditation Package** | Evidence for system security authorization/ATO                                   | 🔴 Must Have (gov/defense)        | ISO/IEC 27001:2022, NIST SP 800-37           |

---

## Life Cycle Model Considerations

Per [[05_Life_Cycles_and_Processes]], the formality and timing of these documents depends on the development approach:

| Approach                           | Documentation Style                              | Key SE Documents                                                                    |     |
| ---------------------------------- | ------------------------------------------------ | ----------------------------------------------------------------------------------- | --- |
| **Sequential (Waterfall/V-Model)** | Formal, phase-gated, signed-off baselines        | Comprehensive SEMP, formal SRR/PDR/CDR reviews, full SyRS before design             |     |
| **Incremental**                    | Baselines per increment, progressive elaboration | SyRS allocated to increments, increment-level verification plans                    |     |
| **Evolutionary / Spiral**          | Iterative, risk-driven                           | Evolving requirements, architecture refined each spiral, continuous risk assessment |     |
| **Agile / Lean**                   | Lightweight, continuous                          | User stories > SyRS, MBSE models > documents, continuous integration/verification   |     |
| **Hybrid**                         | Mixed formality                                  | Formal for safety-critical subsystems; agile for software/UI components             |     |

> **Tailoring Note:** Per [[05_Life_Cycles_and_Processes]] and the tailoring guidance in ISO/IEC/IEEE 15288 Annex A, adjust document type, detail, and review rigor based on: system criticality (safety/mission-critical → upgrade to 🔴), domain standards (aerospace → DO-178C; medical → IEC 62304; automotive → ISO 26262), organizational maturity, regulatory environment, and contract requirements. *No single system needs every document listed.*

### Key Standards Referenced

| Standard | Title |
| --- | --- |
| ISO/IEC/IEEE 15288 | Systems and Software Engineering — System Life Cycle Processes |
| ISO/IEC/IEEE 42010 | Systems and Software Engineering — Architecture Description |
| ISO/IEC/IEEE 29148 | Systems and Software Engineering — Requirements Engineering |
| ISO/IEC/IEEE 12207 | Systems and Software Engineering — Software Life Cycle Processes |
| ISO/IEC/IEEE 24641 | Systems and Software Engineering — Model-Based Systems Engineering |
| ISO/IEC/IEEE 15939 | Systems and Software Engineering — Measurement Process |
| ISO/IEC/IEEE 24748-1 | Systems and Software Engineering — Life Cycle Management |
| ISO/IEC/IEEE 15289 | Systems and Software Engineering — Content of Life Cycle Information Items |
| ISO/IEC 20000-1 | Information Technology — Service Management |
| ISO 31000 | Risk Management — Guidelines |
| ISO 9001 | Quality Management Systems — Requirements |
| ISO 10006 | Quality Management — Guidelines for Quality Management in Projects |
| ISO 10007 | Quality Management — Guidelines for Configuration Management |
| ISO 21511 | Work Breakdown Structures for Project and Programme Management |
| ISO/IEC 27001:2022 | Information Security, Cybersecurity and Privacy Protection |
| IEC 61508 | Functional Safety of Electrical/Electronic/Programmable Electronic Systems |
| IEC 60812 | Failure Modes and Effects Analysis (FMEA and FMECA) |
| IEC 61025 | Fault Tree Analysis (FTA) |
| ISO 13485 | Medical Devices — Quality Management Systems |
| IEC 62304 | Medical Device Software — Software Life Cycle Processes |
| ISO 26262 | Road Vehicles — Functional Safety |
| DO-178C | Software Considerations in Airborne Systems and Equipment Certification |

---

## Related

- [[SEBoK v2 - Overview]] — Full SEBoK knowledge base
- [[09_Systems_Engineering_Standards]] — Deep dive on SE standards
- [[PMBOK Essential Documents]] — Project management documents by phase
- [[SWEBOK Essential Documents]] — SWEBOK software documents by phase
