---
yaml_tags: [distributed-systems, cyber-security, cybok]
source: "CyBOK v1.1, Chapter 12: Distributed Systems Security"
author: "Neeraj Suri, Lancaster University"
date: "July 2021"
knowledge_area: "Distributed Systems Security"
chapter: 12
---

# 11. Distributed Systems Security (CyBOK Ch12)

## Overview

A **distributed system** is a composition of geo-dispersed resources (computing and communication) that collectively:
- Provides services linking dispersed data producers and consumers
- Delivers on-demand, highly reliable, highly available, and consistent resource access, often using replication schemes to handle resource failures
- Enables a collective aggregated capability from distributed resources to provide an illusion of a logically centralised/coordinated resource or service

Distributed systems security addresses threats arising from the exploitation of vulnerabilities in the attack surfaces created across the resource structure and functionalities of the distributed system. This covers risks to data flows, access control mechanisms, data transport, middleware resource coordination services, and the distributed applications built on them.

---

## 12.1 Classes of Distributed Systems and Vulnerabilities

### 12.1.1 Classes of Distributed Systems

Distributed systems can be broadly classified into **two categories** based on their coordination schema:

#### 1. Decentralised Point-to-Point Interactions (P2P)

Systems without a centralised coordination service. Decentralised un-timed control is a prominent characteristic.

**Examples:** **Kademlia**, Napster, **Gnutella**, distributed file/music sharing systems, wireless sensor networks, online gaming systems.

#### 2. Coordinated Clustering Across Distributed Resources and Services

This broad class sub-divides into:
- **(a) Coordination of resources** — e.g., virtual machine orchestration, elastic scaling
- **(b) Coordination of services** — e.g., transactional databases, ledgers

**Examples:** **Client-Server** models, n-Tier Multi-tenancy models, elastic on-demand geo-dispersed aggregation of resources (Clouds — public, private, hybrid, multi-Cloud), Big Data services, High Performance Computing, transactional services such as **Databases**, **Ledgers**, Storage Systems, Key Value Stores (**KVS**). The **Google File System**, **Amazon Web Services**, **Azure**, and **Apache Cassandra** are examples.

Both sub-classes are typically coordinated via communication exchanges and coordination services with the intended outcome of providing a "virtually centralised system" where properties such as **causality**, **ordering of tasks**, **replication handling**, and **consistency** are ensured.

> **Note:** There are two viewpoints on security in distributed systems: (1) providing security *in* a distributed system where resources are dispersed, and (2) using distribution *as a means* of providing security (e.g., dispersal of keys vs. centralised keystore). This KA focuses mainly on the former.

#### Layered Architecture

A distributed system architecture is often an aggregation of multiple layers:
- **Lowest level:** Resources within a device (memory, computation, storage, communication) accessed through **Operating System** primitives
- **Distributed services:** Naming, time synchronisation, distributed file systems assembled through interaction of different components
- **Higher layers:** Build upon lower layers to provide additional functionalities and applications
- **Middleware:** Supports communication styles such as **Message Passing**, **Remote Procedure Calls** (RPCs), distributed object platforms, **Publish-Subscribe** architectures, enterprise service bus

### 12.1.2 Classes of Vulnerabilities & Threats

Vulnerabilities refer to design or operational weaknesses. Threats reflect the potential for an attacker to cause damage. Security is an **end-to-end systems property**.

The attack surface relates to compromises of:
- Physical resources
- Communication schema
- Coordination mechanisms
- Provided services themselves
- Usage policies on the data underlying the services

#### 12.1.2.1 Access/Admission Control & ID Management

Determines the authorised participation of a resource, user, or service within a distributed system.

**Threats include:**
- Masquerading or **Spoofing** of identity to gain access rights
- **Denial of Service** (DoS) attacks that detrimentally limit access, leading to inaccessibility and unavailability of distributed resources/services
- Identity tampering — since authorisation may be specified in terms of user/resource identity (login names, passwords)

> Resource distribution often entails more points for access control and more information transported to support access control, thus **increasing the attack surface**.

See also: **Authentication, Authorisation & Accountability** (CyBOK Ch14)

#### 12.1.2.2 Data Transportation

Network-level threats spanning routing, message passing, publish-subscribe modalities, event-based response triggering, and threats across the middleware stack.

**Types:**
- **Passive attacks:** Eavesdropping
- **Active attacks:** Data modification
- **Man-in-the-Middle (MITM):** Attacker inserts between victim's browser and web server, establishes two separate connections, actively records all messages and selectively modifies data

See also: **Network Security** (CyBOK Ch19)

#### 12.1.2.3 Resource Management and Coordination Services

Threats to the middleware protocols that provide coordination of resources, including:
- Synchronisation
- Replication management
- View changes
- Time/event ordering
- **Linearisability**
- **Consensus**
- Transactional commit

#### 12.1.2.4 Data Security

The classical **CIA Triad** (Confidentiality, Integrity, Availability) applies to each element of the data chain:
- **Confidentiality:** Information leakage threats such as **Side-Channel Attacks** or **Covert Channel Attacks**
- **Availability:** Any delay or denial of data access
- **Integrity:** Compromise of data correctness, violation of data consistency (strong, weak, relaxed, eventual consistency)

See also: **Malware & Attack Technologies** (CyBOK Ch6)

---

## 12.2 Decentralised P2P Models

Peer-to-Peer (**P2P**) systems constitute the decentralised variant of distributed systems. Their popularity is driven by:
- **Scalability** — no changes to protocol design needed with increasing numbers of peers
- **Decentralised coordination** — inherent resilience against individual peer failures
- **Low cost** — peer population itself represents the service provisioning infrastructure

### Five Core P2P Principles

1. **Symmetry of interfaces** — peers take interchangeable duties as both servers and clients
2. **Resilience** — to perturbations in the underlying communication network substrate and peer failures
3. **Data and service survivability** — through replication schemes
4. **Usage of peer resources at the network's edge** — imposing low infrastructure costs
5. **Address variance** — of resource provisioning among peers

Applications include: file sharing (eMule, KaZaA), social networks, multimedia content distribution, online games, internet telephony, instant messaging, **Internet of Things** (IoT), car-to-car communication, **SCADA** systems, wide area monitoring systems, and **Distributed Ledgers**.

### P2P Protocol Categories

| Category | Topology | Primary Use | Examples |
|----------|----------|-------------|----------|
| **Unstructured** | Tree or mesh-like sub-graphs | Data dissemination | **Freenet**, **Gnutella** |
| **Structured** | Ring structures with shortcuts (small-world) | Data discovery | **Chord**, **Pastry**, **Tapestry**, **Kademlia**, **CAN** |
| **Hybrid** | Integration of unstructured + structured | Data discovery + dissemination | **Napster**, **BitTorrent** |
| **Hierarchical** | Layered (front-end/back-end peers) | Improved lookup performance | eDonkey, **KaZaA** (SuperP2P) |

Basic P2P operations rely on three elements: **(a) identification/naming** of peer nodes, **(b) routing schemas** across peers, and **(c) discovery** of peers.

### 12.2.1 Unstructured P2P Protocols

- Resources searched by name or labels (no structured addressing scheme)
- Discovery via search algorithms: breadth-first search, depth-first search, random walks, expanding ring searches
- Message passing is either direct (underlay network connection) or piggybacked alongside resource discovery
- Peers maintain lists with contact information and periodically ping for liveness
- **Churn:** Rate of peer joins and departures; periodicity dynamically adjusted

### 12.2.2 Structured P2P Protocols

- Pointers to resources stored in a **Distributed Hash Table** (**DHT**)
- Address space is typically an integer scale `[0, ..., 2^w − 1]` with `w` being 128 or 160
- A **distance function** `d(a,b)` enables distance computations between any two identifiers
- Data discovery: compute key of resource identifier → request key and data from responsible peer
- **Lookup mechanism:** Steadily decrease address space distance toward destination on each iteration
- Routing tables store `k · w` entries; more storage for closer peers than distant ones

### 12.2.3 Hybrid P2P Protocols

Integrate elements from unstructured and structured schemas. Example: **BitTorrent** extended with structured P2P features to provide fully decentralised data discovery (abandoning "tracker servers").

### 12.2.4 Hierarchical P2P Protocols

Peers categorised based on bandwidth, latency, storage, or computation cycles:
- **Super-peers** (back-end): Take a coordinating role
- **Front-end peers:** Process service requests at first level

This improves lookup performance and generates fewer network messages. Popular content can be cached locally.

---

## 12.3 Attacking P2P Systems

### P2P Functional Elements Needing Protection

| Element | Description | Accessibility |
|---------|-------------|---------------|
| **P2P Operations (P-OP)** | Discovery, query, routing, download — accessible through service interface | Network level |
| **P2P Data Structures (P-DS)** | Data in routing tables, shared resources | Network or local host level |

Attacks correspond to disrupting connectivity/access to other nodes OR corrupting data structures.

> Most attacks exploit fundamental P2P features: message-exchange-based decentralised coordination and the fact that each peer has only a **partial (local) view** of the entire system.

### 12.3.1 Attack Types

#### Denial of Service (DoS) and Distributed Denial of Service (DDoS)

Manifest as resource exhaustion by limiting access to a node or communication route. In P2P architectures, attackers aim to decrease the overlay network's service availability by excessively sending messages to a specific set of peers.

#### Sybil Attacks

A single adversary creates and controls multiple fake identities (**Sybil nodes**) to:
- Outvote honest nodes in consensus mechanisms
- Disrupt routing by providing incorrect routing information
- Eclipse attacks: isolate honest nodes from the rest of the network

#### Routing Attacks

Exploit the partial-view property of P2P systems:
- **Incorrect routing updates:** Malicious peers provide wrong routing table information
- **Routing table poisoning:** Corrupt routing tables to redirect traffic
- **Partitioning:** Colluding attackers create network partitions that hide views of the system from honest nodes

#### Eclipse Attacks

An attacker monopolises all connections to and from a specific honest peer, effectively "eclipsing" it from the rest of the network. The victim sees only the attacker's view of the network.

#### Data Corruption Attacks

- Corrupting stored resources in P2P file-sharing systems
- Inserting malicious data into DHTs
- Providing incorrect query responses

#### Free-Riding

Peers consume services without contributing resources — while not a direct security attack, it degrades system availability and fairness.

---

## 12.4 Coordinated Distributed Systems: Consensus and Blockchain

### 12.4.1 Consensus in Distributed Systems

**Consensus** is a fundamental problem in distributed systems where a set of processes must agree on a single value despite failures. Key properties:

- **Termination:** Every correct process eventually decides some value
- **Agreement:** All correct processes decide the same value
- **Validity:** If all correct processes propose the same value, they decide that value

#### The FLP Impossibility

The **FLP Impossibility** result (Fischer, Lynch, Paterson, 1985) proves that in an asynchronous system, consensus is impossible with even a single crash failure. This drives practical systems to use:
- **Randomisation** (probabilistic consensus)
- **Partial synchrony** assumptions
- **Failure detectors**

#### Practical Consensus Protocols

| Protocol | Type | Use Case |
|----------|------|----------|
| ****Paxos**** | Crash-fault tolerant | Distributed databases, coordination services |
| ****Raft**** | Crash-fault tolerant | Easier-to-understand alternative to Paxos |
| ****PBFT**** (Practical Byzantine Fault Tolerance) | Byzantine-fault tolerant | Permissioned blockchains |
| ****PoW**** (Proof of Work) | Byzantine-fault tolerant (probabilistic) | **Bitcoin**, permissionless blockchains |
| ****PoS**** (Proof of Stake) | Byzantine-fault tolerant (probabilistic) | **Ethereum** 2.0, modern blockchains |

#### Security Threats to Consensus

- **Sybil attacks:** One entity creates many identities to control consensus
- **51% attacks:** Attacker with majority of computational power (PoW) or stake (PoS) can rewrite history
- **Nothing-at-stake problem:** Validators can vote on multiple conflicting chains without penalty
- **Long-range attacks:** Creating an alternative chain from a point far in the past
- **Selfish mining:** Strategic withholding of blocks to gain advantage

### 12.4.2 Blockchain Security

**Blockchain** is a distributed ledger technology that combines:
- **Cryptographic hash chains** for tamper-evidence
- **Consensus mechanisms** for agreement
- **Peer-to-peer networking** for decentralisation
- **Economic incentives** for honest participation

#### Security Properties

| Property | Mechanism |
|----------|-----------|
| **Immutability** | Cryptographic hashing; each block contains hash of previous block |
| **Transparency** | Public ledger; all transactions visible |
| **Decentralisation** | No single point of failure; P2P network |
| **Censorship Resistance** | No central authority can block transactions |
| **Finality** | Probabilistic (PoW) or deterministic (PBFT-based) |

#### Blockchain-Specific Attack Vectors

1. **51% / Majority Attack**
   - Attacker controls majority of hash power or stake
   - Can double-spend, censor transactions, reorganise the chain
   - Cost scales with network size

2. **Smart Contract Vulnerabilities**
   - **Reentrancy attacks** (e.g., the DAO hack)
   - Integer overflow/underflow
   - **Front-running** — miners/validators reorder transactions for profit
   - Access control flaws
   - Oracle manipulation

3. **Consensus-Level Attacks**
   - **Selfish mining:** Withholding blocks to waste honest miners' work
   - **Block withholding:** Sabotaging mining pools
   - **Bribery attacks:** Paying miners to reorganise the chain
   - **Timejacking:** Manipulating node timestamps

4. **Network-Level Attacks**
   - **Eclipse attacks:** Isolating a node from the honest network
   - **BGP hijacking:** Routing attacks to partition the network
   - **DNS attacks:** Redirecting nodes to malicious peers

5. **Privacy Concerns**
   - Transaction graph analysis / **deanonymisation**
   - Address reuse and clustering
   - **MEV** (Miner/Maximal Extractable Value) exploitation

### 12.4.3 Coordination Services Security

Coordination services like **Apache ZooKeeper**, **etcd**, and **Consul** provide:
- Distributed locking
- Leader election
- Configuration management
- Service discovery

**Security considerations:**
- Authentication and authorisation for all client connections
- Encryption of data in transit (TLS)
- Access Control Lists (ACLs) for znodes/keys
- Protection against **Split-Brain** scenarios
- Audit logging of all coordination operations

---

## 12.5 Security Approaches for Distributed Systems

### 12.5.1 Defence-in-Depth for Distributed Systems

1. **Network Layer**
   - TLS/SSL for encrypted communication
   - Mutual authentication between nodes
   - Firewall rules and network segmentation
   - DDoS mitigation (rate limiting, challenge-response)

2. **Middleware Layer**
   - Secure RPC/RMI with authentication
   - Message integrity verification (HMAC, digital signatures)
   - Rate limiting and request validation
   - Secure serialisation (to prevent deserialisation attacks)

3. **Application Layer**
   - Input validation and sanitisation
   - Principle of least privilege for service accounts
   - Secure defaults for configuration
   - Regular security patching

4. **Coordination Layer**
   - Byzantine fault tolerance for critical consensus
   - Reputation systems for P2P networks
   - Secure group membership protocols
   - Threshold cryptography for distributed key management

### 12.5.2 Distributed Access Control

- **Capability-based security:** Tokens that prove authorisation (see also **Operating Systems & Virtualisation** Ch11)
- ****OAuth 2.0** / **OpenID Connect**:** Delegated authorisation for distributed services
- ****SAML**:** Federated identity for cross-organisational distributed systems
- ****RBAC** / **ABAC**:** Role-based and attribute-based access control in distributed contexts

### 12.5.3 Secure Replication and Consistency

- **State Machine Replication (SMR):** Replicas execute same sequence of operations; consensus ensures ordering
- **Quorum-based approaches:** Read/write quorums with cryptographic verification
- **Merkle trees:** Efficient verification of data integrity across replicas
- **Vector clocks and version vectors:** Detecting conflicts in eventually consistent systems

---

## Key Concepts Summary

| Concept | Description |
|---------|-------------|
| **Distributed System** | Geo-dispersed resources providing a logically centralised service |
| **P2P (Peer-to-Peer)** | Decentralised model where peers are both clients and servers |
| **DHT (Distributed Hash Table)** | Structured P2P lookup mechanism for efficient resource discovery |
| **Consensus** | Agreement among distributed processes despite failures |
| **Byzantine Fault Tolerance** | Tolerance to arbitrary (including malicious) failures |
| **Blockchain** | Distributed ledger combining cryptographic hashing, consensus, and P2P networking |
| **Sybil Attack** | Single adversary creating multiple fake identities |
| **Eclipse Attack** | Isolating a node from the honest network |
| **FLP Impossibility** | Consensus impossible in purely asynchronous systems with even one crash |
| **Proof of Work (PoW)** | Consensus via computational puzzle solving |
| **Proof of Stake (PoS)** | Consensus via economic stake |
| **Smart Contract** | Self-executing code on a blockchain |

---

## Cross-References

- **Authentication, Authorisation & Accountability** (CyBOK Ch14) — Authentication and authorisation in distributed systems
- **Network Security** (CyBOK Ch19) — Network-level threats and protections
- **Operating Systems & Virtualisation** (CyBOK Ch11) — Resource access and isolation primitives
- **Malware & Attack Technologies** (CyBOK Ch6) — Malicious applications, code, and viruses
- **Hardware Security** (CyBOK Ch20) — TPM, secure elements, hardware root of trust
- **Cyber-Physical Systems Security** (CyBOK Ch21) — IoT and embedded systems security
- **Software Security** (CyBOK Ch15) — Secure coding, memory safety

---

## Further Reading

- Tanenbaum, A.S. and Van Steen, M. — *Distributed Systems: Principles and Paradigms*
- Coulouris, G. et al. — *Distributed Systems: Concepts and Design*
- Narayanan, A. et al. — *Bitcoin and Cryptocurrency Technologies*
- Antonopoulos, A.M. — *Mastering Bitcoin*
- Castro, M. and Liskov, B. — *Practical Byzantine Fault Tolerance* (OSDI 1999)
- Lamport, L. — *The Part-Time Parliament* (Paxos)
- Ongaro, D. and Ousterhout, J. — *In Search of an Understandable Consensus Algorithm* (Raft)
- Urdaneta, G., Pierre, G. and van Steen, M. — *A Survey of DHT Security Techniques* (ACM Computing Surveys)
- Wallach, D.S. — *A Survey of Peer-to-Peer Security Issues* (ISSS 2002)
