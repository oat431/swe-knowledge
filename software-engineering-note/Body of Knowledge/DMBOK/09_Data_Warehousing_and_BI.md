---
tags: [data-warehousing, bi, data-management, dmbok]
source: "DMBoK v2, Chapter 11"
created: 2026-07-09
---

# Data Warehousing and Business Intelligence

## 1. Introduction

The concept of the Data Warehouse emerged in the 1980s as technology enabled organizations to integrate data from a range of sources into a common data model. Integrated data promised to provide insight into operational processes and open up new possibilities for leveraging data to make decisions and create organizational value. Data warehouses were also seen as a means to reduce the proliferation of decision support systems (DSS), most of which drew on the same core enterprise data. The concept of an enterprise warehouse promised a way to reduce data redundancy, improve the consistency of information, and enable an enterprise to use its data to make better decisions.

Data warehouses began to be built in earnest in the 1990s. Since then — especially with the co-evolution of [[business-intelligence|Business Intelligence]] as a primary driver of business decision-making — data warehouses have become mainstream. Most enterprises have data warehouses and warehousing is the recognized core of enterprise [[data-management|data management]]. Even though well established, the data warehouse continues to evolve. As new forms of data are created with increasing velocity, new concepts such as [[data-lake|data lakes]] are emerging that will influence the future of the data warehouse.

> **Definition:** Planning, implementation, and control processes to provide decision support data and support knowledge workers engaged in reporting, query, and analysis.

**Goals:**
1. Build and maintain the technical environment and technical/business processes needed to deliver integrated data in support of operational functions, compliance requirements, and business intelligence activities
2. Support and enable effective business analysis and decision making by knowledge workers

---

## 2. Business Drivers

The primary driver for data warehousing is to support operational functions, compliance requirements, and [[business-intelligence|Business Intelligence]] (BI) activities — though not all BI activities depend on warehouse data. Increasingly organizations are being asked to provide data as evidence that they have complied with regulatory requirements. Because they contain historical data, warehouses are often the means to respond to such requests.

Nevertheless, BI support continues to be the primary reason for a warehouse. BI promises insight about the organization, its customers, and its products. An organization that acts on knowledge gained from BI can improve operational efficiency and competitive advantage. As more data has become available at a greater velocity, BI has evolved from retrospective assessment to [[predictive-analytics|predictive analytics]].

## 3. Goals and Principles

Organizations implement data warehouses in order to:
- Support Business Intelligence activity
- Enable effective business analysis and decision-making
- Find ways to innovate based on insights from their data

**Guiding Principles:**

| Principle | Description |
|---|---|
| **Focus on business goals** | Ensure DW serves organizational priorities and solves business problems |
| **Start with the end in mind** | Let business priority and scope of end-data-delivery in the BI space drive creation of DW content |
| **Think global, build local** | Let end-vision guide architecture, but build and deliver incrementally through focused projects or sprints |
| **Summarize last, not first** | Build on atomic data; aggregate and summarize to meet requirements and ensure performance, not to replace detail |
| **Promote transparency and self-service** | The more context (metadata) provided, the better data consumers can get value out of the data |
| **Build metadata with the warehouse** | Critical to DW success is the ability to explain the data — capture metadata as part of development and manage it in ongoing operations |
| **Collaborate** | Collaborate with other data initiatives, especially [[data-governance|Data Governance]], [[data-quality|Data Quality]], and [[metadata-management|Metadata]] |
| **One size does not fit all** | Use the right tools and products for each group of data consumers |

---

## 4. Essential Concepts

### 4.1 Business Intelligence (BI)

The term **Business Intelligence** has two meanings:

1. **A type of data analysis** aimed at understanding organizational activities and opportunities. Results are used to improve organizational success. If an organization asks the right questions of its own data, it can gain insights about its products, services, and customers that enable better decisions.

2. **A set of technologies** that support this kind of data analysis. An evolution of decision support tools, BI tools enable querying, data mining, statistical analysis, reporting, scenario modeling, data visualization, and dashboarding. They are used for everything from budgeting to advanced analytics.

### 4.2 Data Warehouse (DW)

A **Data Warehouse** is a combination of two primary components:
- An integrated decision support database
- Related software programs used to collect, cleanse, transform, and store data from a variety of operational and external sources

To support historical, analytical, and BI requirements, a data warehouse may also include **dependent data marts** — subset copies of data from the warehouse. In its broadest context, a data warehouse includes any data stores or extracts used to support the delivery of data for BI purposes.

An **Enterprise Data Warehouse (EDW)** is a centralized data warehouse designed to service the BI needs of the entire organization. An EDW adheres to an [[enterprise-data-model|enterprise data model]] to ensure consistency of decision support activities across the enterprise.

### 4.3 Data Warehousing

**Data Warehousing** describes the operational extract, cleansing, transformation, control, and load processes that maintain the data in a data warehouse. The data warehousing process focuses on enabling an integrated and historical business context on operational data by enforcing business rules and maintaining appropriate business data relationships. Data warehousing also includes processes that interact with [[metadata-management|metadata repositories]].

Traditionally, data warehousing focuses on **structured data** — elements in defined fields, whether in files or tables, as documented in [[data-modeling|data models]]. With recent advances, the BI and DW space now embraces **semi-structured** and **unstructured data**.

### 4.4 Approaches to Data Warehousing

Two influential thought leaders have driven the conversation about data warehouse architecture:

| Aspect | Bill Inmon | Ralph Kimball |
|---|---|---|
| **Definition** | "A subject-oriented, integrated, time-variant and non-volatile collection of data in support of management's decision-making process" | "A copy of transaction data specifically structured for query and analysis" |
| **Data Model** | Normalized relational model (3NF) | Dimensional model (star schema) |
| **Architecture** | Corporate Information Factory (CIF) | Dimensional Data Warehouse (DW Bus) |
| **Integration** | Enterprise-wide, top-down | Departmental/process, bottom-up via conformed dimensions |
| **Focus** | Corporate data management | End-user query and analysis performance |

Despite different approaches, both recognize these core ideas:
- Warehouses store data from other systems
- The act of storage includes organizing data in ways that increase its value
- Warehouses make data accessible and usable for analysis
- Organizations build warehouses to make reliable, integrated data available to authorized stakeholders
- Warehouse data serves many purposes — from workflow support to operational management to predictive analytics

### 4.5 Corporate Information Factory (Inmon)

Bill Inmon's **Corporate Information Factory (CIF)** is one of the two primary patterns for data warehousing. The defining characteristics of a data warehouse in the CIF approach:

| Characteristic | Description |
|---|---|
| **Subject-oriented** | Organized based on major business entities, not functional applications |
| **Integrated** | Unified and cohesive — same key structures, encoding, data definitions, naming conventions applied consistently |
| **Time-variant** | Stores data as it exists at a point in time; queries by time period produce consistent results |
| **Non-volatile** | Records are not normally updated; new data is appended to existing data |
| **Aggregate and detail** | Contains atomic-level transactions as well as summarized data |
| **Historical** | Contains vast amounts of historical data, not just current state |

**CIF Components:**

```
Applications → Staging Area → Integration/Transformation → ODS → DW → Data Marts → Analysis
                                       ↕                        ↓
                              Reference/Master Data      Operational Reports
```

- **Applications:** Perform operational processes; detail data is brought into DW and ODS
- **Staging Area:** Database between operational sources and target databases where ETL takes place; not used by end users
- **Integration and Transformation:** Data from disparate sources is transformed into standard corporate representation
- **Operational Data Store (ODS):** Integrated database of current/near-term operational data (30-90 days); volatile
- **Data Warehouse:** Single integration point for corporate data; supports management decision-making and strategic analysis
- **Data Marts:** Subsets of warehouse data designed to support particular kinds of analysis or specific consumer groups; often use dimensional modeling
- **Operational Data Mart (OpDM):** Focused on tactical decision support; sourced directly from ODS
- **Reference, Master, and External Data:** Required to understand transactions; DW requires historical values with validity timeframes

**Data Flow Characteristics (Left → Right):**
- Purpose shifts from operational execution to analysis
- End users move from front-line workers to decision-makers
- System usage moves from fixed operations to ad hoc uses
- Response time requirements are relaxed
- Much more data is involved in each operation
- Data is organized by subject rather than function
- Data is integrated rather than siloed
- Data has higher latency, significantly more history

### 4.6 Dimensional DW (Kimball)

Kimball's **Dimensional Data Warehouse** is the other primary pattern. Data is stored in a **dimensional data model** — specifically designed to enable data consumers to understand and use the data while enabling query performance. It is not normalized like an entity-relationship model.

**Star Schema:** Dimensional models comprise:
- **Facts:** Quantitative data about business processes (e.g., sales numbers)
- **Dimensions:** Descriptive attributes related to fact data (e.g., product, date, store)

A fact table joins with many dimension tables, appearing as a star when diagrammed. Multiple fact tables share common (**conformed**) dimensions via a **bus**, similar to a bus in a computer.

**DW Bus Matrix:** Shows the intersection of business processes (that generate fact data) and data subject areas (that represent dimensions). Opportunities for conformed dimensions exist where multiple processes use the same data.

| Business Processes | Date | Product | Store | Vendor | Warehouse |
|---|---|---|---|---|---|
| Sales | ✗ | ✗ | ✗ | | |
| Inventory | ✗ | ✗ | ✗ | ✗ | ✗ |
| Orders | ✗ | ✗ | | ✗ | |
| **Conformed Candidate** | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes | ❌ No |

**Kimball DW Components:**

- **Operational Source Systems:** Operational/transactional applications creating data integrated into the DW
- **Data Staging Area:** Set of processes to integrate and transform data for presentation; combines integration, transformation, and DW components
- **Data Presentation Area:** Similar to CIF Data Marts, unified by the DW Bus of conformed dimensions
- **Data Access Tools:** End-user tools driven by data consumer requirements

### 4.7 DW Architecture Components

The data warehouse environment includes a collection of architectural components. Data moves from source systems → staging area (cleansed/enriched/integrated) → DW/ODS → marts/cubes → reporting and analysis.

#### 4.7.1 Source Systems

Include operational systems and external data brought into the DW/BI environment:
- CRM, Accounting, HR applications
- Industry-specific operational systems
- Vendor and external data sources
- [[data-as-a-service|DaaS]], web content, Big Data computation results

#### 4.7.2 Data Integration

Covers [[etl|Extract, Transform, and Load (ETL)]], data virtualization, and other techniques for getting data into a common form and location. In a SOA environment, the data services layer is part of this component. See [[data-integration|Data Integration and Interoperability]].

#### 4.7.3 Data Storage Areas

| Storage Area | Description |
|---|---|
| **Staging Area** | Intermediate store between source and centralized repository; data is transformed, integrated, and prepped for loading |
| **Reference & Master Data** | May be stored in separate repositories; DW feeds new master data and is fed by conformed dimension contents |
| **Central Warehouse (Atomic Layer)** | Maintains all historical atomic data and latest batch instance; uses business/surrogate keys, indices, foreign keys, and CDC techniques |
| **Operational Data Store (ODS)** | Supports lower latency for operational use; contains a time window of data (not full history); refreshed more quickly than warehouse |
| **Data Marts** | Support presentation layers; departmental/functional subsets for integrated reporting, query, and analysis |
| **Cubes** | Support Online Analytical Processing (OLAP) — three approaches: Relational (ROLAP), Multi-dimensional (MOLAP), and Hybrid (HOLAP) |

### 4.8 Types of Load Processing

#### 4.8.1 Historical Data

One advantage of a data warehouse is capturing detailed history. Different methods exist:

| Approach | How History Is Stored |
|---|---|
| **Inmon** | All data in a single atomic layer; cleansed, standardized, governed; common integration layer; star-structured data marts for consumption |
| **Kimball** | Combination of departmental data marts containing cleansed, standardized, governed data at atomic level; conformed dimensions and facts deliver enterprise view |
| **Data Vault** | History in normalized atomic structure; dimensional surrogate/primary/alternate keys defined; vault available to variety of data marts; supports reloading facts when grain changes occur; can virtualize presentation layer for agile delivery |

#### 4.8.2 Batch Change Data Capture (CDC)

Data Warehouses are often loaded daily via a nightly batch window. Different CDC techniques accommodate varying source system capabilities:

| Method | Source Requirement | Complexity | Fact Load | Dim Load | Overlap | Tracks Deletes |
|---|---|---|---|---|---|---|
| **Time Stamped Delta** | Source changes are time-stamped with system date/time | Low | Fast | Fast | Yes | No |
| **Log Table Delta** | Source changes captured and stored in log tables | Medium | Nominal | Nominal | Yes | Yes |
| **Database Transaction Log** | Database captures changes in transaction log | High | Nominal | Nominal | No | Yes |
| **Message Delta** | Source changes published as near-real-time messages | Extreme | Slow | Slow | No | Yes |
| **Full Load** | No change indicator; tables extracted in full and compared | Simple | Slow | Nominal | Yes | Yes |

See [[data-integration#change-data-capture|CDC Techniques]] for more details.

#### 4.8.3 Near-Real-Time and Real-Time

With **Operational BI** pushing for lower latency, new architectural approaches emerged for including volatile data. Two key design concepts:
- **Isolation of change:** Impact of new volatile data must be isolated from bulk historical, non-volatile DW data (using partitions and union queries)
- **Alternatives to batch processing:** Three main types:

| Type | Accumulation | Description |
|---|---|---|
| **Trickle Feeds** | Source accumulation | Batch loads on more frequent schedules (hourly, every 5 min) or when threshold reached |
| **Messaging** | Bus accumulation | Small data packets published to a bus as they occur; target systems subscribe and incrementally process |
| **Streaming** | Target accumulation | Target system collects data in buffer/queue as received; processes in order |

See [[data-integration#real-time-integration|Real-Time Integration]].

---

## 5. Activities

### 5.1 Understand Requirements

Developing a data warehouse differs from developing an operational system. Data warehouses bring together data used in a range of different ways, and usage evolves over time as users analyze and explore.

**Requirements gathering approach:**
1. Begin with business goals and strategy
2. Identify and scope business areas
3. Interview appropriate business people — ask what they do and why
4. Capture specific questions they ask now and want to ask of the data
5. Document how they categorize important aspects of information
6. Define and capture key performance metrics and calculations (uncovers business rules)
7. Catalog and prioritize requirements: necessary for go-live vs. can wait
8. Look for simple, valuable items to jump-start productivity of initial release

### 5.2 Define and Maintain the DW/BI Architecture

The DW/BI architecture describes:
- **Where** data comes from
- **Where** it goes
- **When** it goes
- **Why** and **how** it goes into a warehouse

#### 5.2.1 Technical Architecture

Best DW/BI architectures design a mechanism to connect back to transactional-level and operational-level reports in an atomic DW — protecting the DW from carrying every transactional detail. Example: providing a viewing mechanism for key operational reports based on a transactional key (e.g., Invoice Number).

- Start with a **conceptual architecture**
- Use **prototyping** to prove/disprove key points before expensive technology commitments
- Empower business community with **knowledge and adoption programs** through a sanctioned change management team
- Validate physical deployment against the **logical/enterprise data model**

#### 5.2.2 Management Processes

- Establish a coordinated, integrated maintenance process delivering regular releases
- Manage each update as a **software release** provisioning additional functionality
- Establish a **standard release schedule** for annual demand/resource planning
- Work pro-actively and collaboratively in a cross-functional team

### 5.3 Develop the Data Warehouse and Data Marts

DW/BI projects have three concurrent development tracks:

| Track | Focus |
|---|---|
| **Data** | Identifying best sources, designing rules for remediation/transformation/integration/storage, handling data that doesn't fit expectations |
| **Technology** | Back-end systems and processes supporting data storage and movement; integration with existing enterprise architecture |
| **BI Tools** | Suite of applications necessary for data consumers to gain meaningful insight from deployed data products |

#### 5.3.1 Map Sources to Targets

**Source-to-target mapping** establishes transformation rules for entities and data elements from individual sources to a target system. It also documents **lineage** for each data element available in the BI environment back to its respective source(s).

Key challenges:
- Determining valid links between data elements in multiple systems (equivalent data rarely has same names/structures)
- A solid **taxonomy** (typically the logical data model) is necessary
- Must address whether data is appended, changed in place, or inserted

#### 5.3.2 Remediate and Transform Data

**Data remediation (cleansing):** Enforces standards and corrects/improves domain values. Particularly necessary for initial loads with significant history. Source systems should ideally be made responsible for data remediation.

**Load strategies:**
- **Optimistic:** Create dimension entries to accommodate fact data; must account for update/expiry
- **Pessimistic:** Recycle area for fact data that cannot be associated with corresponding dimension keys; requires notification and alerting

**Data transformation:** Implements business rules within a technical system. Essential to [[data-integration|data integration]]. Requires direct involvement from Data Stewards and SMEs. Rules should be documented for governance.

### 5.4 Populate the Data Warehouse

The largest part of any DW/BI effort is data preparation and processing. Key factors to consider when defining a population approach:

- Required latency
- Availability of sources
- Batch windows or upload intervals
- Target databases
- Dimensional aspects
- Timeframe consistency of DW and data marts
- Data quality processing
- Time to perform transformations
- Late-arriving dimensions and data rejects

**CDC considerations:** Detecting changes in source systems, integrating changes together, and aligning changes across time. Databases increasingly provision log capture functionality that data integration tools can operate on directly.

The **first increment** paves the way for additional capability development and onboarding new business units. Create processes to facilitate and automate timely identification of data errors with end-user workflow integration.

### 5.5 Implement the Business Intelligence Portfolio

Implementing the BI Portfolio involves identifying the right tools for the right user communities.

#### 5.5.1 Group Users According to Needs

There is a spectrum of BI needs:

| User Type | Needs |
|---|---|
| **IT Developers** | Extracting data; focus on advanced functionality |
| **Information Consumers** | Fast access to previously developed/executed reports; some interactivity (drill, filter, sort) or static reports |
| **Power Users (Analysts, Managers)** | Highly interactive reports; drill into reports, slice and dice data, identify root causes |
| **Executives** | Combination of fixed reports, dashboards, and scorecards |
| **External Customers** | May use any of these tools as part of their experience |

Users may move between classes as skills increase or functions change.

#### 5.5.2 Match Tools to User Requirements

- Market offers impressive range of reporting and analytics tools
- Major BI vendors offer classic pixel-perfect report capabilities
- Many application vendors offer **embedded analytics** with standard content from pre-populated cubes or aggregate tables
- **Virtualization** blurs lines between on-premises and external data sources
- Common delivery mechanisms: web, email, applications
- **BI Suites** (combined tools via M&A or development) are primary option at Enterprise Architecture level
- Every BI tool comes with a price: system resources, support, training, architectural integration

### 5.6 Maintain Data Products

An implemented warehouse and its customer-facing BI tools constitute a **data product**. Enhancements should be implemented incrementally.

#### 5.6.1 Release Management

Critical to incremental development processes. A sample quarterly release schedule:

- **3 business-driven releases per year:** Each providing incremental capabilities
- **1 technology-based release:** Addressing internal warehouse requirements
- Work scope managed with **MoSCoW** prioritization (Must, Should, Could, Won't)
- Time managed with **TimeBoxes**

#### 5.6.2 Manage Data Product Development Lifecycle

| Stage | Description |
|---|---|
| **Development** | Iterations aligned to releases with backorder work list prioritized by business units |
| **Pilot/Sandbox** | Items promoted for business users to investigate, experiment, develop new models; less governance but requires sandbox prioritization |
| **Production** | Items passing pilot and deemed production-ready by business and IT; promoted as new data products |
| **Reject/Return** | Items not passing pilot returned for fine-tuning or rejected entirely |

#### 5.6.3 Monitor and Tune Load Processes

- Monitor load processing for bottlenecks and dependencies
- Employ database tuning: partitioning, tuned backup/recovery strategies
- Archiving is difficult in DW — users often view DW as an active archive due to long histories

#### 5.6.4 Monitor and Tune BI Activity and Performance

Best practices:
- Define and display **customer-facing satisfaction metrics** (avg query response time, users per day/week/month)
- Conduct regular **customer surveys**
- Review **usage statistics and patterns** regularly
- Create indexes and aggregations based on usage patterns
- Post completed daily results to frequently-run reports for performance gains
- Provide a **dashboard** exposing high-level status of data delivery activities with drill-down capability
- Add **data quality measures** to enhance dashboard value
- Use **heat maps** to visualize workload on infrastructure, data throughput, and compliance

---

## 6. Key Relationships

Data Warehousing and BI intersects with all other DMBOK knowledge areas:

| Knowledge Area | Relationship |
|---|---|
| [[data-governance|Data Governance]] | Governs DW/BI priorities, standards, and data ownership |
| [[data-architecture|Data Architecture]] | Defines the overall DW/BI technical and data architecture |
| [[data-modeling|Data Modeling & Design]] | Provides dimensional models, star schemas, and enterprise data models for DW |
| [[data-storage|Data Storage & Operations]] | Manages the physical storage, backup, and operations of DW environments |
| [[data-security|Data Security]] | Controls access to sensitive data within BI reports and DW tables |
| [[data-integration|Data Integration & Interoperability]] | Provides ETL/ELT processes that populate the warehouse |
| [[document-management|Document & Content Management]] | Manages unstructured data that may complement DW structured data |
| [[reference-and-master-data|Reference & Master Data]] | Supplies conformed dimensions and master data to the DW |
| [[metadata-management|Metadata Management]] | Tracks lineage, definitions, and business context of DW data |
| [[data-quality|Data Quality]] | Ensures data meets quality thresholds before and after loading into DW |
| [[big-data|Big Data & Data Science]] | Complements traditional DW with predictive analytics, data lakes, and ELT processing |

---

## 7. DW/BI vs. Big Data Architecture

The biggest difference between DW/BI and Big Data processing:

| Aspect | Traditional DW/BI | Big Data |
|---|---|---|
| **Integration Pattern** | **ETL** — Extract, TRANSFORM, Load | **ELT** — Extract, Load, Transform |
| **Integration Timing** | Before loading into warehouse | After loading (or during use) |
| **Data Model** | Typically relational / dimensional | May not produce an enterprise data model |
| **Processing** | Batch-oriented, nightly windows | Streaming, real-time, batch |
| **Data Types** | Primarily structured | Structured, semi-structured, unstructured |
| **Analysis Focus** | Descriptive (hindsight), some predictive | Predictive, prescriptive (foresight) |

This difference has significant implications for how data is managed. The risk with Big Data is that much knowledge about data can be lost if ingestion and use processes are executed in an ad hoc way — hence the critical need for [[metadata-management|metadata management]].

See [[big-data|Big Data and Data Science]] for more on Data Lakes, Services-Based Architecture, and Machine Learning.

---

## 8. Context Diagram Summary

| Aspect | Details |
|---|---|
| **Inputs** | Business requirements; scalability/operational/infrastructure requirements; data quality/security/access requirements; IT strategy; IT policies & standards; internal data feeds; master & reference data; industry & external data |
| **Activities** | Understand requirements; define/maintain DW/BI architecture; develop DW and data marts; populate DW; implement BI portfolio; maintain data products |
| **Deliverables** | DW and BI architecture; data products; population processes; governance activities; lineage dictionary; learning & adoption; release plan; production support; load tuning; BI activity monitoring |
| **Suppliers** | Business executives; governance body; enterprise architecture; data producers; information consumers |
| **Participants** | Sponsors & product owners; architects & analysts; DW/BI specialists; project management; change management; subject matter experts |
| **Consumers** | Information consumers; customers; managers & executives |
| **Techniques** | Prototypes to drive requirements; self-service BI; queryable audit data |
| **Tools** | Metadata repositories; data integration tools; analytic applications |
| **Metrics** | Usage metrics; customer/user satisfaction; subject area coverage %; response/performance metrics |
