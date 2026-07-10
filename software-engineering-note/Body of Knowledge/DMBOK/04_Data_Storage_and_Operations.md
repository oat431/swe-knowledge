---
title: "Data Storage and Operations"
chapter: 6
source: "DMBoK v2"
tags: [data-storage, data-management, dmbok, database-administration, operations]
created: 2026-07-09
---

# Data Storage and Operations

> **DMBOK2 Chapter 6** — The design, implementation, and support of stored data to maximize its value throughout its lifecycle, from creation/acquisition to disposal.

## Overview

**Data Storage and Operations** includes the design, implementation, and support of stored data, to maximize its value throughout its lifecycle. It comprises two sub-activities:

1. **Database support** — Activities related to the data lifecycle: initial implementation, obtaining, backing up, and purging data, plus ensuring database performance through monitoring and tuning.
2. **Database technology support** — Defining technical requirements, technical architecture, installing and administering technology, and resolving technology-related issues.

**Database Administrators** (DBAs) play key roles in both aspects. The DBA role is the most established and widely adopted data professional role.

### Goals

1. Manage the availability of data throughout the data lifecycle
2. Ensure the integrity of data assets
3. Manage the performance of data transactions

### Guiding Principles

- **Identify and act on automation opportunities** — Automate database development processes to shorten cycles, reduce errors, and minimize impact on development teams
- **Build with reuse in mind** — Develop abstracted, reusable data objects that prevent tight coupling to database schemas
- **Understand and appropriately apply best practices** — Promote standards as requirements but be flexible when deviations are justified
- **Connect database standards to support requirements** — SLAs should reflect DBA-recommended methods
- **Set expectations for the DBA role in project work** — Onboard DBAs in the project definition phase

---

## 1. Essential Concepts

### 1.1 Database Terms

| Term | Definition |
|------|-----------|
| **Database** | Any collection of stored data, regardless of structure or content |
| **Instance** | An execution of database software controlling access to a specific area of storage |
| **Schema** | A subset of database objects within a database/instance, used to organize objects and isolate sensitive data |
| **Node** | An individual computer hosting processing or data as part of a distributed database |
| **Database abstraction** | A common API (e.g., ODBC) that allows applications to connect to multiple databases without knowing all function calls |

### 1.2 Data Lifecycle Management

DBAs maintain and assure the accuracy and consistency of data over its entire lifecycle through design, implementation, and usage. Data lifecycle management includes policies and procedures for:

- Acquisition
- Migration
- Retention
- Expiration
- Disposition

> DBAs should use a controlled, documented, and auditable process for moving application database changes to QA and Production environments. Always have a back-out plan.

### 1.3 Administrator Roles

#### Production DBA
- Ensures database performance and reliability (tuning, monitoring, error reporting)
- Implements backup and recovery mechanisms
- Implements clustering and failover for high availability
- Manages archiving mechanisms
- Creates production database environments with appropriate sizing, security, and availability

#### Application DBA
- Responsible for one or more databases across all environments (dev/test, QA, production)
- Viewed as integral member of application support team
- Collaborates closely with data analysts, modelers, and architects

#### Procedural and Development DBA
- **Procedural DBA**: Leads review and administration of procedural database objects (stored procedures, triggers, UDFs)
- **Development DBA**: Focuses on data design activities, sandbox environments, and exploration areas

#### Network Storage Administrator (NSA)
- Concerned with hardware and software supporting data storage arrays
- Manages network storage array systems separately from database applications

### 1.4 Database Architecture Types

#### Centralized Databases
All data resides in one system at one location. Simple but risky — a single point of failure.

#### Distributed Databases
Data spread across multiple nodes for quick access. Designed to scale out using commodity hardware. The software replicates data among servers for high availability and handles node failures automatically.

**MapReduce**: Divides data requests into small fragments executed on any node, with data co-located on compute nodes.

#### Federated Databases
Maps multiple autonomous database systems into a single federated database without additional persistence or duplication. Constituent databases remain autonomous but participate in a federation for controlled data sharing.

- **Loosely coupled**: Component databases construct their own federated schema
- **Tightly coupled**: Independent processes construct and publish an integrated federated schema

#### Blockchain Databases
A type of federated database for secure transaction management. Uses chains of time-bound transaction groups (blocks) with hash algorithms. Once a block is created, its hash should never change — tampering is detectable.

#### Virtualization / Cloud Platforms

| Method | Description |
|--------|-------------|
| **Virtual Machine Image** | Run a database on purchased VM instances |
| **Database-as-a-Service (DaaS)** | Database service provider handles installation/maintenance |
| **Managed Database Hosting** | Cloud provider hosts and manages the database on behalf of the owner |

Key virtualization strategies:
- **Standardization/Consolidation**: Reduce number of data storage locations
- **Server Virtualization**: Replace/consolidate equipment across data centers
- **Automation**: Automate provisioning, configuration, patching, release management, compliance

### 1.5 Database Processing Types

#### ACID (Relational)
| Property | Description |
|----------|-------------|
| **Atomicity** | All operations performed, or none |
| **Consistency** | Transaction meets all system-defined rules |
| **Isolation** | Each transaction is independent |
| **Durability** | Once complete, transaction cannot be undone |

#### BASE (Non-relational / Big Data)
| Property | Description |
|----------|-------------|
| **Basically Available** | System guarantees some availability even with node failures |
| **Soft State** | Data in constant flux; responses not guaranteed current |
| **Eventual Consistency** | Data will eventually be consistent across all nodes |

#### CAP Theorem (Brewer's Theorem)
A distributed system cannot simultaneously guarantee all three:
- **Consistency** — System operates as expected at all times
- **Availability** — System responds to each request
- **Partition Tolerance** — System continues during data loss or partial failure

> At most two of the three properties can exist in any shared-data system. This drives architectures like Lambda Architecture (speed path: availability + partition tolerance; batch path: consistency + availability).

### 1.6 Data Storage Media

- **Disk and SAN**: Stable persistent storage; data moved on backplane without network
- **In-Memory (IMDB)**: Loaded into volatile memory for faster response; higher investment required
- **Columnar Compression**: Handles highly repetitive data sets, reducing I/O bandwidth
- **Flash Memory / SSD**: Combines memory access speed with disk persistence
- **Cloud-based Storage**: Private, Public, and Hybrid cloud storage solutions

### 1.7 Database Environments

| Environment | Purpose | Characteristics |
|-------------|---------|-----------------|
| **Production** | Where all business processes occur | Mission-critical; no development/testing |
| **Development** | Create and test code changes | Slimmer version; less disk/CPU/RAM; first place for patches |
| **Test / QA** | Quality assurance, UAT, performance testing | Should match production software/hardware; especially for performance testing |
| **Sandbox** | Experimentation and proof-of-concept | Read-only to production; isolated; user-managed |

> **Key rule**: Production data must not be moved to non-production environments without determining regulatory restrictions.

### 1.8 Database Organization

```
Hierarchical ──────────── Relational ──────────── Non-Relational
(Tree Schema)            (Schema on Write)       (Schema on Read)
More Controlled ◄─────────────────────────────── Less Controlled
```

#### Hierarchical
Oldest model; tree-like structure with mandatory parent/child (1-to-many) relationships. Examples: directory trees, XML.

#### Relational
Based on set theory and relational algebra. Data elements (columns) related into tuples (rows). Uses SQL. Structure must be known in advance (**schema on write**).

- **Multidimensional**: Stores data for simultaneous multi-filter searching (Data Warehousing/BI); uses MDX
- **Temporal**: Built-in support for valid time and transaction time; bi-temporal or multi-temporal

#### Non-Relational (NoSQL)
Less constrained consistency models. Structure can be interpreted at read time (**schema on read**).

| Type | Description |
|------|-------------|
| **Column-oriented** | Compresses redundant data; efficient for aggregates over many rows; suited for OLAP |
| **Spatial** | Optimized for geometric objects; uses spatial indexes; follows Open Geospatial Consortium standard |
| **Object/Multi-media** | Hierarchical storage management with object classes |
| **Flat File** | Single file; rows and columns; used in Hadoop |
| **Key-Value Pair** | Document databases (XML/JSON) and Graph databases |
| **Triplestore** | Subject-predicate-object expressions (RDF); best for taxonomy, linked data, knowledge portals |

### 1.9 Common Database Processes

| Process | Description |
|---------|-------------|
| **Archiving** | Moving data to lower-cost, lower-performance media; must align with partitioning strategy |
| **Capacity & Growth Projections** | Estimating storage size, growth rate, and expansion needs |
| **Change Data Capture (CDC)** | Detecting and propagating data changes via versioning or log reading |
| **Purging** | Completely removing data that has no further value or has become a liability |
| **Replication** | Storing same data on multiple devices; active (real-time) or passive (primary→secondary); mirroring vs. log shipping |
| **Resiliency & Recovery** | System tolerance to errors; immediate, critical, or non-critical recovery types |
| **Retention** | How long data is kept; driven by regulatory requirements and risk management |
| **Sharding** | Isolating small database chunks that can be updated independently |

---

## 2. Activities

### 2.1 Manage Database Technology

#### 2.1.1 Understand Database Technology Characteristics
DBAs and Database Architects combine knowledge of available tools with business requirements to suggest optimal technology applications. Understand:
- Transaction capabilities (commit/rollback)
- Volume and velocity limits
- Application profiles (transaction processing, BI, profiles)

#### 2.1.2 Evaluate Database Technology
Selection factors:
- **Technical**: Architecture, volume/velocity limits, application profile, hardware/OS support, scalability, resiliency
- **Organizational**: Risk appetite, trained staff availability, cost of ownership
- **Vendor**: Reputation, support policy, release schedule, customer references

> Start with a pilot project or proof-of-concept before full production implementation.

#### 2.1.3 Manage and Monitor Database Technology
DBAs serve as Level 2 technical support. Key practices:
- Ensure training plans and budgets for all involved
- Cross-train DBAs in application development skills
- Regular backups and recovery tests
- Document processes and procedures for administering products
- Deploy new technology in pre-production environments first

### 2.2 Manage Databases

#### 2.2.1 Understand Requirements

**Define Storage Requirements:**
- Initial capacity estimate for first year + growth projection for following years
- Account for indexes, logs, and redundant images (mirrors)
- Address regulatory data retention requirements
- Plan for adding space well in advance of need

**Identify Usage Patterns:**
- Transaction-based
- Large data set write/retrieval-based
- Time-based (month-end peaks, weekend valleys)
- Location-based
- Priority-based (department/batch priorities)

**Define Access Requirements:**
- ACID-type access: SQL, ODBC, JDBC, XQJ, ADO.NET, XML, XQuery, XPath, Web Services
- BASE-type access: C, C++, REST, XML, Java

#### 2.2.2 Plan for Business Continuity

Recovery plans must cover:
- Loss of physical database server
- Loss of disk storage devices
- Loss of database (master, temp, transaction log)
- Corruption of database index or data pages
- Loss of database or log segment filesystems
- Loss of backup files

> Keep a copy of the recovery plan, DBMS software, and security codes in a secure off-site location. Regular backups are essential — and they must be tested for readability.

**Backup Strategy:**
- Backup frequency specified in SLA
- Balance data importance against protection cost
- Periodic complete backups + incremental backups
- Hot backups (while running) preferred for continuous availability
- Store daily backups off-site

#### 2.2.3 Create and Maintain Databases

**Create Storage Containers:** All data stored on physical drives, organized for load, search, and retrieval. Relational databases use schemas containing tables; non-relational use filesystems containing files.

**Implement Physical Data Models:** DBAs create the complete physical data storage environment including storage objects, indexing objects, and encapsulated code objects. For third-party (COTS/ERP) databases, reverse-engineer physical models from the storage tool catalog.

**Load Data:** DBAs fill empty databases. Bulk load capabilities require data matching target database objects. For external data sources, document the source in the logical data model and data dictionary.

**Manage Data Replication:** DBAs advise on:
- Active vs. passive replication
- Distributed concurrency control
- Change identification methods (timestamps, version numbers)
- Refresh vs. merge strategies based on data change volume

#### 2.2.4 Manage Database Performance

Performance = **Availability** + **Speed**. An unavailable database has zero performance.

DBAs manage performance by:
- Setting and tuning OS and application parameters
- Managing database connectivity
- Tuning OS, networks, and transaction processing middleware
- Dedicating appropriate storage and enabling storage management software
- Providing volumetric growth studies
- Supporting SLA management with workload benchmarks

**Set Database Performance Service Levels:** SLAs between IT data management services and data owners govern performance, availability, recovery, and response expectations.

**Manage Database Availability:**
| Factor | Description |
|--------|-------------|
| Manageability | Ability to create and maintain an environment |
| Recoverability | Ability to reestablish service after interruption |
| Reliability | Ability to deliver service at specified levels |
| Serviceability | Ability to identify, diagnose, and repair problems |

Causes of unavailability include planned outages (maintenance, upgrades), unplanned outages (hardware/software failure), application problems (security, performance, recovery failures), data problems (corruption, loss, replication failure), and human error.

**Maintain Database Performance Service Levels:**

| Performance Issue | Mitigation |
|-------------------|------------|
| **Memory allocation/contention** | Tune buffer/cache sizes |
| **Locking and blocking** | Kill blocking processes; DBMS auto-terminates deadlocks |
| **Inaccurate database statistics** | Update statistics frequently, especially in active databases |
| **Poor coding** | Understand query optimizer; use stored procedures for complex SQL |
| **Inefficient complex table joins** | Use views; avoid complex SQL in database functions |
| **Insufficient indexing** | Create necessary indexes; avoid too many on heavily updated tables |
| **Application activity** | Run applications on separate servers from DBMS |
| **Overloaded servers** | Create new database servers; archive less-used data |
| **Database volatility** | Turn off statistics updates during heavy insert/delete periods |
| **Runaway queries** | Use rankings/query governors to kill or pause |

> For OLTP databases, restructure only as last resort. For read-only/analytical databases, denormalization is the rule, not the exception.

#### 2.2.5 Maintain Alternate Environments

| Environment | Purpose | Characteristics |
|-------------|---------|-----------------|
| **Development** | Create and test changes for production | Closely resemble production with scaled-down resources |
| **Test** | QA, integration testing, UAT, performance testing | Same software/hardware as production; performance testing NOT scaled down |
| **Sandboxes** | Test hypotheses; develop new data uses | Isolated from production; DBAs set up, grant access, monitor |
| **Alternate Production** | Offline backups, failover, resiliency | Identical to production; backup system can have scaled-down compute |

#### 2.2.6 Manage Test Data Sets

Software testing accounts for nearly half of system development cost. Test data can be:
- **Fabricated**: Completely generated with meaningless values
- **Sample data**: Subset of production data (by content or structure) or generated from production data
- **Masked**: Production data with protected/restricted data obfuscated

> DBAs should monitor test data and ensure obsolete test data is purged regularly to preserve capacity.

#### 2.2.7 Manage Data Migration

Data migration is transferring data between storage types, formats, or computer systems with minimal change. Reasons include:
- Server/storage equipment replacement or upgrade
- Website consolidation
- Server maintenance
- Data center relocation

**Migration phases**: Design → Extraction → Remediation → Load → Verification

Smaller mapping granularity = faster Metadata update, less extra capacity needed, quicker freeing of old storage.

---

## 3. Tools

| Tool Category | Function |
|---------------|----------|
| **Data Modeling Tools** | Generate DDL, reverse-engineer databases, validate naming standards, store Metadata |
| **Database Monitoring Tools** | Monitor capacity, availability, cache performance, user statistics; alert to issues |
| **Database Management Tools** | Configuration, patch/upgrade installation, backup/restore, cloning, test management, data clean-up |
| **Developer Support Tools** | Visual interface for connecting to and executing commands on databases |

---

## 4. Techniques

### 4.1 Test in Lower Environments
Install and test upgrades/patches on the lowest environment first (development), then progressively higher environments, with production last. This ensures installer experience and minimizes production disruption.

### 4.2 Physical Naming Standards
Consistency in naming speeds understanding. Reference: **ISO/IEC 11179** — Metadata registries (MDR), particularly Part 5 — Naming and Identification Principles.

### 4.3 Script Usage for All Changes
Place all database changes into update script files and test thoroughly in non-production environments before applying to production. Direct data changes are extremely risky.

---

## 5. Implementation Guidelines

### 5.1 Readiness Assessment / Risk Assessment

**Data Loss Risks:**
- Technical or procedural errors
- Malicious intent
- Mitigate through SLAs, well-documented procedures, ongoing security assessment
- SLA audits and data audits recommended

**Technology Readiness:**
- NoSQL, Big Data, triple stores, FDMS require new skills
- DBAs, systems engineers, developers, and business users must be ready
- Assess organizational skill gaps before adopting new technologies

### 5.2 Organization and Cultural Change

DBAs often fail to promote the value of their work. Common challenges:
- Organizations view IT through applications, not data
- Application development sees data management as an impediment
- DBAs slow to adapt to new technologies and development methods (Agile, XP, Scrum)

**Recommendations for DBAs:**
- **Proactively communicate** — Close communication with project teams during and after implementation
- **Communicate on their level** — Business terms for business people; technical terms for developers
- **Stay business-focused** — Objective is meeting business requirements
- **Be helpful** — Avoid always saying "no"; help people succeed
- **Learn continually** — Apply lessons learned from setbacks

---

## 6. Data Storage and Operations Governance

### 6.1 Metrics

**Data Storage Metrics:**
- Count of databases by type
- Aggregated transaction statistics
- Capacity metrics (storage used, containers, data objects, data in queue)
- Storage service usage and requests

**Performance Metrics:**
- Transaction frequency and quantity
- Query performance
- API service performance

**Operational Metrics:**
- Aggregated data retrieval time statistics
- Backup size
- Data quality measurement
- Availability

**Service Metrics:**
- Issue submission, resolution, and escalation counts by type
- Issue resolution time

### 6.2 Information Asset Tracking
- Ensure compliance with all licensing agreements and regulatory requirements
- Track and audit software licenses, annual support costs, server lease agreements
- Determine Total Cost of Ownership (TCO) for each technology
- Regularly evaluate obsolete, unsupported, or too-expensive technologies

### 6.3 Data Audits and Data Validation

**Data Audit**: Evaluation of a data set against defined criteria to determine compliance with contractual and methodological requirements.

**Data Validation**: Evaluation of stored data against acceptance criteria to determine quality and usability.

DBAs support audits and validation by:
- Helping develop and review the approach
- Performing preliminary data screening and review
- Developing data monitoring methods
- Applying statistical techniques
- Supporting sampling and analysis
- Reviewing data and providing data discovery support

---

## References

- Brewer, Eric. "Toward Robust Distributed Systems." PODC Keynote, 2000.
- Dunham, Jeff. *Database Performance Tuning Handbook*. McGraw-Hill, 1998.
- EMC Education Services. *Information Storage and Management*. 2nd ed. Wiley, 2012.
- Haerder, T. and A. Reuter. "Principles of Transaction-Oriented Database Recovery." ACM Computing Surveys, 1983.
- Hoffer, Jeffrey, et al. *Modern Database Management*. 7th ed. Prentice Hall, 2004.
- Kroenke, D. M. *Database Processing: Fundamentals, Design, and Implementation*. 10th ed. Pearson, 2005.
- Mullins, Craig S. *Database Administration: The Complete Guide to Practices and Procedures*. Addison-Wesley, 2002.
- Pascal, Fabian. *Practical Issues in Database Management*. Addison-Wesley, 2000.
- Piedad, Floyd and Michael Hawkins. *High Availability: Design, Techniques and Processes*. Prentice Hall, 2001.
- Rob, Peter and Carlos Coronel. *Database Systems: Design, Implementation, and Management*. 7th ed. Course Technology, 2006.
- Sadalage, Pramod J. and Martin Fowler. *NoSQL Distilled*. Addison-Wesley, 2012.

---

## Related DMBOK Chapters

- [[00_Introduction_to_CyBOK]] — Data lifecycle and core concepts
- [[01_Data_Governance]] — Governance and stewardship
- [[02_Data_Architecture]] — Enterprise architecture and data design
- [[03_Data_Modeling_and_Design]] — Logical and physical data modeling
- [[05_Data_Security]] — Access controls, encryption, and auditing
- [[06_Data_Integration_and_Interoperability]] — ETL and data movement
- [[08_Reference_and_Master_Data]] — Master data lifecycle management
- [[09_Data_Warehousing_and_BI]] — OLAP, cubes, and analytics
