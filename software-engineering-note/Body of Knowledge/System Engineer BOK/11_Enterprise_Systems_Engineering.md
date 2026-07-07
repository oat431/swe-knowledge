---
tags: [enterprise-se, applications, systems-engineering, sebok]
---

> *Source: SEBoK v2, Part 4 — Enterprise Systems Engineering*

## Purpose

Enterprise Systems Engineering (ESE) is the application of systems engineering principles, concepts, and methods to the planning, design, improvement, and operation of an enterprise. ESE expands beyond traditional systems engineering (TSE) — which focuses on development projects and product engineering — to address the inherent complexities of enterprises as socio-technical systems. ESE deals with both solving problems and exploiting opportunities for better ways to achieve enterprise goals.

**Lead Authors**: James Martin, Dick Fairley, and Bud Lawson (with contributions from Alan Faisandier and Judith Dahmann)

---

## Enterprise SE Background

### What Is an Enterprise?

An **enterprise** consists of a purposeful combination of interdependent resources — people, processes, organizations, supporting technologies, and funding — that interact with each other and their environment to achieve business and operational goals across geography and time. Key definitions:

| Source | Definition |
|--------|------------|
| ISO 15704 (2000) | One or more organizations sharing a definite mission, goals, and objectives to offer a product or service |
| CIO Council (1999) | An organization supporting a defined business scope with interdependent resources coordinating functions |
| MOD (2004) | Tightly bounded single-executive entity OR loosely bounded multi-owner entity, both existing to achieve outcomes |
| Giachetti (2010) | A complex, adaptive socio-technical system of interdependent resources of people, processes, information, and technology |

### Enterprise vs. Organization

An enterprise is **not** equivalent to an organization. An enterprise includes not only participating organizations but also people, knowledge, processes, principles, policies, practices, facilities, land, and intellectual property. Some enterprises are self-organizing (emergent) rather than mandated — often more flexible and agile. Giachetti frames the organization as a *view* of the enterprise that defines structure and relationships of organizational units.

### Extended Enterprise

The **extended enterprise** encompasses upstream suppliers, downstream consumers, end-user organizations, and sidestream partners — all entities that directly or indirectly collaborate in design, development, production, and delivery of products or services to end users.

### Capabilities in the Enterprise

Three kinds of capability operate at different levels:

1. **Organizational Capability** — collective ability of people, teams, and business units (addressed in SE Organizational Strategy)
2. **System Capability** — inherent ability of a developed or acquired system
3. **Operational Capability** — the enterprise's overall ability to achieve its mission, enabled by system capabilities

Individual **competence** translates to organizational capabilities through work practices. System capabilities are developed to enhance operational capability in response to stakeholder concerns.

### Enterprise Components

Enterprise components span both **asset domains** (application/software, infrastructure/hardware, IT services, locations) and **concept domains** (policy, market, strategy, transition, financial, knowledge, process, analysis). A typical enterprise architecture schema can contain over a hundred types of modeling objects.

### Essential Challenges (Rouse 2009)

- **Growth**: Increasing impact, perhaps in saturated/declining markets
- **Value**: Enhancing relationships of processes to benefits and costs
- **Focus**: Pursuing opportunities and avoiding diversions
- **Change**: Competing creatively while maintaining continuity
- **Future**: Investing in inherently unpredictable outcomes
- **Knowledge**: Transforming information to insights to programs
- **Time**: Carefully allocating the organization's scarcest resource

### Enterprise Transformation

Enterprises constantly transform — at individual and enterprise levels — in response to evolving opportunities and emerging threats. Transformation drivers (Rouse):

1. **Value Opportunities** — lure of greater success via market/technology
2. **Value Threats** — danger of anticipated failure
3. **Value Competition** — other players' transformation initiatives
4. **Value Crises** — steadily declining performance requiring survival action

Transformation occurs in the context of economy, markets (constituency), and the "intraprise" — internal systems including information systems, social systems, and cultural systems.

---

## The Enterprise as a System

### Core Distinction

What distinguishes enterprise systems from product systems is the **inclusion of people as a component of the system**, not merely as users/operators. As Giachetti (2010) states: the greater view of enterprise systems is inclusive of the processes the system supports, the people who work in it, and the information and knowledge content.

### Creating Value

The primary purpose of an enterprise is to create value for society, stakeholders, and participating organizations. Three types of organizations matter: **businesses**, **projects**, and **teams**. A typical business participates in multiple enterprises through its portfolio of projects.

### Resource Optimization

Key organizational choices:

| Type | Description |
|------|-------------|
| **Product-Oriented** | Projects responsible for hiring, training, firing staff and managing all assets |
| **Functional** | Projects delegate almost all work to functional groups |
| **Matrix** | Functional specialists have a "home" between project assignments |

The framework from *Enterprise Architecture as Strategy* (Ross, Weill, and Robertson): enterprises choose whether to unify operations and/or unify their information base.

### Knowledge in the Enterprise

Two kinds: **explicit** (written down, coded) and **tacit** (in people's heads, in relationships). The ability to create value depends critically on people, what they know, how they work together, and how they are motivated.

### Programs vs. Projects (OGC 2010)

- **Programme**: Temporary flexible organization structure to coordinate, direct, and oversee related projects to deliver outcomes of strategic relevance — spans several years
- **Project**: Shorter duration (months), focused on creating deliverables within agreed cost, time, and quality parameters

### Practical Considerations (Rebovich and White 2011)

- Set **enterprise fitness** as the key measure of system success
- Deal with uncertainty through adaptation: variety, selection, exploration, experimentation
- Leverage layered architectures with loose couplers
- Govern through shaping the **POET landscape** (Political, Operational, Economic, Technical) — don't try to control the enterprise like a TSE project

### Executive Concerns and SE Enablers

| Executive Concern | SE Enabler |
|-------------------|------------|
| Identifying ends, means, scope, and candidate changes | System complexity analysis ("as is" vs. "to be") |
| Evaluating changes in process behaviors and performance | Organizational simulation of process flows |
| Assessing economics (investments, costs, returns) | Economic modeling (cash flows, volatility, options) |
| Defining the new enterprise (processes, integration) | Enterprise architecting (workflow, maturity levels) |
| Designing strategy for culture change | Leadership, vision, strategy, incentives |
| Developing transformation action plans | Implementation planning (tasks, schedule, people) |

### Enterprise Engineering

Enterprise design does not occur at a single point in time — enterprises evolve and are constantly changing. Giachetti calls this "enterprise engineering," encompassing modeling, simulation, total quality management, change management, and bottleneck/cost/workflow/value-added analysis.

### Supersystem Constructs

**System of Systems (SoS)** — per Maier (1998): an assemblage of components that are individually regarded as systems and possess **(a) operational independence** and **(b) managerial independence** of components. Four SoS types: Directed, Acknowledged, Collaborative, Virtual.

**Federation of Systems (FoS)** — characterized by significant autonomy, heterogeneity, and geographic dispersion. Each system strongly controls its own destiny but chooses to participate. Handy's federalist principles adapted for SE: subsidiarity, interdependence, uniform business practices, separation of powers, dual citizenship, and scales of SE.

**Key distinction**: Not every enterprise is an SoS. Enterprises explicitly establish operational *dependence* between systems to maximize efficiency and effectiveness. An enterprise system and an SoS are different types of things with different properties and characteristics.

---

## Related Business Activities

ESE supports the following business management activities (Martin 2010):

- **Mission and Strategic Planning** — setting enterprise vision, goals, and objectives
- **Business Processes and Information Management** — value stream design and knowledge management
- **Performance Management** — measuring and improving enterprise effectiveness
- **Portfolio Management** — centralized management of processes, methods, and technologies to analyze and collectively manage projects/programs
- **Resource Allocation and Budgeting** — assigning people, facilities, money, and infrastructure based on capability gap criticality
- **Program and Project Management** — coordinating development, production, and operations projects

### Business Management Cycles

- **PDCA Cycle** (Plan-Do-Check-Act / Deming / Shewhart): ESE uses PDCA as a fundamental tenet — plan transformation, execute, monitor ("check"), and act to correct
- **OODA Loop** (Observe-Orient-Decide-Act / Boyd): Continuous situation awareness, complementary to discrete PDCA cycles

### Portfolio Management

Project Portfolio Management (PPM) determines the optimal resource mix and schedules activities to achieve organizational goals while honoring constraints. Enterprise portfolios may include systems, platforms, facilities, land, and intellectual property — not just projects.

### Enterprise Governance

Governance frameworks provide structure and controls for both steady-state operations and business transformation. Governance bodies approve project initiation and regularly review progress, continuing relevance, and mutual coherence. Governance may be top-down, peer-to-peer, or both.

### Multi-Level Enterprises

Enterprises may not always have full control over resources. Example: the Internet Engineering Task Force (IETF) is responsible for smooth Internet operation yet controls none of the requisite resources. In multi-level structures, a **parent enterprise** may have no resources — these are owned by subordinate **child enterprises**. ESE processes are allocated accordingly, with performance feedback enabling course correction.

---

## Enterprise SE Key Concepts

### TSE vs. ESE

| Aspect | Traditional SE (TSE) | Enterprise SE (ESE) |
|--------|---------------------|---------------------|
| Focus | System acquisition and implementation | Strategic planning, investment analysis, and system acquisition |
| Driver | User requirements (frozen for design) | Evolving visions, goals, governance, technology, user expectations |
| Methodology | Output-based (functional analysis, OO analysis) | Outcome-based (business analysis, mission needs analysis) |
| Nature | Technical | Entrepreneurial, business-driven, economic |
| Agents | Users as external stakeholders | People as purposeful agents within the system |

### Closing the Gap — MITRE ESE Process Areas

In addition to TSE processes, MITRE includes (DeRosa 2005):

1. **Strategic Technical Planning** — establishing overall technical strategy
2. **Enterprise Architecture** — modeling how parts fit together
3. **Capabilities-Based Planning Analysis** — translating vision into current and future capabilities
4. **Technology Planning** — characterizing technology trends and roadmaps
5. **Enterprise Analysis and Assessment** — measuring progress toward enterprise vision

ESE is more like a **"regimen"** (Kuras and White 2005) — responsible for identifying outcome spaces, shaping the development environment, coupling development to operations, and rewarding results rather than perceived promises.

### Role of Requirements in ESE

TSE freezes requirements long enough for design and delivery. ESE must account for the fact that enterprises are driven not by stable requirements but by **continually changing** organizational visions, goals, governance priorities, evolving technologies, and user expectations. People act as purposeful agents who choose both their goals and the means for accomplishing them.

### Enterprise Entities and Relationships

Enterprise entities group into two categories:

| Asset Domains | Concept Domains |
|---------------|-----------------|
| Application/Software, Infrastructure/Hardware, IT Products, IT Services, Locations, Organizations, Products/Services | Data, Documents, Analysis, Financial, Knowledge/Skill, Market, Policy, Process, Resource, Strategy, Timeline, Transition |

### Enterprise Architecture Frameworks

Prominent frameworks (Urbaczewski and Mrdalj 2006):

| Framework | Description |
|-----------|-------------|
| **Zachman Framework** | Matrix-based classification of architecture artifacts |
| **DoDAF** | U.S. Department of Defense Architecture Framework |
| **FEAF** | Federal Enterprise Architecture Framework |
| **TEAF** | Treasury Enterprise Architecture Framework |
| **TOGAF** | The Open Group Architecture Framework |

ISO/IEC 42010 specifies how to create architecture descriptions. Architecture descriptions can be informal (simple graphics and tables) or formal (rigorous modeling tools).

---

## Enterprise SE Process Activities

### The SE Role in Enterprise Transformation

ESE determines the balance between **complexity and order**, and in turn between **effectiveness and efficiency**. As Ackoff states: efficiency is evaluated efficiency multiplied by value; intelligence increases efficiency, but wisdom increases effectiveness. ESE has a key role in establishing the right balance.

### Value Stream Analysis

Value stream analysis treats the enterprise as a system, providing insights into where value is added as activities move toward final delivery. It relates each step to costs (money, time, energy, materials) and highlights unnecessary space, excessive distance, and processing inefficiencies. Associated with "lean enterprise" initiatives (originated at Toyota as "material and information mapping").

### Four Core ESE Processes (Martin 2010)

**1. Strategic Technical Planning (STP)**
Establish the overall technical strategy — balancing standards adoption and new technologies with trans-disciplinary people aspects (psychology, sociology, organizational change management). Maps technology roadmaps against capability roadmaps to identify alignment, synergy, risks, and opportunities.

**2. Capability-Based Planning Analysis**
Translate enterprise vision and goals into current and future capabilities. Analyze missions, identify capability gaps, and assess program/project/system opportunities. Describes capabilities in hierarchies and roadmaps — technology roadmaps for system capabilities, business capability roadmaps (BCRMs) for operational capabilities.

**3. Technology and Standards Planning**
Characterize technology trends, predict technology readiness levels, and define technology roadmaps. Assess technical standards to determine how they inhibit or enhance new technology incorporation. Forecast future standards and identify standardization activities.

**4. Enterprise Evaluation and Assessment (EE&A)**
Determine if the enterprise is heading in the right direction by measuring progress toward the enterprise vision. Establish a measurement program, project measures into the future, identify risks and opportunities, and diagnose problems. EE&A must go beyond traditional metrics — it looks for **break points** where capabilities are significantly enhanced or totally disabled (Roberts 2006).

Key characteristics: multi-scale analysis, early and continuous operational involvement, lightweight C2 representations, developmental versions for assessment, minimal infrastructure, flexible M&S/OITL/HWIL capabilities, and in-line continuous performance monitoring.

### Enterprise Architecture and Requirements

Enterprise architecture (EA) provides a model to understand how parts of the enterprise fit together. Enterprise requirements focus on cross-cutting measures for overall enterprise success — applying to product systems, business processes, inter-organizational commitments, hiring practices, and investment directions.

Architecture descriptions follow standardized viewpoints, views, and models. Government agencies increasingly use architecture-based investment processes (e.g., FEAF, DoDAF).

### Opportunity vs. Risk at the Enterprise Scale

At the enterprise level, the focus should be on **opportunity management** more than risk management (White 2006, Rebovich and White 2011). The greatest enterprise risk may be in *not pursuing* enterprise opportunities. Traditional risk management typically addresses only threats (negative uncertainties), failing to address opportunities (positive uncertainties).

### Complete ESE Process Elements

1. Strategic Technical Planning
2. Capability-Based Planning Analysis
3. Technology and Standards Planning
4. Enterprise Evaluation and Assessment
5. Opportunity and Risk Assessment and Management
6. Enterprise Architecture and Conceptual Design
7. Enterprise Requirements Definition and Management
8. Program and Project Detailed Design and Implementation
9. Program Integration and Interfaces
10. Program Validation and Verification
11. Portfolio and Program Deployment and Post Deployment
12. Portfolio and Program Life Cycle Support

---

## Enterprise Capability Management

### Three Kinds of Capability

1. **Organizational Capability** — ability of people, teams, and business units (addressed in Enabling Businesses and Enterprises)
2. **System Capability** — ability of developed/acquired systems (addressed in Technical Management and Product/Service Life Management)
3. **Operational Capability** — the enterprise's overall ability to perform its mission (described here)

The purpose of enterprise capability management is to **vector** the enterprise from its current baseline trajectory to a more desirable position that better meets strategic goals and objectives, given all resource constraints.

### Needs Identification and Assessment

- **Operational needs**: expressions of something desirable in direct support of end-user activities (e.g., "Provide transportation services to commuters in metropolitan London")
- **Enterprise needs**: expressions of something desirable in support of internal activities — maximizing efficient utilization of assets, enhancing productivity, eliminating waste (e.g., "Decrease power required for data center operations")

Enterprise needs go beyond eliminating waste — they may involve countering threats, meeting policy goals, doing business more efficiently, exploiting technology opportunities, replacing obsolete systems, or creating integrated enterprises.

### Capability Identification and Assessment

Capabilities exist solely to meet mission and enterprise needs. The desired levels are compared to the baseline to determine **capability gaps** (and excesses). Gaps and excesses are mapped into timeframes (near-term, mid-term, far-term) to understand the timing and intensity of change required.

### Enterprise Architecture Formulation and Assessment

EA is used to determine how best to fill capability gaps and minimize excess capabilities. A baseline architecture characterizes the current state and trajectory. Needs and gaps determine where elements need to be added, dropped, or changed — each modification representing a potential benefit, cost, and risk. EA supports the entire capability management activity with enterprise-wide views of strategy, priorities, plans, resources, activities, locations, facilities, products, and services.

### Opportunity Identification and Assessment

EA helps identify improvement opportunities that require investment of time, money, facilities, and personnel — as well as "divestment" opportunities (selling assets, reducing capacity, canceling projects). Opportunities are assessed as a portfolio with dependencies and interfaces. A business case assessment is required for each opportunity or set.

### Enterprise Portfolio Management

For large or complicated opportunity sets, portfolio management techniques are employed. Portfolio elements may include bids, projects, products, services, technologies, intellectual property, or any combination.

### Enterprise Improvement Planning and Execution

Results are compiled into an enterprise plan considering system capabilities, organizational capabilities, funding constraints, legal commitments, partner arrangements, intellectual property, and personnel development. The plan typically spans a decade or more, aligned with strategic goals and leadership priorities. Planned changes have associated performance targets monitored to ensure progress — the plan is adjusted as needed.

### Enterprise Capability Change Management

Seven lenses for change management (Nightingale and Srinivasan 2011): strategic objectives, stakeholders, processes, performance metrics, current state alignment, resources, and maturity assessment.

Capability management balances economy in meeting current operational requirements with sustainable use of current capabilities and development of future capabilities — helping organizations understand, integrate, re-align, and apply total enterprise ability to achieve strategic and current operational objectives.

ITIL (Information Technology Infrastructure Library) best practices for operations management can inform operational capability elements, especially for IT service management alignment with business needs.

---

## Related Chapters

- [[01_Systems_Fundamentals]] — definitions, concepts, principles, and heuristics
- [[02_Nature_of_Systems]] — engineered, natural, and social systems; FFF triad
- [[03_Systems_Science]] — complexity, emergence, and systems theory
- [[04_Systems_Thinking]] — perspectives, principles, and patterns
- [[06_Systems_Approach_Applied]] — structured methods for SE practice
- [[10_Product_Systems_Engineering]] — PSE context in SEBoK Part 4
- [[12_Service_Systems_Engineering]] — service systems as related enterprise components
- [[13_Systems_of_Systems]] — SoS engineering and its relationship to enterprises
- [[SEBoK_Part_3]] — Life Cycle Processes (ISO/IEC/IEEE 15288)
- [[SEBoK_Part_5]] — Enabling SE (organizational strategy, individuals, businesses)
