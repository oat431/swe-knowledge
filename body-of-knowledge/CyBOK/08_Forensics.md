---
title: "CyBOK v1.1 Chapter 9: Forensics"
aliases: [Digital Forensics, Forensic Science, CyBOK Forensics, Ch9 Forensics]
tags: [forensics, cyber-security, cybok, digital-forensics, evidence]
source: "CyBOK v1.1 Chapter 9"
author: "Vassil Roussev, University of New Orleans"
date: 2021-07
---

# 9. Forensics

> **Author:** Vassil Roussev, University of New Orleans

## Introduction

**Digital forensic science**, or digital forensics, is the application of scientific tools and methods to identify, collect and analyse digital (data) artifacts in support of legal proceedings. From a technical perspective, it is the process of identifying and reconstructing the relevant sequence of events that has led to the currently observable state of a target IT system or (digital) artifacts.

The importance of digital evidence has grown in lockstep with the fast societal adoption of information technology, which has resulted in the continuous accumulation of data at an exponential rate. Simultaneously, there has been rapid growth in network connectivity and the complexity of IT systems, leading to more complex behaviour that may need investigation.

This Knowledge Area provides a technical overview of digital forensic techniques and capabilities, and puts them into a broader perspective with regard to other related areas in the cybersecurity domain. Discussion on legal aspects is limited to general principles and best practices, as specifics vary across jurisdictions. The **Law & Regulation** Knowledge Area (Chapter 3) discusses specific concerns related to jurisdiction and the legal process to obtain, process, and present digital evidence.

---

## 9.1 Definitions and Conceptual Models

> **References:** [919, 920, 921, 922, 923]

Broadly, **forensic science** is the application of scientific methods to collect, preserve and analyse evidence related to legal cases. Historically, this involved the systematic analysis of physical material to establish causal relationships between various events, as well as to address issues of provenance and authenticity. The rationale behind it, **Locard's exchange principle**, is that physical contact between objects inevitably results in the exchange of matter, leaving traces that can be analysed to (partially) reconstruct the event.

### Digital Forensic Traces

A **digital (forensic) trace** is an explicit, or implicit, record that testifies to the execution of specific computations, or the communication and/or storage of specific data. These events can be the result of human-computer interaction, or the result of the autonomous operation of the IT system.

- **Explicit traces** directly record the occurrence of certain types of events as part of the normal operation of the system; most prominently, these include a variety of timestamped system and application event logs.
- **Implicit traces** allow the occurrence of some events to be deduced from the observed state of the system, or artifact, and engineering knowledge of how the system operates (e.g., the presence of a unique chunk of data that is part of a known file demonstrating that the file was likely present once and was subsequently deleted and partially overwritten).

**Key implication:** Although traces frequently exist, they are the result of conscious engineering decisions that are not usually taken to specifically facilitate forensics. This has important implications with respect to the provenance and authenticity of digital evidence, given the ease with which digital information can be modified.

### 9.1.1 Legal Concerns and the Daubert Standard

The first published accounts of misuse and manipulation of computer systems for illegal purposes date back to the 1960s. In the early-to-mid 1980s, targeted computer crime legislation emerged across Europe and North America.

In the UK, the **Computer Misuse Act 1990** defines computer-specific crimes:
- **S1** — Unauthorised Access To Computer Material
- **S2** — Unauthorised Access with Intent to Commit Other Offences
- **S3** — Unauthorised Acts with Intent to Impair Operation
- **S3A** — Making, Supplying or Obtaining

The Police & Criminal Evidence Act 1984 and Criminal Justice & Police Act 2001 address computer-specific concerns with respect to warrants, search and seizure.

#### Daubert Standard

The US supreme court, using three specific cases — Daubert v. Merrell Dow Pharmaceuticals (1993), General Electric Co. v. Joiner (1997), and Kumho Tire Co. v. Carmichael (1999) — established the **Daubert standard** for the presentation of scientific evidence in legal proceedings. The four basic criteria:

1. The theoretical underpinnings of the methods must yield testable predictions by means of which the theory could be falsified.
2. The methods should preferably be published in a peer-reviewed journal.
3. There should be a known rate of error that can be used in evaluating the results.
4. The methods should be generally accepted within the relevant scientific community.

#### ACPO Good Practice Guide for Digital Evidence

Four basic principles for the acquisition and handling of digital evidence:

1. **No action** taken by law enforcement agencies, persons employed within those agencies or their agents should change data which may subsequently be relied upon in court.
2. In circumstances where a person finds it necessary to access original data, that person must be **competent** to do so and be able to give evidence explaining the relevance and the implications of their actions.
3. An **audit trail** or other record of all processes applied to digital evidence should be created and preserved. An independent third party should be able to examine those processes and achieve the same result.
4. The **person in charge** of the investigation has overall responsibility for ensuring that the law and these principles are adhered to.

#### ISO Accreditation

In the UK, the forensic science regulator mandates that any provider of digital forensic science must be accredited to:
- **BS EN ISO/IEC 17020:2012** for any crime scene activity
- **BS EN ISO/IEC 17025:2005** for any laboratory function (such as the recovery or imaging of electronic data)

### 9.1.2 Definitions

In 2001, the first **Digital Forensics Research Workshop (DFRWS)** was organised in response to the need to replace the prevalent ad hoc approach to digital evidence with a systematic, multi-disciplinary effort.

**DFRWS Definition:**
> Digital forensics is the use of scientifically derived and proven methods toward the preservation, collection, validation, identification, analysis, interpretation, documentation and presentation of digital evidence derived from digital sources for the purpose of facilitating or furthering the reconstruction of events found to be criminal, or helping to anticipate unauthorised actions shown to be disruptive to planned operations.

**NIST Definition:**
> Digital forensics is the application of science to the identification, collection, examination, and analysis of data while preserving the integrity of the information and maintaining a strict chain of custody for the data.

**Working Definition:**
> Digital forensics is the process of identifying and reconstructing the relevant sequence of events that has led to the currently observable state of a target IT system or (digital) artifacts.

The notion of **relevance** is inherently case-specific. A critical component of forensic analysis is the **causal attribution** of event sequences to specific human actors or system entities. The provenance, reliability, and integrity of the data used as evidence is of **primary importance**.

### 9.1.3 Conceptual Models

There are two possible approaches to rebuilding the relevant sequence of events:

| Approach | Starting Point | Challenge |
|---|---|---|
| **State-centric** | Snapshot of the system state (e.g., hard drive content) | Dearth of historical data points |
| **Log-centric / History-centric** | Explicit, timestamped history of events (logs) | Sifting through millions of log entries |

Historically, the principal approach to forensics has been predominantly **state-centric**. However, over the last ten to fifteen years, technology advances have made storage and bandwidth plentiful, leading to a massive increase in log data. The current period marks an important evolution in digital forensic methodology toward **log-centric approaches**.

#### 9.1.3.1 Cognitive Task Model

The **Pirolli & Card cognitive model** describes the sense-making process for intelligence analysis — a cognitive task very similar to forensic analysis. The model maps the investigative process:

```
External Data → Shoebox → Evidence File → Schema → Hypothesis → Presentation
     (raw)       (filtered)   (relevant)   (organized)  (tentative)   (final)
```

Two main activity loops:
- **Foraging loop:** Finding potential sources of information, querying and filtering for relevance
- **Sense-making loop:** Developing a conceptual model supported by the evidence in an iterative fashion

Information transformation processes can be classified as:
- **Bottom-up (synthetic):** Building higher-level representations from specific pieces of evidence
- **Top-down (analytical):** Using partial conclusions to drive the search for supporting and contradictory evidence

##### Bottom-Up Processes

1. **Search and filter** — External data sources are searched based on keywords, time constraints
2. **Read and extract** — Collections are analysed to extract individual facts and relationships
3. **Schematize** — Facts are organised into schemas (timelines, graphs, charts) that help identify significance
4. **Build case** — Testable theories/working hypotheses that explain the evidence
5. **Tell story** — Final report and oral presentation in court

##### Top-Down Processes

1. **Re-evaluate** — Feedback from clients may necessitate re-evaluations
2. **Search for support** — Hypothesis testing against alternative explanations
3. **Search for evidence** — Re-evaluation of evidence significance/provenance
4. **Search for relations** — New searches for facts and relations
5. **Search for information** — New sources or re-examination of filtered-out information

#### 9.1.3.6 Data Extraction vs. Analysis vs. Legal Interpretation

- **Tool developers** provide the means to acquire digital evidence, extract data objects, and essential tools to search, filter, and organize it.
- **Forensic investigators** are the primary users employing these capabilities to analyse specific cases and present legally-relevant conclusions.
- **Legal experts** operate in the upper right-hand corner — building/disproving legal theories.

#### 9.1.3.7 Forensic Process

The defining characteristic of forensic investigations is that their results must be **admissible in court**. This entails:

##### Data Provenance and Integrity
- Acquiring a truthful copy of the evidence from the original source using validated tools
- Keeping custodial records and detailed case notes
- Using validated tools to perform the analysis
- Cross-validating critical pieces of evidence
- Correctly interpreting the results based on peer-reviewed scientific studies

The traditional **gold standard** is a bit-level copy of the forensic target. As storage devices increase in complexity and encryption becomes default, a (partial) logical acquisition may be the only possibility.

##### Scientific Methodology
- **Reproducibility** — Starting with the same data and following the same process should allow a third party to arrive at the same result.
- Processing methods should have scientifically established **error rates**.
- Different forensic tools implementing the same type of data processing should yield identical results, or results within known statistical error boundaries.

##### Tool Validation
Forensic tool validation is a scientific and engineering process that subjects specific tools to systematic testing to establish the validity of the results produced. **NIST** maintains the Computer Forensic Tool Testing (CFTT) project.

##### Forensic Procedure
Strict adherence to established standards and court-imposed restrictions is the most effective means of demonstrating that the results of the forensic analysis are truthful and trustworthy.

##### Triage
**Triage** is a partial forensic examination conducted under (significant) time and resource constraints. The focus is to quickly identify relevant data and filter out the irrelevant. Triage results are inherently less reliable than a deep examination.

---

## 9.2 Operating System Analysis

> **References:** [923, 938, 939]

Modern computer systems follow the original **von Neumann architecture** — CPU, main memory, and secondary storage connected via data buses. The actual investigative targets are the different OS modules controlling the hardware subsystems and their respective data structures.

The OS functions at a higher level of privilege relative to user applications and directly manages all the computer system's resources. System analysis employs knowledge of how operating systems function to reach conclusions about events and actions of interest.

### 9.2.1 Storage Forensics

Persistent storage (HDDs, SSDs, optical disks, external USB-connected media) is the primary source of evidence for most digital forensic investigations.

#### 9.2.1.1 Data Abstraction Layers

Computer systems organise raw storage in successive layers of abstraction. Forensic analysis can be performed at several levels:

| Layer | Description |
|---|---|
| **Physical Media** | Lowest level — extracting data bit by bit. May involve chip-off approaches, JTAG interfaces, or Host Bus Adapter (HBA) protocols (SATA, SCSI, NVMe). |
| **Block Device** | Medium presented as a sequence of fixed-size blocks (512 or 4096 bytes). Typical data acquisition (imaging) works at this level. |
| **File System** | Organises block storage into a file-based store with files, directories, metadata (name, size, owner, timestamps, access permissions). |
| **Application Artifacts** | User applications store artifacts — documents, images, messages. Application-level analysis yields the most immediately relevant results. |

**Why independent forensic reconstruction is critical:**
1. Enables recovery of evidentiary data not available through the normal data access interface
2. Forms the basis for recovering partially overwritten data
3. Allows discovery and analysis of malware agents that have subverted normal system functioning

### 9.2.2 Data Acquisition

Best practices dictate that analysing data at rest is **not carried out on a live system**. The target machine is powered down, an exact bit-wise copy of the storage media is created, the original is stored in an evidence locker, and all forensic work is performed on the copy.

#### Physical vs. Logical Acquisition

| Type | Description |
|---|---|
| **Physical acquisition** | Obtaining data directly from hardware media, without the mediation of any (untrusted) third-party software. Block-level copy. |
| **Logical acquisition** | Relies on one or more software layers as intermediaries (API or message protocol) to acquire data. Higher level of abstraction, closer to user actions. |

**Block-level acquisition tools:** The workhorse is the `dd` Unix/Linux utility. A hardware **write blocker** is often used to eliminate the possibility of operator error.

**Cryptographic hashes** are computed for the entire image and preferably for every block. NIST maintains the **Computer Forensic Tool Testing (CFTT)** project.

#### Encryption Concerns

Two possible paths to obtaining encrypted data:
- **Technical:** Finding algorithmic, implementation, or administrative errors that allow data protection to be subverted
- **Legal:** Compelling the person with knowledge of the relevant encryption keys to surrender them (varies across jurisdictions; in the UK, the Regulation of Investigatory Powers Act 2000 applies)

### 9.2.3 Filesystem Analysis

A **file system** is an OS subsystem responsible for the persistent storage and organisation of user and system files on a partition/volume. It provides a high-level standard API (such as POSIX).

#### Key Concepts

| Concept | Definition |
|---|---|
| **Block/Sector** | Smallest unit of data transfer (historically 512 bytes, now commonly 4096 bytes) |
| **Cluster** | Contiguous sequence of blocks; smallest unit for storage allocation/reclamation |
| **Partition** | Contiguous area of raw drive for a designated use |
| **Volume** | Physical volume maps onto a single partition; logical volume integrates multiple partitions |
| **File** | Named (opaque) sequence of bytes stored persistently |

**Filesystem forensics** uses knowledge of the filesystem's data structures and algorithms to:
1. Extract data content from devices independently of the OS instance that created it
2. Extract leftover artifacts to which the regular filesystem API does not offer access

### 9.2.4 Block Device Analysis

- A **partition** (or physical volume) is a contiguous allocation of blocks for a specific purpose, such as the organisation of a filesystem.
- A **logical volume** is a collection of physical volumes presented and managed as a single unit. Logical volumes allow storage capacity pooling and automated block-level replication (RAIDs).

### 9.2.5 Data Recovery & File Content Carving

**File (content) carving** is the process of recovering and reconstructing file content directly from block storage without using the filesystem metadata. More generally, **data (structure) carving** is the process of reconstructing logical objects (such as files and database records) from a bulk data capture (disk/RAM image) without using metadata.

The basic carving algorithm:
1. Scan the capture sequentially until a known **header** is found (e.g., JPEG: `FF D8 FF`)
2. Scan sequentially until a corresponding **footer** is found (e.g., JPEG: `FF D9`)
3. Copy the data in between as the recovered artifact

#### Common File Content Layouts During Carving

| Layout | Description |
|---|---|
| **No fragmentation** | Most typical; file content is contiguous |
| **Nested content** | Often result of deletion; solved by multiple passes |
| **Bi-fragmented** | File split into two contiguous pieces with other content in between |
| **Interleaved content** | Larger files fill gaps created by deletion of smaller ones |

#### Slack Space Recovery

**Slack space** is the difference between the allocated storage for a data object and the storage in actual use. It can be used to hide data that would be inaccessible via the standard filesystem interface.

#### Upcoming Challenges

As SSDs continue to grow in capacity, file carving's utility is set to diminish. **TRIM** and **UNMAP** commands allow the filesystem to indicate which blocks need garbage collection. King & Vidas established experimentally that file carving would only work in a narrow set of circumstances on modern SSDs. For a TRIM-aware OS (Windows 7+), data recovery rates are almost universally zero.

---

## 9.3 Main Memory Forensics

> **References:** [949]

The early view of best forensic practices was to literally pull the plug. However, studies have demonstrated that data tends to persist for a long time in volatile memory, and the loss of important forensic information (open connections, encryption keys) was rarely justified.

### Information Extractable from Memory Snapshots

| Type | Content |
|---|---|
| **Process information** | Running processes, threads, loaded system modules; code, stack, heap, data segments |
| **File information** | Open files, shared libraries, shared memory, anonymously mapped memory |
| **Network connections** | Open and recently closed connections, protocol information, sending/receiving queues |
| **Artifacts and fragments** | Cached disk and network data traces |

### Live vs. Snapshot Analysis

| Method | Characteristics |
|---|---|
| **Live forensics** | Trusted agent pre-installed; remote operator has full control. Primarily used in large enterprise deployments. Main problem: if compromised, results are not trustworthy. |
| **Snapshot (memory dump)** | Analysis of RAM capture. More difficult than live analysis (no APIs available). Forensic tools need to rebuild semantic information from raw capture — this is the **semantic gap problem**. |

Memory content can also be obtained from a system hibernation file, page swap, or a crash dump.

---

## 9.4 Application Forensics

**Application forensics** is the process of establishing a data-centric theory of operation for a specific application. The goal is to objectively establish causal dependencies between data input and output, as a function of user interactions with the application.

The analytical effort required varies from reading detailed specifications (open source) to reverse engineering code, data structures, and communication protocols (closed source), or performing time-consuming black-box differential analysis experiments.

The big advantage: better chance of observing and documenting **direct evidence of user actions**, which is of primary importance to the legal process.

### 9.4.1 Case Study: the Web Browser

Six main sources of forensically interesting information:

| Source | Forensic Value |
|---|---|
| **URL/search history** | Complete browsing history; search queries encoded in URLs provide clear clues about user intent |
| **Form data** | Auto-completed passwords and other form data (address, credit card info) |
| **Temporary files** | Local file cache provides chronology of web activities with stored versions of downloaded objects |
| **Downloaded files** | By default, never deleted — valuable source of activity information |
| **HTML5 local storage** | Standard means for web applications to store information locally |
| **Cookies** | Opaque data used by servers; may be time-limited access tokens to online accounts |

Most local browser information is stored in **SQLite databases**, which provide a secondary target for data recovery — ostensibly deleted records may persist until the database is explicitly 'vacuumed'.

---

## 9.5 Cloud Forensics

Cloud computing brings both disruptive challenges for current forensic tools, methods and processes, as well as qualitatively new forensic opportunities.

### 9.5.1 Cloud Basics

Cloud systems have five essential characteristics: **on-demand self-service, broad network access, resource pooling, rapid elasticity, and measured service**.

Three canonical service models:

| Model | Description | Ownership Split |
|---|---|---|
| **SaaS** (Software as a Service) | Application delivered over the web | CSP manages everything below application |
| **PaaS** (Platform as a Service) | Platform for developing/deploying apps | Customer manages applications and data |
| **IaaS** (Infrastructure as a Service) | Virtualised compute resources | Customer manages OS, middleware, applications |

### 9.5.2 Forensic Challenges

| Challenge | Description |
|---|---|
| **Logical acquisition is the norm** | Physical acquisition is almost completely inapplicable to the cloud. Cloud service APIs are emerging as the primary new interface for data acquisition. |
| **The cloud is the authoritative data source** | Cloud services store the primary historical record of computations. Most residual information on the client is transient and of uncertain provenance. |
| **Logging is pervasive** | Cloud-based software enables extensive logging of every aspect of the system. Logs are becoming the primary source of forensic information. |
| **Distributed computations are the norm** | Computation is split between client and server, with the server performing the heavy computational lifting. The vast majority of the established forensic toolchain becomes irrelevant. |

### 9.5.3 SaaS Forensics

The traditional analytical model of digital forensics is **physical device-centric**. The new software delivery model — **Software as a Service (SaaS)** — completely breaks with this approach. Computation is split between client and server, code and data are downloaded on demand and have no persistent place on the client.

#### Case Study: Cloud Drive Acquisition

Three major concerns with client-side acquisition of cloud drives:

1. **Partial replication** — No guarantee that any client has a complete copy; cloud metadata is needed to ascertain contents
2. **Revision acquisition** — Most drive services provide revision history that only resides in the cloud; client-side acquisition misses prior revisions
3. **Cloud-native artifacts** — Web-based artifacts (e.g., Google Docs) have no serialised representation in the local filesystem; only an opaque link exists locally

**Conclusion:** A new approach is needed — one that obtains the data directly from the cloud service.

---

## 9.6 Artifact Analysis

> **References:** [954, 955, 956, 957, 958]

### 9.6.1 Finding a Known Data Object: Cryptographic Hashing

**Cryptographic hashing** is the first tool of choice when investigating any case. It provides a basic means of validating data integrity and identifying known artifacts.

**Key properties:**
- A hash function takes an arbitrary string of binary data and produces a **digest** within a predefined range
- **Collision-resistant** — computationally infeasible to find two different inputs with the same output
- Standard functions: MD5, RIPEMD-160, SHA-1, SHA-2, SHA-3 (128- to 512-bit results)

**Usage:**
- **Integrity validation:** Compare before-and-after hashes at important points in the investigation (demonstrating chain of custody)
- **Known file identification:** Remove common files (OS/application installations) or pinpoint known files of interest (malware, contraband)

**NIST** maintains the **National Software Reference Library (NSRL)** — covering the most common OS installation and application packages.

### 9.6.2 Block-Level Analysis

To discover known file remnants (from partially overwritten deleted files), the granularity of hashes is increased by splitting files into **fixed-size blocks** (commonly 4 KiB) and storing the hash for each individual block.

A block is considered **distinct** if the probability that its exact content arises by chance more than once is vanishingly small. High-entropy data (e.g., compressed content) tends to produce distinct blocks. Block hashes can also be used to improve file carving results.

### 9.6.3 Approximate Matching

**Approximate Matching (AM)** is a generic term describing any technique designed to identify similarities between two digital artifacts.

#### Two Variations

| Type | Description |
|---|---|
| **Resemblance** | Comparing two comparably-sized objects to infer how closely related they are. Applications: object similarity detection, cross-correlation. |
| **Containment** | Comparing artifacts with large size disparity to establish whether a larger one contains (pieces of) a smaller one. Applications: embedded object detection, fragment detection. |

#### Three Classes of AM Algorithms

| Class | Description |
|---|---|
| **Bytewise matching** | Considers objects as byte sequences; no parsing. Small changes → small serialised changes (e.g., plain text) means good correlation. |
| **Syntactic matching** | Relies on parsing the format of an object to split it into logical features (e.g., zip archive, PDF). More accurate but more specialised. |
| **Semantic matching** | (Partially) interprets data content to derive semantic features. Examples: perceptual hashes (visually similar images), NLP (text document similarity). |

A **similarity digest** (fingerprint/signature) is a compressed representation of the feature set, often employing hashing to minimise footprint and facilitate fast comparison.

### 9.6.4 Cloud-Native Artifacts

Analysis of **cloud-native artifacts** — data objects that maintain the persistent state of web/SaaS applications — is a new and promising area. Unlike traditional applications where persistent state takes the form of files, web apps download necessary state on the fly.

From a forensic perspective, the most interesting API calls involve **complete state transfer** (e.g., opening a document triggers transfer of its full content). Cloud artifacts often have a completely different structure — for example, Google Docs documents are internally represented as the complete history (log) of every editing action. Obtaining only a PDF snapshot ignores potentially critical information on the evolution of a document over time.

---

## 9.7 Conclusion

Digital forensics identifies and reconstructs the relevant sequence of events that has led to a currently observable state of a target IT system or (digital) artifacts. The **provenance and integrity** of the data source and the **scientific grounding** of the investigative tools and methods employed are of primary importance in determining their admissibility to a court of law.

Following the rapid cloud-based transition from **Software as a Product (SaaP)** to **Software as a Service (SaaS)**, forensic methods and tools are also in transition:

- **State-centric → Log-centric analysis:** A change of emphasis from deducing events by looking at snapshots to employing explicitly collected log entries
- **Physical → Logical acquisition:** Transition from low-level physical acquisition of storage device images to high-level logical acquisition of application artifacts via cloud service APIs

Some of the most important emerging questions in digital forensics:
- Analysis of the large variety of **IoT devices** (forecast to reach 125 billion by 2030)
- Employment of **machine learning/AI** to automate and scale up forensic processing

---

## Cross-Reference of Topics vs. Reference Material

| Section | Topics | Key References |
|---|---|---|
| 9.1 Definitions and Conceptual Models | Legal standards, Daubert, ACPO, DFRWS definitions, cognitive models | [919–923] |
| 9.2 Operating System Analysis | Storage forensics, data acquisition, filesystem analysis, carving, slack space | [923, 938, 939] |
| 9.3 Main Memory Forensics | RAM acquisition, live vs. snapshot analysis, semantic gap | [949] |
| 9.4 Application Forensics | Web browser forensics, SQLite recovery | — |
| 9.5 Cloud Forensics | SaaS/PaaS/IaaS challenges, cloud drive acquisition, cloud-native artifacts | — |
| 9.6 Artifact Analysis | Cryptographic hashing, block-level analysis, approximate matching, NSRL | [954–958] |

---

## See Also

- **Security Operations & Incident Management** (Chapter 8) — complementary operational perspective
- **Law & Regulation** (Chapter 3) — legal processes for digital evidence
- **Malware & Attack Technologies** (Chapter 6) — malware detection and analysis
- **Network Security** — network-based evidence collection and analysis
- **Software Security** (Chapter 10) — relates to application-level forensic analysis
- **Hardware Security** — physical acquisition and hardware-level forensics
