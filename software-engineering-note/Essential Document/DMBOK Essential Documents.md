---
tags: [overview, data-management, dmbok, essential-documents]
---

# DMBOK — Essential Documents by Knowledge Area

> **Source:** [[DMBoK v2 - Overview|DMBoK v2 (DAMA International, 2017)]] — 11 Knowledge Areas
> Organized by data management domain: Governance → Architecture → Modeling → Storage → Integration → Quality
>
> ⚠️ **This is a document-only extract.** For the full body of knowledge, see:
> `F:\projects\orlita_md\software-engineering-note\Body of Knowledge\DMBOK\`

**Priority Legend:**
| Icon | Level | Meaning |
|---|---|---|
| 🔴 | **Must Have** | Essential for virtually all data management programs; skipping creates significant risk |
| 🟡 | **Nice to Have** | Recommended for medium+ maturity; adds substantial value |
| 🟢 | **Optional** | Situational — depends on domain, scale, regulatory needs, or data volume |

---

## 1. Data Governance

> **Owner:** CDO / Data Governance Council

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Data Governance Charter** | Business drivers, vision, mission, principles, and success criteria for DG program | 🔴 Must Have | ISO/IEC 38505 |
| **Data Governance Strategy** | Implementation roadmap: policies, business glossary, architecture, asset valuation, standards | 🔴 Must Have | ISO/IEC 38505 |
| **Data Governance Operating Framework** | Structure, roles, responsibilities, and accountabilities for DG | 🔴 Must Have | ISO/IEC 38505 |
| **Data Policy** | Global directives codifying principles into rules for data creation, integrity, security, quality, and use | 🔴 Must Have | ISO/IEC 38505, ISO 9001 |
| **Data Standards** | Mandatory codified decisions ensuring consistent results across all data management KAs | 🔴 Must Have | ISO/IEC 11179 |
| **Business Glossary** | System of record for business terms: definitions, synonyms, metrics, lineage, business rules, stewards | 🔴 Must Have | ISO/IEC 11179 |
| **Data Stewardship Assignment** | Designated stewards per data domain with documented responsibilities | 🔴 Must Have | — |
| **Data Management Maturity Assessment** | Current-state evaluation of data management capabilities and capacity to change | 🟡 Nice to Have | CMMI DMM, ISO/IEC 33000 |
| **DG Scorecard / Metrics Dashboard** | Automated reporting: policy compliance, issue resolution rates, steward activity, data quality trends | 🟡 Nice to Have | — |
| **Data Asset Valuation Report** | Economic valuation of data assets using replacement cost, market value, or risk cost methods | 🟡 Nice to Have | — |
| **Regulatory Compliance Register** | Mapping of applicable regulations (GDPR, BCBS 239, PCI-DSS, Solvency II) to data controls | 🔴 Must Have | GDPR, BCBS 239, PCI-DSS |

## 2. Data Architecture

> **Owner:** Data Architect / Enterprise Architect

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Enterprise Data Model (EDM)** | High-level conceptual model of enterprise-wide data entities and relationships | 🔴 Must Have | ISO/IEC 11179 |
| **Data Architecture Blueprint** | Current and target state data architecture: data flows, data stores, data integration patterns | 🔴 Must Have | TOGAF, ISO/IEC/IEEE 42010 |
| **Data Flow Diagram** | End-to-end data movement across systems, applications, and organizational boundaries | 🔴 Must Have | — |
| **Data Lineage Documentation** | Source-to-target mappings; origin, transformations, and destinations of critical data elements | 🔴 Must Have | ISO/IEC 11179 |
| **Data Technology Roadmap** | Planned evolution of data platforms, tools, and infrastructure | 🟡 Nice to Have | — |

## 3. Data Modeling & Design

> **Owner:** Data Modeler / Data Architect

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Conceptual Data Model (CDM)** | Business-level entity-relationship diagram with high-level concepts and relationships | 🔴 Must Have | ISO/IEC 11179 |
| **Logical Data Model (LDM)** | Normalized model: all entities, attributes, keys, relationships; technology-independent | 🔴 Must Have | ISO/IEC 11179 |
| **Physical Data Model (PDM)** | DBMS-specific: tables, columns, data types, indexes, partitions, constraints, views | 🔴 Must Have | ISO/IEC 11179 |
| **Dimensional Model** | Star/snowflake schema for data warehousing: fact tables, dimension tables, granularity | 🔴 Must Have | — |
| **Entity-Relationship Diagram (ERD)** | Visual representation using IE/Crow's Foot, IDEF1X, or UML notation | 🔴 Must Have | ISO/IEC 19501 (UML) |
| **Data Dictionary** | Formal definitions of all data elements, attributes, domains, and valid values | 🔴 Must Have | ISO/IEC 11179 |
| **Data Modeling Standards** | Naming conventions, notation rules, abbreviation standards, domain definitions | 🔴 Must Have | ISO/IEC 11179 |
| **Data Model Scorecard®** | Quality assessment across 10 categories: requirements coverage, completeness, structural soundness, definitions | 🟡 Nice to Have | — |
| **Data Lineage & Mapping Specification** | Source-to-target attribute-level mappings with transformation rules | 🔴 Must Have | — |
| **Data Model Review Records** | Review findings, approval, and version history for CDM, LDM, and PDM | 🔴 Must Have | — |

## 4. Data Storage & Operations

> **Owner:** DBA / Data Operations Manager

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Database Design Document (DDL)** | Physical schema: tables, columns, keys, indexes, partitions, storage parameters | 🔴 Must Have | — |
| **Database Operational Runbook** | Routine procedures: backup/restore, patching, capacity monitoring, performance tuning | 🔴 Must Have | — |
| **Backup & Recovery Plan** | Backup schedules, retention policies, RPO/RTO targets, restore procedures | 🔴 Must Have | ISO/IEC 27031 |
| **Capacity Plan** | Storage growth projections, performance thresholds, scaling strategy | 🔴 Must Have | — |
| **High Availability / DR Configuration** | Replication, failover, clustering, and disaster recovery architecture | 🔴 Must Have | ISO/IEC 27031 |
| **Data Retention & Archival Policy** | Legal hold, retention periods, archival and purging procedures per regulatory requirements | 🔴 Must Have | GDPR, ISO 15489 |

## 5. Data Security

> **Owner:** Data Security Officer / CISO

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Data Classification Schema** | Categories: Public, Internal, Confidential, Restricted; with handling procedures per level | 🔴 Must Have | ISO/IEC 27001 |
| **Data Access Control Policy** | Role-based, attribute-based, or mandatory access controls; least privilege | 🔴 Must Have | ISO/IEC 29146 |
| **Data Encryption Standards** | Encryption at rest, in transit, and in use; approved algorithms and key management | 🔴 Must Have | ISO/IEC 18033, NIST SP 800-57 |
| **Data Masking / Anonymization Rules** | Obfuscation standards for non-production environments and analytics | 🔴 Must Have | ISO/IEC 27002 |
| **Privacy Impact Assessment (PIA)** | Evaluation of privacy risks for systems processing personal data | 🔴 Must Have | GDPR, ISO/IEC 27701 |
| **Data Breach Response Plan** | Notification procedures, containment, forensic investigation, regulatory reporting | 🔴 Must Have | GDPR, ISO/IEC 27035 |
| **Data Security Audit Report** | Audit findings for data access, encryption, masking, and compliance controls | 🔴 Must Have | ISO/IEC 27001 |

## 6. Data Integration & Interoperability

> **Owner:** Data Integration Architect / ETL Developer

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Data Integration Architecture** | ETL/ELT patterns, data pipelines, message queues, API integration topology | 🔴 Must Have | — |
| **ETL/ELT Specification** | Extraction, transformation, and loading logic: source-to-target mappings, business rules, error handling | 🔴 Must Have | — |
| **Data Interface Agreement (DIA)** | Contract between data provider and consumer: schema, frequency, quality SLAs, ownership | 🔴 Must Have | — |
| **API Data Contract** | OpenAPI/GraphQL schema, authentication, rate limits, versioning for data APIs | 🔴 Must Have | OpenAPI Spec |
| **Data Replication & Synchronization Spec** | Near-real-time or batch replication rules, conflict resolution, consistency model | 🔴 Must Have | — |
| **Data Virtualization Specification** | Virtualized data views, federated queries, abstraction layer definitions | 🟡 Nice to Have | — |

## 7. Document & Content Management

> **Owner:** ECM Manager / Records Manager

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Enterprise Content Management (ECM) Strategy** | Governance, lifecycle, and technology for unstructured data and documents | 🔴 Must Have | ISO 15489 |
| **Records Retention Schedule** | Legal and regulatory retention periods per document type with disposition procedures | 🔴 Must Have | ISO 15489, GDPR |
| **Content Classification & Taxonomy** | Metadata schema, tagging standards, and content types for ECM | 🔴 Must Have | ISO 23081 |
| **Unstructured Data Inventory** | Catalog of document repositories, file shares, SharePoint sites, and content stores | 🔴 Must Have | — |

## 8. Reference & Master Data

> **Owner:** MDM Architect / Data Steward

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Master Data Management (MDM) Strategy** | Domains (customer, product, location, etc.), hub architecture, governance model | 🔴 Must Have | — |
| **Reference Data Catalog** | Code tables, lookup values, country codes, currency codes, status codes with ownership | 🔴 Must Have | ISO/IEC 11179 |
| **Golden Record Definition** | Rules for survivorship, matching, merging, and confidence scoring in MDM | 🔴 Must Have | — |
| **MDM Data Quality Rules** | Validation, deduplication, and standardization rules for master data | 🔴 Must Have | — |
| **Cross-Reference / Mapping Table** | System-to-system identifier mappings for master data entities | 🔴 Must Have | — |

## 9. Data Warehousing & Business Intelligence

> **Owner:** BI Architect / Data Warehouse Manager

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Data Warehouse Architecture** | Layers: staging, integration, presentation, semantic; with ETL topology | 🔴 Must Have | — |
| **Dimensional Model (Star/Snowflake)** | Fact and dimension tables, granularity, SCD strategies, aggregate tables | 🔴 Must Have | — |
| **BI Semantic Layer Definition** | Business-friendly views, calculated measures, KPIs, drill paths | 🔴 Must Have | — |
| **Report & Dashboard Catalog** | Inventory of reports, dashboards, KPIs with data sources and refresh schedules | 🔴 Must Have | — |
| **Analytics Governance Policy** | Data access, model validation, bias assessment, and ethical use guidelines | 🟡 Nice to Have | — |

## 10. Metadata Management

> **Owner:** Metadata Manager / Data Architect

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Metadata Repository** | Centralized store for business, technical, and operational metadata | 🔴 Must Have | ISO/IEC 11179 |
| **Business Glossary** (Metadata perspective) | Business terms tied to physical data assets with lineage, rules, and stewards | 🔴 Must Have | ISO/IEC 11179 |
| **Data Lineage & Impact Analysis** | End-to-end data flow from source to consumption with dependency mapping | 🔴 Must Have | ISO/IEC 11179 |
| **Metadata Standards** | Metadata capture requirements: naming, definitions, domains, classifications, relationships | 🔴 Must Have | ISO/IEC 11179 |
| **Data Catalog** | Searchable inventory of data assets with ownership, quality scores, usage statistics | 🔴 Must Have | — |

## 11. Data Quality

> **Owner:** Data Quality Manager / Data Steward

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Data Quality Strategy** | Framework: dimensions (accuracy, completeness, consistency, timeliness, uniqueness, validity), measurement, improvement | 🔴 Must Have | ISO 8000 |
| **Data Profiling Report** | Statistical analysis of data content: patterns, distributions, nulls, outliers, anomalies | 🔴 Must Have | ISO 8000 |
| **Data Quality Rules** | Validation rules, business rules, and integrity constraints per data domain | 🔴 Must Have | ISO 8000 |
| **Data Quality Scorecard** | Per-dimension metrics at enterprise, domain, and application levels with trends | 🔴 Must Have | ISO 8000 |
| **Data Cleansing Specification** | Standardization, deduplication, enrichment, and correction rules | 🔴 Must Have | ISO 8000 |
| **Root Cause Analysis Report** | Investigation of recurring data quality issues with preventive recommendations | 🟡 Nice to Have | — |
| **Data Quality Issue Log** | Tracked issues: description, severity, assigned steward, resolution status | 🔴 Must Have | — |

---

## Related

- [[SWEBOK Essential Documents]] — Software engineering documents by SDLC phase
- [[PMBOK Essential Documents]] — Project management documents by phase
- [[BABOK Essential Documents]] — Business analysis documents by KA
- [[CyBOK Essential Documents]] — Cyber security documents by domain
- [[DMBoK v2 - Overview]] — Full DMBOK knowledge base
