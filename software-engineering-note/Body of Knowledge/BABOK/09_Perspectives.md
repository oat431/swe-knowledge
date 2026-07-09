---
tags:
- perspectives
- business-analysis
- babok
---

# Perspectives

> *Source: BABOK® Guide v3 by IIBA, Chapter 10 — Perspectives*

> **Purpose:** Perspectives provide ways to approach business analysis work in a more focused manner suitable to the context. While the business analysis tasks detailed in the BABOK® Guide are applicable across all areas, each perspective interprets and understands the knowledge areas and tasks from the lens in which one is currently working. Each perspective follows a common structure: Change Scope, Business Analysis Scope, Methodologies/Approaches/Techniques, Underlying Competencies, and Impact on Knowledge Areas.

---

## The Agile Perspective

### Overview

The Agile Perspective highlights business analysis practiced in agile environments — characterized by a flexible mindset, constant change, and continuous reassessment. Business analysts perform analysis at the *last responsible moment*: detailed work is done just in time, not ahead of time. BA work is performed continuously throughout an initiative and relies heavily on interpersonal skills (communication, facilitation, coaching, negotiation).

**Core questions BAs help agile teams answer:**
- What need are we trying to satisfy?
- Is that need worth satisfying?
- Should we deliver something to satisfy that need?
- What is the right thing to do to deliver that need?

### Change Scope

- **Breadth:** Agile is most commonly used in software development, but increasingly applied to process engineering and business improvement. Can span single departments or multiple teams/divisions.
- **Depth:** Frequently part of larger programs including organizational transformation. Best suited where there is clear customer commitment, complex/unknown business needs, and empowered SMEs.
- **Value Delivery:** Emphasis on delivering value early through adaptive planning and continuous improvement. Stakeholders review the product frequently; the solution evolves with rapid response to change.
- **Scope Management:** Scope is constantly evolving, managed through a prioritized backlog list that is continually reviewed and re-prioritized.

### BA Impact

- **BA Position:** BA activities can be performed by a dedicated business analyst, the product owner/customer representative, or distributed throughout the team. Cross-skilled team members expand BA practice beyond a single specialist role.
- **Key Roles:** Agile team leader (scrum master/coach), product owner/customer representative, team members (domain experts), external stakeholders.
- **Outcomes:** Open communication and collaboration; strategic alignment of vision with organizational goals; just-enough, just-in-time documentation; shared responsibility for acceptance criteria and product vision.

### Key Techniques

| Technique | Purpose |
|---|---|
| **Behaviour Driven Development (BDD)** | Express product needs as concrete examples |
| **Kano Analysis** | Understand which features drive customer satisfaction |
| **Lightweight Documentation** | Ensure documentation fulfills an impending need with clear value |
| **MoSCoW Prioritization** | Prioritize stories: Must/Should/Could/Won't have |
| **Personas** | Fictional archetypes of typical users |
| **Planning Workshops** | Collaborative determination of deliverable value per time period |
| **Purpose Alignment Model** | Assess ideas in context of customer and value |
| **Real Options** | Know *when* to make decisions |
| **Relative Estimation** | Story points or ideal days for complexity/effort estimation |
| **Retrospectives** | Continuous improvement after every iteration |
| **Story Decomposition / Mapping / Storyboarding** | Structure requirements at appropriate detail; visualize sequences |
| **Value Stream Mapping** | Time-series representation of activities to deliver value |

### Key Agile Approaches

Scrum, Kanban, Extreme Programming (XP), Dynamic Systems Development Method (DSDM), Feature Driven Development (FDD), Crystal Clear, Disciplined Agile Delivery (DAD), Scaled Agile Framework (SAFe), Evolutionary Project Management (Evo).

### Underlying Competencies

Communication & collaboration, patience & tolerance, flexibility & adaptability, ability to handle change, ability to recognize business value, continuous improvement mindset.

---

## The Business Intelligence Perspective

### Overview

The BI Perspective focuses on transforming data into value-added information: where to source it, how to integrate it, and how to enhance and deliver it as analytic insight to support business decision making. BI initiatives apply data-centric architectures and technologies to deliver reliable, consistent, high-quality information for strategic, tactical, and operational performance management.

### Change Scope

- **Breadth:** Establishes a "single point of truth" across the organization. Integrates multiple data sources (internal, external, unstructured) into an enterprise data view.
- **Depth:** Supports decision making at three levels — executive (strategic), management (tactical), process (operational). Needs may involve reporting, analytic functionality, or data integration.
- **Value Delivery:** Timely, accurate, high-value, actionable information for better decisions — leading to improved performance in strategic, tactical, and operational processes (increased revenues, reduced costs).
- **Delivery:** Phased/incremental; extensible architecture supports progressive introduction at different organizational levels and functional areas. Often uses COTS packages.

### BA Impact

- **BA Position:** Acts as liaison between BI stakeholders and solution providers. May also participate in enterprise data modelling, decision modelling, dashboard design, and ad hoc query design.
- **BA Roles:** Business analyst (requirements/assessment), BI functional analyst (data mining/visualization), data analyst (source systems), data modeller/architect (logical data models).
- **Key Outcomes:** Business process coverage, decision models, source/target logical data models and data dictionaries, source data quality assessment, transformation rules, business analytics requirements (reports, dashboards, OLAP, data mining, predictive modelling), solution architecture.

### Key Techniques & Approaches

**Types of Analytics (incremental complexity and value):**
- **Descriptive:** Historical data for understanding past performance (dashboards, scorecards, standard reports)
- **Predictive:** Statistical analysis to identify patterns and predict future events (data mining, forecasting, alerts)
- **Prescriptive:** Optimization and simulation to determine best actions (decision automation)

**Supply-Driven vs. Demand-Driven:** Supply-driven maps existing data to define what's available; demand-driven starts with information needed for decisions and traces back to data sources.

**Structured vs. Unstructured Data:** Structured (schema-on-write, traditional data warehouse) vs. unstructured (schema-on-read, text/images/audio/video).

| Technique | Purpose |
|---|---|
| **Data Modelling / Data Dictionary** | Define source and target data structures |
| **Data Flow Diagrams** | Map dynamic data-in-motion, latency, accessibility |
| **Decision Modelling** | Specify data analytic requirements and business rules for decisions |
| **ETL Specification** | Extract, Transform, Load rules for data integration |
| **Balanced Scorecard** | Strategic performance measurement framework |
| **Prototyping (BI tools)** | Elicit and clarify stakeholder information requirements |

### Underlying Competencies

Business data and functional usage, complex data structure analysis, KPIs and metrics, decision modelling, data analysis techniques (statistics, profiling, pivoting), data warehouse/BI architecture, logical and physical data models, ETL best practices, BI reporting tools.

---

## The Information Technology Perspective

### Overview

The IT Perspective highlights BA when undertaken from the viewpoint of the impact of change on information technology systems. IT BAs deal with a wide range of complexity — from minor bug fixes to re-engineering entire IT infrastructures. They serve as translators between business and technology stakeholders.

**Key factors for IT BAs:** Solution impact (value/risk), organizational maturity (formality/flexibility of change processes), and change scope (breadth, depth, complexity).

### Change Scope

- **Triggers:** Create new capability, enhance existing capability, facilitate operational improvement, maintain existing systems, repair broken systems.
- **Breadth:** May focus on a single system or multiple interacting systems (in-house, COTS, outsourced). Scope is often narrowly focused on software/hardware but may broaden during analysis.
- **Depth:** Frequently requires explicit technical detail (individual data elements, interfaces between systems). BAs must understand the organizational whole to know which details deliver value.
- **Value Delivery:** Reduce operating costs, decrease wasted effort, increase strategic alignment/reliability, automate error-prone processes, repair problems, scale capabilities, implement new functionality.

### BA Impact

- **BA Position:** May be filled by a business analyst, IT BA (liaison), SME, software user, systems analyst, business process owner, technical specialist, or COTS representative.
- **Key Distinction:** In IT contexts, "design" traditionally refers to technical design by developers/architects. IT BAs define "solution requirements" (processes, UI, reports) while maintaining separation from technical design.
- **Outcomes:** Defined/testable/prioritized requirements, analysis of alternatives, business rules, gap analysis, functional decomposition, use cases/user stories, interface analysis, prototypes, process/state/decision/data models.

### Key Techniques

| Technique | Purpose |
|---|---|
| **Use Cases and Scenarios** | Describe actor-system interactions |
| **User Stories** | Capture stakeholder needs in agile format |
| **Data Modelling / Data Dictionary** | Define data structures and elements |
| **Data Flow Diagrams** | Model information flow between systems |
| **Interface Analysis** | Define interactions between IT systems |
| **Sequence Diagrams / State Modelling** | Model behavioral aspects of systems |
| **Prototyping** | Explore and validate solution concepts |
| **Functional Decomposition** | Break down complex systems |
| **Non-Functional Requirements Analysis** | Define performance, security, reliability constraints |
| **Vendor Assessment** | Evaluate COTS solutions |

### IT Methodologies

- **Predictive:** Structured, plan-driven, sequential phases (e.g., SSADM, Waterfall)
- **Adaptive:** Iterative and incremental (e.g., Unified Process)
- **Hybrid:** Overall vision with detailed definition within cycles
- **Requirements Engineering (RE):** Structured approach for requirements development/management across approaches

### Underlying Competencies

Systems thinking (seeing the larger picture), understanding of technical feasibility, influencing and facilitation skills, negotiation skills for cost vs. outcome trade-offs, understanding of detail required for technical solutions.

---

## The Business Architecture Perspective

### Overview

Business architecture models the enterprise to show how strategic concerns of key stakeholders are met and to support ongoing business transformation. It provides architectural descriptions and views (blueprints) to align strategic objectives with tactical demands. Solutions may include changes in business model, operating model, organizational structure, or drive other initiatives.

**Fundamental architectural principles:**
- **Scope:** The entire enterprise — not a single project, process, or piece of information
- **Separation of concerns:** Separates what the business does from how, who, where, when, why, and how well
- **Scenario driven:** Different business questions require different blueprints
- **Knowledge based:** Architectural components are catalogued in a knowledge base/repository

### Change Scope

- **Breadth:** Across the enterprise as a whole, a single line of business, or a single functional division. Broad scope manages consistency and integration at enterprise level.
- **Depth:** Focuses on executive (strategic) and management (execution) levels. Assesses processes at the value stream level, not operational/process level.
- **Value Delivery:** Provides insights into how organizational elements fit together; enables prioritization, resource allocation, impact analysis, and identification of needed changes. Facilitates coordinated action aligned with vision, goals, and strategy.

### BA Impact

- **BA Position:** The business architect understands the entire enterprise context, provides balanced insight across elements and their relationships, and delivers a holistic, understandable view of all specialties.
- **Required Knowledge:** Business strategy and goals, conceptual business information, enterprise IT architecture, process architecture, business performance and intelligence architecture.
- **Key Outcomes (Blueprints):** Business capability maps, value stream maps, organization maps, business information concepts, high-level process architecture, business motivation models.

### Key Techniques

| Technique | Purpose |
|---|---|
| **Archimate®** | Open standard modelling language for enterprise architecture |
| **Business Motivation Model (BMM)** | Formalize mission, vision, strategy, tactics, goals, objectives, policies, rules |
| **Capability Map** | Hierarchical catalogue of business capabilities (strategic, core, supporting) |
| **Customer Journey Map** | Depict customer journey through touch points and stakeholders |
| **Enterprise Core Diagram** | Model integration and standardization of the organization |
| **Information Map** | Catalogue of business concepts; common business vocabulary/taxonomy |
| **Organizational Map** | Relationships between business units, partners, capabilities, information |
| **Project Portfolio Analysis** | Holistic view of organizational initiatives |
| **Roadmap** | Actions, dependencies, responsibilities from current → transition → future state |
| **Value Mapping / Value Stream Mapping** | End-to-end value delivery representation |
| **TOGAF® / Zachman Framework** | Enterprise architecture development methods/ontologies |

### Reference Models

ACORD (insurance/finance), BMM (generic), COBIT (IT governance), eTOM/FRAMEWORX (communications), FEA SRM (government), ITIL (IT service management), PCF (multi-sector process classification), SCOR (supply chain), VRM (value chain).

### Underlying Competencies

High tolerance for ambiguity, ability to put things in broader context, transform requirements into solution concepts, suppress unnecessary detail for higher-level views, think in long time frames (years), deliver tactical outcomes that contribute to long-term strategy, interact at executive level, consider multiple scenarios, lead/direct organizational change, political acumen.

---

## The Business Process Management Perspective

### Overview

BPM is a management discipline and set of enabling technologies that focuses on how the organization performs work to deliver value across multiple functional areas. It views the organization through a process-centric lens. BPM determines how manual and automated processes are created, modified, cancelled, and governed.

**BPM life cycle activities:**
- **Designing:** Identify processes, define as-is and to-be states
- **Modelling:** Graphical representation; simulation with quantitative data to analyze potential value
- **Execution and Monitoring:** Collect actual process flow data for objective analysis
- **Optimizing:** Ongoing iteration — remove inefficiencies, add value

### Change Scope

- **Breadth:** May address a single process or all organizational processes. Comprehensive BPM spans the entire enterprise; individual initiatives improve specific processes/sub-processes.
- **Depth:** Uses BPM frameworks for deep understanding. Supply chain analysis decomposes group-level processes to individual tasks.
- **Value Delivery:** Improve operational performance (effectiveness, efficiency, adaptability, quality); reduce costs and risks; provide transparency into processes and operations.
- **BPM Drivers:** Cost reduction, quality increase, productivity, competition, risk management, compliance, process automation, core system implementation, innovation, post-merger rationalization, standardization, transformation, agility.

### BA Impact

- **BA Roles:**
  - **Process Architect:** Model, analyze, deploy, monitor, and continuously improve business processes. Develop standards and repository of reference models.
  - **Process Analyst/Designer:** Expert in documenting/understanding process design and performance trends; perform as-is analysis and recommend changes.
  - **Process Modeller:** Capture and document as-is and to-be processes. Often combined with analyst/designer role.
- **Key Outcomes:** Business process models (as-is, to-be, transition), business rules, process performance measures, business decisions, process performance assessment.

### Delivery Approaches

- **Business Process Re-engineering (BPR):** Major process redesign across the enterprise
- **Evolutionary change:** Overall objectives with incremental sub-process improvements
- **Substantial discovery:** Reveal actual (undocumented) processes
- **Process benchmarking:** Compare against industry best practices (quality, time, cost)
- **Specialized BPMS applications:** Tools that execute process models directly

**Organizing principles:** Top-down (enterprise-spanning) vs. bottom-up (tactical/departmental); people-centric (workflow changes) vs. IT-centric (automation).

### Key Frameworks & Methodologies

| Framework | Focus |
|---|---|
| **ACCORD** | Map current state models and unstructured data to conceptual models |
| **eTOM** | Telecommunications/service-oriented process framework |
| **GSRM** | Generic government processes and patterns |
| **MIPI** | Cyclical process improvement (assess → model → redesign → implement → review) |
| **PCF** | Process classification for benchmarking and measurement |

| Methodology | Focus |
|---|---|
| **Adaptive Case Management (ACM)** | Non-fixed processes with high human interaction |
| **Business Process Re-engineering (BPR)** | Fundamental rethinking and radical redesign |
| **Continuous Improvement (CI)** | Ongoing monitoring and adjustment toward performance targets |
| **Lean** | Eliminate waste (work customer won't pay for) |
| **Six Sigma** | Eliminate variations; statistically oriented, data-centric |
| **Theory of Constraints (TOC)** | Optimize by managing throughput, operational expense, inventory |
| **Total Quality Management (TQM)** | Highest quality products/services meeting customer expectations |

### Key Techniques

| Technique | Purpose |
|---|---|
| **Cost Analysis (Activity Based Costing)** | Cost per activity for process cost understanding |
| **Critical to Quality (CTQ)** | Align process improvement to customer requirements |
| **Cycle-time Analysis** | Time each activity takes within a process |
| **DMADV / DMAIC** | Six Sigma roadmaps for new design or improvement |
| **Drum-Buffer-Rope (DBR)** | Maximize constraint throughput |
| **Failure Mode and Effect Analysis (FMEA)** | Investigate process failures and identify causes |
| **House of Quality / Voice of Customer** | Relate customer desires to organizational capabilities |
| **IGOE (Inputs, Guide, Outputs, Enablers)** | Describe process context |
| **Kaizen Event** | Focused, rapid improvement of a specific activity |
| **Process Simulation** | Assess process variations under actual conditions |
| **SIPOC (Suppliers, Inputs, Process, Outputs, Customers)** | Summarize process inputs and outputs |
| **TOC Thinking Processes** | Cause-and-effect models to diagnose conflicts |
| **Value Added Analysis** | Identify improvement opportunities per process step |
| **Value Stream Analysis** | Assess value added by each functional area end-to-end |
| **5Ws (Who, What, When, Where, Why + How)** | Foundation for basic information gathering |

### Underlying Competencies

Challenge the status quo, dig to understand root causes, encourage new ideas, move between internal and external process views, negotiation and arbitration skills, neutral and independent facilitation, communication across organizational boundaries.

---

## Impact on Knowledge Areas (Common Themes Across Perspectives)

Each perspective maps its practices to the six BABOK® Guide knowledge areas, modifying how each KA is applied:

| Knowledge Area | How Perspectives Modify It |
|---|---|
| **BA Planning & Monitoring** | Agile defers detailed planning until activity is ready; BI considers stakeholder experience with BI concepts; IT plans around organizational standards; Business Architecture considers strategy, operating model, and change capacity; BPM uses progressive elaboration |
| **Elicitation & Collaboration** | Agile uses progressive elicitation just-in-time; BI employs specialized data-oriented tools and cross-functional workshops; IT brings business and technical staff together; Business Architecture requires executive access and strategy advocacy; BPM focuses on process modelling-driven elicitation |
| **Requirements Life Cycle Management** | Agile re-prioritizes backlog continuously; BI manages structural dependencies across phased delivery; IT emphasizes traceability and change control; Business Architecture uses architecture review boards and portfolio management; BPM connects process changes to requirements and solution support |
| **Strategy Analysis** | Agile constantly reassesses solution against business context; BI uses data models to map current/future state; IT analyzes ripple effects across systems; Business Architecture provides architectural views for current/future/transition states; BPM analyzes the process role in enterprise value chains |
| **Requirements Analysis & Design Definition** | Agile uses just-in-time analysis with user stories and acceptance criteria; BI applies data modelling, decision modelling, and business rules; IT separates solution requirements from technical design; Business Architecture provides architectural context to avoid duplication; BPM defines to-be process models with business rules and decisions |
| **Solution Evaluation** | Agile evaluates evolving solution at end of every cycle; BI explores additional value beyond existing reporting; IT evaluates system interactions and end-to-end quality; Business Architecture assesses performance against SMART objectives; BPM evaluates process performance repeatedly, triggering optimization cycles |

## Related Chapters

- [[00_Introduction_to_BABOK]]: BABOK® Guide overview, structure, and key concepts
- [[00_Introduction_to_BABOK]]: BACCM™ core concepts that underpin all perspectives
- [[01_BA_Planning_and_Monitoring]]: Planning BA work across different contexts
- [[02_Elicitation_and_Collaboration]]: Elicitation techniques adapted per perspective
- [[03_Requirements_Life_Cycle_Management]]: Managing requirements across predictive and adaptive life cycles
- [[04_Strategy_Analysis]]: Current/future state analysis from each perspective's viewpoint
- [[05_Requirements_Analysis_and_Design]]: How analysis and design differ across perspectives
- [[06_Solution_Evaluation]]: Evaluating solutions within each discipline's context
- [[08_Techniques_Catalog]]: Comprehensive reference for all techniques referenced across perspectives
- [[07_Underlying_Competencies]]: Core competencies required across all BA disciplines
