---
tags:
  - architecture
  - cloud
  - edge
  - microservices
  - software-architecture
source: "Bass, Clements & Kazman — Software Architecture in Practice, Chapters 26–27"
created: 2026-07-21
---

# 11 — Cloud and Edge Architecture

## Chapter 26: Architecture in the Cloud

### 26.1 Overview

The cloud provides an **elastic set of resources** through virtual machines, virtual networks, and virtual file systems. It has become a viable hosting alternative primarily for economic reasons. The cloud can be used to provide infrastructure, platforms, or services — each with its own characteristics.

### 26.2 Cloud Service Models

Three fundamental service models:

| Model | What's Provided | Consumer Manages |
|-------|-----------------|------------------|
| **IaaS** (Infrastructure as a Service) | Virtual machines, storage, networking | OS, deployed applications, limited networking |
| **PaaS** (Platform as a Service) | Pre-provisioned development stack (language runtime, DB, web server) | Only the application code |
| **SaaS** (Software as a Service) | Complete application | Nothing (end-user consumption) |

### 26.2 Deployment Models

| Model | Ownership |
|-------|-----------|
| **Private Cloud** | Single organization, for its own applications only |
| **Public Cloud** | Owned by an organization selling cloud services to the general public |
| **Community Cloud** | Shared by several organizations with shared concerns (mission, compliance, security) |
| **Hybrid Cloud** | Composition of 2+ clouds (private + public); enables **cloud bursting** — offloading demand spikes to public cloud |

### 26.3 Economic Justification

Three economic distinctions that can yield up to **80% cost savings** for a 100K-server data center vs. a 10K-server one:

#### Economies of Scale
- **Power costs** (15–20% of total): large buyers negotiate ~50% discounts; can locate near cheap energy
- **Infrastructure labor**: one admin services ~140 servers in traditional DC vs. **thousands** in cloud DC (automation)
- **Security & reliability**: fixed investment amortized over more servers
- **Hardware costs**: up to 30% volume discounts

#### Utilization of Equipment
Non-virtualized DCs run **one app per server** at **10–15% utilization**. Virtualization enables co-location and higher utilization by exploiting five variation sources:

1. **Random access** — more users → more uniform load
2. **Time of day** — co-locate workplace (day) and consumer (evening) apps
3. **Time of year** — predictable seasonal spikes (Christmas, tax season)
4. **Resource usage patterns** — co-locate CPU-heavy and storage-heavy apps
5. **Uncertainty** — demand spikes (news events, startup growth); addressed by **cloud bursting**

#### Multi-Tenancy
Single application instance supporting distinct user sets (e.g., Salesforce, Office 365). Benefits:
- Updates available to all users simultaneously → no version incompatibilities
- Upgrade costs amortized across all users, not borne by each IT department

### 26.4 Base Mechanisms

#### Hypervisor
The OS that creates and manages virtual machines. Each VM has its own guest OS; the application runs under **two OS layers** (hypervisor + guest OS). Key services:

- **Page Mapper** — adds a second level of indirection: consumer app address → VM page table → hypervisor page table → physical address
- **Scheduler** — decides which VM gets the CPU (e.g., round-robin); real-time scheduling is an active research area for embedded virtualization

#### Storage: Hadoop Distributed File System (HDFS)

**Component-and-Connector view:**
- **NameNode** (one per cluster) — manages metadata only; not involved in data transfer
- **DataNodes** (multiple) — store actual data blocks
- **Client library** — buffers writes until 64 MB block collected, then streams to DataNodes

**Key design decisions:**
- **No locking** — single-writer, multiple-reader model
- **64 MB blocks** only — no substructure; application manages typing
- **Default replication = 3** — replicas spread across racks; lightly loaded DataNodes preferred
- **Pipeline write**: Client → DataNode₁ → DataNode₂ → DataNode₃

> Pattern: moving application-specific functionality **up the stack** rather than down into infrastructure.

#### Network: IP Addressing and NAT
- **IPv4**: 32-bit address (e.g., `192.0.2.235`); **IPv6**: 128-bit
- **Public IP**: unique on the Internet
- **Private IP**: reused across organizations; accessed through a gateway
- **NAT (Network Address Translation)**: gateway replaces internal source address with its own public IP for outgoing messages, reverses for incoming

### 26.5 Sample Technologies

#### IaaS Platform Architecture (Allocation View)

```
Web Browser / SOAP-REST tools
        ↓
  Virtual Resource Manager  ←→  Persistent Object Manager
        ↓
  ┌──────────┐    ┌──────────┐
  │ Cluster  │    │ Cluster  │
  │ Manager  │    │ Manager  │
  │   +      │    │   +      │
  │ File Sys │    │ File Sys │
  │ Manager  │    │ Manager  │
  └──────────┘    └──────────┘
        ↓               ↓
   NodeManagers     NodeManagers
```

Key components and responsibilities:

| Component | Responsibility |
|-----------|---------------|
| **Virtual Resource Manager** | Gateway; routes new-resource requests to cluster managers; forwards messages to correct instances |
| **Persistent Object Manager** | Maintains files across clusters/geographies; survives VM deletion |
| **Cluster Manager** | Controls VM execution within cluster; manages virtual networking |
| **File System Manager** | Per-cluster file system; replicates blocks; handles failures |
| **Node Manager** (Hypervisor) | Controls individual VM lifecycle (execution, inspection, termination) |

**Automatic services offered:** IP reallocation on failure, auto-scaling, failure recovery for infrastructure (but **not** for applications — architect's responsibility).

#### Platform as a Service (PaaS)
Pre-provisioned integrated stack (e.g., **LAMP**: Linux, Apache, MySQL, PHP/Perl/Python). Adds: automatic scaling, failure detection/recovery, backup/restore, security patches, built-in persistence.

- **Google App Engine**: Python/Java environment + auto-replicated DB
- **Microsoft Azure**: .NET on Windows + automatic scaling/replication; detects instance failure and redeploys automatically

#### NoSQL Databases

Forces driving emergence of NoSQL:
1. **Massive web data** — billions of records, sequential processing; RDBMS indexing/query optimization unnecessary
2. **Admin complexity** — traditional RDBMS requires skilled DBAs
3. **CAP theorem** — can't simultaneously achieve Consistency, Availability, and Partition tolerance; many systems sacrifice **consistency** for **immediate availability** ("eventual consistency")
4. **Relational model limitations** — joins are expensive; denormalized storage preferred

**HBase** (Key-Value, based on Google BigTable):
- Schemaless tables; indexed by `(row, column, timestamp)`
- Multiple versions of same `(row, column)` keyed by timestamp
- Application manages version selection
- Use case: web crawling (URL = row, attributes = columns, timestamp = crawl time)

**MongoDB** (Document-Centric):
- Stores **objects** (JSON/BSON), not tables
- Objects may share some, all, or no field names
- Links support "joining" without indices/query optimization (application follows links)
- Fields indexed wherever they occur

**What NoSQL leaves out** (application must implement if needed):
- **Schemas** — no field-name consistency checking
- **Transactions** — use timestamps to detect concurrent modifications
- **Consistency** — "eventually consistent"; successive queries may return different values
- **Normalization / Joins** — not supported

### 26.6 Architecting in a Cloud Environment

Architecturally significant quality attributes with cloud-specific differences:

#### Security
Beyond standard Internet security, **multi-tenancy** introduces four attack vectors:

| Attack | Description |
|--------|-------------|
| **Inadvertent information sharing** | Residual data on shared physical resources leaks between tenants |
| **VM escape** | Exploiting hypervisor bugs to cross VM boundaries (extremely rare) |
| **Side-channel attacks** | Deducing keys via cache-timing analysis (primarily academic) |
| **Denial-of-service** | Co-tenant consumes host resources, starving your application |

**Mitigation:** Some providers allow reserving entire machines (defeats some cloud economics but prevents multi-tenancy attacks).

#### Performance
- **Instantaneous capacity varies** with co-tenant load → applications must self-monitor
- **Elasticity**: acquire/release resources on demand; takes time to allocate/free
- Applications should have their **own usage model** — better than general provider algorithms — to predict scaling needs
- Consider **charging algorithm** when allocating/freeing resources

#### Availability
- Everything can fail: physical hosts, virtual networks
- Amazon EC2 SLA: **99.95%** — architect must plan for the **0.05%**
- **Netflix case study** (survived 4-day EC2 outage unnoticed by customers):
  - **Stateless services** — any instance serves any request (spare tactic)
  - **Data across zones** — multiple redundant hot copies (active redundancy)
  - **Graceful degradation** — fail fast (aggressive timeouts), fallbacks (lower-quality representation), feature removal (noncritical features removed)

#### The CAP Theorem (Brewer)
For distributed systems managing shared data, only **two of three** can be fully satisfied:

| Property | Meaning |
|----------|---------|
| **C**onsistency | Data is consistent throughout the distributed system |
| **A**vailability | Data is highly available |
| **P**artition tolerance | System tolerates network partitioning |

In practice:
- **Latency** has become a de facto fourth concern (CLAP)
- Most large-scale systems favor **availability + latency over consistency**
- **"Eventual consistency"** — partitions become temporarily inconsistent within engineered bounds
- The tradeoff space is **richer and more subtle** than simply "pick two"

---

## Chapter 27: Architectures for the Edge

### 27.1 The Ecosystem of Edge-Dominant Systems

An **edge-dominant system** depends crucially on user inputs for its success. Examples: Wikipedia, YouTube, Facebook, Twitter, Flickr. Value comes *from* users, not *for* users.

#### The Metropolis Model

```
      ┌──────────────────────────┐
      │     End Users (Masses)    │  ← contribute requirements, not content
      │  ┌────────────────────┐   │
      │  │  Developers &       │   │  ← produce software/content
      │  │  Prosumers (Edge)   │   │
      │  │  ┌──────────────┐   │   │
      │  │  │   CORE        │   │   │  ← platform, APIs, kernel
      │  │  └──────────────┘   │   │
      │  └────────────────────┘   │
      └──────────────────────────┘
```

Three stakeholder communities:
- **Customers / End Users** — consume value
- **Developers** — write software and key content
- **Prosumers** — both produce and consume content

Three realms:
- **Outer ring (Masses)** — end users contributing requirements
- **Middle ring (Edge)** — developers and prosumers creating value
- **Core** — software platform providing APIs (Linux kernel, Apache core, Wikipedia wiki, Facebook platform, iPhone/Android)

**Permeability:**
- *Open source* (Linux, MySQL, Apache): possible to move from user → developer → core architect (meritocracy)
- *Open content* (Wikipedia, YouTube, Facebook): no path into core development regardless of contribution volume

### 27.2 Changes to the Software Development Life Cycle

Traditional SDLC assumptions **break** in edge-dominant systems:

| Traditional Assumption | Edge-Dominant Reality |
|------------------------|----------------------|
| **Requirements can be known** | Requirements emerge from individuals operating independently; they conflict and are never globally knowable |
| **Software is developed in planned increments/releases** | System is constantly changing; no "latest release" — parts are created, modified, torn down at all times |
| **Projects have dedicated finite resources** | Staffed by volunteers; subject to whims; but large numbers ameliorate individual unreliability; no natural limit on resources |
| **Management can "manage" resources** | Developers are volunteers; no authority to order action — only lead and persuade (Linus Torvalds: "shepherding") |

### 27.3 Implications for Architecture

The key architectural choice: **bifurcation into core and periphery**.

#### The Core/Periphery Pattern

```
┌─────────────────────────────────────┐
│           PERIPHERY                 │
│  (crowdsourced, rapidly changing,   │
│   delivers end-user value)          │
│  ┌─────────────────────────────┐    │
│  │          CORE                │    │
│  │  (small, tightly controlled, │    │
│  │   slow-changing, quality-    │    │
│  │   attribute focused)         │    │
│  └─────────────────────────────┘    │
└─────────────────────────────────────┘
```

**Examples:** Linux (kernel = core, applications/libraries = periphery), Firefox, Apache. Linux applies this pattern **twice**: once between kernel and apps, again *within* the kernel (modular subsystems).

**Requirements split:**
- **Core requirements** — deliver little end-user value directly; focus on quality attributes (performance, modularity, security, reliability); slow to change
- **Periphery requirements** — unknowable in advance; contributed by the peer network; deliver majority of end-user function/value; change rapidly

**Three implications for core design:**

1. **Highly modular** — enables independent contributions by distributed, unknown-to-each-other programmers. The core constrains and enables but does not specify periphery.

2. **Highly reliable** — core is small, controlled, slow-changing, heavily tested. If core can't be small, components must be as independent as possible to ease testing.

3. **Highly robust to environmental errors** — periphery reliability is in hands of the community (masses as testers — Mozilla claims 3M). Failures in periphery **must not cause core failures**. Requires monitoring mechanisms (system state) and control mechanisms (quarantine bugs).

**Core as a platform (services):** Amazon EC2 has 110+ APIs. Requirements for serving peripheral developers:

- **Well-written, up-to-date documentation** — barrier to entry for volunteers
- **Discovery + Registration service** — navigate hundreds of services
- **Error detection** is extremely complicated (bugs anywhere in the service chain)
- **Throttling, monitoring, quotas** — every peer service is a potential DoS attacker

### 27.4 Implications of the Metropolis Model

Eight ways the Metropolis Model changes system development:

| # | Change | Description |
|---|--------|-------------|
| 1 | **Indifference to phases** | No waterfall/spiral/V-model phases; bull's-eye metaphor; focus on including periphery and masses |
| 2 | **Crowd management** | Align policies with strategic goals; crowds good for some tasks, not all; for-profit orgs must align tasks with volunteers' gift-culture values |
| 3 | **Core vs. periphery** | Different tools, processes, roles for each; core = small, tightly controlled, quality-focused; enables unbridled periphery growth |
| 4 | **Requirements process** | Requirements **asserted** (not elicited) by periphery through emails, wikis, forums; community creates requirements — not the core team |
| 5 | **Focus on architecture** | Architecture cannot "emerge" (as in Agile); must be **designed up front** by small, experienced team focusing on modularity + core QAs; lead architect with veto rights |
| 6 | **Distributed testing** | Core = heavily tested (small, nightly builds, frequent releases); periphery = "many eyes"; built-in bug reporting with low effort |
| 7 | **Automated delivery** | Flexible, asynchronous delivery; tolerant of incompleteness, older versions, multiple coexisting versions |
| 8 | **Management of the periphery** | Governance policies enforced proactively (IP assignment, app screening, API constraints) or reactively (editors, violation reporting, market punishment) |

**Zoning analogy:** A zoning board (governance) produces building codes; building inspectors (reactive enforcers) verify conformance. This maps to core/periphery governance — proactive constraints (APIs, standards) and reactive enforcement (editors, reporting).

### 27.5 Summary

- Edge-dominant systems depend on user participation for value ("crowdsourcing," "folksonomy")
- The **Metropolis Model** captures the ecosystem: masses → developers/prosumers → core
- Traditional SDLC is broken for edge systems: requirements emerge, no planned releases, volunteer resources, no central management
- The **Core/Periphery pattern** is the dominant architectural response: tightly controlled core + loosely controlled periphery
- Core must be **modular, reliable, and robust**; typically implemented as services with well-documented APIs, discovery mechanisms, and error detection
- The Metropolis Model demands: up-front architecture design, crowd-aligned governance, distributed testing, automated delivery, and proactive/reactive periphery enforcement

---

## Key Concepts Cross-Reference

| Concept | Chapter 26 (Cloud) | Chapter 27 (Edge) |
|---------|-------------------|-------------------|
| **Core platform** | IaaS/PaaS provide the platform | Core/periphery pattern; core is the platform |
| **Elasticity** | Acquire/release VMs on demand | Unbounded volunteer resources |
| **Multi-tenancy** | Shared physical resources; security risk | Shared community producing value |
| **Failure handling** | Architect's responsibility; statelessness, redundancy, degradation | Core must be robust to periphery failures; monitoring + control |
| **Modularity** | IaaS components (cluster mgr, node mgr, file sys) | Core must be highly modular for independent periphery contributions |
| **API-driven** | REST/SOAP for IaaS management | Well-documented APIs are the core/periphery contract |
| **CAP Theorem** | Consistency vs. Availability vs. Partition tolerance | N/A (but applies to distributed edge backends) |


## Related

- [[Software Architecture Overview]] — All architecture topics
- [[Microservice/Microservice Overview]] — Cloud-native microservice patterns
- [[06_Tactics_and_Patterns]] — Architecture patterns
- [[10_Economics_and_Product_Lines]] — Economic justification (CBAM)
