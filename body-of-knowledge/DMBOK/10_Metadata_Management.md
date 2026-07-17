---
title: "Metadata Management"
tags:
  - metadata
  - data-management
  - dmbok
source: "DMBoK v2 by DAMA"
chapter: 11
created: 2026-07-09
---

# Metadata Management

**Definition:** Planning, implementation, and control activities to enable access to high quality, integrated Metadata.

**Goals:**
1. Provide organizational understanding of business terms and usage
2. Collect and integrate Metadata from diverse sources
3. Provide a standard way to access Metadata
4. Ensure Metadata quality and security

---

## Business Drivers

Data cannot be managed without Metadata. In addition, Metadata itself must be managed. Reliable, well-managed Metadata helps:

- **Increase confidence in data** by providing context and enabling the measurement of **Data Quality**
- **Increase the value of strategic information** (e.g., **Master Data**) by enabling multiple uses
- **Improve operational efficiency** by identifying redundant data and processes
- **Prevent the use of out-of-date or incorrect data**
- **Reduce data-oriented research time**
- **Improve communication** between data consumers and IT professionals
- **Create accurate impact analysis** thus reducing the risk of project failure
- **Improve time-to-market** by reducing system development life-cycle time
- **Reduce training costs** and lower the impact of staff turnover through thorough documentation of data context, history, and origin
- **Support regulatory compliance**

Metadata assists in representing information consistently, streamlining workflow capabilities, and protecting sensitive information, particularly when regulatory compliance is required.

Organizations get more value out of their data assets if their data is of high quality. Quality data depends on **governance**. Because it explains the data and processes that enable organizations to function, Metadata is critical to data governance. If Metadata is a guide to the data in an organization, then it must be well managed.

**Poorly managed Metadata leads to:**
- Redundant data and data management processes
- Replicated and redundant dictionaries, repositories, and other Metadata storage
- Inconsistent definitions of data elements and risks associated with data misuse
- Competing and conflicting sources and versions of Metadata which reduce the confidence of data consumers
- Doubt about the reliability of Metadata and data

**Well-executed Metadata management** enables a consistent understanding of data resources and more efficient cross-organizational development.

> As technology has evolved, the speed at which data is generated has also increased. Technical Metadata has become integral to the way in which data is moved and integrated. ISO's Metadata Registry Standard, **IEC 11179**, is intended to enable Metadata-driven exchange of data in a heterogeneous environment, based on exact definitions of data. To be data-driven, an organization must be Metadata-driven.

---

## Goals and Principles

The goals of Metadata management include:

1. **Document and manage organizational knowledge** of data-related business terminology in order to ensure people understand data content and can use data consistently
2. **Collect and integrate Metadata from diverse sources** to ensure people understand similarities and differences between data from different parts of the organization
3. **Ensure Metadata quality, consistency, currency, and security**
4. **Provide standard ways to make Metadata accessible** to Metadata consumers (people, systems, and processes)
5. **Establish or enforce the use of technical Metadata standards** to enable data exchange

The implementation of a successful Metadata solution follows these guiding principles:

| Principle | Description |
|-----------|-------------|
| **Organizational commitment** | Secure senior management support and funding for Metadata management as part of an overall strategy to manage data as an enterprise asset |
| **Strategy** | Develop a Metadata strategy that accounts for how Metadata will be created, maintained, integrated, and accessed. Requirements should be defined before evaluating, purchasing, and installing Metadata management products. Align with business priorities |
| **Enterprise perspective** | Take an enterprise perspective to ensure future extensibility, but implement through iterative and incremental delivery to bring value |
| **Socialization** | Communicate the necessity of Metadata and the purpose of each type; socialization of the value of Metadata will encourage business use and contribution of business expertise |
| **Access** | Ensure staff members know how to access and use Metadata |
| **Quality** | Recognize that Metadata is often produced through existing processes (data modeling, SDLC, business process definition) and hold process owners accountable for the quality of Metadata |
| **Audit** | Set, enforce, and audit standards for Metadata to simplify integration and enable use |
| **Improvement** | Create a feedback mechanism so that consumers can inform the Metadata Management team of Metadata that is incorrect or out-of-date |

---

## Essential Concepts

### Metadata vs. Data

Metadata is a kind of data, and it should be managed as such. One question that some organizations face is where to draw the line between data that is not Metadata and data that is Metadata. Conceptually, this line is related to the level of abstraction represented by the data. A rule of thumb: **one person's Metadata is another's data.** Even something that seems like Metadata (e.g., a list of column names) may be just plain data — if it were the input for an analysis aimed at understanding data content across different organizations.

To manage their Metadata, organizations should not worry about the philosophical distinctions. Instead they should define Metadata requirements focused on **what they need Metadata for** (to create new data, understand existing data, enable movement between systems, access data, to share data) and source data to meet these requirements.

### Types of Metadata

Metadata is often categorized into three types: **business**, **technical**, and **operational**. These categories enable people to understand the range of information that falls under the overall umbrella of Metadata, as well as the functions through which Metadata is produced. It is best to think of these categories in relation to **where Metadata originates**, rather than how it is used. In relation to usage, the distinctions between Metadata types are not strict — technical and operational staff use "business" Metadata and vice versa.

Outside of information technology (e.g., in library or information science), Metadata is described using a different set of categories:
- **Descriptive Metadata** — describes a resource and enables identification and retrieval (e.g., title, author, subject)
- **Structural Metadata** — describes relationships within and among resources and their component parts (e.g., number of pages, number of chapters)
- **Administrative Metadata** — used to manage resources over their lifecycle (e.g., version numbers, archive dates)

#### Business Metadata

Business Metadata focuses largely on the content and condition of the data and includes details related to data governance. Business Metadata includes the non-technical names and definitions of concepts, subject areas, entities, and attributes; attribute data types and other attribute properties; range descriptions; calculations; algorithms and business rules; valid domain values and their definitions.

Examples of Business Metadata include:
- Definitions and descriptions of data sets, tables, and columns
- Business rules, transformation rules, calculations, and derivations
- Data models
- Data quality rules and measurement results
- Schedules by which data is updated
- Data provenance and **data lineage**
- Data standards
- Designations of the system of record for data elements
- Valid value constraints
- Stakeholder contact information (e.g., data owners, data stewards)
- Security/privacy level of data
- Known issues with data
- Data usage notes

#### Technical Metadata

Technical Metadata provides information about the technical details of data, the systems that store data, and the processes that move it within and between systems. Examples of Technical Metadata include:

- Physical database table and column names
- Column properties
- Database object properties
- Access permissions
- Data CRUD (create, replace, update and delete) rules
- Physical data models, including data table names, keys, and indexes
- Documented relationships between the data models and the physical assets
- ETL job details
- File format schema definitions
- Source-to-target mapping documentation
- Data lineage documentation, including upstream and downstream change impact information
- Program and application names and descriptions
- Content update cycle job schedules and dependencies
- Recovery and backup rules
- Data access rights, groups, roles

#### Operational Metadata

Operational Metadata describes details of the processing and accessing of data. Examples include:

- Logs of job execution for batch programs
- History of extracts and results
- Schedule anomalies
- Results of audit, balance, control measurements
- Error Logs
- Reports and query access patterns, frequency, and execution time
- Patches and Version maintenance plan and execution, current patching level
- Backup, retention, date created, disaster recovery provisions
- SLA requirements and provisions
- Volumetric and usage patterns
- Data archiving and retention rules, related archives
- Purge criteria
- Data sharing rules and agreements
- Technical roles and responsibilities, contacts

### ISO/IEC 11179 Metadata Registry Standard

ISO's Metadata Registry Standard, ISO/IEC 11179, provides a framework for defining a Metadata registry. It is designed to enable Metadata-driven data exchange, based on exact definitions of data, beginning with data elements. The standard is structured in several parts:

- **Part 1:** Framework for the Generation and Standardization of Data Elements
- **Part 3:** Basic Attributes of Data Elements
- **Part 4:** Rules and Guidelines for the Formulation of Data Definitions
- **Part 5:** Naming and Identification Principles for Data Elements
- **Part 6:** Registration of Data Elements

### Metadata for Unstructured Data

Metadata is as essential to the management of unstructured data as it is to the management of structured data — perhaps even more so. Metadata for unstructured data includes:

- **Descriptive Metadata** — catalog information and thesauri keywords
- **Structural Metadata** — tags, field structures, format
- **Administrative Metadata** — sources, update schedules, access rights, navigation information
- **Bibliographic Metadata** — library catalog entries
- **Record keeping Metadata** — retention policies
- **Preservation Metadata** — storage, archival condition, rules for conservation

New practices are emerging around managing unstructured data in data lakes. Organizations wanting to take advantage of data lakes, using Big Data platforms such as Hadoop, are finding that they must catalog ingested data in order to enable later access. Most put in place processes to collect Metadata as part of data ingestion. A minimum set of Metadata attributes needs to be collected about each object ingested in the data lake (e.g., name, format, source, version, date received, etc.). This produces a catalog of data lake contents. (See **Document and Content Management**.)

### Sources of Metadata

Metadata can be collected from many different sources. If Metadata from applications and databases has been well-managed, it can simply be harvested and integrated. However, most organizations do not manage Metadata well at the application level, because Metadata is often created as a by-product of application processing rather than as an end product (i.e., it is not created with consumption in mind).

The majority of operational Metadata is generated as data is processed. The key to using this Metadata is to collect it in a usable form and to ensure that those responsible for interpreting it have the tools they need to do so. A large portion of technical Metadata can be harvested from database objects.

**It is better to be intentional about developing definitions than to simply accept existing ones.** Development of definitions takes time and the right skill set (e.g., writing and facilitation skills). This is why the development of business Metadata requires stewardship. (See **Data Governance**.)

Creating Metadata for its own sake rarely works well. Metadata should be created as the product of a well-defined process, using tools that will support its overall quality.

The key sources of Metadata include:

| # | Source | Description |
|---|--------|-------------|
| 1 | **Application Metadata Repositories** | Physical tables in which Metadata is stored; often built into modeling tools, BI tools, and other applications |
| 2 | **Business Glossary** | Documents and stores business concepts, terminology, definitions, and relationships between terms. Serves business users, data stewards, and technical users |
| 3 | **BI Tools** | Produce Metadata relevant to BI design: overview information, classes, objects, derived items, filters, reports, report fields, layout, users, distribution |
| 4 | **Configuration Management Tools (CMDB)** | Manage Metadata related to IT assets, their relationships, and contractual details. Each asset is a Configuration Item (CI) |
| 5 | **Data Dictionaries** | Define the structure and contents of data sets, often for a single database, application, or warehouse. Manage names, descriptions, structure, characteristics, storage requirements, default values, relationships, uniqueness, and other attributes |
| 6 | **Data Integration Tools** | Document lineage as data moves between systems. Provide APIs to allow external Metadata repositories to extract lineage information and transient files Metadata. Also provide execution Metadata (last successful run, duration, job status) |
| 7 | **Database Management and System Catalogs** | Describe database content, along with sizing information, software versions, deployment status, network uptime, infrastructure uptime, availability, and other operational Metadata attributes |
| 8 | **Data Mapping Management Tools** | Transform requirements into mapping specifications, which can be consumed directly by data integration tools or used by developers to generate data integration code |
| 9 | **Data Quality Tools** | Assess data quality through validation rules. Provide capability to exchange quality scores and profile patterns with other Metadata repositories |
| 10 | **Directories and Catalogs** | Contain information about systems, sources, and locations of data within an organization. Particularly useful to developers and data super users |
| 11 | **Event Messaging Tools** | Move data between diverse systems and generate Metadata describing this movement. Include graphic interfaces to manage data movement logic |
| 12 | **Modeling Tools and Repositories** | Build conceptual, logical, and physical data models. Produce Metadata relevant to application or system model design (subject areas, entities, attributes, relationships, tables, columns, indexes, keys, constraints) |
| 13 | **Reference Data Repositories** | Document business values and descriptions of enumerated data (domains) and their contextual use. Provide capabilities to associate Reference Data to business glossary and physical implementations |
| 14 | **Service Registries** | Manage technical information about services and service end-points from an SOA perspective (definitions, interfaces, operations, input/output parameters, policies, versions, deployment info) |
| 15 | **Other Metadata Stores** | Specialized lists such as event registries, source lists or interfaces, code sets, lexicons, spatial and temporal schema, spatial reference, repositories of repositories, and business rules |

---

### Types of Metadata Architecture

Like other forms of data, Metadata has a lifecycle. Conceptually, all Metadata management solutions include architectural layers that correspond to points in the Metadata lifecycle:

1. Metadata creation and sourcing
2. Metadata storage in one or more repositories
3. Metadata integration
4. Metadata delivery
5. Metadata usage
6. Metadata control and management

Different architectural approaches can be used to source, store, integrate, maintain, and make Metadata accessible to consumers.

#### Centralized Metadata Architecture

A centralized architecture consists of a **single Metadata repository** that contains copies of Metadata from the various sources.

**Advantages:**
- High availability, since it is independent of the source systems
- Quick Metadata retrieval, since the repository and the query reside together
- Resolved database structures not affected by the proprietary nature of third party or commercial systems
- Extracted Metadata may be transformed, customized, or enhanced with additional Metadata that may not reside in the source system, improving quality

**Limitations:**
- Complex processes are necessary to ensure that changes in source Metadata are quickly replicated into the repository
- Maintenance of a centralized repository can be costly
- Extraction could require custom modules or middleware
- Validation and maintenance of customized code can increase demands on both internal IT staff and software vendors

> Organizations with limited IT resources, or those seeking to automate as much as possible, may choose to avoid this architecture option. Organizations seeking a high degree of consistency within the common Metadata repository can benefit from a centralized architecture.

#### Distributed Metadata Architecture

A completely distributed architecture maintains a **single access point**. The Metadata retrieval engine responds to user requests by retrieving data from source systems in real time; there is no persistent repository.

**Advantages:**
- Metadata is always as current and valid as possible because it is retrieved from its source
- Queries are distributed, possibly improving response and process time
- Metadata requests from proprietary systems are limited to query processing rather than requiring a detailed understanding of proprietary data structures
- Development of automated Metadata query processing is likely simpler, requiring minimal manual intervention
- Batch processing is reduced, with no Metadata replication or synchronization processes

**Limitations:**
- No ability to support user-defined or manually inserted Metadata entries since there is no repository
- Difficulty in standardizing the presentation of Metadata from various systems
- Query capabilities are directly affected by the availability of the participating source systems
- The quality of Metadata depends solely on the participating source systems

#### Hybrid Metadata Architecture

A hybrid architecture combines characteristics of centralized and distributed architectures. Metadata still moves directly from the source systems into a centralized repository. However, the repository design only accounts for the user-added Metadata, the critical standardized items, and the additions from manual sources.

**Benefits:**
- Near-real-time retrieval of Metadata from its source
- Enhanced Metadata to meet user needs most effectively
- Lower effort for manual IT intervention and custom-coded access functionality
- Metadata is as current and valid as possible at the time of use

**Limitations:**
- Does not improve system availability
- The availability of source systems is still a concern
- Additional overhead is required to link distributed query results with Metadata augmentation in the central repository

> Many organizations can benefit from a hybrid architecture, including those with rapidly-changing operational Metadata, those needing consistent uniform Metadata, and those experiencing substantial growth in Metadata and Metadata sources.

#### Bi-Directional Metadata Architecture

Allows Metadata to change in any part of the architecture (source, data integration, user interface) and then feedback is coordinated from the repository (broker) into its original source.

**Challenges:**
- The design forces the Metadata repository to contain the latest version of the Metadata source and forces it to manage changes to the source
- Changes must be trapped systematically, and then resolved
- Additional sets of process interfaces to tie the repository back to the Metadata source(s) must be built and maintained

---

## Activities

### 1. Define Metadata Strategy

A Metadata strategy describes how an organization intends to manage its Metadata and how it will move from current state to future state practices. It should provide a framework for development teams to improve Metadata management.

Steps include:

1. **Initiate Metadata strategy planning** — Define short- and long-term goals. Draft a charter, scope, and objectives aligned with overall governance efforts. Establish a communications plan. Involve key stakeholders.
2. **Conduct key stakeholder interviews** — Interviews with business and technical stakeholders provide a foundation of knowledge.
3. **Assess existing Metadata sources and information architecture** — Determine the relative degree of difficulty in solving Metadata and systems issues. Conduct detailed interviews of key IT staff and review documentation of system architectures, data models, etc.
4. **Develop future Metadata architecture** — Refine and confirm the future vision, and develop the long-term target architecture. Account for: organization structure, alignment with data governance and stewardship, managed Metadata architecture, Metadata delivery architecture, technical architecture, and security architecture.
5. **Develop a phased implementation plan** — Validate, integrate, and prioritize findings. Document the Metadata strategy and define a phased implementation approach.

> The strategy will evolve over time, as Metadata requirements, the architecture, and the lifecycle of Metadata are better understood.

### 2. Understand Metadata Requirements

Metadata requirements start with content: What Metadata is needed and at what level. Metadata content is wide-ranging and requirements will come from both business and technical data consumers.

Functionality-focused requirements associated with a comprehensive Metadata solution:

| Requirement | Description |
|-------------|-------------|
| **Volatility** | How frequently Metadata attributes and sets will be updated |
| **Synchronization** | Timing of updates in relation to source changes |
| **History** | Whether historical versions of Metadata need to be retained |
| **Access rights** | Who can access Metadata and how, along with specific user interface functionality |
| **Structure** | How Metadata will be modeled for storage |
| **Integration** | The degree of integration of Metadata from different sources; rules for integration |
| **Maintenance** | Processes and rules for updating Metadata (logging and referring for approval) |
| **Management** | Roles and responsibilities for managing Metadata |
| **Quality** | Metadata quality requirements |
| **Security** | Some Metadata cannot be exposed because it will reveal the existence of highly protected data |

### 3. Define Metadata Architecture

A Metadata Management system must be capable of extracting Metadata from many sources. Design the architecture to be capable of scanning the various Metadata sources and periodically updating the repository. The system must support the manual updates of Metadata, requests, searches, and lookups of Metadata by various user groups.

A managed Metadata environment should isolate the end user from the various and disparate Metadata sources. The architecture should provide a single access point for the Metadata repository. The access point must supply all related Metadata resources transparently to the user. Users should be able to access Metadata without being aware of the differing environments of the data sources.

Three technical architectural approaches to building a common Metadata repository mimic the approaches to designing data warehouses: **centralized**, **distributed**, and **hybrid** (see **#Types of Metadata Architecture**). These approaches all take into account implementation of the repository, and how the update mechanisms operate.

#### Create MetaModel

Create a data model for the Metadata repository, or metamodel, as one of the first design steps after the Metadata strategy is complete and the business requirements are understood. Different levels of metamodel may be developed as needed:

- A **high-level conceptual model** that explains the relationships between systems
- A **lower level metamodel** that details the attributions, to describe the elements and processes of a model

In addition to being a planning tool and a means of articulating requirements, the metamodel is in itself a valuable source of Metadata.

A typical metamodel includes entities such as: Architecture, Business Glossary, System, Logical Data Model, Physical Data Model, Data Store, Application, Entity, File/Table, Attribute, Field/Column, Codified Domain, Code Sets, Code Value, Business Value, and Glossary Terms — integrating both Business Metadata and Technical Metadata.

#### Apply Metadata Standards

The Metadata solution should adhere to agreed-upon internal and external standards as identified in the Metadata strategy. Metadata should be monitored for compliance by governance activities.

- **Internal Metadata standards** — naming conventions, custom attributions, security, visibility, and processing documentation
- **External Metadata standards** — data exchange formats and application-programming interfaces design

#### Manage Metadata Stores

Implement control activities to manage the Metadata environment. Control of repositories is control of Metadata movement and repository updates performed by the Metadata specialist. These activities are administrative in nature and involve monitoring and responding to reports, warnings, job logs, and resolving various issues.

**Control activities include:**
- Job scheduling and monitoring
- Load statistical analysis
- Backup, recovery, archive, purging
- Configuration modifications
- Performance tuning
- Query statistics analysis
- Query and report generation
- Security management

**Quality control activities include:**
- Quality assurance, quality control
- Frequency of data update – matching sets to timeframes
- Missing Metadata reports
- Aging Metadata report

**Metadata management activities include:**
- Loading, scanning, importing and tagging assets
- Source mapping and movement
- Versioning
- User interface management
- Linking data sets Metadata maintenance – for NoSQL provisioning
- Linking data to internal data acquisition – custom links and job Metadata
- Licensing for external data sources and feeds
- Data enhancement Metadata (e.g., link to GIS)

**Training:**
- Education and training of users and data stewards
- Management metrics generation and analysis
- Training on the control activities and query and reporting

### 4. Create and Maintain Metadata

Metadata is created through a range of processes and stored in many places within an organization. To be of high quality, Metadata should be managed as a product. Good Metadata is not created by accident — it requires planning. (See **Data Quality**.)

Several general principles of Metadata management describe the means to manage Metadata for quality:

- **Accountability** — Recognize that Metadata is often produced through existing processes (data modeling, SDLC, business process definition) and hold process owners accountable for the quality of Metadata
- **Standards** — Set, enforce, and audit standards for Metadata to simplify integration and enable use
- **Improvement** — Create a feedback mechanism so that consumers can inform the Metadata Management team of Metadata that is incorrect or out-of-date

Like other data, Metadata can be profiled and inspected for quality. Its maintenance should be scheduled or completed as an auditable part of project work.

#### Integrate Metadata

Integration processes gather and consolidate Metadata from across the enterprise, including Metadata from data acquired outside the enterprise. The Metadata repository should integrate extracted technical Metadata with relevant business, processes, and stewardship Metadata.

Metadata can be extracted using adapters, scanners, bridge applications, or by directly accessing the Metadata in a source data store.

Two approaches to repository scanning:

| Approach | Description |
|----------|-------------|
| **Proprietary interface** | Single-step scan and load process. A scanner collects Metadata from a source system, then directly calls the format-specific loader component to load the Metadata into the repository. No format-specific file output. |
| **Semi-proprietary interface** | Two-step process. A scanner collects Metadata from a source system and outputs it into a format-specific data file. The receiving repository reads and loads the file appropriately. More open architecture. |

A scanning process uses and produces several types of files:
- **Control file** — containing the source structure of the data model
- **Reuse file** — containing the rules for managing reuse of process loads
- **Log files** — produced during each phase, one for each scan/extract and one for each load cycle
- **Temporary and backup files** — used during the process or for traceability

Use a non-persistent Metadata staging area to store temporary and backup files. The staging area supports rollback and recovery processes, and provides an interim audit trail.

#### Distribute and Deliver Metadata

Metadata is delivered to data consumers and to applications or tools that require Metadata feeds. Delivery mechanisms include:

- Metadata intranet websites for browse, search, query, reporting, and analysis
- Reports, glossaries and other documents
- Data warehouses, data marts, and **BI tools**
- Modeling and software development tools
- Messaging and transactions
- Web services and Application Programming Interfaces (APIs)
- External organization interface solutions (e.g., supply chain solutions)

The Metadata solution often links to a Business Intelligence solution, so that both the scope and the currency of Metadata synchronize with the BI content.

Metadata is exchanged with external organizations using files (flat, XML, or JSON structured) or through web services.

### 5. Query, Report, and Analyze Metadata

Metadata guides the use of data assets. Use Metadata in:

- **Business Intelligence** (reporting and analysis)
- **Business decisions** (operational, tactical, strategic)
- **Business semantics** (what they say, what they mean — business lingo)

A Metadata repository must have a front-end application that supports the search-and-retrieval functionality required for all this guidance and management of data assets. The interface provided to business users may have a different set of functional requirements than that for technical users and developers. Some reports facilitate future development such as change impact analysis, or trouble shoot varying definitions for data warehouse and Business Intelligence projects, such as data lineage reports.

---

## Tools

### Metadata Repository Management Tools

The primary tool used to manage Metadata is the **Metadata repository**. This will include an integration layer and often an interface for manual updates. Tools that produce and use Metadata become sources of Metadata that can be integrated into a Metadata repository.

Metadata Management tools provide capabilities to manage Metadata in a centralized location (repository). The Metadata can be either manually entered or extracted from various other sources through specialized connectors. Metadata repositories also provide capabilities to exchange Metadata with other systems.

Metadata management tools and repositories themselves are also a source of Metadata, especially in a hybrid Metadata architectural model or in large enterprise implementations. They allow for the exchange of collected Metadata with other Metadata repositories, enabling:

- Collection of various and diverse Metadata from different sources into a centralized repository
- Enriching and standardizing diverse Metadata as it moves between repositories

---

## Techniques

### Data Lineage and Impact Analysis

A key benefit of discovering and documenting Metadata about the physical assets is to provide information on how data is transformed as it moves between systems. Many Metadata tools carry information about what is happening to the data within their environments and provide capabilities to view the lineage across the span of the systems or applications they interface.

Two types of lineage:
- **"As Implemented Lineage"** — the current version of lineage based on programming code
- **"As Designed Lineage"** — lineage described in mapping specification documents

The process of connecting the pieces of the data lineage is referred to as **stitching**. It results in a holistic visualization of the data as it moves from its original locations (official source or system of record) until it lands in its final destination.

Higher levels of lineage (e.g., "System Lineage") summarize movement at the system or application level. Many visualization tools provide zoom-in / zoom-out capability, to show data element lineage in the context of system lineage.

**Strategies for successful lineage discovery:**

| Focus | Approach |
|-------|----------|
| **Business focus** | Limit lineage discovery to data elements prioritized by the business. Start from target locations and trace back to source systems. If coupled with data quality measurements, lineage can pinpoint where system design adversely impacts data quality |
| **Technical focus** | Start at source systems and identify all immediate consumers, then identify subsequent consumers, repeating until all systems are identified. Enables answering questions like "Where is social security number?" or generating impact reports like "What systems are impacted if a column width changes?" |

Documented lineage helps both business and technical people use data. Without it, much time is wasted in investigating anomalies, potential change impacts, or unknown results. Impact reports outline which components are affected by a potential change, expediting and streamlining estimating and maintenance tasks.

### Metadata for Big Data Ingest

Many data management professionals are familiar and comfortable with structured data stores, where every item can be clearly identified and tagged. Nowadays, much data comes in less structured formats. Successful data management in a data lake depends on managing Metadata.

Metadata tags should be applied to data upon ingestion. Metadata then can be used to identify data content available for access in the data lake. Many ingestion engines profile data as it is ingested. Data profiling can identify data domains, relationships, and data quality issues. It can also enable tagging. On ingestion, Metadata tags can be added to identify sensitive or private data (like Personally Identifiable Information – PII). Data scientists may add confidence, textual identifiers, and codes representing behavior clusters. (See **Big Data and Data Science**.)

---

## Implementation Guidelines

Implement a managed Metadata environment in incremental steps in order to minimize risks to the organization and to facilitate acceptance. Implement Metadata repositories using an open relational database platform. This allows development and implementation of various controls and interfaces that may not be anticipated at the start of a repository development project.

**Key guidelines:**
- The repository contents should be generic in design, not merely reflecting the source system database designs
- Design contents in alignment with the enterprise subject area experts, and based on a comprehensive Metadata model
- Planning should account for integrating Metadata so that data consumers can see across different data sources
- The repository should house current, planned, and historical versions of the Metadata
- Often, the first implementation is a pilot to prove concepts and learn about managing the Metadata environment
- Integration of Metadata projects into the IT development methodology is necessary

### Readiness Assessment / Risk Assessment

Having a solid Metadata strategy helps everyone make more effective decisions. Assess the degree to which the lack of high quality Metadata might result in:

- **Errors in judgment** due to incorrect, incomplete or invalid assumptions or lack of knowledge about the context of the data
- **Exposure of sensitive data**, which may put customers or employees at risk, or impact the credibility of the business and lead to legal expenses
- **Risk that the small set of SMEs who know the data will leave** and take their knowledge with them

Risk is reduced when an organization adopts a solid Metadata strategy. Organizational readiness is addressed by a formal assessment of the current maturity in Metadata activities. The assessment should include: critical business data elements, available Metadata glossaries, lineage, data profiling and data quality processes, MDM (Master Data Management) maturity, and other aspects. Findings from the assessment, aligned with business priorities, will provide the basis for a strategic approach to improvement of Metadata Management practices.

### Organizational and Cultural Change

Like other data management efforts, Metadata initiatives often meet with cultural resistance. Moving from an unmanaged to a managed Metadata environment takes work and discipline.

Metadata Management is a low priority in many organizations. An essential set of Metadata needs coordination and commitment in an organization. Look for a good example where control will reap immediate quality benefits for data in the company. Build the argument from concrete business-relevant examples.

Implementation of an enterprise data governance strategy needs senior management support and engagement. It requires that business and technology staff be able to work closely together in a cross-functional manner.

---

## Metadata Governance

Organizations should determine their specific requirements for the management of the Metadata lifecycle and establish governance processes to enable those requirements. It is recommended that formal roles and responsibilities be assigned to dedicated resources, especially in large or business critical areas.

### Process Controls

The **Data Governance** team should be responsible for defining the standards and managing status changes for Metadata — often with workflow or collaboration software — and may be responsible for promotional activities and training development or actual training across the organization.

More mature Metadata governance will require business terms and definitions to progress through varying status changes or governance gates; for example, from a **candidate term**, to **approved**, to **published**, and to a final point in the lifecycle of **replaced or retired**. The governance team may also manage business term associations such as related terms, as well as the categorization of and grouping of the terms.

Integration of the Metadata strategy into the SDLC is needed to ensure that changed Metadata is collected when it is changed. This helps ensure Metadata remains current.

### Documentation of Metadata Solutions

A master catalog of Metadata will include the sources and targets currently in scope. This is a resource for IT and business users and can be published as a guide to "what is where" and to set expectations:

- Metadata implementation status
- Source and the target Metadata store
- Schedule information for updates
- Retention and versions kept
- Contents
- Quality statements or warnings (e.g., missing values)
- System of record and other data source statuses (e.g., data contents history coverage, retiring or replacing flags)
- Tools, architectures, and people involved
- Sensitive information and removal or masking strategy for the source

In documents and content management, data maps show similar information. Visualizations of the overall Metadata integration systems landscape are also maintained as part of Metadata documentation. (See **Document and Content Management**.)

### Metadata Standards and Guidelines

Metadata standards are essential in the exchange of data with operational trading partners. Companies realize the value of information sharing with customers, suppliers, partners, and regulatory bodies. The need for sharing common Metadata to support the optimal usage of shared information has spawned many sector-based standards.

- Adopt industry-based and sector-sensitive Metadata standards early in the planning cycle
- Use the standards to evaluate Metadata Management technologies
- Many leading vendors support multiple standards, and some can assist in customizing industry-based and sector-sensitive standards
- Tool vendors provide XML and JSON or REST support to exchange Metadata; they use XML schema definitions (XSD) accessed through proprietary interfaces — custom development is required to integrate these tools into a Metadata management environment

Guidelines include templates and associated examples and training on expected inputs and updates including such rules as "do not define a term by using the term" and completeness statements. Different templates are developed for different types of Metadata.

The ISO standards for Metadata provide guidance for tool developers but are unlikely to be a concern for organizations who implement using commercial tools, since the tools should meet the standards.

---

## Metrics

It is difficult to measure the impact of Metadata without first measuring the impact of the lack of Metadata. As part of risk assessment, obtain metrics on the amount of time data consumers spend searching for information, in order to show improvement after the Metadata solution is put in place.

Suggested metrics on Metadata environments include:

| Metric | Description |
|--------|-------------|
| **Metadata repository completeness** | Compare ideal coverage of the enterprise Metadata (all artifacts and all instances within scope) to actual coverage. Reference the strategy for scope definitions |
| **Metadata Management Maturity** | Metrics developed to judge the Metadata maturity of the enterprise, based on the Capability Maturity Model (CMM-DMM) approach to maturity assessment. (See **Data Management Maturity Assessment**) |
| **Steward representation** | Organizational commitment to Metadata as assessed by the appointment of stewards, coverage across the enterprise for stewardship, and documentation of the roles in job descriptions |
| **Metadata usage** | User uptake on the Metadata repository usage can be measured by repository login counts. Reference to Metadata by users in business practice is a more difficult measure to track — anecdotal measures on qualitative surveys may be required |
| **Business Glossary activity** | Usage, update, resolution of definitions, coverage |
| **Master Data service data compliance** | Shows the reuse of data in SOA solutions. Metadata on the data services assists developers in deciding when new development could use an existing service |
| **Metadata documentation quality** | Assess through automatic methods (collision logic comparing sources, percentage of attributes with definitions trending over time) and manual methods (random or complete survey based on enterprise quality definitions) |
| **Metadata repository availability** | Uptime, processing time (batch and query) |

---

## Works Cited / Recommended

- Aiken, Peter. *Data Reverse Engineering: Slaying the Legacy Dragon.* 1995.
- Foreman, John W. *Data Smart: Using Data Science to Transform Information into Insight.* Wiley, 2013.
- Loshin, David. *Enterprise Knowledge Management: The Data Quality Approach.* Morgan Kaufmann, 2001.
- Marco, David. *Building and Managing the Meta Data Repository: A Full Lifecycle Guide.* Wiley, 2000.
- Milton, Nicholas Ross. *Knowledge Acquisition in Practice: A Step-by-step Guide.* Springer, 2007. Decision Engineering.
- Park, Jung-ran, ed. *Metadata Best Practices and Guidelines: Current Implementation and Future Trends.* Routledge, 2014.
- Pomerantz, Jeffrey. *Metadata.* The MIT Press, 2015. The MIT Press Essential Knowledge ser.
- Schneier, Bruce. *Data and Goliath: The Hidden Battles to Collect Your Data and Control Your World.* W. W. Norton and Company, 2015.
- Tannenbaum, Adrienne. *Implementing a Corporate Repository: The Models Meet Reality.* Wiley, 1994. Wiley Professional Computing.
- Warden, Pete. *Big Data Glossary.* O'Reilly Media, 2011.
- Zeng, Marcia Lei and Jian Qin. *Metadata.* 2nd ed. ALA Neal-Schuman, 2015.
