---
tags: [data-architecture, data-management, dmbok]
source: "DMBoK v2, Chapter 4"
created: 2026-07-09
---

# Data Architecture

## 1. Introduction

Data Architecture is fundamental to effective [[data-management|data management]]. Architects seek to design in a way that brings value to the organization through an optimal technical footprint, operational and project efficiencies, and the increased ability of the organization to use its data. Achieving this requires good design, planning, and the ability to ensure that designs and plans are executed effectively.

Data Architects define and maintain specifications that:

- Define the current state of data in the organization
- Provide a standard business vocabulary for data and components
- Align Data Architecture with enterprise strategy and business architecture
- Express strategic data requirements
- Outline high-level integrated designs to meet these requirements
- Integrate with the overall [[enterprise-architecture|enterprise architecture]] roadmap

An overall Data Architecture practice includes:

- Using Data Architecture artifacts (master blueprints) to define data requirements, guide data integration, control data assets, and align data investments with business strategy
- Collaborating with, learning from, and influencing various stakeholders engaged with improving business or IT systems development
- Using Data Architecture to establish the semantics of an enterprise via a common business vocabulary

---

## 2. Essential Concepts

### 2.1 Enterprise Architecture Domains

Data Architecture operates in the context of other architecture domains, including business, application, and technical architecture. Architects from different domains must address development directions and requirements collaboratively, as each domain influences and constrains the others.

| Domain | Enterprise Business Architecture | Enterprise Data Architecture | Enterprise Applications Architecture | Enterprise Technology Architecture |
|---|---|---|---|---|
| **Purpose** | Identify how an enterprise creates value for customers and other stakeholders | Describe how data should be organized and managed | Describe the structure and functionality of applications | Describe the physical technology needed to enable systems to function and deliver value |
| **Elements** | Business models, processes, capabilities, services, events, strategies, vocabulary | Data models, data definitions, data mapping specifications, data flows, structured data APIs | Business systems, software packages, databases, integration tools | Technical platforms, networks, security, integration tools |
| **Dependencies** | Establishes requirements for the other domains | Manages data created and required by business architecture | Acts on specified data according to business requirements | Hosts and executes application architecture |
| **Roles** | Business architects and analysts, business stewards | Data architects and modelers, data stewards | Applications architects | Infrastructure architects |

### 2.2 Enterprise Architecture Frameworks

An architecture framework is a foundational structure used to develop a broad range of related architectures. It provides ways of thinking about and understanding architecture — an "architecture for architecture." IEEE Computer Society maintains ISO/IEC/IEEE 42010:2011 as a standard for Enterprise Architecture Frameworks.

#### 2.2.1 Zachman Framework for Enterprise Architecture

The most well-known enterprise architectural framework, developed by John A. Zachman in the 1980s, recognizes that different audiences have different perspectives on architecture. The Zachman Framework is an **ontology** — a 6×6 matrix comprising the complete set of models required to describe an enterprise and the relationships between them.

**Two Dimensions:**

- **Communication Interrogatives (Columns):** What, How, Where, Who, When, Why
- **Reification Transformations (Rows):** Identification, Definition, Representation, Specification, Configuration, Instantiation

**Column Meanings (translated to enterprise architecture):**

| Column | Meaning | Description |
|---|---|---|
| **What** | Inventory | Entities used to build the architecture |
| **How** | Process | Activities performed |
| **Where** | Distribution | Business location and technology location |
| **Who** | Responsibility | Roles and organizations |
| **When** | Timing | Intervals, events, cycles, and schedules |
| **Why** | Motivation | Goals, strategies, and means |

**Row Perspectives (reification transformations):**

| Perspective | Role | Focus |
|---|---|---|
| Executive (Scope Context) | Planner | Lists of business elements defining scope in identification models |
| Business Management (Business Concepts) | Owner | Clarification of relationships between business concepts in definition models |
| Architect (System Logic) | Designer | System logical models detailing requirements and unconstrained design |
| Engineer (Technology Physics) | Builder | Physical models optimizing design for implementation under constraints |
| Technician (Tool Components) | Implementer | Technology-specific view of how components are assembled and operate |
| User (Operational Instances) | Participant | Actual functioning instances; no models at this level |

Each cell in the Zachman Framework represents a unique type of design artifact, defined by the intersection of its row and column.

### 2.3 Enterprise Data Architecture

Enterprise Data Architecture defines standard terms and designs for elements important to the organization. It includes depiction of business data — collection, storage, integration, movement, and distribution. As data flows through an organization, it is secured, integrated, stored, recorded, catalogued, shared, reported on, analyzed, and delivered to stakeholders. Along the way, data may be verified, enhanced, linked, certified, aggregated, anonymized, and used for analytics until archived or purged.

Enterprise Data Architecture descriptions must include both:

1. **Enterprise Data Models (EDM)** — data structures and data specifications
2. **Data Flow Design** — requirements and blueprints for storage and processing

#### 2.3.1 Enterprise Data Model (EDM)

The EDM is a holistic, enterprise-level, implementation-independent conceptual or logical data model providing a common consistent view of data across the enterprise. It includes:

- Key enterprise data entities (business concepts)
- Their relationships
- Critical guiding business rules
- Some critical attributes

The EDM sets the foundation for all data and data-related projects. Any project-level data model must be based on the EDM, which should be reviewed by stakeholders for consensus.

**Building the EDM:**

- Adopting an industry standard model can jumpstart development, but producing enterprise-wide data models still requires significant investment
- EDMs can be built at different levels of detail; resource availability influences initial scope
- Most successful EDMs are built **incrementally and iteratively**, using layers

**Model Layers (Figure 23 — Enterprise Data Model Hierarchy):**

1. **Enterprise Conceptual Model:** 1 diagram with 12–20 significant business subject areas and relationships
2. **Subject Area Models:** 1+ diagrams per subject area, 50+ significant entities within each subject area with relationships
3. **Subject Area Logical Models:** Increased detail by adding attributes and less-significant entities and relationships
4. **Application/Project Logical Data Models (LDM):** Limited scope to the subject and one-step external relationships
5. **Application/Project Physical Data Models (PDM):** Limited scope to subject physical objects and relationships

**Vertical and Horizontal Linkages:**

- **Vertical:** Models at each level map to models at other levels, creating model lineage (e.g., a `MobileDevice` table links to `MobileDevice` entity in logical, subject area, and conceptual models)
- **Horizontal:** Entities and relationships may appear in multiple models at the same level; entities in one subject area may relate to entities in other subject areas as external links

**Subject Area Structure:**

The Subject Area discriminator (the principles forming the Subject Area structure) must be consistent throughout the EDM. Common discriminator principles include:

- Normalization rules (most effective for Data Architecture)
- Systems portfolios (funding-based)
- Data governance structure and data ownership (organizational)
- Top-level processes (business value chains)
- Business capabilities (enterprise architecture-based)

**Building Approaches:**

- **Top-down:** Start with forming Subject Areas, then populate them with models
- **Bottom-up:** Build Subject Area structure based on existing data models
- **Combined (recommended):** Start bottom-up using existing models, then complete via delegated Subject Area modeling to projects

#### 2.3.2 Data Flow Design

Data flows are a type of [[data-lineage|data lineage]] documentation depicting how data moves through business processes and systems. End-to-end data flows illustrate where data originated, where it is stored and used, and how it is transformed as it moves inside and between diverse processes and systems.

**Data flows map and document relationships between data and:**

- Applications within a business process
- Data stores or databases in an environment
- Network segments (useful for security mapping)
- Business roles, depicting CRUD responsibilities (Create, Read, Update, Delete)
- Locations where local differences occur

**Documentation levels:** Subject Area, business entity, or attribute level. Systems can be represented by network segments, platforms, common application sets, or individual servers.

**Representation formats:**

- **Two-dimensional matrices (CRUD matrices):** Show which processes create/use which data entities. Takes into account that data does not flow in only one direction — exchanges between processes are many-to-many. Originated from IBM's Business Systems Planning (BSP) method and popularized by James Martin's Information Systems Planning (ISP).
- **Data flow diagrams:** Traditional high-level diagrams depicting what kind of data flows between systems.

---

## 3. Activities

Data and enterprise architecture deal with complexity from two viewpoints:

- **Quality-oriented:** Focus on improving execution within business and IT development cycles. Unless architecture is managed, it deteriorates — systems become more complex and inflexible, creating risk.
- **Innovation-oriented:** Focus on transforming business and IT to address new expectations and opportunities, driving innovation with disruptive technologies.

### 3.1 Establish Data Architecture Practice

Ideally, Data Architecture should be an integral part of enterprise architecture. If no enterprise architecture function exists, a Data Architecture team can still be established. The organization should adopt a framework that helps articulate goals and drivers.

**Work streams (executed serially or in parallel):**

| Work Stream | Description |
|---|---|
| **Strategy** | Select frameworks, state approaches, develop roadmap |
| **Acceptance and Culture** | Inform and motivate changes in behavior |
| **Organization** | Organize Data Architecture work by assigning accountabilities and responsibilities |
| **Working Methods** | Define best practices and perform Data Architecture work within development projects |
| **Results** | Produce Data Architecture artifacts within an overall roadmap |

**Enterprise Data Architecture influences project scope through:**

- Defining project data requirements
- Reviewing project data designs (conceptual, logical, physical model consistency)
- Determining data lineage impact (ensuring consistent and traceable business rules)
- Data replication control (balancing performance with consistency)
- Enforcing Data Architecture standards (principles, procedures, guidelines, blueprints)
- Guiding data technology and renewal decisions

#### 3.1.1 Evaluate Existing Data Architecture Specifications

Identify existing documentation for current systems and evaluate for accuracy, completeness, and level of detail. Update as necessary to reflect the current state.

#### 3.1.2 Develop a Roadmap

A roadmap describes the architecture's 3–5 year development path. Together with business requirements, actual conditions, and technical assessments, it describes how the target architecture will become reality. The roadmap must be integrated into an overall enterprise architecture roadmap with milestones, resources, and cost estimations divided by business capability work streams.

**Business-data-driven roadmap approach:** Start with business capabilities that are most independent (least dependency from other activities), ending with those most dependent on others. This follows the overall business data origination order:

1. **Master Data** (lowest dependency): Product Management, Customer Management
2. **Intermediate:** Product Part Management, BOM Management, Sales Item Management
3. **High Dependency:** Sales Order Management, Assembly Structure Management, Production Order Management, Customer's Invoice Management

#### 3.1.3 Manage Enterprise Requirements within Projects

Data models at the architectural level should have a global view of the enterprise with clear definitions understood throughout the organization. At the project level:

1. **Define Scope:** Ensure scope and interfaces align with the EDM. Understand project contribution to overall Data Architecture. Determine dependencies with stakeholders outside project scope.
2. **Understand Business Requirements:** Capture data-related requirements (entity, source, availability, quality, pain points) and estimate business value.
3. **Design:** Form detailed target specifications including business rules from a data lifecycle perspective. Validate outcomes and reuse shareable constructs from the enterprise logical data model and architecture repository.
4. **Implement:**
   - **When buying (COTS):** Reverse engineer purchased applications, map against data structures, identify gaps
   - **When reusing data:** Map application data models against common data structures to understand CRUD operations; enforce use of system of record
   - **When building:** Implement data storage according to data structure; integrate per standardized specifications

**Development methodology considerations:**

| Methodology | Approach |
|---|---|
| **Waterfall** | Sequential phases with tollgates; include enterprise perspective |
| **Incremental** | Gradual steps (mini-waterfalls); create comprehensive data design in early iterations |
| **Agile/Iterative** | Discrete sprints; include specifications for data models, capture, storage, and distribution. DevOps experience shows improved data design when programmers and data architects have strong working relationships |

### 3.2 Integrate with Enterprise Architecture

Enterprise Data Architecture specifications from subject area to detailed levels are typically developed within funded projects. However, enterprise-wide Data Architecture matters should be addressed proactively. Data Architecture may influence the scope of projects. Integrate Enterprise Data Architecture matters with project portfolio management to enable roadmap implementation and better project outcomes.

---

## 4. Tools

### 4.1 Data Modeling Tools

Data modeling tools and model repositories are necessary for managing the enterprise data model at all levels. Most include lineage and relation tracking functions, enabling architects to manage linkages between models created for different purposes and at different levels of abstraction. (See [[data-modeling|Data Modeling and Design]])

### 4.2 Asset Management Software

Asset management software inventories systems, describes their content, and tracks relationships between them. These tools collect valuable Metadata about systems and the data they contain — helpful for creating data flows or researching current state.

### 4.3 Graphical Design Applications

Used to create architectural design diagrams, data flows, data value chains, and other architectural artifacts.

---

## 5. Techniques

### 5.1 Lifecycle Projections

Architecture designs can be aspirational, implemented, or planned for retirement. Status should be clearly documented:

| Status | Description |
|---|---|
| **Current** | Products currently supported and used |
| **Deployment period** | Products deployed for use in the next 1–2 years |
| **Strategic period** | Products expected to be available in the next 2+ years |
| **Retirement** | Products retired or intended for retirement within a year |
| **Preferred** | Products preferred for use by most applications |
| **Containment** | Products limited to use by certain applications |
| **Emerging** | Products being researched and piloted for possible future deployment |
| **Reviewed** | Products evaluated, results documented, not in any other status |

### 5.2 Diagramming Clarity

Models and diagrams must use visual conventions consistently. Characteristics that maximize useful information:

- **Clear and consistent legend:** Identify all objects and lines and what they signify; placed in the same spot in all diagrams
- **Match between diagram objects and legend:** All diagram objects should match legend objects
- **Clear and consistent line direction:** All flows should start at one side (generally left) and flow toward the opposite side. Loops should flow out and around to be clear
- **Consistent line cross display method:** Use line jumps; do not join lines to lines; minimize crossing lines
- **Consistent object attributes:** Differences in sizes, colors, line thickness should signify something
- **Linear symmetry:** Objects placed in lines and columns are more readable than random placement

---

## 6. Implementation Guidelines

Implementing Enterprise Data Architecture involves:

- Organizing Enterprise Data Architecture teams and forums
- Producing initial versions of Data Architecture artifacts (enterprise data model, enterprise-wide data flow, roadmaps)
- Forming and establishing a data architectural way of working in development projects
- Creating awareness throughout the organization of the value of Data Architecture efforts

Implementation can begin in a part of the organization or a data domain (e.g., product data or customer data), then grow wider after learning and maturing.

**Implementation approaches based on organizational drivers:**

- **Innovation-oriented culture:** Agile implementation — outlined subject area model at overall level while participating in detail in agile sprints. Enterprise Data Architecture evolves incrementally. Ensures data architects are engaged early.
- **Quality-oriented culture:** Traditional approach — starts with Master Data areas needing improvement, then expands to business event (transactional) data. Enterprise Data Architects produce blueprints and templates used throughout the system landscape, ensuring compliance via governance.

### 6.1 Readiness Assessment / Risk Assessment

Architecture initiation projects expose more risks than other projects:

| Risk | Mitigation |
|---|---|
| **Lack of management support** | Enlist more than one member of top-level/senior management who understands Data Architecture benefits |
| **No proven record of accomplishment** | Have a sponsor with confidence in the team; enlist senior architect colleague for critical steps |
| **Apprehensive sponsor** | Establish workplace independence; build sponsor's confidence |
| **Counter-productive executive decisions** | Communicate more clearly and frequently with management |
| **Culture shock** | Consider how working culture will change for affected employees |
| **Inexperienced project leader** | Encourage sponsor to change or educate the project manager |
| **Dominance of a one-dimensional view** | Guard against a single application owner dictating the overall enterprise-level view (e.g., ERP owners) |

### 6.2 Organization and Cultural Change

The speed of adopting architectural practices depends on cultural adaptability. Output-oriented, strategically aligned organizations are best positioned to adopt architectural practices.

**Factors affecting adoption:**

- Cultural receptivity to architectural approach
- Organizational recognition of data as a business asset, not just an IT concern
- Ability to let go of local perspective and adopt an enterprise perspective on data
- Ability to integrate architectural deliverables into project methodology
- Level of acceptance of formal [[data-governance|data governance]]
- Ability to look holistically at the enterprise, rather than being focused solely on project delivery and IT solutioning

---

## 7. Data Architecture Governance

Data Architecture activities directly support the alignment and control of data. Data architects often act as business liaisons for data governance activities. Enterprise Data Architecture and the Data Governance organization must be well aligned. Ideally, both a data architect and a Data Steward should be assigned to each subject area and even to each entity within a subject area.

**Governance activities include:**

- **Overseeing Projects:** Ensuring projects comply with required Data Architecture activities, use and improve architectural assets, and implement according to architectural standards
- **Managing architectural designs, lifecycle, and tools:** Defining, evaluating, and maintaining architectural designs. Enterprise Data Architecture serves as a "zoning plan" for long-term integration.
- **Defining standards:** Setting rules, guidelines, and specifications for how data is used within the organization
- **Creating data-related artifacts:** Artifacts that enable compliance with governance directives

### 7.1 Metrics

Performance metrics reflect architectural goals: architectural compliance, implementation trends, and business value.

| Metric Category | Measurements |
|---|---|
| **Architecture Standard Compliance Rate** | How closely projects comply with established Data Architectures; project exceptions tracking |
| **Implementation Trends** | Use/reuse/replace/retire measurements; project execution efficiency (lead times, resource costs) |
| **Business Value** | Business agility improvements (cost of delay); business quality (whether business cases are fulfilled); business operation quality (improved accuracy, reduced correction time/expense); business environment improvements (client retention, reduced regulatory remarks) |

---

## 8. Key Relationships to Other DMBOK Knowledge Areas

- **[[data-modeling|Data Modeling and Design]] (Chapter 5):** Data models at all levels are developed using data modeling techniques. The EDM is built from conceptual, logical, and physical models.
- **[[data-governance|Data Governance]] (Chapter 3):** Data Architecture and Data Governance must be well aligned. Data architects act as business liaisons for governance activities.
- **[[data-storage-and-operations|Data Storage & Operations]] (Chapter 6):** Data flow design defines requirements for storage and processing across databases and platforms.
- **[[data-integration-and-interoperability|Data Integration & Interoperability]] (Chapter 8):** Data Architecture guides integration by defining standard terms, designs, and data flow specifications.
- **[[metadata-management|Metadata Management]] (Chapter 12):** Asset management software collects valuable Metadata about systems and data, helpful for data flows and current state research.
- **[[master-and-reference-data|Master & Reference Data]] (Chapter 10):** Implementation typically starts with Master Data areas; the roadmap prioritizes master data capabilities with the least dependencies.
- **[[data-warehousing-and-bi|Data Warehousing & Business Intelligence]] (Chapter 11):** Data lineage from data flows supports warehousing and analytics.
- **[[data-quality|Data Quality]] (Chapter 13):** Architecture quality improvements are accomplished incrementally; business operation quality metrics track improved accuracy.

---

## 9. Works Cited / Recommended

- Ahlemann, Frederik, et al. *Strategic Enterprise Architecture Management*. Springer, 2012.
- Bernard, Scott A. *An Introduction to Enterprise Architecture*. 2nd ed. Authorhouse, 2005.
- Brackett, Michael H. *Data Sharing Using a Common Data Architecture*. Wiley, 1994.
- Carbone, Jane. *IT Architecture Toolkit*. Prentice Hall, 2004.
- Cook, Melissa. *Building Enterprise Information Architectures*. Prentice Hall, 1996.
- Edvinsson, Hakan and Lottie Aderinne. *Enterprise Architecture Made Simple*. Technics Publications, 2013.
- Fong, Joseph. *Information Systems Reengineering and Integration*. 2nd ed. Springer, 2006.
- Gane, Chris and Trish Sarson. *Structured Systems Analysis: Tools and Techniques*. Prentice Hall, 1979.
- Hagan, Paula J., ed. *EABOK: Guide to the (Evolving) Enterprise Architecture Body of Knowledge*. MITRE, 2004.
- Harrison, Rachel. *TOGAF Version 8.1.1 Enterprise Edition - Study Guide*. Van Haren Publishing, 2007.
- Hoberman, Steve, et al. *Data Modeling for the Business*. Technics Publications, 2009.
- Hoberman, Steve. *Data Modeling Made Simple*. 2nd ed. Technics Publications, 2009.
- Hoogervorst, Jan A. P. *Enterprise Governance and Enterprise Engineering*. Springer, 2009.
- Inmon, W. H., John A. Zachman, and Jonathan G. Geiger. *Data Stores, Data Warehousing and the Zachman Framework*. McGraw-Hill, 1997.
- Lankhorst, Marc. *Enterprise Architecture at Work*. Springer, 2005.
- Martin, James and Joe Leben. *Strategic Information Planning Methodologies*. 2nd ed. Prentice Hall, 1989.
- Osterwalder, Alexander and Yves Pigneur. *Business Model Generation*. Wiley, 2010.
- Perks, Col and Tony Beveridge. *Guide to Enterprise IT Architecture*. Springer, 2003.
- Poole, John, et al. *Common Warehouse Metamodel*. Wiley, 2001.
- Radhakrishnan, Rakesh. *Identity and Security: A Common Architecture and Framework For SOA and Network Convergence*. futuretext, 2007.
- Ross, Jeanne W., Peter Weill, and David Robertson. *Enterprise Architecture As Strategy*. Harvard Business School Press, 2006.
- Schekkerman, Jaap. *How to Survive in the Jungle of Enterprise Architecture Frameworks*. Trafford Publishing, 2006.
- Spewak, Steven and Steven C. Hill. *Enterprise Architecture Planning*. 2nd ed. Wiley-QED, 1993.
- Ulrich, William M. and Philip Newcomb. *Information Systems Transformation*. Morgan Kaufmann, 2010.

---

## 10. Summary

Data Architecture is the master blueprint for managing data assets across the enterprise. It encompasses:

- **Enterprise Data Models** that provide a common, consistent view of data at conceptual, logical, and physical levels
- **Data Flow Designs** that map how data moves through business processes, applications, and systems
- **A structured practice** with strategy, culture, organization, methods, and results
- **Integration with enterprise architecture** to ensure alignment with business strategy and other architecture domains
- **Governance** to ensure compliance, manage lifecycle, define standards, and measure business value

The Zachman Framework provides the foundational ontology for understanding the different perspectives and artifacts needed. Successful implementation requires both quality-oriented and innovation-oriented approaches, strong management support, cultural readiness, and an incremental, iterative build-out of the enterprise data model — typically starting with Master Data domains and expanding outward.
