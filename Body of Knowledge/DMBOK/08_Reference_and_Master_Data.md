---
title: "Reference and Master Data"
source: "DMBOK v2, Chapter 9"
tags: [master-data, data-management, dmbok, reference-data, mdm]
created: "2026-07-09"
---

# Reference and Master Data

> **Definition:** Managing shared data to meet organizational goals, reduce risks associated with data redundancy, ensure higher quality, and reduce the costs of data integration.

## Goals

1. Enable sharing of information assets across business domains and applications within an organization.
2. Provide authoritative source of reconciled and quality-assessed master and reference data.
3. Lower cost and complexity through use of standards, common data models, and integration patterns.

---

## 1. Introduction

In any organization, certain data is required across business areas, processes, and systems. The overall organization and its customers benefit if this data is shared — customer lists, geographic location codes, business unit lists, delivery options, part lists, accounting cost center codes, governmental tax codes, and other data used to run the business.

Systems and data often evolve organically, particularly through mergers and acquisitions, resulting in multiple systems executing essentially the same functions but isolated from each other. These conditions lead to inconsistencies in data structure and values, increasing costs and risks. Both can be reduced through the management of **Master Data** and **Reference Data**.

### 1.1 Business Drivers

The most common drivers for initiating a Master Data Management program:

- **Meeting organizational data requirements:** Multiple areas need access to the same data sets with confidence they are complete, current, and consistent.
- **Managing data quality:** Inconsistencies and gaps lead to incorrect decisions or lost opportunities; MDM enables a consistent representation of critical entities.
- **Managing the costs of data integration:** MDM reduces variation in how critical entities are defined and identified, lowering integration costs.
- **Reducing risk:** MDM simplifies data sharing architecture to reduce costs and risk.

Drivers for managing Reference Data are similar: consistent Reference Data reduces integration risks and costs, and centrally managed Reference Data improves quality.

### 1.2 Goals and Principles

**Program goals:**
- Ensure complete, consistent, current, authoritative Master and Reference Data across organizational processes
- Enable Master and Reference Data to be shared across enterprise functions and applications
- Lower cost and complexity through standards, common data models, and integration patterns

**Guiding principles:**
- **Shared Data:** Reference and Master Data must be shareable across the organization.
- **Ownership:** They belong to the organization — not to a particular application or department. Require high-level stewardship.
- **Quality:** Require ongoing **Data Quality** monitoring and governance.
- **Stewardship:** **Business Data Stewards** are accountable for controlling and ensuring quality.
- **Controlled Change:** Changes to values should follow a defined, approved process. Identifier merges/splits must be reversible.
- **Authority:** Values should be replicated only from the **System of Record**.

---

## 2. Essential Concepts

### 2.1 Differences Between Master and Reference Data

Malcolm Chisholm proposed a six-layer taxonomy of data: Metadata, Reference Data, Enterprise Structure Data, Transaction Structure Data, Transaction Activity Data, and Transaction Audit Data. Master Data is an aggregation of Reference Data, Enterprise Structure Data, and Transaction Structure Data:

| Data Type | Description |
|-----------|-------------|
| **Reference Data** | Code and description tables; characterizes other data or relates it to external information |
| **Enterprise Structure Data** | Chart of accounts; enables reporting by business responsibility |
| **Transaction Structure Data** | Customer identifiers, products, vendors; things that must be present for transactions |

**Key differences in management focus:**

| | Master Data Management (MDM) | Reference Data Management (RDM) |
|---|---|---|
| **Focus** | Control over values and identifiers for business entities | Control over defined domain values and definitions |
| **Challenge** | Entity resolution (identity management) | Ownership and responsibility for definition/maintenance |
| **Complexity** | Large, volatile data sets | Smaller, less volatile, no matching/merging needed |
| **Goal** | Accurate, current values; reduce ambiguous identifiers | Complete set of accurate, current values for each concept |

### 2.2 Reference Data

Reference Data is any data used to characterize or classify other data, or to relate data to information external to an organization. It exists in virtually every data store — statuses, types, geographic codes, standards information.

Common storage techniques:
- Code tables in relational databases (linked via foreign keys)
- Reference Data Management systems (maintain entities, allowed/deprecated values, term mapping rules)
- Object-attribute specific **Metadata** (permissible values for API/UI access)

#### 2.2.1 Reference Data Structure

**Lists:** The simplest form — pairs a code value with a description. May include additional attributes (definitions, context). Example: Country codes (US → United States of America, GB → United Kingdom).

**Cross-Reference Lists:** Translate between different code sets representing the same concept at different granularities or with different values. Example: USPS State Codes ↔ FIPS State Codes ↔ ISO State Codes. Multi-language lists are a special case.

**Taxonomies:** Capture information at different levels of specificity in hierarchies. Enable content classification and multi-faceted navigation for **Business Intelligence**. Examples: UNSPSC (product classification), NAICS (industry classification). Stored with recursive parent-child relationships.

**Ontologies:** Used to manage website content and characterize data beyond organizational boundaries. Similar management requirements to Reference Data. Primary use case: content management (see **Chapter 9**).

#### 2.2.2 Types of Reference Data

| Type | Description | Example |
|------|-------------|---------|
| **Proprietary/Internal** | Created by the organization for internal processes | Account status codes, department lists |
| **Industry Standard** | Created/maintained by industry associations or government bodies | ICD codes (healthcare), ISO country codes |
| **Geographic/Geo-statistical** | Enables classification by geography | Census data, weather history, postal codes |
| **Computational** | Common consistent calculations; changes frequently | Foreign exchange rates, tax tables |

#### 2.2.3 Reference Data Set Metadata

Critical Metadata attributes for managing Reference Data sets:

| Attribute | Description |
|-----------|-------------|
| Formal Name | Official name (e.g., ISO 3166-1991 Country Code List) |
| Internal Name | Organization's name for the data set |
| Data Provider | External (ISO), internal, or external-extended |
| Data Provider Source | URI for obtaining data |
| Latest Version Number | External provider's latest version |
| Internal Version Number | Current internal version |
| Reconciliation Date | When last updated from external source |
| Last Update Date | When data set was last changed |

### 2.3 Master Data

Master Data is data about business entities (employees, customers, products, financial structures, assets, locations) that provide context for business transactions and analysis.

Common organizational Master Data:
- **Parties:** Individuals and organizations, and their roles (customers, citizens, patients, vendors, suppliers, employees, students)
- **Products and Services:** Internal and external
- **Financial structures:** Contracts, general ledger accounts, cost centers, profit centers
- **Locations:** Addresses and GPS coordinates

#### 2.3.1 System of Record vs. System of Reference

| Term | Definition |
|------|------------|
| **System of Record (SOR)** | Authoritative system where data is created/captured and/or maintained (e.g., ERP for sell-to customers) |
| **System of Reference** | Authoritative system where consumers obtain reliable data — even if data didn't originate there (MDM hubs, Data Warehouses) |

#### 2.3.2 Trusted Source and Golden Record

- **Trusted Source:** The "best version of the truth" based on automated rules and manual stewardship. Also called Single View or 360° View.
- **Golden Record:** Records within a trusted source representing the most accurate data about entity instances.

> ⚠️ **Caution:** The term "Golden Record" can be misleading. Data from multiple sources rarely aligns perfectly into "a single version of truth." Finance and Marketing often have different perspectives on "Customer." The term "Trusted Source" better emphasizes how data is defined and managed to achieve the best version.

#### 2.3.3 Master Data Management (MDM)

Gartner defines MDM as: *"A technology-enabled discipline in which business and IT work together to ensure the uniformity, accuracy, stewardship, semantic consistency, and accountability of the enterprise's official shared Master Data assets."*

MDM is a **discipline** (people + processes + technology), not a specific application. Using an MDM application does not guarantee Master Data is being managed properly.

**Assessing MDM requirements** includes identifying:
- Which roles, organizations, places, and things are referenced repeatedly
- What data describes them
- How data is defined and structured (including granularity)
- Where data is created, stored, made available, and accessed
- How data changes as it moves through systems
- Who uses the data and for what purposes
- What criteria define quality and reliability

**Best practice:** Approach MDM one data domain at a time. Start small, with a handful of attributes, and build out.

**MDM Planning Steps (within a domain):**
1. Identify candidate sources for comprehensive view of entities
2. Develop rules for accurately matching and merging entity instances
3. Establish approach to identify and restore inappropriately matched/merged data
4. Establish approach to distribute trusted data to systems across the enterprise

**MDM Lifecycle Activities:**
- Establishing context of Master Data entities (definitions, attributes, conditions of use) → requires governance
- Identifying multiple instances of the same entity across data sources; building identifiers and cross-references
- Reconciling and consolidating data to provide a master record ("best version of truth")
- Identifying and resolving improperly matched or merged instances
- Provisioning access to trusted data (direct reads, data services, replication)
- Enforcing use of Master Data values → requires governance and change management

#### 2.3.4 MDM Key Processing Steps

The five key processing steps for MDM (Figure 76):

```
Data Model Management → Data Acquisition → Data Validation, Standardization & Enrichment → Entity Resolution → Data Sharing & Stewardship
```

##### 2.3.4.1 Data Model Management

The model helps overcome "system speak" — terms that make sense within a source system may not make sense at the enterprise level. Enterprise-level definitions must be in the context of business conducted across the organization. Source systems may present identical attribute names with different data values, or different attribute names that coalesce to a single enterprise attribute.

##### 2.3.4.2 Data Acquisition

Incorporating new data sources must be a reliable, repeatable process:
- Receive and respond to new source acquisition requests
- Perform rapid match and data quality assessments using profiling/cleansing tools
- Assess and communicate integration complexity for cost-benefit analysis
- Pilot acquisition and assess impact on match rules
- Finalize data quality metrics for new sources
- Determine responsibility for ongoing quality monitoring
- Complete integration into the data management environment

##### 2.3.4.3 Data Validation, Standardization, and Enrichment

To enable entity resolution, data must be made consistent:

- **Validation:** Identifying provably erroneous or likely incorrect data (e.g., fake email addresses)
- **Standardization:** Ensuring data conforms to standard Reference Data values (country codes), formats (telephone numbers), or fields (addresses)
- **Enrichment:** Adding attributes to improve entity resolution (e.g., D&B DUNS Number, Acxiom/Experian Consumer IDs)

##### 2.3.4.4 Entity Resolution and Identifier Management

Entity resolution determines whether two references to real-world objects refer to the same object. It is a decision-making process critical to MDM — matching and merging records enables construction of the Master Data set.

Activities: reference extraction → reference preparation → reference resolution → identity management → relationship analysis.

###### Matching (Candidate Identification)

Risks:
- **False positives:** Two references that do NOT represent the same entity are linked → one identifier refers to multiple entities.
- **False negatives:** Two references representing the same entity are NOT linked → multiple identifiers for one entity.

Both addressed through **similarity analysis** — scoring the degree of similarity between records using weighted approximate matching. If score exceeds threshold → match.

**Two matching approaches:**

| Approach | Method | Characteristics |
|----------|--------|-----------------|
| **Deterministic** | Defined patterns and rules for weights/scores | Predictable, always same results; only as good as the rules |
| **Probabilistic** | Statistical techniques; training on sample data | Self-adjusting; improves precision with more data; non-deterministic |

###### Identity Resolution

Matches vary in confidence. Examples of ambiguity:
- Same last name, first name, birth date, SSN — but different street address → changed mailing address?
- Same SSN, street address, first name — but different last name → changed name? (Depends on gender and age?)

Match history must be maintained so matches can be undone if discovered incorrect. Match rate metrics monitor effectiveness.

###### Matching Workflows / Reconciliation Types

| Workflow | Description | Complexity |
|----------|-------------|------------|
| **Duplicate Identification** | Identify merge opportunities without automatic action; stewards review case-by-case | Low |
| **Match-Link** | Cross-reference records to a master without updating content; easy to reverse | Medium |
| **Match-Merge** | Merge data into a single, unified, reconciled record across sources; complex field-level trust rules | High |

Match-merge is complex because it must identify which field from which source is trusted. New sources can change trust rules. Match-link is simpler — it operates on the cross-reference registry, not individual attributes. Periodically re-evaluate rules as confidence levels change.

###### Master Data ID Management

Two types of identifiers:
- **Global ID:** MDM-assigned unique identifier attached to reconciled records. Only one authorized solution should generate Global IDs (can be numbers or GUIDs).
- **Cross-Reference (X-Ref):** Maps source IDs to Global IDs. Must maintain history for match rate metrics and expose lookup services for data integration.

###### Affiliation Management

Establishing and maintaining relationships between Master Data records (e.g., Company X is a subsidiary of Company Y; Person XYZ works at Company X).

Two relationship approaches:
- **Affiliation relationships:** Greatest flexibility through programming logic; can expose parent-child hierarchies.
- **Parent-Child relationships:** Less programming logic needed; navigation structure is implied. But if relationships change without affiliation structure, data quality and BI dimensions suffer.

##### 2.3.4.5 Data Sharing and Stewardship

Much MDM work can be automated, but stewardship is required to resolve incorrectly matched data. Lessons learned from stewardship should improve matching algorithms and reduce manual work.

#### 2.3.5 Master Data Domains

##### Party Master Data

Data about individuals, organizations, and their roles in business relationships. Varies by sector:

| Sector | Parties |
|--------|---------|
| Commercial | Customers, employees, vendors, partners, competitors |
| Public Sector | Citizens |
| Law Enforcement | Suspects, witnesses, victims |
| Non-profit | Members, donors |
| Healthcare | Patients, providers |
| Education | Students, faculty |

**Customer Relationship Management** systems manage customer Master Data. The goal: complete and accurate information about each customer, with robust rules for identifying duplicates, resolving conflicts, and reconciling differences.

**Unique challenges of Party Master Data:**
- Complexity of roles and relationships
- Difficulties in unique identification
- Number of data sources and differences between them
- Multiple mobile and social communications channels
- Importance and sensitivity of the data
- Customer engagement expectations

##### Financial Master Data

Data about business units, cost centers, profit centers, general ledger accounts, budgets, projections, and projects. **ERP** systems typically serve as the central hub (chart of accounts). Financial MDM solutions can simulate how structural changes affect the bottom line — modeling versions of financial structures to understand potential impacts before disseminating changes.

##### Legal Master Data

Data about contracts, regulations, and other legal matters. Enables analysis of contracts across entities for better negotiation or combining into Master Agreements.

##### Product Master Data

Focuses on internal products/services or industry-wide (including competitor) products. Different solutions for different functions:

| System | Focus |
|--------|-------|
| **PLM** (Product Lifecycle Management) | Lifecycle from conception through development, manufacturing, sale, service, disposal |
| **PDM** (Product Data Management) | Engineering/manufacturing: CAD drawings, recipes, SOPs, bills of materials |
| **ERP** | SKUs for order entry to inventory level |
| **MES** (Manufacturing Execution Systems) | Raw inventory, semi-finished goods, finished goods |
| **CRM** | Product families, brands, sales rep association, territory management, marketing campaigns |

##### Location Master Data

Distinction between Reference and Master Data for location:

| Type | Content | Change Frequency |
|------|---------|------------------|
| **Location Reference Data** | Countries, states, counties, cities, postal codes, GPS coordinates | Rarely changes (external) |
| **Location Master Data** | Business party addresses, facility addresses | More frequent (as organization changes) |

Specialized earth science and sociological data may be needed by industry (seismic faults, flood plains, population demographics, terrorism risk).

##### Industry Master Data – Reference Directories

Authoritative listings of Master Data entities that organizations purchase as a basis for transactions (e.g., D&B Company Directory, AMA Prescriber Database).

Benefits:
- Starting point for matching/linking new records (fewer comparison points)
- Additional data elements not easily available at record creation (medical license status, NAICS classification)

### 2.4 Data Sharing Architecture

Three basic approaches to implementing a Master Data hub environment:

| Approach | Description | Pros | Cons |
|----------|-------------|------|------|
| **Registry** | Index pointing to Master Data in systems of record | Easy to implement; few changes to SORs | Complex queries to assemble data; multiple places for business rules |
| **Transaction Hub** | Master Data exists only in the hub; hub is the SOR | Better governance; consistent source; single system for business rules | Costly to remove update functionality from existing SORs |
| **Consolidated** | Hybrid; SORs manage local data; Master Data consolidated in common repository | Enterprise view; limited SOR impact | Data replication; latency between hub and SORs |

---

## 3. Activities

### 3.1 MDM Activities

#### 3.1.1 Define MDM Drivers and Requirements

Drivers include improved customer service, operational efficiency, and risk reduction (privacy/compliance). Obstacles include semantic differences between systems and cultural barriers. Prioritize based on cost/benefit and complexity. Start with the simplest category.

#### 3.1.2 Evaluate and Assess Data Sources

Understand structure, content, and collection processes. Assess completeness and quality. Semantic issues will arise — **Data Stewards** must collaborate on reconciliation of attribute naming and enterprise-level definitions. Never assume data is high quality. Disparity between sources is the biggest challenge.

For client/customer/vendor entities, standardized data can be purchased from vendors (Reference Directories).

#### 3.1.3 Define Architectural Approach

Depends on business strategy, existing platforms, data lineage, volatility, and latency requirements. Architecture must account for consumption and sharing models. Small organizations may use a transaction hub; global organizations with multiple systems likely use a registry; siloed business units may choose consolidated.

The data sharing hub architecture is particularly useful when no clear SOR exists — new data from one system can be reconciled with data from another.

#### 3.1.4 Model Master Data

MDM is a data integration process. A logical/canonical model over subject areas within the data-sharing hub enables enterprise-level definitions of entities and attributes.

#### 3.1.5 Define Stewardship and Maintenance Processes

Technical solutions handle matching, merging, and identifier management — but stewardship is needed for records that fall out of the process and to remediate root causes. Account for resources to support ongoing quality.

#### 3.1.6 Establish Governance Policies

Real benefits come when people and systems start using Master Data. Include a roadmap for systems to adopt master values. Establish unidirectional closed loops between systems to maintain consistency.

### 3.2 Reference Data Activities

#### 3.2.1 Define Drivers and Requirements

Primary drivers: operational efficiency and higher data quality. Central management is more cost-effective than multiple business units maintaining their own sets. Prioritize most important Reference Data sets first.

#### 3.2.2 Assess Data Sources

Industry standard sets can be obtained from creating organizations (free or paid). Intermediaries package and sell value-added Reference Data. Internal Reference Data sources must be identified, compared, assessed. Owners must understand benefits of central management.

#### 3.2.3 Define Architectural Approach

Account for volatility, update frequency, consumption models, and whether historical data on changes must be kept. The RDM tool should enable stewards to make ad hoc updates without technical support, with automated approval workflows.

#### 3.2.4 Model Reference Data Sets

Beyond simple codes and descriptions — Reference Data often includes complex relationships (e.g., ZIP codes with state/county/geo-political attributes). Models help consumers understand relationships and establish quality rules.

#### 3.2.5 Define Stewardship and Maintenance Processes

Stewards ensure values are complete, current, and clearly defined. Capture Metadata: steward name, originating organization, update frequency, processes using the data, whether historical versions are retained. RDM tools should include review/approval workflows.

#### 3.2.6 Establish Reference Data Governance Policies

Value only comes if people actually use centrally managed Reference Data. Policies must mandate use — whether directly from the repository or indirectly from a system of reference.

---

## 4. Tools and Techniques

MDM requires tooling specifically designed for identity management. Can be implemented through:
- Data integration tools
- Data remediation tools
- Operational Data Stores (ODS)
- Data Sharing Hubs (DSH)
- Specialized MDM applications

Packaged solutions for product, account, and party domains, as well as packaged data quality check services, can jumpstart large programs.

---

## 5. Implementation Guidelines

MDM and RDM are forms of **data integration**. Implementation principles for data integration apply. Solutions require specialized knowledge and should be implemented incrementally through a roadmap prioritized by business needs.

> ⚠️ **MDM programs will fail without proper governance.** (See **Chapter 15**.)

### 5.1 Adhere to Master Data Architecture

The integration approach must account for organizational structure, number of SORs, governance implementation, access/latency importance, and number of consuming systems.

### 5.2 Monitor Data Movement

Data integration processes should ensure timely extraction and distribution. Monitor data flow to:
- Show how data is shared and used
- Identify lineage from/to administrative systems
- Assist root cause analysis
- Show effectiveness of ingestion/consumption techniques
- Denote latency of values from source to consumption
- Determine validity of business rules and transformations

### 5.3 Manage Reference Data Change

Reference Data is a shared resource — it cannot be changed arbitrarily. Key to success: organizational willingness to relinquish local control.

**Types of changes:**
- Row-level changes to external sets
- Structural changes to external sets (e.g., ICD-9 → ICD-10: 13,000 → 68,000 codes, different format and granularity)
- Row-level changes to internal sets
- Structural changes to internal sets
- Creation of new Reference Data sets

**Change request process:** Receive Request → Identify Stakeholder → Identify Impact → Decide and Approve → Update and Communicate

### 5.4 Data Sharing Agreements

Require collaboration between multiple parties. Agreements must stipulate what data can be shared and under what conditions. **SLAs** and metrics should measure availability and quality of shared data. Standard communications approach keeps all affected parties informed.

---

## 6. Organization and Cultural Change

Reference and Master Data Management require people to relinquish control of their data and processes to create shared resources. While data management professionals see the risk of locally managed data, practitioners may perceive MDM/RDM as adding complication.

Fortunately, most people recognize the fundamental value: it is better to have one accurate, complete view of a customer than multiple partial views.

**Most challenging cultural change:** Determining who is accountable for which decisions — business Data Stewards, Architects, Managers, Executives — and which decisions governance bodies should make collaboratively.

---

## 7. Reference and Master Data Governance

Because they are shared resources, Reference and Master Data require governance and stewardship. Not all inconsistencies can be resolved through automation — some require conversation. Without governance, MDM/RDM solutions are just additional data integration utilities.

**Governance processes determine:**
- Data sources to be integrated
- Data quality rules to be enforced
- Conditions of use rules
- Activities to monitor and frequency
- Priority and response levels of stewardship efforts
- How information represents stakeholder needs
- Standard approval gates and expectations

Governance also brings compliance and legal stakeholders together with information consumers to ensure risks are mitigated through privacy, security, and retention policies.

### 7.1 Metrics

| Metric | Description |
|--------|-------------|
| **Data Quality and Compliance** | DQ dashboards describing confidence (% fit-for-purpose) of entities and attributes |
| **Data Change Activity** | Rate of change of data values; provides insight to supplying systems; tunes MDM algorithms |
| **Data Ingestion and Consumption** | Which systems contribute data; which business areas subscribe |
| **Service Level Agreements (SLAs)** | Adherence to SLAs; insight into support processes and technical problems |
| **Data Steward Coverage** | Name/group responsible for content; frequency of evaluation; identifies gaps |
| **Total Cost of Ownership** | Infrastructure, licenses, support staff, consulting, training |
| **Data Sharing Volume and Usage** | Volume and velocity of data defined, ingested, and subscribed |

---

## 8. Works Cited / Recommended

- Allen, Mark and Dalton Cervo. *Multi-Domain Master Data Management: Advanced MDM and Data Governance in Practice.* Morgan Kaufmann, 2015.
- Berson, Alex and Larry Dubov. *Master Data Management and Customer Data Integration for a Global Enterprise.* McGraw-Hill, 2007.
- Cervo, Dalton and Mark Allen. *Master Data Management in Practice: Achieving True Customer MDM.* Wiley, 2011.
- Chisholm, Malcolm. *Managing Reference Data in Enterprise Databases.* Morgan Kaufmann, 2000.
- Chisholm, Malcolm. "What is Master Data?" BeyeNetwork, 2008.
- Dreibelbis, Allen, et al. *Enterprise Master Data Management: An SOA Approach to Managing Core Information.* IBM Press, 2008.
- Dyche, Jill and Evan Levy. *Customer Data Integration: Reaching a Single Version of the Truth.* Wiley, 2006.
- Loshin, David. *Master Data Management.* Morgan Kaufmann, 2008.
- Talburt, John. *Entity Resolution and Information Quality.* Morgan Kaufmann, 2011.
- Talburt, John and Yinle Zhou. *Entity Information Management Lifecycle for Big Data.* Morgan Kaufmann, 2015.

---

## Related DMBOK Chapters

- **Chapter 2: Data Governance** — stewardship and accountability
- **Chapter 3: Data Architecture** — architectural approaches
- **Chapter 4: Data Modeling and Design** — canonical/logical models
- **Chapter 7: Data Integration and Interoperability** — integration principles
- **Chapter 8: Document and Content Management** — ontologies and taxonomies
- **Chapter 10: Data Warehousing and BI** — data sharing hubs as systems of reference
- **Chapter 11: Metadata Management** — Reference Data metadata
- **Chapter 12: Data Quality** — quality monitoring and metrics
