---
title: "Data Modeling and Design"
source: "DMBoK v2 — Chapter 5"
tags:
  - data-modeling
  - data-management
  - dmbok
  - erd
  - dimensional-modeling
  - normalization
  - database-design
created: 2026-07-09
---

# Data Modeling and Design

> **DMBOK2 Chapter 5.** Data Modeling and Design is the process of discovering, analyzing, representing, and communicating data requirements in a precise form called a **data model**. This chapter covers the essential concepts, activities, tools, best practices, and governance around data modeling — including ERD, dimensional modeling, normalization, abstraction, forward/reverse engineering, and quality measurement.

---

## 1. Essential Concepts

### 1.1 Data Modeling Schemes

A data model can be built following different **schemes** (approaches), each suited to particular use cases:

| Scheme | Description | Best For |
|--------|-------------|----------|
| **Relational** | Data organized into relations (tables) with rows and columns; uses keys and joins. Based on set theory and relational algebra. | OLTP, operational systems, most general-purpose applications. |
| **Dimensional** | Facts (measures) surrounded by dimensions (context); star/snowflake schemas. Denormalized for query performance. | Data warehousing, BI, OLAP, reporting. |
| **Object-Oriented** | Data modeled as objects with attributes and methods; supports inheritance, encapsulation. | CAD/CAM, embedded real-time systems, multimedia. |
| **Fact-Based / ORM** | Models facts as relationships between objects using natural language; no attributes — everything is a relationship. Object-Role Modeling (ORM). | Conceptual modeling, business rule capture, terminology alignment. |
| **Time-Based / Temporal** | Data vault, anchor modeling; tracks data changes over time with timestamps. | Data warehousing with history tracking, audit trails. |
| **NoSQL** | Schema-on-read; document, key-value, graph, column-family, triplestore. | Big Data, real-time web apps, unstructured/semi-structured data. |

### 1.2 Data Model Levels

Data models exist at three levels of abstraction:

```
Conceptual Data Model (CDM)
        │
        ▼
 Logical Data Model (LDM)
        │
        ▼
Physical Data Model (PDM)
```

| Level | Focus | Audience | Contains |
|-------|-------|----------|----------|
| **Conceptual (CDM)** | Business scope and terminology | Business stakeholders, executives | High-level entities/concepts (nouns) and relationships (verbs). No attributes, no keys. |
| **Logical (LDM)** | Business solution detail | Business analysts, data architects | All entities, attributes, keys, relationships, normalization. Independent of technology. |
| **Physical (PDM)** | Technical implementation | DBAs, developers | Tables, columns, data types, indexes, partitions, constraints, denormalization. Tied to a specific DBMS. |

### 1.3 Entity-Relationship Diagrams (ERD)

An **ERD** (Entity-Relationship Diagram) is the most common notation for data models. Core components:

- **Entity:** A thing of significance to the business about which data must be held (e.g., Customer, Order, Product). Represented as a rectangle. In physical models, becomes a **table**.
- **Attribute:** A property or characteristic of an entity (e.g., Customer Name, Order Date). In physical models, becomes a **column**.
- **Relationship:** An association between two entities (e.g., Customer *places* Order). Represented as a line.
- **Cardinality:** The number of instances on each side of a relationship — one-to-one (1:1), one-to-many (1:M), many-to-many (M:N).
- **Primary Key (PK):** An attribute or set of attributes that uniquely identifies each entity instance.
- **Foreign Key (FK):** An attribute in one entity that references the primary key of another entity.
- **Alternate Key (AK):** A candidate key not chosen as the primary key.

Common ERD notations include **Information Engineering (IE/Crow's Foot)**, **IDEF1X**, **Bachman**, **UML Class Diagrams**, and **ORM**.

### 1.4 Dimensional Modeling

**Dimensional modeling** is the predominant scheme for data warehousing and business intelligence. It optimizes for query performance and user understandability.

- **Fact Table:** Contains quantitative measures/metrics (e.g., Sales Amount, Quantity). Usually the largest tables. Each row represents a business event/transaction. Associative entities from logical models typically become fact tables.
- **Dimension Table:** Contains descriptive attributes that provide context for facts (e.g., Customer, Product, Time, Geography). Usually smaller than fact tables. Highly denormalized.
- **Star Schema:** A central fact table surrounded by dimension tables directly joined to it. Simplest and most performant.
- **Snowflake Schema:** Dimensions are normalized into multiple related tables. More complex but saves storage.
- **Granularity:** The level of detail represented by a single row in the fact table (e.g., daily sales per store vs. hourly sales per register).
- **Surrogate Keys:** Dimension tables typically use meaningless integer surrogate keys rather than natural business keys.
- **Slowly Changing Dimensions (SCD):** Strategies for handling changes to dimension attributes over time (Type 0–6).

### 1.5 Normalization

Normalization is the process of organizing data to reduce redundancy and improve data integrity. Each successive normal form builds on the previous ones.

| Normal Form | Rule | Purpose |
|-------------|------|---------|
| **1NF** | Each entity has a valid primary key; every attribute depends on the PK; no repeating groups; all attributes atomic. Resolves many-to-many relationships via associative entities. | Eliminate repeating groups |
| **2NF** | Each entity has the minimal primary key; every non-key attribute depends on the *complete* primary key (no partial dependencies). | Eliminate partial dependencies |
| **3NF** | No hidden primary keys; each non-key attribute depends on "the key, the whole key and nothing but the key" (no transitive dependencies). | Eliminate transitive dependencies |
| **BCNF** | Resolves overlapping composite candidate keys. A candidate key is a primary or alternate key. "Composite" means two or more attributes; "overlapping" means hidden business rules between the keys. | Handle overlapping candidate keys |
| **4NF** | Resolves all many-to-many-to-many relationships in pairs until they cannot be broken down further. | Eliminate multi-valued dependencies |
| **5NF** | Resolves inter-entity dependencies into basic pairs; all join dependencies use parts of primary keys. | Eliminate join dependencies |

> **In practice, "normalized" typically means 3NF.** BCNF, 4NF, and 5NF are needed only in rare situations.

**Denormalization** is the deliberate introduction of redundancy for performance gains — common in dimensional models and read-heavy workloads. The benefit of improved query performance must outweigh the cost of duplicate storage and synchronization overhead.

### 1.6 Abstraction

Abstraction removes details to broaden applicability while preserving essential properties. The classic example is the **Party/Role** structure, capturing how people and organizations play roles (e.g., Employee, Customer).

- **Generalization:** Grouping common attributes and relationships into **supertype** entities.
- **Specialization:** Separating distinguishing attributes into **subtype** entities. Subtypes inherit all properties from the supertype.

**Example:** `Party` (supertype) → `Individual` and `Organization` (subtypes). `School` (supertype) → `University` and `High School` (subtypes).

Trade-off: Abstract structures are more flexible and extensible but cost more to develop and maintain upfront. The modeler must weigh this against the cost of future rework on a non-abstracted structure.

### 1.7 Domains

A **domain** is a named set of valid values and formats for an attribute. Using domains ensures consistency within and across projects:

- **Data type domain:** Standard formats (e.g., `Amount`, `Date`, `PhoneNumber`).
- **Data value domain:** Allowable values (e.g., valid country codes, status codes).
- **Reference data tables:** Small lookup tables for code/value pairs.

`Student Tuition Amount` and `Instructor Salary Amount` can both be assigned the `Amount` domain, ensuring consistent currency formatting.

---

## 2. Activities

### 2.1 Plan for Data Modeling

A data modeling plan includes:
- Evaluating organizational requirements
- Creating modeling standards (naming conventions, notation, tool usage)
- Determining data model storage and versioning

**Deliverables:**
- **Diagram:** Visual representation capturing requirements in precise form (CDM, LDM, or PDM; using a chosen scheme and notation).
- **Definitions:** Clear definitions for all entities, attributes, and relationships.
- **Issues and Outstanding Questions:** Document of unresolved questions (e.g., "If a Student leaves and returns, are they assigned a new Student Number?").
- **Lineage:** Source-to-target mappings showing where data comes from. Validates model accuracy and provides impact analysis capability.

### 2.2 Build the Data Model

Modeling is **highly iterative**. Modelers draft, clarify with business professionals, update, and ask more questions. The cycle: **Elicit → Document → Ask more questions → Increase precision → repeat**.

#### 2.2.1 Forward Engineering

Forward engineering builds a new application from requirements: CDM → LDM → PDM.

##### 2.2.1.1 Conceptual Data Modeling (CDM)

1. **Select Scheme:** Relational, dimensional, fact-based, or NoSQL — depends on the use case.
2. **Select Notation:** IE/Crow's Foot, IDEF1X, UML, ORM — depends on organizational standards and audience familiarity.
3. **Complete Initial CDM:** Capture the viewpoint of a user group without trying to reconcile enterprise-wide. Collect highest-level nouns (Time, Geography, Customer, Product, Transaction) and verbs (relationships).
4. **Incorporate Enterprise Terminology:** Align the model with enterprise standards (e.g., reconcile "Client" → "Customer").
5. **Obtain Sign-off:** Review for best practices and requirements coverage.

##### 2.2.1.2 Logical Data Modeling (LDM)

1. **Analyze Information Requirements:** Elicit, organize, document, review, refine, and approve business data requirements. Maintain a balanced view of data (nouns) and processes (verbs).
2. **Analyze Existing Documentation:** Leverage existing data models, databases, industry data models, and reusable patterns. Validate with SMEs.
3. **Add Associative Entities:** Resolve M:N relationships with associative entities that carry relationship-specific attributes (effective dates, expiration dates). These become fact tables in dimensional models or nodes in graph databases.
4. **Add Attributes:** Each attribute must be atomic — one piece of data that cannot be divided. Example: `Phone Number` → `Phone Type Code`, `Country Code`, `Area Code`, `Prefix`, `Base Number`, `Extension`.
5. **Assign Domains:** Apply consistent formats and value sets across entities.
6. **Assign Keys:** Distinguish key attributes (identify unique instances) from non-key attributes (describe instances). Identify primary and alternate keys.

##### 2.2.1.3 Physical Data Modeling (PDM)

1. **Resolve Logical Abstractions:** Supertypes/subtypes become physical objects via:
   - **Subtype Absorption:** Subtype attributes become nullable columns in the supertype table.
   - **Supertype Partition:** Supertype attributes are duplicated into separate tables for each subtype.
2. **Add Attribute Details:** Technical names, physical data types, lengths, nullability, default values, constraints.
3. **Add Reference Data Objects:** Small reference value sets can be implemented as:
   - Separate code tables (many small tables)
   - Master shared code table (one consolidated table — watch for code collisions)
   - Embedded constraints/rules in the object definition
4. **Assign Surrogate Keys:** Meaningless, business-invisible unique identifiers. Optional — used when the natural key is large, composite, or has values that change over time. **Always define an alternate key on the original natural key.**
5. **Denormalize for Performance:** Add controlled redundancy where the query performance gain outweighs storage and sync costs. Dimensional structures are the primary means.
6. **Index for Performance:** Alternate access paths to optimize query performance. Types: unique/non-unique, clustered/non-clustered, partitioned/non-partitioned, single-column/multi-column, B-tree/bitmap/hash. Index on large tables using frequently referenced columns (keys, especially).
7. **Partition for Performance:** Strategically split data, especially when facts have sparse dimensional keys. Date-key partitioning is recommended.
8. **Create Views:** Control access to data elements, embed common joins/filters, standardize common objects/queries. Views should be requirements-driven.

#### 2.2.2 Reverse Engineering

Reverse engineering documents an existing database: PDM → LDM → CDM.

- Data modeling tools can reverse-engineer from a variety of databases.
- Creating a readable layout still requires a modeler — contextual organization (grouping by subject area/function) is largely manual.
- Common layouts: orthogonal, dimensional, hierarchical.

### 2.3 Review the Data Models

Quality control is essential. Review models for **correctness, completeness, and consistency**. Techniques include:
- Time-to-value analysis
- Support cost evaluation
- Data Model Scorecard® assessment (see §5.2)

### 2.4 Maintain the Data Models

- Keep models current as requirements and business processes change.
- When one level changes, corresponding higher levels often need updating (e.g., new PDM column → new LDM attribute).
- **Best practice:** At the end of each development iteration, reverse-engineer the latest physical model and verify consistency with the logical model. Many tools automate this comparison.

---

## 3. Tools

| Tool Category | Purpose | Examples |
|---------------|---------|----------|
| **Data Modeling Tools** | Create entities, relationships, diagrams; forward engineer (CDM→LDM→PDM→DDL); reverse engineer; naming standards validation; Metadata storage; web publishing. | ER/Studio, ERwin, PowerDesigner, Oracle SQL Developer Data Modeler |
| **Lineage Tools** | Capture and maintain source-to-target mappings for impact analysis. | Excel (commonly used but limited), data modeling tools, Metadata repositories, data integration tools |
| **Data Profiling Tools** | Explore data content, validate against Metadata, identify data quality gaps and anomalies. | See Ch.8 (Data Quality) and Ch.13 (Metadata) |
| **Metadata Repositories** | Store descriptive information about models, diagrams, definitions, plus imported Metadata from other tools/processes. Must be easily navigable. | Data modeling tool repositories, enterprise Metadata repositories |
| **Data Model Patterns** | Reusable modeling structures: elementary (M:N resolution, self-referencing hierarchies), assembly (assets, documents, people/organizations), integration (linking patterns in common ways). | Giles (2011), Silverston (2001–2008), Hay (2006, 2011) |
| **Industry Data Models** | Pre-built models for entire industries (healthcare, telecom, insurance, banking, manufacturing, retail). Thousands of entities/attributes. Must be customized. | ARTS (retail), SID (communications), ACORD (insurance) |

---

## 4. Best Practices

### 4.1 Naming Conventions

Reference standard: **ISO 11179** (Metadata Registry).

- **Logical names:** Meaningful to business users; full words; avoid abbreviations except most familiar ones. Use spaces as word separators.
- **Physical names:** Conform to DBMS maximum length limits; use abbreviations where necessary. Use underscores as word separators.
- **Environment independence:** Names should not reflect their environment (test, QA, production).
- **Class words:** The last term in an attribute name (e.g., `Quantity`, `Name`, `Code`). Distinguish attributes from entities, and quantitative from qualitative attributes.
- Names must be **unique** and **as descriptive as possible**.
- Publish and enforce naming standards for all object types: entities, tables, attributes, keys, views, indexes.

### 4.2 Database Design Principles — **PRISM**

| Principle | Description |
|-----------|-------------|
| **Performance & Ease of Use** | Quick, easy access to data for approved users in a usable, business-relevant form. Maximize business value of applications and data. |
| **Reusability** | Multiple applications can use the same data; data serves multiple purposes (analysis, quality improvement, strategic planning, CRM). Avoid coupling a database to a single application. |
| **Integrity** | Data always has valid business meaning and reflects a valid business state. Enforce integrity constraints as close to the data as possible. Detect and report violations immediately. |
| **Security** | True and accurate data available to authorized users only. Meet privacy concerns of all stakeholders. Enforce security as close to the data as possible. |
| **Maintainability** | Cost of creating, storing, maintaining, using, and disposing of data does not exceed its value. Fastest possible response to changes in business requirements. |

---

## 5. Data Model Governance

### 5.1 Data Model and Design Quality Management

Data analysts and designers must balance:
- **Short-term needs:** Timely data delivery for business obligations and opportunities; project time/budget constraints.
- **Long-term needs:** Secure, recoverable, sharable, reusable, correct, timely, relevant, and usable data.

#### 5.1.1 Develop Data Modeling and Design Standards

Standards should include:
- List and description of standard deliverables
- Standard names, acceptable abbreviations, and abbreviation rules
- Standard naming formats for all object types (including class words)
- Standard methods for creating and maintaining deliverables
- Roles and responsibilities
- Metadata properties to capture (business and technical), including lineage
- Metadata quality expectations
- Guidelines for using data modeling tools
- Guidelines for design reviews
- Guidelines for versioning data models
- Discouraged practices

#### 5.1.2 Review Data Model and Database Design Quality

- Conduct reviews of CDM, LDM, and PDM.
- Agenda: starting model (if any), changes made, rejected alternatives, conformance to standards.
- Include SMEs with diverse backgrounds, skills, and opinions.
- Chair with a leader who facilitates; use a scribe to capture discussion.
- If there's no approval, the modeler must rework; unresolved issues go to the system owner.

#### 5.1.3 Manage Data Model Versioning and Integration

Each change should document:
- **Why** the change was needed
- **What/How** the objects changed
- **When** the change was approved and made
- **Who** made the change
- **Where** the change was made (which models)

Use data modeling tool repositories or standard source code management (DDL exports, XML files, check-in/check-out like application code).

### 5.2 Data Modeling Metrics — The Data Model Scorecard®

The Data Model Scorecard® (Hoberman, 2015) provides 11 quality metrics across 10 categories:

| # | Category | Max Score |
|---|----------|-----------|
| 1 | How well does the model capture the requirements? | 15 |
| 2 | How complete is the model? (requirements + Metadata) | 15 |
| 3 | How well does the model match its scheme? | 10 |
| 4 | How structurally sound is the model? | 15 |
| 5 | How well does the model leverage generic structures? | 10 |
| 6 | How well does the model follow naming standards? | 5 |
| 7 | How well has the model been arranged for readability? | 5 |
| 8 | How good are the definitions? | 10 |
| 9 | How consistent is the model with the enterprise? | 5 |
| 10 | How well does the Metadata match the data? | 10 |
| **Total** | | **100** |

---

## 6. Related DMBOK Chapters

- [[01_Data_Governance|Data Governance]] — Policies, standards, and stewardship for data models.
- [[02_Data_Architecture|Data Architecture]] — Enterprise data architecture that data models must conform to.
- [[04_Data_Storage_and_Operations|Data Storage and Operations]] — Physical implementation and database administration.
- [[05_Data_Security|Data Security]] — Security constraints on data models and databases.
- [[06_Data_Integration_and_Interoperability|Data Integration & Interoperability]] — Source-to-target mappings, ETL/ELT, data lineage.
- [[07_Document_and_Content_Management|Document & Content Management]] — Unstructured and semi-structured data modeling.
- **Data Warehousing & BI** — Dimensional modeling, star schemas, OLAP.
- **Metadata Management** — Metadata repositories, data dictionaries, model definitions.
- **Data Quality** — Data profiling, quality validation against models.
- **Reference & Master Data** — Reference data tables, master data entities.

---

## 7. Key References

- Hoberman, Steve. *Data Model Scorecard®.* Technics Publications, 2015.
- Hoberman, Steve. *Data Modeling Made Simple.* 2nd ed. Technics Publications, 2009.
- Hoberman, Steve. *Data Modeling Master Class Training Manual.* 7th ed. Technics Publications, 2017.
- Giles, John. *The Nimble Elephant: Agile Delivery of Data Models using a Pattern-based Approach.* Technics Publications, 2012.
- Silverston, Len. *The Data Model Resource Book* (Vols. 1–3). Wiley, 2001–2008.
- Hay, David C. *Data Model Patterns: A Metadata Map.* Morgan Kaufmann, 2006.
- Simsion, Graeme & Graham Witt. *Data Modeling Essentials.* 3rd ed. Morgan Kaufmann, 2004.
- Date, C.J. *An Introduction to Database Systems.* 8th ed. Addison-Wesley, 2003.
- ISO 11179 — Metadata Registry (naming standards reference).
