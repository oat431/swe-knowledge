---
title: "Data Integration and Interoperability"
tags: [data-integration, data-management, dmbok]
source: "DMBoK v2, Chapter 8"
chapter: 8
created: 2026-07-09
---

# Data Integration and Interoperability (DII)

> **Definition:** Managing the movement and consolidation of data within and between applications and organizations.

*Data Integration and Interoperability* (DII) describes processes related to the movement and consolidation of data within and between data stores, applications, and organizations. **Integration** consolidates data into consistent forms, either physical or virtual. **Data Interoperability** is the ability for multiple systems to communicate.

---

## Goals

1. Provide data securely, with regulatory compliance, in the format and timeframe needed.
2. Lower cost and complexity of managing solutions by developing shared models and interfaces.
3. Identify meaningful events and automatically trigger alerts and actions.
4. Support business intelligence, analytics, master data management, and operational efficiency efforts.

---

## 1. Essential Concepts

### 1.1 Extract, Transform, and Load (ETL)

Central to all areas in DII is the basic process of **Extract, Transform, and Load (ETL)**. Whether executed physically or virtually, in batch or real-time, these are the essential steps in moving data around and between applications and organizations.

| Phase | Description |
|-------|-------------|
| **Extract** | Selecting the required data and extracting it from its source. Extracted data is staged in a physical data store on disk or in memory. Designed to use minimal resources on operational systems. |
| **Transform** | Making the selected data compatible with the target structure. Includes format changes, structure changes, semantic conversion, de-duplication, and re-ordering. |
| **Load** | Physically storing or presenting the result of transformations in the target system. |

#### ELT (Extract, Load, Transform)

If the target system has more transformation capability than the source or intermediary, the order may be switched to **ELT**. This allows transformations to occur after loading to the target system. Common in Big Data environments where ELT loads the data lake. (See [[Data Warehousing and Business Intelligence]].)

#### Mapping

A synonym for transformation, a **mapping** defines:
- Sources to be extracted
- Rules for identifying data for extraction
- Targets to be loaded
- Rules for identifying target rows for update
- Transformation rules or calculations to be applied

### 1.2 Latency

Latency is the time difference between when data is generated in the source system and when it becomes available in the target system.

| Type | Description | Latency |
|------|-------------|---------|
| **Batch** | Data moves in clumps or files, on request or periodic schedule | High |
| **Micro-batch** | Batch processing at higher frequency (e.g., every 5 minutes) | Medium |
| **Change Data Capture (CDC)** | Filters to include only data changed within a timeframe; data-based or log-based | Medium-Low |
| **Near-real-time / Event-driven** | Smaller sets spread across the day; triggered by events (e.g., data update) | Low |
| **Asynchronous** | Sending system does not wait for acknowledgment; loosely coupled | Low (seconds/minutes) |
| **Real-time, Synchronous** | No delay acceptable; data sets kept perfectly in sync (e.g., two-phase commit) | Very Low |
| **Low Latency / Streaming** | Extremely fast data movement; streaming data flows continuously as events occur | Ultra-Low |

#### Change Data Capture (CDC) Techniques

- **Data-based CDC:**
  - Source system populates timestamps, codes, or flags as change indicators
  - Source system maintains a list of changed objects/identifiers
  - Source system copies changed data into a separate object during the transaction
- **Log-based CDC:** Database activity logs are copied and processed; changes translated and applied to target

### 1.3 Replication

To provide better response time for globally distributed users, some applications maintain exact copies of data sets in multiple physical locations. Replication:
- Minimizes the performance impact of analytics on primary transactional environments
- Uses database management system utilities that monitor change logs
- Works best when source and target data sets are exact copies
- Risks arise when data may be changed at multiple copy sites simultaneously

See [[Data Storage and Operations]].

### 1.4 Archiving

Data used infrequently may be moved to an alternate data structure or storage solution that is less costly. ETL functions transport and possibly transform archive data. Monitor archive technology to ensure data remains accessible when technology changes.

See [[Document and Content Management]].

### 1.5 Enterprise Message Format / Canonical Model

A **canonical data model** is a common model used by an organization or data exchange group that standardizes the format in which data will be shared. In a hub-and-spoke design pattern:
- All systems interact only with a central information hub
- Data is transformed to/from a common enterprise message format
- Each system needs to transform only to and from the central canonical model
- Greatly reduces complexity in environments with 100+ application systems

See [[Data Modeling and Design]].

### 1.6 Interaction Models

#### Point-to-Point
Systems pass data directly to each other. Efficient for a small set of systems but becomes inefficient as systems multiply:
- Number of interfaces approaches $$n^2$$
- Impacts operational system processing
- Potential for data inconsistency across downstream systems

#### Hub-and-Spoke
Consolidates shared data (physically or virtually) in a central data hub. Examples: Data Warehouses, Data Marts, Operational Data Stores, MDM hubs. Benefits:
- Consistent views of data with limited performance impact on source systems
- Adding new systems only requires building interfaces to the data hub
- Cost-justified even for relatively small numbers of systems

#### Publish-Subscribe
Systems push data out (publish) and other systems pull data in (subscribe). When data is published, it is automatically sent to subscribers. Ensures all constituents receive consistent data in a timely manner.

### 1.7 DII Architecture Concepts

#### Application Coupling
- **Tight Coupling:** Synchronous interfaces where one system waits for a response; both systems effectively share availability risk
- **Loose Coupling** (preferred): Data passed between systems without waiting for a response; one system may be unavailable without affecting the other

#### Orchestration and Process Controls
**Orchestration** describes how multiple processes are organized and executed to preserve consistency and continuity.

**Process Controls** ensure shipment, delivery, extraction, and loading of data is accurate and complete:
- Database activity logs
- Batch job logs
- Alerts
- Exception logs
- Job dependence charts with remediation options
- Job clock information (timing, expected length, computing window)

#### Enterprise Application Integration (EAI)
Software modules interact only through well-defined interface calls (APIs). Data stores are updated only by their own software modules. Built on object-oriented concepts emphasizing reuse and module independence.

#### Enterprise Service Bus (ESB)
A system acting as an intermediary between systems, passing messages between them. Applications can send/receive messages or files through the ESB. An example of loose coupling; acts as the service between applications.

#### Service-Oriented Architecture (SOA)
Well-defined interaction between self-contained software modules. Each module provides services to other modules or human consumers. Key concepts:
- Service has no foreknowledge of the calling application
- Implementation is a black box to the calling application
- Implemented via web services, messaging, RESTful APIs, etc.
- Requires strong governance around design and registration of services/APIs

#### Complex Event Processing (CEP)
Tracks and analyzes streams of information about events, combining data from multiple sources to:
- Identify meaningful events (opportunities or threats)
- Predict behavior or activity
- Automatically trigger real-time response
- Often tied to Big Data and ultra-low latency technologies

#### Data Federation and Virtualization
**Data Federation** provides access to a combination of individual data stores, regardless of structure. **Data Virtualization** enables distributed databases and multiple heterogeneous data stores to be accessed and viewed as a single database.

See [[Data Storage and Operations]].

#### Data-as-a-Service (DaaS)
Data licensed from a vendor and provided on demand, rather than stored/maintained in the licensing organization's data center. Also used internally to provide enterprise data or data services via a catalog of services, service levels, and pricing.

#### Cloud-based Integration (IPaaS)
Integration Platform-as-a-Service: systems integration delivered as a cloud service addressing data, process, SOA, and application integration use cases. Emerged to meet demand for integrating data from SaaS applications located outside organizational data centers.

### 1.8 Data Exchange Standards

Formal rules for the structure of data elements (e.g., ISO standards, industry-specific standards). An exchange pattern defines a structure for data transformations. Example: **National Information Exchange Model (NIEM)** — developed for document and transaction exchange across US government organizations using XML.

---

## 2. Data Integration Activities

DII activities follow a development lifecycle: Plan → Design → Develop → Implement → Monitor.

### 2.1 Plan and Analyze

#### 2.1.1 Define Data Integration and Lifecycle Requirements
Understand business objectives, required data, technology initiatives, relevant laws/regulations, and data retention policies. Requirements determine the DII interaction model, technology, and services needed.

#### 2.1.2 Perform Data Discovery
Identify potential data sources for the integration effort through:
- Technical search using tools that scan Metadata and/or actual data contents
- Subject matter expertise (interviewing people working with the data)
- High-level data quality assessment

#### 2.1.3 Document Data Lineage
Document how data flows through an organization: where it is acquired/created, where it moves and changes, and how it is used for analytics, decision-making, or event triggering. May identify opportunities for improvements in existing data flows.

#### 2.1.4 Profile Data
Profile data to understand content and structure before integration design. Basic profiling includes analysis of:
- Data format (defined and inferred)
- Data population (null, blank, defaulted levels)
- Data values and correspondence to valid values
- Internal patterns and relationships (related fields, cardinality rules)
- Relationships to other data sets

See [[Metadata Management]].

#### 2.1.5 Collect Business Rules
Business rules are statements that define or constrain business processing. Categories:
- Definitions of business terms
- Facts relating terms to each other
- Constraints or action assertions
- Derivations

For MDM: match rules, merge rules, survivorship rules, trust rules. For archiving/warehousing: data retention rules.

### 2.2 Design Data Integration Solutions

#### 2.2.1 Design Data Integration Architecture
Specify solutions at both enterprise and individual solution levels. A solution architecture indicates:
- Techniques and technologies to be used
- Inventory of data structures (persistent and transitive)
- Orchestration and frequency of data flow
- Regulatory and security concerns
- Operating concerns (backup, recovery, availability, archive/retention)

#### 2.2.2 Model Data Hubs, Interfaces, Messages, and Data Services
Model both persistent structures (MDM hubs, data warehouses/marts, ODS) and transient structures (interfaces, message layouts, canonical models).

See [[Data Modeling and Design]].

#### 2.2.3 Map Data Sources to Targets
Specify rules for transforming data from source to target. For each attribute mapped:
- Technical format of source and target
- Transformations required for all intermediate staging points
- How each attribute will be populated
- Whether data values need transformation (lookups)
- Required calculations

#### 2.2.4 Design Data Orchestration
Document the pattern of data flows from start to finish. Batch orchestration involves scheduling with frequency, dependencies. Real-time orchestration is triggered by events and may be non-linear.

### 2.3 Develop Data Integration Solutions

#### 2.3.1 Develop Data Services
Develop services to access, transform, and deliver data using tools or vendor suites (data transformation, MDM, data warehousing). Consistent tools across the organization simplify support.

#### 2.3.2 Develop Data Flows
- **Batch:** Developed in a scheduler managing order, frequency, and dependency
- **Real-time:** Monitor for events that trigger execution of services; best implemented with cross-technology solutions

#### 2.3.3 Develop Data Migration Approach
Move data when applications are implemented, retired, or merged. Migration projects are frequently under-estimated. Profiling core operational data often reveals data that was migrated from previous generations and doesn't meet current standards.

See [[Data Storage and Operations]].

#### 2.3.4 Develop a Publication Approach
Systems that create/maintain critical data should push new/changed data to other systems (especially data hubs and ESBs) either at the time of change or on a periodic schedule. Define common message definitions (canonical model) and let consumers subscribe.

#### 2.3.5 Develop Complex Event Processing Flows
Requires:
1. Preparation of historical data and pre-population of predictive models
2. Processing real-time data streams to populate the predictive model and identify meaningful events
3. Executing the triggered action in response to the prediction

#### 2.3.6 Maintain DII Metadata
Document data structures of all systems involved (source, target, staging) including:
- Business definitions and technical definitions
- Transformation rules between persistent data stores
- SOA registry with controlled access to the catalog of available services

See [[Metadata Management]].

### 2.4 Implement and Monitor
Activate developed and tested data services. Establish parameters for potential processing problems with automated and human monitoring. Data interaction capabilities must meet the service level of the most demanding target application or data consumer.

---

## 3. Tools

| Tool | Description |
|------|-------------|
| **Data Transformation Engine / ETL Tool** | Primary tool for data integration; supports both design and operation of transformation activities. Consider batch vs. real-time and structured vs. unstructured requirements |
| **Data Virtualization Server** | Performs extract, transform, and integrate virtually; can combine structured and unstructured data |
| **Enterprise Service Bus (ESB)** | Message-oriented middleware for near real-time messaging between heterogeneous systems; implements message queues with adapters/agents |
| **Business Rules Engine** | Allows non-technical users to manage business rules; supports changes to predictive models without technical code changes |
| **Data and Process Modeling Tools** | Design target and intermediate data structures, message structures, and process flows |
| **Data Profiling Tool** | Statistical analysis of data set contents (format, completeness, consistency, validity, structure) |
| **Metadata Repository** | Contains information about data structure, content, and business rules; documents technical structure and business meaning across the integration lifecycle |

---

## 4. Techniques

Key techniques for designing DII solutions:
- Keep applications **loosely coupled**
- Limit the number of interfaces using **hub-and-spoke** approach
- Create **standard (canonical) interfaces**
- Use **enterprise perspective** in design, but implement through iterative and incremental delivery
- Balance **local data needs** with enterprise data needs
- Ensure **business accountability** for DII design and activity

---

## 5. Implementation Guidelines

### 5.1 Readiness Assessment / Risk Assessment
- Design enterprise solutions to support movement of data between **many** applications, not just the first one implemented
- Focus on implementing solutions where **none or limited integration** currently exists, rather than replacing working solutions
- Data projects (data warehouse, MDM hub) can justify a focused DII solution; additional uses add value to the investment
- Sponsor enterprise DII programs from a level with sufficient authority over solution design and technology purchase
- Retain focus on **business goals and requirements** — include business/application-oriented participants, not just tool experts

### 5.2 Organization and Cultural Change
- Determine centralized vs. decentralized responsibility for managing DII implementations
- Develop a **Center of Excellence** for enterprise DII solutions
- Local teams understand their data; central teams build deep knowledge of tools/technologies
- Data analysis and modeling should be performed by **business-oriented resources**
- Development of a canonical message model requires large resource commitment involving both business and technical resources
- Review all data transformation mapping design with **business subject matter experts**

---

## 6. DII Governance

Decisions about data messages, data models, and transformation rules must be **business-driven**. A purely technical approach can lead to errors in data mappings and transformations.

| Governance Area | Key Points |
|-----------------|------------|
| **Business Ownership** | Business stakeholders define and approve rules for data modeling and transformation |
| **Governance Triggers** | Determine what events trigger governance reviews (exceptions, critical events); map triggers to reviews aligned with SDLC Stage Gates or User Stories |
| **Controls** | Mandated reviews of models, auditing of Metadata, gating of deliverables, required approvals for transformation rule changes |
| **SLA/BCP** | Real-time DII solutions must be included in the same backup/recovery tier as the most critical system they serve |
| **Enterprise Policies** | Ensure SOA principles are followed; new services created only after review of existing services; data flows through the ESB |

### 6.1 Data Sharing Agreements
Prior to developing interfaces, develop a **data sharing agreement** or **memorandum of understanding (MOU)** stipulating:
- Responsibilities and acceptable use of data to be exchanged
- Anticipated use and access to the data
- Restrictions on use
- Expected service levels (system uptime, response times)

### 6.2 DII and Data Lineage
- Data lineage is required for data consumers and increasingly important for inter-organizational integration
- Emerging compliance standards (e.g., Solvency II) require organizations to describe data origins and transformation history
- Forward and backward data lineage is critical for **impact analysis** when making changes

### 6.3 Data Integration Metrics

| Category | Metrics |
|----------|---------|
| **Data Availability** | Availability of data requested |
| **Data Volumes and Speed** | Volumes transported/transformed; volumes analyzed; speed of transmission; latency between update and availability; latency between event and triggered action; time to availability of new data sources |
| **Solution Costs and Complexity** | Cost of developing/managing solutions; ease of acquiring new data; complexity of solutions and operations; number of systems using DII solutions |

---

## 7. Dependencies on Other Knowledge Areas

DII depends on:
- **[[Data Governance]]** — Governing transformation rules and message structures
- **[[Data Architecture]]** — Designing solutions
- **[[Data Security]]** — Ensuring solutions protect data security (persistent, virtual, or in motion)
- **[[Metadata Management]]** — Tracking technical inventory, business meaning, transformation rules, operational history, and lineage
- **[[Data Storage and Operations]]** — Managing physical instantiation of solutions
- **[[Data Modeling and Design]]** — Designing data structures (persistent, virtual, and message structures)

DII is critical to:
- **[[Data Warehousing and Business Intelligence]]** — Transforming and integrating data from source systems to consolidated hubs
- **[[Reference and Master Data]]** — Integrating data from sources to hubs and from hubs to target systems
- **Big Data Management** — Integrating structured, unstructured, audio, video, and streaming data for mining and predictive models

---

## 8. Summary

| Principle | Description |
|-----------|-------------|
| **ETL/ELT** | Core process: Extract → Transform → Load (or Extract → Load → Transform for Big Data) |
| **Latency Spectrum** | From batch (high latency) to streaming (ultra-low latency), choose based on business requirements |
| **Interaction Models** | Hub-and-spoke preferred over point-to-point for enterprise scale; publish-subscribe for event-driven |
| **Loose Coupling** | Preferred architectural approach — SOA, ESB, asynchronous messaging |
| **Canonical Model** | Common message format reduces transformation complexity across many systems |
| **Business-Driven** | Rules, mappings, and transformations must be owned by business stakeholders |
| **Metadata Foundation** | All DII activities depend on and produce Metadata that must be managed |

---

## References

- Aiken, P. and Allen, D. M. *XML in Data Management*. Morgan Kaufmann, 2004.
- Bobak, Angelo R. *Connecting the Data: Data Integration Techniques for Building an Operational Data Store (ODS)*. Technics Publications, 2012.
- Brackett, Michael. *Data Resource Integration*. Technics Publications, 2012.
- Doan, AnHai, Alon Halevy, and Zachary Ives. *Principles of Data Integration*. Morgan Kaufmann, 2012.
- Giordano, Anthony David. *Data Integration Blueprint and Modeling*. IBM Press, 2011.
- Hohpe, Gregor and Bobby Woolf. *Enterprise Integration Patterns*. Addison-Wesley Professional, 2003.
- Linthicum, David S. *Enterprise Application Integration*. Addison-Wesley Professional, 1999.
- Reeve, April. *Managing Data in Motion: Data Integration Best Practice Techniques and Technologies*. Morgan Kaufmann, 2013.
- Sherman, Rick. *Business Intelligence Guidebook: From Data Integration to Analytics*. Morgan Kaufmann, 2014.
- Van der Lans, Rick. *Data Virtualization for Business Intelligence Systems*. Morgan Kaufmann, 2012.
