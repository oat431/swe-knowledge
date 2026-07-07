---
tags: [standards, systems-engineering, sebok]
---

> *Source: SEBoK v2, Part 3d — Systems Engineering Standards*

## Purpose

This knowledge area focuses on the standards and key references relevant to systems engineering (SE). It examines the importance of standards, the types of standards, the key standards themselves, the alignment efforts to achieve a consistent set of standards, and the suitable application of these standards. Many of these standards have been used as references throughout Part 3 of the SEBoK.

## Why Standards?

ISO defines a **standard** as "a document, established by consensus and approved by a recognized body, that provides rules, guidelines, or characteristics for activities or their results." Standards help ensure quality, consistency, safety, efficiency, and effectiveness for products and projects throughout the life cycle, leading to improved communication, reduced errors, and faster cycle times.

### Benefits and Challenges of Standardization

| Focus | Pros | Cons |
|-------|------|------|
| **Timing & Effectiveness** | Can incorporate broad lessons learned and proven practices; helps ensure technologies are secure and safe. | Potential immaturity of a topic; standards can be developed too early, before evidence of effectiveness. Building consensus takes time. |
| **Innovation** | Provides a baseline for tool development, allowing engineers to focus on innovation rather than reinventing basic tasks. | Can somewhat stifle innovation when novel situations require new approaches. |
| **Quality & Consistency** | Standardized processes ensure products are built consistently, reducing unproductive variations and improving quality. | May challenge responsiveness or agility. |
| **Communication & Collaboration** | Converges practices, aiding communication, teaming, and knowledge management through standardized formats, symbols, and conventions. | Can drive communication paths or workflows that are not optimal. |
| **Efficiency & Productivity** | Promotes faster development cycles via reuse of pre-approved elements; streamlined processes reduce waste. | Without project-specific tailoring, can lead to inefficiencies — especially for projects with high uncertainty. |
| **Knowledge Management** | Helps document lessons learned and provides organizational memory. | When there is high likelihood of rapid change, standards as "memory" can become obsolete quickly. |
| **Costs** | Reduces material costs for stable product lines; lowers training and knowledge-transfer costs. | May not reduce costs if change is frequent; can create resistance to adaptation. |
| **Scalability** | Easier to scale engineering processes and services with reusable common practices and tools. | |
| **Interoperability** | Helps ensure different systems work together, reducing integration challenges. | |

### Key Considerations for Standardization

- **Full Life Cycle Approach**: Organizations should evaluate alternative standards, select, adapt/tailor, assess results, and take improvement actions. ISO/IEC/IEEE 24748-1 and 24748-2 provide guidance on life cycle management and process tailoring.
- **Readiness Assessment**: Assess the maturity of concepts, convergence of principles, proven practices, and industry consensus before standardizing a topic. Also assess the relevance and maturity of a standard for the specific project application.
- **Agility and Standardization**: These are not mutually exclusive. Organizations can tailor standards like ISO/IEC/IEEE 15288 to incorporate agility for situations with uncertainty and likelihood of change. ISO/IEC/IEEE 24748-10 provides guidelines for SE agility. The NDIA/INCOSE paper "ISO/IEC/IEEE 15288 Meets Lean Agile" details using 15288 in agile defense environments.
- **Balance**: Projects are unique — "copy and paste" rarely works. Tailor standardized elements to project needs while avoiding the "not invented here" mindset.

### Stakeholder Perspectives

- **ISO**: "Standards ensure consistency of essential features of goods and services, such as quality, ecology, safety, economy, reliability, compatibility, interoperability, efficiency and effectiveness."
- **IEEE**: "Standards help fuel compatibility and interoperability and simplifies product development, and speeds time-to-market."
- **Government/Defense**: "Technical standards provide the corporate process memory needed for a disciplined systems engineering approach and help ensure that the government and its contractors understand the critical processes and practices."

## Evolution of Systems Engineering Standards

Systems engineering standards have evolved as the SE discipline has matured, incorporating lessons learned and changes in approaches to ensure they reflect current, proven practices.

### Key Milestones

1. **Early Roots — MIL-STD-499 (1969)**: The U.S. Air Force published MIL-STD-499 ("System Engineering Management") to guide program managers and contractors on SE planning, processes, tailoring, documentation, and technical oversight. Updated to MIL-STD-499A in 1974. A draft MIL-STD-499B was never formally released. Over time, U.S. military standards including 499A were deprecated or withdrawn under defense acquisition reform.

2. **Emergence of Commercial Standards — ANSI/EIA-632 and IEEE 1220 (1990s)**: After defense policy shifted away from mandating military standards, the EIA produced EIA/IS-632 (1994), closely mirroring MIL-STD-499B for commercial and defense applications. This evolved into ANSI/EIA-632 ("Processes for Engineering a System"), defining 13 processes across technical, management, acquisition, and supply categories. IEEE independently developed IEEE 1220 (1995) for application and management of SE processes, later revised to align with ISO/IEC 15288.

3. **Harmonization and ISO/IEC/IEEE 15288 (2002–present)**: Growing recognition of the need for a common, internationally accepted life cycle process standard led ISO/IEC JTC1/SC7 to develop ISO/IEC 15288 (first edition, 2002). IEEE adopted it in 2004. Subsequent revisions in 2008, 2015, and 2023 expanded collaboration. The latest version (2023) defines **30 processes** grouped into four categories:
   - Organizational Project-Enabling Processes
   - Agreement Processes
   - Technical Management Processes
   - Technical Processes

   Each process is defined with purpose, outcomes, activities, and tasks. ISO/IEC/IEEE 15288 is now widely accepted as the central canonical systems engineering process standard.

### Standards Evolution Tree

The evolution can be visualized as a tree:
- **Roots**: MIL-STD-499, EIA/IS-632, IEEE 1220, and other early standards
- **Trunk**: ISO/IEC/IEEE 15288 — the central process standard
- **Branches**: Other works developed or revised as a result of harmonization with 15288, including SAE 1001, INCOSE SE Handbook, SEBoK, NATO AAP-48, and numerous elaboration standards

## SE Related Standards Landscape (ISO/IEC/IEEE)

### Types of SE Standards

| Standard Type | Description |
|---------------|-------------|
| **Concepts and Terminology** | Defines terminology and describes concepts of a specific domain. |
| **Process** | Defines a specific process or set of processes with normative requirements. |
| **Requirements** | Defines requirements for actions, activities, or practices. |
| **Procedure (Practice, Activity)** | Defines instructions, approach, or requirements on how to do something. |
| **Application Guides / Guidance** | Provides guidelines or interpretation of a published standard. |
| **Management System** | Defines requirements for management of a specific focus area. |
| **Specification** | Defines, describes, or identifies something precisely, including its requirements. |
| **Artifact Description** | Defines the form, attributes, content requirements, or properties of an artifact. |
| **Reference Model** | Defines an abstract or conceptual framework for understanding a system, architecture, or element. |
| **Process Reference Model (PRM)** | Defines a collection of processes necessary to achieve a nominated business outcome. |
| **Process Assessment Model (PAM)** | Defines requirements and guidance for assessing attributes of nominated processes. |
| **Guide to Body of Knowledge (BOK)** | Defines a collection and description of current knowledge sources in a domain. |

### Taxonomy of SE Standards (Categories)

- **Foundation — Vocabulary**: Common terminology across standards (ISO/IEC/IEEE 24765, SEVOCAB).
- **Foundation — Body of Knowledge**: Resources covering the breadth of SE (SEBoK, INCOSE SE Handbook).
- **Life Cycle Processes and Concepts**: Top-level standards defining system life cycle processes and concepts.
- **Assessment / Governance**: Standards for assessment or governance of processes and products.
- **Process Elaborations**: Standards focusing on specific processes with lower-level details.
- **Application Guides**: Guidance for understanding and executing standards (e.g., ISO/IEC/IEEE 24748-2).
- **Artifact Descriptions**: Descriptions/requirements for specific life cycle artifacts.
- **Tools**: Standards related to tools applicable to system life cycle processes.
- **Other Guidance**: Any guidance outside the scope of other categories.

### Key SE Standards (Partial List)

**Core Process Standards:**
- **ISO/IEC/IEEE 15288** — System life cycle processes (30 processes, 4 groups)
- **ISO/IEC/IEEE 12207** — Software life cycle processes (aligned with 15288)
- **SAE 1001** — Integrated Project Processes for Engineering a System (replaced ANSI/EIA 632)
- **NATO AAP-48** — NATO System Life Cycle Processes

**Life Cycle Management (24748 series):**
- **ISO/IEC/IEEE 24748-1** — Guidelines for life cycle management
- **ISO/IEC/IEEE 24748-2** — Guidelines for the application of 15288
- **ISO/IEC/IEEE 24748-4** — Systems engineering management planning
- **ISO/IEC/IEEE 24748-6** — System and software integration
- **ISO/IEC/IEEE 24748-7** — Application on defense programs
- **ISO/IEC/IEEE 24748-8** — Technical reviews and audits on defense programs
- **ISO/IEC/IEEE 24748-9** — Application in epidemic prevention and control systems
- **ISO/IEC/IEEE 24748-10** — Guidelines for systems engineering agility

**Specialty Process Standards:**
- **ISO/IEC/IEEE 15026 series** — Systems and software assurance (concepts, assurance case, integrity levels, assurance in life cycle)
- **ISO/IEC/IEEE 15289** — Content of life-cycle information items (documentation)
- **ISO/IEC/IEEE 15939** — Measurement process
- **ISO/IEC/IEEE 16085** — Risk management
- **ISO/IEC/IEEE 16326** — Project management
- **ISO/IEC/IEEE 21839** — System of systems (SoS) considerations
- **ISO/IEC/IEEE 21840** — Guidelines for 15288 in SoS context
- **ISO/IEC/IEEE 21841** — Taxonomy of system of systems

**Architecture Standards:**
- **ISO/IEC/IEEE 42010** — Architecture description
- **ISO/IEC/IEEE 42020** — Architecture processes
- **ISO/IEC/IEEE 42024** — Architecture fundamentals
- **ISO/IEC/IEEE 42030** — Architecture evaluation framework
- **ISO/IEC/IEEE 42042** — Reference architectures

**Requirements and Modeling:**
- **ISO/IEC/IEEE 29148** — Requirements engineering
- **ISO/IEC/IEEE 24641** — Methods and tools for model-based systems and software engineering
- **OMG SysML v2.0** — Systems Modeling Language
- **ISO 19450** — Object Process Methodology (OPM)

**Assessment and Governance:**
- **ISO/IEC TS 33060** — Process assessment model for system life cycle processes
- **CMMI® for Development V3.0** — Capability Maturity Model Integration
- **ISO 9001** — Quality Management Systems - Requirements

**Domain-Specific Standards (Examples):**
- **ECSS series** (European Cooperation for Space Standardization) — Space engineering and project management
- **NIST SP 800-160 Vol 1 & 2** — Systems security engineering and cyber-resilient systems
- **ISO/IEC 29110** — Standards for very small entities (VSEs)
- **ISO/IEC/IEEE 26550 / 26580** — Product line engineering

**Other Key References:**
- **INCOSE SE Handbook (5th Edition)** — Core SE reference, used for SE Certification
- **SEBoK** — Guide to the Systems Engineering Body of Knowledge
- **INCOSE SE Vision 2035** — Future vision for systems engineering
- **ISO/IEC/IEEE 24765** — Systems and Software Engineering Vocabulary (SEVocab)

### Practical Considerations

**Key Pitfalls:**
| Pitfall | Description |
|---------|-------------|
| Turnkey Process Provision | Expecting the standard to fully provide SE processes without elaboration or tailoring. |
| No Need for Product Knowledge | Expecting the standard can be used without functional or domain knowledge. |
| No Process Integration | Lack of integrating standards requirements with organization or project processes. |

**Good Practices:**
| Practice | Description |
|----------|-------------|
| Tailor for Business Needs | Tailor the standard within conformance requirements to best meet business needs. |
| Integration into Project | Integrate requirements of the standard into the project via processes or product/service requirements. |

## Harmonization of Systems Engineering Standards

### Problem Statement

Historically, many organizations attempted to define or codify their best practices independently, resulting in different terminology, process sets, concepts, and levels of prescription — both within and across SE and software engineering (SwE) disciplines. Competing standards introduced inconsistencies. New challenges (agile, model-based, digital approaches) introduced further possibilities for inconsistency.

### Root Causes

- **Competition**: Many SDOs exist, each motivated to have its own set of standards contributing to revenue. Competing organizations, tool vendors, and consultants promote differentiation.
- **Organizational**: Different structures across SDOs, teams, and committees; changes in team structures as agile approaches became common.
- **Culture**: "We're different" and "not invented here" mindsets; complicated by diverse engineering team environments.
- **Domains**: A narrow, domain-specific view that doesn't look beyond for commonality.

### Impact of Inconsistency

- Less effective or efficient processes — redundancy, incompatibilities, and difficulty in concurrent use
- Less effective solutions not focused on common approaches
- Obstacles for communication, integrated teams, and resource leveraging
- Stove-piping due to incompatibilities

### The Harmonization Approach

Collaboration between **ISO/IEC JTC1/SC7**, **IEEE Computer Society**, **INCOSE**, and others has achieved alignment through:

1. **Common vocabulary, concepts, and process structures** via ISO/IEC/IEEE 24765 (Vocabulary), ISO/IEC/IEEE 24774 (Process description specification), and ISO/IEC/IEEE 24748-1 (Life cycle management guide).
2. **Aligned core standards**: ISO/IEC/IEEE 15288 (System life cycle processes) and ISO/IEC/IEEE 12207 (Software life cycle processes) share consistent concepts, structure, and terminology — enabling concurrent use on a single project.
3. **Supporting standards**: Elaboration of specific processes, description of practices (e.g., systems/software assurance), artifact descriptions, and application guidance.

### Co-Evolution Model

ISO/IEC/IEEE 15288 sits at the center of a co-evolution model, serving as the basis for terminology and concepts. Three key resources — **ISO/IEC/IEEE 15288**, the **INCOSE SE Handbook** (which adopted 15288's process set in 2006), and the **SEBoK** (which adopted 15288 as a key resource from 2009) — positively influence and strengthen each other, acting as a catalyst for alignment of other SE-related documents.

### Key Outcomes

1. ISO/IEC/IEEE 12207 (software) aligned with 15288, creating a coherent systems/software lifecycle framework.
2. Common vocabulary established via SEVocab and ISO/IEC/IEEE 24765, shared across ISO, IEEE, INCOSE, and PMI.
3. Process-specific elaboration documents produced for implementation across sectors.
4. Multiple IEEE documents adopted by ISO with revisions for consistency.
5. SAE 1001 adopted ISO/IEC/IEEE 15288's process concepts, process set, and terminology.
6. The 15288 process framework adopted by INCOSE SE Handbook, SEBoK, and other references.
7. Adopted by U.S. DoD and other defense organizations; defense-addendum standards developed (ISO/IEC/IEEE 24748-7, -8).

### Ongoing Challenges

- Standards' position in their life cycle can influence timing and ability to harmonize
- Agreements between collaborating parties regarding aligned content
- Standards continue to proliferate
- Emerging topics do not necessarily align with core or existing standards

## Application of Systems Engineering Standards

### Standards and Their Utilization

A standard is an agreed-upon, repeatable way of doing something — a published document containing a technical specification or precise criteria designed to be used consistently as a rule, guideline, or definition. Standards are designed for **voluntary use** and do not impose regulations. However, laws and regulations may reference certain standards and make compliance compulsory. Organizations may choose to use standards to provide uniformity in operations and products/services.

### Requirements, Recommendations, and Permissions (ISO Verb Forms)

| Verb | Meaning |
|------|---------|
| **shall** | Indicates requirements strictly to be followed; no deviation permitted. |
| **should** | Indicates a recommendation — preferred but not necessarily required. |
| **may / can** | Indicates a permissible course of action. (*May* is deprecated; *can* is preferred.) |

Standards should avoid *will*, *must*, and other imperatives. The term *ensure* should be avoided (use *help ensure* instead).

### Certification, Conformance, and Compliance

- **Certification**: Issuing of written assurance by an independent external body that a management system conforms to requirements (typically for management system standards like ISO 9001, ISO 14000). Most SE-specific standards are not subject to certification.
- **Conformance**: Voluntary action to meet the requirements (or tailored requirements) of a consensus standard. Many standards include a Conformance Clause. Conformance testing determines whether a product/system meets specified standards.
- **Compliance**: Adhering to mandatory legal or regulatory requirements, often driven by external authorities. In contractual contexts, compliance is with the agreement rather than conformance with the standard.

### Key Aspects of Standards Compliance

1. Identify and select relevant standards for the agreement.
2. Identify and understand applicable requirements.
3. Integrate requirements into organization/project processes with appropriate adaptation and tailoring; document incorporation and compliance demonstration.
4. Verify compliance through appropriate means and document it.

### Challenges for Compliance

- Keeping current — standards and requirements can change
- Balancing compliance with standards and leveraging innovation
- Capturing and maintaining accurate documentation requiring structured data management

### Tailoring of Standards

Since SE standards provide guidelines, they are most often **tailored** to fit the needs of organizations and projects. ISO/IEC/IEEE 15288 (2023) addresses tailoring through:

- **Full conformance**: Declares the set of processes for which conformance is claimed; all requirements satisfied using outcomes as evidence.
- **Tailored conformance**: Uses the standard as a basis but selects, excludes, or modifies specific requirements per the tailoring process. A clear statement defines the scope of tailored conformance.
- **Agreement context**: When used to develop an acquirer-supplier agreement, clauses can be selected with or without modification. It is more appropriate to claim compliance with the agreement than conformance with the standard.
- **Organizational imposition**: Any organization imposing the standard as a condition of trade should specify and make public the minimum set of required processes, activities, and tasks constituting supplier conformance.

ISO/IEC/IEEE 24748-2 states: "Life cycle models, as well as the processes from ISO/IEC/IEEE 15288, may be adapted for an individual project to reflect the variations appropriate to the organization, project and system while still being able to claim tailored conformance." It also provides an informational list of circumstances influencing tailoring.

## Related Chapters

- [[02_Nature_of_Systems]]
- [[03_Systems_Engineering_Overview]]
- [[04_Life_Cycle_Models]]
- [[05_System_Definition]]
- [[06_Systems_Engineering_Management]]
- [[07_Systems_Engineering_Process]]
- [[08_Systems_Engineering_Measurement]]
- [[10_Applications_of_Systems_Engineering]]
