---
tags: [product-se, service-se, applications, systems-engineering, sebok]
---

> *Source: SEBoK v2, Part 4 — Product & Service Systems Engineering*

## Purpose

**Product Systems Engineering (PSE)** and **Service Systems Engineering (SSE)** are two distinct but interrelated applications of systems engineering — both focus on the creation, delivery, and life cycle management of engineered systems, but from complementary perspectives. PSE centers on tangible/intangible artifacts (products) and their enabling systems, while SSE centers on value co-creation through stakeholder interactions and service transactions. Together with Enterprise SE and Systems of Systems, they form the four SE application contexts of SEBoK Part 4.

---

## Product Systems Engineering

### Background and Fundamentals

**Product Systems Engineering (PSE)** is at the core of the **New Product Development Process (NPDP)** — the integrated activity of bringing a new product to market. NPDP has two overlapping streams:

1. **Systems Engineering** — concept generation, engineering design/development, and deployment.
2. **Market Development** — market research, analysis, product acceptance, and adoption/diffusion.

NPDP also encompasses manufacturability, logistics, distribution, quality, disposal, conformance to standards, stakeholder value, and customer expectations. Internal enterprise capabilities (customer support, sales, maintenance, training) must also be factored in.

#### What is a Product?

A **product** is an artifact created by labor or process: hardware, software, personnel, facilities, data, materials, techniques, procedures, or media (Martin 1997; Lawson 2010). Product domains may be data-intensive (transportation), facilities-intensive (chemical plants), hardware-intensive (defense), or technique-intensive (search and rescue). Most are composites.

A **product system** = **end products + enabling products** (ANSI/EIA 632). Enabling products support the development, delivery, operation, and disposal of the end product. The end product itself may contain subsystems, each with their own enabling products.

#### Three Related Systems

| System | Role |
|--------|------|
| **Product System** (Intervention System) | The system delivered to address a perceived problem |
| **Product Realization System** | Enables the product system to be conceived, developed, produced, tested, and deployed (may be a service system or enterprise system) |
| **Product Sustainment System** | Keeps the deployed product operational via enabling products and operational services |

PSE must engineer the product system *alongside* the realization and sustainment systems, making trade-offs between their features, functions, and costs.

#### Product Elements and Connections

Product systems consist of **product elements** and two kinds of **connections**:

- **Interactions**: exchanges of data, materials, forces, or energy across interfaces (captured in interface control documents)
- **Relationships**: spatial (inside/underneath), motion-related (relative velocity), temporal (before/simultaneous), and social (ownership, understanding, feelings)

Social relationships among human and non-human elements are often overlooked but can determine success or failure. Organizational behavior theories and human factors must be considered.

#### Product Taxonomy

Product types are not limited to hardware/software. Martin's taxonomy includes: **hardware, software, personnel, facilities, data, materials, media, and techniques**. Each may require its own SE lifecycle.

#### Product Architecture, Modeling, and Analysis

**Architecture** (per IEEE 1471 / ISO/IEC 42010) is the fundamental organization of a system embodied in its components, their relationships, and design principles. The systems architect:

1. Starts at the highest abstraction level — needs, functions, characteristics, constraints
2. Documents architectural descriptions (models) representing stakeholder views
3. Uses modeling techniques: hierarchical decomposition, functional/architectural block diagrams, use case diagrams, sequence diagrams, state diagrams, data flow diagrams
4. Transforms requirements into products and processes

Common architecture frameworks: **Zachman**, **TOGAF**, **DoDAF**, **MODAF**, **FEAF**, **e-TOM**.

#### Specialty Engineering Integration

Specialty engineering analyzes specific system features requiring special skills: **manufacturability, reliability & maintainability, certification, logistics support, electromagnetic compatibility, environmental impact, human factors, safety & health, and training**. These must be integrated early in the life cycle.

### Business Activities Related to Product SE

PSE interfaces with multiple business functions:

| Business Activity | PSE Contribution |
|-------------------|------------------|
| **Marketing & PLM** | Elicit product needs, define roll-out phases, manage product-lines and platforms |
| **Quality Management** | ISO 9000/9001, AS9100 (aerospace), QS-16949 (automotive), CMMI for process improvement |
| **Project Management** | SEMP coordinates activities, milestones, organization, and resources |
| **Business Development** | Identify new product/service opportunities, strategic alliances |
| **Supply Chain Management** | Specifications, acceptance criteria, testing plans, interface specs, supplier certification |
| **Distribution Channel** | User manuals, packaging, qualification data, certification data, operator criteria |
| **Capability Management** | Derive product system requirements from capability needs; trade-off processes |
| **Operations Management** | Transition plans, business continuity, spares/repairs, obsolescence management |
| **Manufacturing, Test & Certification** | Manufacturability assessment, test methods, certification requirements |
| **Product Delivery & Support** | Maintenance, reliability, and support requirements defined early |

### Key Aspects and Special Activities

#### Acquired vs. Offered Products

- **Acquired Products**: An acquirer specifies needs, selects a supplier, and owns/operates/maintains the product. Government contracts often include strict design specifications.
- **Offered Products**: The supplier determines product properties through market surveys/forecasts and develops for a broader marketplace.

#### Supply Chains and Distribution Channels

Supply chains form a complex web of acquirer-supplier relationships (outsourcing holism). Distribution channels include warehouses, depots, wholesalers, dealers, and retail stores. PSE may need special product features for distribution (protective packaging, training versions, demonstration units).

#### Product Lifecycle and Adoption Rates

Product lifecycle follows stages: **identification → concept → design/manufacturing → launch** (the Integrated Product Development Process, IPDP). The **Rogers Innovation Adoption Curve** drives product model lifecycles: innovators → early adopters → early majority → late majority → laggards. PSE determines optimal timing for new version delivery.

#### Integrated Product Teams (IPTs)

IPTs are **cross-functional groups** of stakeholders brought together to deliver integrated products. IPTs carry out tailored IPDPs and follow SE processes. The INCOSE SE Handbook describes IPT types, organization, and pitfalls.

#### Readiness Level Assessments

| Readiness Level | Focus |
|-----------------|-------|
| **TRL** (Technology Readiness Levels) | Technology maturity (1–9 scale, from basic principles to flight-proven) |
| **MRL** (Manufacturing Readiness Levels) | Producibility and cost of production |
| **IRL** (Integration Readiness Levels) | Interface maturity among technology components |
| **SRL** (Systems Readiness Levels) | Whole-system maturity (function of TRL and IRL) |

#### Product Certification

Domain- and product-specific certifications for safety, health, government regulation, or insurance. Examples: airworthiness (FAA), medical devices (FDA), electronic safety (CE, UL). Enabling product certifications (operator certification, facility certification, HEMP, TEMPEST) must also be considered.

#### Technology Planning and Insertion

Technology planning defines the technical evolution roadmap, forecasts, maturation requirements, and insertion points. Critical technologies must be available before dependent development activities begin.

#### Configuration and Risk Management

**Configuration Management (CM)** covers identification, control, auditing, and traceability. Changes go through a Configuration Control Board (CCB) via Engineering Change Requests (ECR). **Risk Management** identifies, assesses, and prioritizes technical, cost, schedule, and programmatic risks.

#### Increasing Role of Software

Software implements an ever-increasing portion of product functionality — from embedded systems (cars, phones, medical devices) to the "internet of things." Software engineers need systems thinking to bridge problem-solving with business objectives.

---

## Service Systems Engineering

### Fundamentals of Services

**Service Systems Engineering (SSE)** is a multidisciplinary approach to managing and designing **value co-creation** in service systems. Services are activities that cause a transformation of the state of an entity (person, product, business, region, or nation) by mutually agreed terms between provider and customer.

#### The Shift to Service Economies

Economies follow a progression: agriculture/mining → manufacturing → services. As of 2006, services accounted for **67.8% of U.S. GDP**. The **Product-Service System (PSS)** concept evolved to bring added value and environmentally favorable products to market. The internet has accelerated service-based applications (SBAs) through platforms like cloud services, SaaS, PaaS, and social networking.

#### Service System Definition

A **service system** is a system of interacting and interdependent parts (people, technologies, organizations) that is externally oriented to achieve and maintain competitive advantage. Service system entities dynamically configure four resource types:

1. **People** — service providers, support providers, customers
2. **Technology/Environment Infrastructure** — buildings, equipment, IT
3. **Organizations/Institutions** — governance, processes, rules
4. **Shared Information/Symbolic Knowledge** — data, knowledge, procedures

Service systems can be **formal** (governed by Service Level Agreements, SLAs) or **informal** (based on relationships and promises, e.g., emergency services).

#### Service Value Chain

The value chain links entities through network-centric operations: B2B, B2C, B2G, G2B, G2C, etc. Services are:

- **Information-driven** — creation, management, and sharing of information
- **Customer-centric** — customers are co-producers
- **E-oriented** — e-access, e-commerce, e-customer management
- **Productivity-focused** — both efficiency and effectiveness matter
- **Value-adding** — value for customers assures profitability

#### Value from Multiple Stakeholder Perspectives

| Stakeholder | Measure | Basic Question |
|-------------|---------|----------------|
| Customer | Quality (Revenue) | Should we offer it? |
| Provider | Productivity (Profit, Sustainability) | Can we deliver it? |
| Authority | Compliance (Taxes, Regulation) | May we offer it? |
| Competitor | Sustainable Innovation (Market Share) | Will we invest? |

#### Service System Entities (Meta-model)

Spath and Fähnrich define nine entity types: **Customers, Goals, Inputs, Outputs, Processes, Human Enablers, Physical Enablers, Informatics Enablers, and Environment** (PESTEL factors).

#### Service System Hierarchy and Attributes

Service systems form a hierarchy from individuals → organizations → ecosystems. Key attributes: **togetherness, structure, behavior, and emergence**. Services are "real-time in nature and consumed at the time they are co-produced" (Tien and Berg 2003). Adaptive behavior demands trans-disciplinary design incorporating social science, management, and cybernetics.

### Properties of Services

#### Service Level Agreements (SLAs)

An **SLA** is a negotiated set of technical (functional) and non-technical (non-functional) parameters between customers and providers. SLAs include:

- **Business properties**: price, payment method, SLA duration
- **QoS properties**: availability, resilience, security, reliability, scalability, response times, repair times
- **Context properties**: time and location

SLAs are the basis for **Service Level Management (SLM)** — ensuring providers meet and maintain prescribed quality of service.

#### Key Performance Indicators (KPIs)

KPIs decompose into:
- **Service Process Measures (SPM)** — provisioning time, time-to-restore, end-to-end response times
- **Technical Performance Measures (TPM)** — latency, throughput, jitter, defect rates

Both objective (TPM/SPM) and subjective (customer perception via surveys and MOS techniques) measures are correlated for **Continuous Service Improvement (CSI)**.

#### Non-Inventory and Demand-Variability

- Services **cannot be inventoried** — they are rendered at request time
- Demand loads vary by time of day, day of week, season, or unexpected events
- Design and operations require balancing resources with demands for optimal QoS

#### Evolution and Classification

Spohrer classifies service systems into three categories:
1. **Flow of Things**: transportation/supply chains, water/waste, food, energy, ICT/cloud
2. **Human Activities & Development**: buildings, retail/hospitality, banking/finance, healthcare, education
3. **Governing**: cities, states, nations

Thirteen service sectors in the US economy span professional services, healthcare, government, leisure, education, retail, finance, transportation, wholesale, information, and utilities.

### Scope and Value of Service SE

#### SSE and the Enterprise

SSE takes a **customer-centric, end-to-end holistic view** to select and combine service system entities. It identifies linkages, relationships, constraints, interoperability standards, and interface agreements. SSE mandates participation from engineering, business operations, management science, behavioral science, social science, network science, and computer science.

#### Interoperability

Service system entities are typically designed and operated to satisfy their own objectives, which may not converge with overall service objectives. This demands: governance frameworks, service strategies, business objectives alignment, ICT/technology alignment, and end-to-end OAM&P procedures.

#### Service Life Cycle Stages

| Stage | Purpose | Key Outputs |
|-------|---------|-------------|
| **Service Strategy/Concept** | Elicit enterprise needs; explore service concepts | Service Description |
| **Requirements Analysis & Engineering** | Define service functions, entities, SLAs, QoS | Service Requirements Document (SRD) |
| **Systems Design/Development** | Architecture frameworks, trade-off analysis, entity specs | Service Specification Document |
| **Integration, Verification & Validation** | End-to-end IV&V procedures | Service IV&V Plans |
| **Transition/Deployment** | Service insertion, minimal impact to existing services | Service Operation Plans |
| **Operations / CSI** | Day-to-day delivery, monitoring, continuous improvement | CSI Plan |

#### SSE Knowledge and Skills (T-Shaped Professional)

Chang's 12 SSME skills: **Management of Service Systems, Operations, Service Processes, Business Management, Analytical Skills, Interpersonal Skills, Knowledge Management, Creativity & Innovation, Financial & Cost Analysis, Marketing Management, Ethics & Integrity, Global Orientation**.

#### Service Architecture Frameworks

Key frameworks: **Zachman, TOGAF, eTOM, SOA, DoDAF, NIST Smart Grid Reference Model, ITIL v.3, WS-BPEL**. No single framework covers all service system views; combinations are typically required. Example: the NIST Smart Grid model identifies standards/protocols for interoperability, cyber security, and architectures across energy subsystems.

Dynamic frameworks are an active research area — enabling real-time analysis of service changes on business processes, organizations, and revenue.

### Service SE Stages

#### 1. Service Strategy/Concept

Entry point: concept generated by end-users, business managers, engineering, new technologies, or IT trends. An **Integrated Service Development Team (ISDT)** assesses feasibility on enterprise processes, operations, technology, governance, and social/cultural impacts. ±30% time/cost estimate feeds the business case. Output: **Service Description**.

#### 2. Requirements Analysis and Engineering

Customer-centric view analyzing SLA, QoS, value co-creation, monitoring, and assessment requirements. Includes:
- Service Level Management (SLM) process requirements
- Governance, business, service, operations, and support system requirements
- Interface requirements, information flows, and data requirements
- Output: **Service Requirements Document (SRD), Service Operations Plans (SOPs), Operations Technical Plans (OTPs)**

#### 3. Systems Design/Development

Apply architecture frameworks, trade-off analyses among service system entities, allocate relationships (interactions) at all architecture levels. ITIL v.3 design processes: service catalog management, service level management, capacity management, availability management, service continuity management, security management, and supplier management.

#### 4. Service Integration, Verification & Validation

End-to-end IV&V from multiple perspectives: service validation, operational readiness, network validation, entity interoperability, content validation, and user acceptance test plans. Systems engineer plays the **integrator role** to ensure proper data flow across all systems.

#### 5. Service Transition/Deployment

Plan service insertion, technology insertion, process adaptation with minimal impact to existing services. ITIL v.3 processes: transition planning, change management, service asset & configuration management, release & deployment management, service validation & testing, evaluation, and knowledge management.

#### 6. Service Operations / Continuous Service Improvement (CSI)

Day-to-day management of end-to-end service delivery. ITIL v.3 operations processes: event management, incident management, problem management, request fulfillment, access management. CSI plans implement technologies for monitoring, measuring, and analyzing process and service metrics using PDCA (Plan-Do-Check-Act), Lean, Six Sigma, benchmarking, and gap analysis.

#### Tools and Technologies

Three domains:
- **Business Process Management (BPM)**: BPMN, WS-BPEL, workflow coordination
- **Service Design Process**: UML, SysML, MBSE, MDA, MOSES, optimization/queuing/stochastic modeling
- **Service Design Management**: IEEE 1220, ISO/IEC 9126, ISO 27001, ITIL v.3

---

## Related Chapters

- [[02_Nature_of_Systems]] — Product, Service, Enterprise, and SoS contexts
- [[05_Representing_Systems_with_Models]] — architecture frameworks, modeling techniques
- [[06_Systems_Approach_Applied]] — structured methods for SE practice
- [[07_Life_Cycle_Processes]] — generic SE life cycle models
- [[08_Systems_Engineering_Management]] — planning, risk, configuration, quality management
- [[09_Organizational_Strategy]] — SE in enterprise context
- [[11_Enterprise_Systems_Engineering]] — ESE as a related application context
- [[12_Systems_of_Systems]] — SoS as a related application context
- [[SEBoK_Part_3]] — Life Cycle Processes
- [[SEBoK_Part_4]] — Full SEBoK Part 4: Applications of Systems Engineering
