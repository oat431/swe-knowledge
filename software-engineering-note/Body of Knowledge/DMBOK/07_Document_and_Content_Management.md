---
title: "Document and Content Management"
source: "DMBoK v2 (Chapter 8)"
tags: [document-management, data-management, dmbok, ecm, records-management, content-management, information-governance]
created: 2026-07-09
updated: 2026-07-09
---

# Document and Content Management

> **DMBoK v2 — Chapter 8**  
> *Managing documents, records, and unstructured content as critical organizational assets.*

## Overview

Document and Content Management encompasses the processes, techniques, and technologies for controlling and organizing documents, records, and unstructured content throughout their lifecycle. It includes [[DMBOK/10_Reference_and_Master_Data|controlled vocabularies]], taxonomies, ontologies, search capabilities, enterprise content management (ECM) systems, and records management — all governed by [[DMBOK/02_Data_Governance|information governance]] frameworks.

---

## 1. Essential Concepts

### 1.1 Controlled Vocabularies and Terminology Management

#### 1.1.1 Controlled Vocabulary

A **controlled vocabulary** is a defined list of explicitly allowed terms from which users must choose values. It provides consistency in naming and classification. Unlike a simple list, a controlled vocabulary may include:

- **Vocabulary Views**: Subsets of a controlled vocabulary tailored for specific user communities. They must be internally consistent and map back to the broader vocabulary.
- **Micro-Controlled Vocabularies**: Highly specialized term sets not present in the general vocabulary (e.g., medical discipline-specific dictionaries). These map to the hierarchical structure of the broad controlled vocabulary.

#### 1.1.2 Term and Pick Lists

**Term lists** are simple enumerations with no relationships between terms. **Pick lists** (dropdowns, menu choices) help control ambiguity by reducing the domain of permissible values. Content management software can transform pick lists into faceted taxonomies searchable from a portal homepage.

#### 1.1.3 Term Management

Per ANSI/NISO Z39.19-2005, a **term** is "one or more words designating a concept." Term management includes:

- Defining and classifying terms initially
- Maintaining terms as they propagate across systems
- Governance processes for term changes, usually arbitrated by [[DMBOK/03_Data_Stewardship|Data Stewards]]

**Term relationships** (three types per Z39.19):

| Relationship | Description |
|---|---|
| **Equivalent** | Cross-reference indicating one term should be used instead of another (most common in IT integration) |
| **Hierarchical** | Broader (general) to narrower (specific) or whole-part relationships |
| **Related (Associative)** | Associatively but not hierarchically linked |

#### 1.1.4 Synonym Rings and Authority Lists

- **Synonym Ring**: A set of terms with roughly equivalent meaning. Used for retrieval (not indexing); treats all synonyms equally. Common in search engines and Metadata registries.
- **Authority List**: A controlled vocabulary where one term is **preferred** and others are variants. Cross-references guide users from non-preferred to preferred terms. Example: US Library of Congress Subject Headings.

#### 1.1.5 Taxonomies

A **taxonomy** is a naming structure containing a controlled vocabulary used for outlining topics and enabling navigation and search. Taxonomy structures include:

| Structure | Description | Example |
|---|---|---|
| **Flat Taxonomy** | No relationships; all categories equal | List of countries |
| **Hierarchical Taxonomy** | Tree structure with parent/child relationships; bi-directional | Geography: continent → country → state → city |
| **Polyhierarchy** | Tree-like with multiple node relation rules; child nodes may have multiple parents | Complex classification systems |
| **Facet Taxonomy** | Star structure; each node associated with a center object | Metadata attributes (creator, title, keywords) |
| **Network Taxonomy** | Combines hierarchical and facet; any two nodes linked by associations | Recommender engines, thesauri |

Taxonomies must be maintained — unmaintained taxonomies produce incorrect results and create regulatory compliance risks.

#### 1.1.6 Classification Schemes and Tagging

- **Classification schemes**: Codes representing controlled vocabulary, often hierarchical (e.g., Dewey Decimal System).
- **Folksonomies**: Social tagging by users; no hierarchical structure or preferred terms. Not authoritative but useful for enhancing information retrieval. Folksonomy terms can be linked to structured vocabularies.

#### 1.1.7 Thesauri

A **thesaurus** combines synonym lists and taxonomies for content retrieval. It provides relationships (hierarchical, associative, equivalent) plus definitions and citations. Standards: ISO 25964, ANSI/NISO Z39.19.

Uses: organizing unstructured content, uncovering cross-media relationships, improving website navigation, optimizing search.

#### 1.1.8 Ontology

An **ontology** represents a set of concepts and their relationships within a domain — the primary knowledge representation in the **Semantic Web** (Web 3.0 / Linked Data Web). 

Ontology languages: RDFS (Resource Description Framework Schema), OWL (Web Ontology Language).

Key differences from taxonomies/data models:
- **Taxonomy/Data Model**: Closed-world assumption — only what is explicitly defined is known
- **Ontology**: Open-world assumption — relationships can be inferred; something not explicitly declared can be true

Common ontology pitfalls: failing to distinguish instance-of vs. subclass-of, modeling events as relations, lack of clarity/unique terms, modeling roles as classes, failure to reuse, mixing semantics.

---

### 1.2 Documents and Records

#### 1.2.1 Document Management

**Documents** are electronic or paper objects containing task instructions, requirements, and logs of execution/decisions. Examples: procedures, protocols, methods, specifications.

**Document management** encompasses processes, techniques, and technologies for controlling and organizing documents and records throughout their lifecycle — including storage, inventory, and control for both electronic and paper documents. Over 90% of documents created today are electronic.

Document management concerns **files as entities** (with little attention to file content).

**Lifecycle management activities:**
1. **Inventory** — Identification of existing and newly created documents/records
2. **Policy** — Creation, approval, and enforcement of document/records policies, including retention policy
3. **Classification** — Categorization of documents/records
4. **Storage** — Short- and long-term storage of physical and electronic documents/records
5. **Retrieval and Circulation** — Access and circulation per policies, security standards, and legal requirements
6. **Preservation and Disposal** — Archiving and destroying per organizational needs, statutes, and regulations

Data management professionals are stakeholders in document classification and retention decisions, supporting consistency between structured data and unstructured data.

#### 1.2.2 Records Management

> *ISO 15489 defines records management as "the field of management responsible for the efficient and systematic control of the creation, receipt, maintenance, use and disposition of records, including the processes for capturing and maintaining evidence of and information about business activities and transactions in the form of records."*

Only a subset of documents are designated as **records** — they provide evidence that actions were taken and decisions were made in keeping with procedures. **Vital Records** are those required to resume an organization's operations in the event of a disaster.

**Characteristics of well-prepared records:**

| Characteristic | Description |
|---|---|
| **Content** | Accurate, complete, and truthful |
| **Context** | Descriptive Metadata (creator, date, relationships) maintained at creation |
| **Timeliness** | Created promptly after the event, action, or decision |
| **Permanency** | Cannot be changed for the legal length of their existence once designated as records |
| **Structure** | Clear appearance/arrangement; correct forms/templates; legible; consistent terminology |

Organizations must determine which copy (electronic or paper) is the official **copy of record**. The non-official copy can then be safely destroyed.

#### 1.2.3 Digital Asset Management (DAM)

**DAM** focuses on the storage, tracking, and use of rich media documents (video, logos, photographs, etc.) — similar to document management but specialized for multimedia.

---

### 1.3 Data Map

A **Data Map** is an inventory of all Electronically Stored Information (ESI) data sources, applications, and IT environments, including application owners, custodians, geographical locations, and data types.

---

### 1.4 E-discovery

**Discovery** is the pre-trial phase of a lawsuit where parties request information from each other. The US Federal Rules of Civil Procedure (FRCP) were amended in 2006 to accommodate ESI in the litigation process.

Key regulations with e-discovery requirements:
- UK Bribery Act, Dodd-Frank Act, FATCA, Foreign Corrupt Practices Act
- EU Data Protection Regulations
- Global anti-trust regulations and sector-specific regulations

**Legal Hold Notification (LHN)**: Identifying information that may be requested in a legal proceeding, locking it to prevent editing/deletion, and notifying all parties.

The **[EDRM (Electronic Discovery Reference Model)](https://edrm.net)** provides an eight-phase framework:

| Phase | Description |
|---|---|
| **Identification** | Early Case Assessment + Early Data Assessment — identifying pertinent Metadata and data locations |
| **Preservation** | Placing identified data in legal hold to prevent destruction |
| **Collection** | Acquiring and transferring data to legal counsel in a legally defensible manner |
| **Processing** | De-duplication, searching, and analysis to identify relevant items |
| **Review** | Identifying documents for response and privileged documents to withhold |
| **Analysis** | Content analysis to understand circumstances, facts, and potential evidence |
| **Production** | Turning data/information over to opposing counsel per agreed specifications |
| **Presentation** | Displaying ESI at depositions, hearings, and trials |

---

### 1.5 Information Architecture

**Information Architecture** is the process of creating structure for a body of information or content. Components include:

- Controlled vocabularies
- Taxonomies and ontologies
- Navigation maps
- Metadata maps
- Search functionality specifications
- Use cases and user flows

Information architecture identifies links/relationships between documents, specifies document requirements and attributes, and defines content structure. A **storyboard** provides a blueprint for web projects.

---

### 1.6 Search Engine

A **search engine** searches for information based on terms and retrieves content containing those terms. Components: search engine software, spider software (roaming the Web storing URLs), indexing of keywords/text, and ranking rules.

---

### 1.7 Semantic Model

**Semantic modeling** describes a network of concepts and their relationships, enabling users to ask questions in a non-technical way. Semantic models contain:

- **Semantic objects**: Things with attributes (cardinality, domains), identifiers, and structures (simple, composite, compound, hybrid, association, parent/subtype, archetype/version)
- **Bindings**: Represent associations or association classes (UML)

Uses in data integration:
- Single ontology as reference model
- Each data source modeled with its own ontology, then mapped
- Hybrid: multiple ontologies integrated with a common vocabulary

---

### 1.8 Semantic Search

**Semantic searching** focuses on meaning and context rather than predetermined keywords. Uses AI to identify query matches based on words and their context. Types of semantic keywords:
- **Core keywords**: With variations
- **Thematic keywords**: Conceptually related terms
- **Stem keywords**: Anticipating what people might ask

BI and analytics tools often have semantic search requirements for finding information across disparate formats.

---

### 1.9 Unstructured Data

An estimated **80%** of all stored data exists outside relational databases. This unstructured (or semi-structured / non-tabular) data includes:

- Word processing documents, email, social media, chats
- Flat files, spreadsheets, XML files, transactional messages
- Reports, graphics, digital images, microfiche
- Video and audio recordings

**Fundamental data management principles apply equally:** storage, integrity, security, content quality, access, and effective use. Unstructured data requires [[DMBOK/02_Data_Governance|governance]], [[DMBOK/04_Data_Architecture|architecture]], [[DMBOK/06_Data_Security|security]], [[DMBOK/11_Metadata_Management|Metadata]], and [[DMBOK/12_Data_Quality|data quality]].

---

### 1.10 Workflow

Content development should be managed through a **workflow** that ensures content is created on schedule and receives proper approvals. Components include: creation, processing, routing, rules, administration, security, electronic signature, deadline, escalation, reporting, and delivery.

A **Content Management System (CMS)** provides version control — content is timestamped, assigned a version number, and tagged with the updater's name when checked in.

---

## 2. Activities

### 2.1 Plan for Lifecycle Management

Lifecycle planning includes developing classification/indexing systems and taxonomies, and creating policy specifically for records.

First, identify the organizational unit responsible for managing documents and records. This unit:
- Coordinates access and distribution internally and externally
- Integrates best practices across departments
- Develops a document management plan including business continuity for vital records
- Ensures compliance with retention policies, standards, and government regulations

#### 2.1.1 Plan for Records Management

Starts with a clear definition of what constitutes a **record**. The defining team should include SMEs from the functional area and people who understand enabling systems. Must account for electronic records, paper records, and unstructured data.

#### 2.1.2 Develop a Content Strategy

A content strategy should directly support the organization's approach to providing relevant, useful content efficiently. Planning should account for:
- Content drivers (why content is needed)
- Content creation and delivery
- Technology decisions driven by content requirements

Start with an **inventory of current state** and a **gap assessment**. A **unified content strategy** emphasizes modular content components for reusability. Enabling content discovery through Metadata categorization and search engine optimization (SEO) is critical.

#### 2.1.3 Create Content Handling Policies

Policies codify requirements — principles, direction, and guidelines for action. Most document management programs have policies for:

| Policy Area | Description |
|---|---|
| **Scope and Compliance** | Audit alignment |
| **Vital Records** | Identification and protection |
| **Retention Schedule** | Purpose and schedule for retaining records |
| **Information Hold Orders** | Responding to legal hold requirements (retaining expired records for lawsuits) |
| **Onsite/Offsite Storage** | Requirements for records storage |
| **Hard Drive / Network Drives** | Use and maintenance |
| **Email Management** | From a content management perspective |
| **Destruction Methods** | Proper methods (pre-approved vendors, destruction certificates) |

##### Social Media Policies

Organizations must define whether content posted on Facebook, Twitter, LinkedIn, blogs, wikis, or forums constitutes a record — especially when employees post using organizational accounts.

##### Device Access Policies

BYOD (bring-your-own-device), BYOA (bring-your-own-apps), and WYOD (wear-your-own-devices) trends require content/records management to ensure compliance, security, and privacy. Policies should distinguish informal content (Dropbox, Evernote) from formal content (contracts, agreements).

##### Handling Sensitive Data

Organizations must identify and protect sensitive data per legal requirements. [[DMBOK/06_Data_Security|Data Security]] and/or [[DMBOK/02_Data_Governance|Data Governance]] establish confidentiality schemes. Content producers must apply these classifications and mark documents accordingly.

##### Responding to Litigation

Proactive e-discovery preparation: create and manage an inventory of data sources and associated risks. Deploy appropriate technologies to automate e-discovery processes. Respond in a timely manner to litigation hold notices to prevent data loss.

#### 2.1.4 Define Content Information Architecture

Searches use either content-based indexing or Metadata. Indexing designs consider:
- Decision options for key index attributes based on user needs
- Vocabulary management
- Syntax for combining terms into headings or search statements

Data management professionals should coordinate controlled vocabulary and taxonomy efforts with data modeling and Metadata efforts.

---

### 2.2 Manage the Lifecycle

#### 2.2.1 Capture Records and Content

Electronic content is often already storable; paper content needs scanning, uploading, indexing, and storage. Use electronic signatures when possible. At capture, content should be tagged with at minimum: document/image identifier, date/time of capture, title, and author(s).

Social media capture options: saving in repositories, web crawlers for websites, APIs, RSS feeds, or manual/automated workflows.

#### 2.2.2 Manage Versioning and Control

Per **ANSI Standard 859**, three levels of control based on criticality and perceived harm:

| Control Level | Description |
|---|---|
| **Formal Control** | Formal change initiation, thorough impact evaluation, change authority decision, full status accounting |
| **Revision Control** | Less formal — notifying stakeholders, incrementing versions when change is required |
| **Custody Control** | Least formal — safe storage and means of retrieval |

Criteria for determining control level: cost of providing/updating the asset, project impact, consequences of change, need for reuse, maintenance of change history.

#### 2.2.3 Backup and Recovery

The document/records management system must be included in corporate backup/recovery, business continuity, and disaster recovery planning. A **vital records program** provides access to records needed during a disaster and for resuming normal business afterward.

#### 2.2.4 Manage Retention and Disposal

A **retention and disposition policy** defines:
- Timeframes for maintaining documents of operational, legal, fiscal, or historical value
- When inactive documents transfer to secondary storage
- Processes for compliance, methods and schedules for disposition
- Legal and regulatory considerations

**Risks of over-retention**: Records past their legally required timeframes remain discoverable for litigation.

**Why organizations don't prioritize removal of non-value-added information:**
- Inadequate policies
- Differing valuations of information
- Inability to foresee future needs
- No buy-in for Records Management
- Inability to decide what to delete
- Perceived cost of decision-making
- Electronic storage is cheap

#### 2.2.5 Audit Documents / Records

Periodic auditing ensures the right information reaches the right people at the right time.

**Sample audit measures:**

| Component | Measure |
|---|---|
| Inventory | Each location uniquely identified |
| Storage | Adequate space for growth |
| Reliability/Accuracy | Spot checks confirm adequate reflection of creation/receipt |
| Classification/Indexing | Metadata and file plans well described |
| Access/Retrieval | End users find critical information easily |
| Retention | Schedule structured logically by function |
| Disposition | Documents disposed as recommended |
| Security/Confidentiality | Breaches recorded and managed appropriately |
| Organizational Understanding | Appropriate training provided |

**Audit steps**: Define organizational drivers and stakeholders → Gather data (how/what/tools) → Report outcomes → Develop action plan.

---

### 2.3 Publish and Deliver Content

#### 2.3.1 Provide Access, Search, and Retrieval

After Metadata tagging and classification within the information content architecture, content is available for retrieval. Portal technology with user profiles helps find unstructured data. Search engines return content based on keywords.

#### 2.3.2 Deliver Through Acceptable Channels

Users expect to consume content on any device. Organizations often create content in MS Word and convert to HTML. Content prepared for one channel may need reformatting for another. Structured data formatted into HTML becomes difficult to recover — separating data from formatting is not straightforward.

---

## 3. Tools

### 3.1 Enterprise Content Management (ECM) Systems

An **ECM** may consist of a platform of core components or integrated applications (in-house or cloud). Reports can be delivered through printers, email, websites, portals, messaging, and document management system interfaces.

The boundaries between **document management** and **content management** are blurring as business processes intertwine and vendors widen product markets.

#### 3.1.1 Document Management System

A **DMS** tracks and stores electronic documents and electronic images of paper documents. Specialized types: document library systems, email systems, image management systems.

**Core capabilities**: storage, versioning, security, Metadata management, content indexing, and retrieval.

Documents are created within the DMS or captured via scanners/OCR software. They must be indexed via keywords or text during capture. Metadata (creator, creation/revision/storage dates) is stored per document.

**Advanced capabilities**: compound document support (integrating spreadsheets, video, audio), content replication.

**Document repository features**: check-in/check-out, versioning, collaboration, comparison, archiving, status states, storage media migration, disposition.

**Workflow types**:
- **Manual**: User decides where to send the document
- **Rules-based**: Rules dictate document flow
- **Dynamic**: Different workflows based on content

**Rights management**: Administrator grants access based on document type and user credentials. Electronic signatures ensure sender identity and message authenticity.

##### 3.1.1.1 Digital Asset Management (DAM)

Management of digital assets: audio, video, music, digital photographs. Tasks: cataloging, storage, retrieval.

##### 3.1.1.2 Image Processing

Captures, transforms, and manages images of paper and electronic documents using:
- **OCR (Optical Character Recognition)**: Conversion of scanned printed/handwritten text into computer-recognizable form
- **ICR (Intelligent Character Recognition)**: Advanced OCR handling printed and cursive handwriting
- **Forms processing**: Capture of printed forms via scanning/recognition

**Image formats**:
- **Vector** (.EPS, .AI, .PDF): Mathematical formulas; resizable without quality loss
- **Raster** (.JPEG, .GIF, .PNG, .TIFF): Fixed number of colored pixels; resolution-dependent

##### 3.1.1.3 Records Management System

Capabilities: automation of retention and disposition, e-discovery support, long-term archiving, vital records program. May integrate with a document management system.

#### 3.1.2 Content Management System (CMS)

A **CMS** collects, organizes, indexes, and retrieves content — storing it as components or whole documents while maintaining links between components. It is independent of where and how documents are stored.

A **Web Content Management System** controls website content through authoring, collaboration, and management tools based on a core repository. Delivery functions include responsive design and adaptive capabilities for various client devices.

Additional components: search, document composition, e-signature, content analytics, mobile applications.

#### 3.1.3 Content and Document Workflow

Workflow tools support business processes, route content/documents, assign work tasks, track status, and create audit trails. Workflows provide for review and approval before publication.

### 3.2 Collaboration Tools

Team collaboration tools enable collection, storage, workflow, and management of documents pertinent to team activities. Social networking enables sharing through blogs, wikis, RSS, and tagging.

### 3.3 Controlled Vocabulary and Metadata Tools

Tools that help develop/manage controlled vocabularies and Metadata:
- Data models as guides to organizational data
- Document management systems and office productivity software
- Metadata repositories, glossaries, directories
- Taxonomies and cross-reference schemes
- Indexes to collections, filesystems, archives, locations
- Search engines
- BI tools incorporating unstructured data
- Enterprise and departmental thesauri
- Published report libraries, contents, bibliographies, catalogs

### 3.4 Standard Markup and Exchange Formats

#### 3.4.1 XML (Extensible Markup Language)

XML provides a language for representing both structured and unstructured data, using Metadata to describe content, structure, and business rules. **XML namespaces** avoid name conflicts between documents using the same element names.

**Why XML-capable content management has grown:**
- Integrates structured data (RDBMS) with unstructured data (BLOBs, XML files)
- Integrates structured data with documents, reports, email, images, graphics, audio, video
- Builds enterprise portals (B2B, B2C) providing single access points
- Identifies/labels unstructured data so applications can process it

**XMI (Extensible Markup Interface)**: Specification for generating XML documents containing Metadata — a "structure" for XML.

#### 3.4.2 JSON (JavaScript Object Notation)

An open, lightweight standard for data interchange — language-independent, easy to parse. Two structures: unordered name/value pairs (objects) and ordered lists (arrays). Emerging as the preferred format in web-centric and NoSQL databases. More compact than XML; either can be returned via REST.

#### 3.4.3 RDF and Related W3C Specifications

**RDF (Resource Description Framework)** is a standard model for data interchange on the Web, using subject-predicate-object triples stored in a **triplestore** queried via SPARQL.

**Key W3C specifications:**

| Specification | Description |
|---|---|
| **RDFS (RDF Schema)** | Data modeling vocabulary for RDF data |
| **SKOS (Simple Knowledge Organization System)** | RDF vocabulary for hierarchies of concepts (classifications, taxonomies, thesauri) |
| **OWL (Web Ontology Language)** | Semantic markup language for publishing/sharing ontologies |
| **Linked Data** | Interrelated data sets using URIs for identification |

RDF helps with Big Data variety — data from different sources can be mixed and SPARQL used to find connections without predefining a schema.

#### 3.4.4 Schema.org

An open-source collection of shared vocabularies/schemas for on-page markup, enabling major search engines to understand web page meaning. **Rich snippets** (detailed search results with ratings, etc.) require properly formatted structured data with semantic markup from Schema.org. Also usable for structured data interoperability (e.g., with JSON).

### 3.5 E-discovery Technology

Capabilities: early case assessment, collection, identification, preservation, processing, OCR, culling, similarity analysis, email thread analysis. **Technology-Assisted Review (TAR)**: A workflow where a team reviews selected documents, marking them relevant/not; these decisions become input for a predictive coding engine that reviews and sorts remaining documents by relevancy.

---

## 4. Techniques

### 4.1 Litigation Response Playbook

A playbook containing objectives, metrics, and responsibilities prepared **before** a major discovery project begins. It defines the target e-discovery environment and assesses gaps.

**Compiling a playbook involves:**
1. Establish inventory of policies/procedures (Legal, Records Management, IT)
2. Draft policies for litigation holds, document retention, archiving, backups
3. Evaluate IT tool capabilities (e-discovery indexing, search, collection, data segregation, protection)
4. Identify and analyze pertinent legal issues
5. Develop communication and training plan
6. Identify materials that may be prepared in advance
7. Analyze vendor services for outside needs
8. Develop notification handling processes; keep playbook current

### 4.2 Litigation Response Data Map

A **data map** is a catalog of information systems describing systems, their uses, contained information, retention policies, and other characteristics. It should be comprehensive and include:

- Systems of record, originating applications, archives, disaster recovery copies, backups
- How email is stored, processed, and consumed
- Business process mappings to systems, user roles and privileges
- Which records are readily accessible vs. not (with reasons documented)
- Offsite storage inventory, including external cloud storage

Metadata is critical for searching — it gives ESI documents context and associates cases, transcripts, and supporting documents.

---

## 5. Implementation Guidelines

ECM implementation is a long-term effort requiring wide stakeholder buy-in and executive funding. **Let content — not technology — drive decisions.** Configure workflow around organizational needs to demonstrate value.

### 5.1 Readiness Assessment / Risk Assessment

Purpose: identify areas needing content management improvement and assess organizational adaptability.

**ECM critical success factors**: executive support, user involvement/training, change management, corporate culture, communication, content audit/classification, appropriate information architecture, content lifecycle support, proper Metadata tag definitions, customization capabilities.

**ECM risks**: project size/complexity, integration challenges, process/organizational issues, content migration effort, insufficient training, lack of policies/processes/procedures, poor stakeholder communication.

#### 5.1.1 Records Management Maturity

**ARMA's Generally Accepted Recordkeeping Principles® (GARP®)** and the **ARMA Information Governance Maturity Model** describe five maturity levels for each of the eight GARP principles:

| Level | Description |
|---|---|
| **1 - Sub-Standard** | Information governance/recordkeeping concerns not addressed or minimally |
| **2 - In Development** | Developing recognition of impact |
| **3 - Essential** | Minimum requirements to meet legal/regulatory requirements |
| **4 - Proactive** | Proactive program with continuous improvement focus |
| **5 - Transformational** | Information governance integrated into corporate infrastructure and processes |

**Applicable standards for technical assessment**: DoD 5015.2, ISO 16175, MoReq2, OMG RMS.

**Risks of inadequate records management**: inability to inventory records, time/cost finding records, lack of adherence to legal/regulatory requirements, costly fines, failure to protect vital records.

#### 5.1.2 E-discovery Assessment

Examine and identify improvement opportunities for the litigation response program. A mature program specifies clear roles/responsibilities, preservation protocols, data collection methodologies, and disclosure processes — all documented, defensible, and auditable.

**Key activities**: understand the information lifecycle, develop an ESI data map, proactively review retention policies, plan for quick litigation hold implementation with IT.

**Risks of no proactive litigation response**: scrambling to find relevant documents, over-specifying data retention ("keep everything"), no deletion policies, legal liabilities from unavailable records.

### 5.2 Organization and Cultural Change

People can be a greater challenge than technology. ECM may add tasks (scanning, Metadata definition). Departmental information silos hinder sharing and proper management.

A **holistic enterprise approach** with a single, centrally managed repository, clearly defined policies, and enforced processes can eliminate the perceived need for local copies. Training and communication are critical to success.

Key issues for document/content management professionals: privacy, data protection, confidentiality, intellectual property, encryption, ethical use, and identity.

**Organizational placement of RIM (Records and Information Management):**
- **Heavily regulated industries**: Align with corporate legal and e-discovery functions
- **Operational efficiency focus**: Align with marketing or operational support
- **IT focus**: Report to CIO or CDO
- Often found within ECM or Enterprise Information Management (EIM) programs

---

## 6. Documents and Content Governance

### 6.1 Information Governance Frameworks

Documents, records, and unstructured content represent organizational risk. Governance drivers include:

- Legal and regulatory compliance
- Defensible disposition of records
- Proactive e-discovery preparation
- Security of sensitive information
- Management of risk areas (email, Big Data)

**ARMA's GARP® Principles** — the foundational framework for Information Governance:

1. **Accountability** — Executive sponsorship
2. **Transparency** — Processes are documented and auditable
3. **Integrity** — Information governance is reliable and authentic
4. **Protection** — Appropriate security for sensitive information
5. **Compliance** — Adherence to laws and regulations
6. **Availability** — Information is accessible when needed
7. **Retention** — Information retained for appropriate time
8. **Disposition** — Secure disposal at end of lifecycle

**Additional governance principles:**
- Assign executive sponsorship for accountability
- Educate employees on information governance responsibilities
- Classify information under correct record codes or taxonomy categories
- Ensure authenticity and integrity of information
- Default to electronic as the official record
- Align business systems and third parties to governance standards
- Store, manage, make accessible, monitor, and audit approved repositories
- Secure confidential/PII information
- Control unnecessary growth of information
- Dispose of information at end of lifecycle
- Comply with information requests (discovery, subpoena)
- Improve continuously

The **[Information Governance Reference Model (IGRM)](https://edrm.net)** complements GARP®, showing the relationship of Information Governance to other organizational functions through stakeholder roles and lifecycle components.

**Governance structure**: Cross-functional senior-level **Information Council** or **Steering Committee** responsible for strategy, operating procedures, technology/standards guidance, communications/training, monitoring, and funding. Policies are written for stakeholder areas; technology ideally enforces them.

### 6.2 Proliferation of Information

Unstructured data grows much faster than structured data. Challenges:
- Not attached to a business function or department — ownership difficult to ascertain
- Content classification difficult — business purpose not always inferable from the system
- Unmanaged unstructured data without Metadata represents risk (misinterpretation, mishandling, privacy concerns)

### 6.3 Govern for Quality Content

Requires effective partnership between [[DMBOK/03_Data_Stewardship|data stewards]], data management professionals, and records managers. Business data stewards help define web portals, enterprise taxonomies, search engine indexes, and content management issues.

**Understanding quality content context:**

| Dimension | Key Questions |
|---|---|
| **Producers** | Who creates the content and why? |
| **Consumers** | Who uses the information and for what purposes? |
| **Timing** | When is information needed? How frequently must it be updated/accessed? |
| **Format** | Do consumers need specific formats? Are some formats unacceptable? |
| **Delivery** | How will information be delivered/accessed? How will security be enforced? |

### 6.4 Metrics

#### 6.4.1 Records Management KPIs

**Strategic**: Compliance with regulatory requirements, governance compliance.

**Operational**: Resource costs, training metrics, SLA fulfillment, integration with business systems.

**Implementation success measures** (percentage-based):
- Total documents/email per user identified as corporate records
- Identified records declared and put under records control
- Stored records with proper retention rules applied

ARMA's GARP® and Information Governance Assessment platform guides KPI definition.

#### 6.4.2 E-discovery KPIs

- Cost reduction
- Efficiency gained (e.g., average turnaround time in days)
- Speed of legal hold notification (LHN) implementation

The **EDRM Metrics Model** uses elements of Volume, Time, and Cost, surrounded by seven aspects: Activities, Custodians, Systems, Media, Status, Format, and QA.

#### 6.4.3 ECM KPIs

**Tangible benefits**: Increased productivity, cost reduction, improved information quality, improved compliance.

**Intangible benefits**: Improved collaboration, simplified job routines and workflow.

**Operational metrics**: Downtime, number of users, storage utilization, search retrieval performance.

**Information retrieval is measured by:**
- **Precision**: Proportion of retrieved documents that are actually relevant
- **Recall**: Proportion of all relevant documents that are actually retrieved

**Long-term business value KPIs:**

| Category | Example KPIs |
|---|---|
| **Financial** | ECM system cost, reduced physical storage costs, operational cost decrease |
| **Customer** | First-contact resolution rate, complaint numbers |
| **Internal Process** | Paperwork reduction %, error reduction via workflow automation |
| **Training** | Number of training sessions, participants |
| **Risk Mitigation** | Discovery cost reduction, audit trails tracking e-discovery requests |

---

## 7. References

- Boiko, Bob. *Content Management Bible*. 2nd ed. Wiley, 2004.
- Diamond, David. *Metadata for Content Management*. CreateSpace, 2016.
- Hedden, Heather. *The Accidental Taxonomist*. Information Today, Inc., 2010.
- Lambe, Patrick. *Organising Knowledge: Taxonomies, Knowledge and Organisational Effectiveness*. Chandos Publishing, 2007.
- Liu, Bing. *Web Data Mining*. 2nd ed. Springer, 2011.
- Nichols, Kevin. *Enterprise Content Strategy: A Project Guide*. XML Press, 2015.
- Read, Judith and Mary Lea Ginn. *Records Management*. 9th ed. Cengage Learning, 2015.
- Rockley, Ann and Charles Cooper. *Managing Enterprise Content: A Unified Content Strategy*. 2nd ed. New Riders, 2012.
- Smallwood, Robert F. *Information Governance: Concepts, Strategies, and Best Practices*. Wiley, 2014.
- ANSI/NISO Z39.19-2005 — Guidelines for the Construction, Format, and Management of Monolingual Controlled Vocabularies
- ISO 15489 — Records Management
- ISO 16175 — Principles and Functional Requirements for Records in Electronic Office Environments
- ISO 25964 — Thesauri and Interoperability with Other Vocabularies
- DoD 5015.2 — Electronic Records Management Software Applications Design Criteria Standard
- MoReq2 — Model Requirements for the Management of Electronic Records
- ARMA International, *Generally Accepted Recordkeeping Principles® (GARP®)*
- ARMA International, *Information Governance Maturity Model*
- EDRM (Electronic Discovery Reference Model), edrm.net

---

## See Also

- [[DMBOK/02_Data_Governance]] — Information governance frameworks and policies
- [[DMBOK/06_Data_Security]] — Sensitive data protection, confidentiality classifications
- [[DMBOK/10_Reference_and_Master_Data]] — Controlled vocabularies, taxonomies, reference data
- [[DMBOK/11_Metadata_Management]] — Metadata for structured and unstructured content
- [[DMBOK/12_Data_Quality]] — Quality dimensions for content and records
- [[DMBOK/05_Data_Storage_and_Operations]] — Backup, recovery, business continuity
