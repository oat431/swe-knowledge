---
title: "CyBOK Ch5: Privacy & Online Rights"
tags: [privacy, cyber-security, cybok, online-rights, data-protection]
source: "CyBOK v1.1, Chapter 5 – Privacy & Online Rights"
author: "Carmela Troncoso, École Polytechnique Fédérale de Lausanne"
date: 2021-07
---

# CyBOK Chapter 5: Privacy & Online Rights

## Introduction

The pervasiveness of data collection, processing, and dissemination raises severe privacy concerns regarding individual and societal harms. Information leaks may cause physical or psychological damage to individuals — e.g., when published information can be used by thieves to infer when users are not home, by enemies to find weak points to launch attacks, or by advertising companies to build profiles and influence users. On a large scale, the use of this information can influence society as a whole, causing irreversible harm to democracy.

Privacy cannot simply be tackled as a [[confidentiality]] issue. Beyond keeping information private, it is important to ensure that systems support **freedom of speech** and individuals' **autonomy of decision** and **self-determination**.

The goal of this Knowledge Area is to introduce system designers to the concepts and technologies used to engineer systems that inherently protect users' privacy — enabling designers to identify privacy problems, describe them from a technical perspective, and select adequate technologies to eliminate or mitigate these problems.

### Privacy as a Fundamental Right

Privacy is recognised as a fundamental human right: *"No one shall be subjected to arbitrary interference with his privacy, family, home or correspondence, nor to attacks upon his honour and reputation."*

Key socio-legal definitions of privacy include:
- **The right to be let alone** (Warren & Brandeis)
- **The right to informational self-determination** (German Constitutional Court)
- **The freedom from unreasonable constraints on the construction of one's own identity** (Agre)

The [[European Data Protection Legislation]] (GDPR) provides one of the best examples of legal frameworks supporting privacy, covered in the [[Law & Regulation]] Knowledge Area (Chapter 3).

### Engineering Privacy

This Knowledge Area conceptualises privacy similarly to how [[security engineering]] conceptualises security problems. Privacy concerns and solutions are defined by:

1. **Adversarial model** — e.g., untrusted third-party services, untrusted service providers, other users
2. **Nature of information to protect** — e.g., content of communications, service usage patterns, existence of users/actions
3. **Nature of protection mechanism** — e.g., access control settings, [[Encryption]]

### Structure

The chapter covers three privacy paradigms:
- **Privacy as Confidentiality** (Section 5.1) — hiding information from adversaries
- **Privacy as Informational Control** (Section 5.2) — enabling users to decide what information they expose
- **Privacy as Transparency** (Section 5.3) — informing users about data exposure and access

These paradigms are complemented by examples of how privacy technologies support **democracy and civil liberties** (Section 5.4), and **privacy engineering** guidelines (Section 5.5).

> [!important] Contextual Integrity
> Privacy requirements are context-dependent. Nissenbaum formalizes this as **contextual integrity**: an information flow may present different privacy needs depending on the entities exchanging information or the environment in which it is exchanged.

---

## 5.1 Privacy as Confidentiality

A technical re-interpretation of the "right to be let alone": the objective is to enable the use of services while **minimising the amount of exposed information** — both data exchanged explicitly and information made implicitly available in [[Metadata]].

### 5.1.1 Data Confidentiality

Two approaches to minimise exposed information:
1. **Provable prevention** of unauthorised access (cryptography-based)
2. **Disclosure control** methods that limit information leakage or unlinkability to individuals

#### 5.1.1.1 Cryptography-Based Access Control

##### Protecting Data in Transit

**End-to-End Encryption (E2EE)** ensures Confidentiality between communication endpoints. No third party — from routers to application servers — can access the communication. E2EE typically also provides [[Integrity]] and [[Authentication]].

- Devices at the endpoints hold the encryption keys
- Usually symmetric encryption keys, agreed via key transport or [[Diffie-Hellman]] exchange
- [[Digital Signatures]] and [[Message Authentication Codes]] provide Integrity and authentication

Canonical examples:
- **[[TLS]]** — widely used in client-server scenarios
- **[[PGP]]** — common encryption mechanism for email

**Off-the-Record Messaging (OTR):**
- Considers adversary that can eventually compromise devices to obtain long-term keys
- Goals: **perfect forward secrecy** and **repudiable Authentication** (deny having sent a message)
- Uses unauthenticated Diffie-Hellman key exchange, with mutual authentication inside the protected channel
- Keys are rotated and old keys deleted for forward secrecy

**Signal Protocol** (formerly Axolotl/TextSecure):
- Used by Signal, WhatsApp, Facebook Messenger, Viber
- Provides authenticated messaging with end-to-end Confidentiality
- Messages kept secret even if messaging server is compromised, even if long-term keys compromised
- Relies on authenticated key exchange mixing multiple Diffie-Hellman shared secrets, plus **double ratcheting** for key refresh

##### Protecting Data During Processing

Two scenarios:

**1. Outsourcing** — sender's data processed by untrusted recipient (e.g., cloud services):

- **[[Private Information Retrieval]] (PIR)**: query a database without revealing which record is accessed (e.g., privacy-preserving social network directories)
- **[[Oblivious Transfer]]**: transfer an item without revealing which item (e.g., privacy-preserving shopping)
- **[[Homomorphic Encryption]]**: perform any operation on encrypted data — high computation cost; partially/somewhat homomorphic variants offer better trade-offs
- **Secure hardware** combined with cryptographic primitives can improve performance, but at the expense of trusting hardware manufacturers
- **Tailored database solutions** (order-preserving encryption, deterministic encryption) provide performance but may significantly impact privacy — only recommended for compliance in trusted environments

**2. Collaborative Computation** — entities collaborate to perform computation without trusting each other:

- **[[MultiParty Computation]] (MPC)**: comparing databases, computing statistics across datasets
- **[[Private Set Intersection]] (PSI)**: find similarities between two databases (contacts, malicious activities, genetic information) without revealing anything except the intersection (or its cardinality)

##### Verification in the Encrypted Domain

**Zero-Knowledge Proofs** enable proving inputs comply with formats/constraints without revealing the inputs:

1. **Private computation-input verification** — prove input adequacy (billing, messaging, private intersection)
2. **Private authentication** — **[[Anonymous Credentials]]** (Attribute-Based Credentials/ABCs):
   - Prove possession of attributes without revealing identity or attribute values
   - Unlinkable between contexts (credentials look different every time shown)
   - Challenges: Anonymity enables misbehaviour → extensions for limiting use, blacklisting, or revoking credentials
   - Implementations: Idemix, U-Prove (various licenses, different functionality subsets)
3. **Private payments** — [[Blind Signatures]] for e-coins; **Zerocash** (blockchain-based) uses [[ZK-SNARKs]] for efficient zero-knowledge proofs

#### 5.1.1.2 Obfuscation-Based Inference Control

Relaxed Confidentiality definition: cannot completely conceal information, but **control the extent of adversarial inferences**. Protection level depends on concrete data and adversarial knowledge — ad-hoc analysis needed.

Four main obfuscation techniques (mostly for numerical/categorical fields; free text is much harder):

| Technique | Description | Examples |
|-----------|-------------|----------|
| **Anonymisation** | Decouple identity from information by removing identifying data | Often insufficient due to uniqueness of data patterns; combined with other techniques |
| **Generalisation** | Reduce precision of shared values to reduce inference accuracy | Database anonymisation (bucketisation), private web searches |
| **Suppression** | Suppress part of information before making it available | Database anonymisation, location data publishing |
| **Perturbation** | Inject noise into data to reduce adversary's inference performance | Location privacy, collaborative learning, census data |
| **Dummy Addition** | Add fake data points indistinguishable from real ones | Web searches, database protection |

> [!warning] Re-identification Risk
> **Quasi-identifiers** — combinations of attributes unique to individuals — enable re-identification by mapping to external data sources. Anonymisation alone is rarely sufficient.

##### k-Anonymity and Beyond

- **k-anonymity**: records indistinguishable from at least *k* other entries (via generalisation + suppression)
- **l-diversity**: for each k-anonymous individual, at least *l* possible values for sensitive attributes
- **t-closeness**: sensitive attribute distribution follows the general population distribution

Limitations: achieving k-anonymity may require unacceptable generalisation; may still not prevent inference of sensitive attributes.

##### Differential Privacy

Introduced by Dwork: a **property of a mechanism** (not a dataset). An algorithm *A* provides ε-differential privacy if, for all datasets *D₁* and *D₂* differing on a single element, and all possible outputs *S*:

$$Pr[A(D_1) \in S] \leq e^\varepsilon \times Pr[A(D_2) \in S]$$

Key considerations:
- Provides a **relative guarantee** — protection relative to adversary's prior knowledge
- The parameter **ε (epsilon)** crucially determines protection level; ε > 1 deserves close scrutiny
- **Sensitivity** of the algorithm determines noise needed — higher sensitivity requires more noise
- Provides **worst-case guarantee** — noise tailored to the most informative data point
- Extensions exist for metrics beyond Hamming distance

Used by: US Census Bureau, collaborative learning ([[Machine Learning]]), location-based services.

### 5.1.2 Metadata Confidentiality

Three types of metadata extremely vulnerable to privacy attacks:

#### Traffic Metadata

Network-layer information (IP addresses, amount/timing of data, connection duration) accessible to observers even when communications are encrypted. Can reveal:
- Medical conditions (communication with specialist doctors)
- Corporate investment intentions (patent database queries)
- Search keywords or even conversations from encrypted flows

**Anonymous Communications Networks:**
- Series of relays preventing direct origin-to-destination communication
- Relays change message appearance through Encryption for bitwise Unlinkability
- Can introduce delays, re-package messages, or introduce dummy traffic

##### Tor (Onion Routing)

- Most popular anonymous communication network
- Uses **Onion Routers (ORs)** — relays that forward encrypted data
- Client builds a circuit of 3 ORs: **entry → middle → exit**
- **Onion encryption**: packet encrypted with exit key, then middle key, then entry key; each node "peels" a layer
- Low-latency (no delays) — suitable for web browsing but traffic patterns are preserved
- Vulnerable to adversary observing both ends of communication (entry and exit nodes)
- Key difference from [[VPN]]: in Tor, no single relay learns sender-receiver link (decentralized trust); VPN provider is a single point of failure

##### Mix Networks

- Routes selected for every message (not persistent circuits)
- Messages are **delayed** at each relay (mix) until a firing condition is met
- Batching strategies and dummy traffic destroy traffic patterns
- **Loopix**: modern low-latency mix network with providers, exponential delays, and dummy traffic for provable Unlinkability guarantees

#### Device Metadata

Device characteristics (User Agent, Language, Plugins, screen resolution, installed fonts, architecture) become quasi-identifiers enabling tracking across the web.

**Device/Browser Fingerprinting:**
- Systematic collection of device attributes for identification and tracking
- **Font fingerprinting**: exploit rendering differences between installed and monospace fonts
- **Canvas fingerprinting**: exploit HTML5 Canvas rendering differences
- Defending while retaining utility is extremely difficult — hiding metadata impacts performance; random combinations may be as unique as original

**Cookie-based tracking** (beyond metadata):
- Third-party cookies for cross-site tracking
- Cookie syncing (redirecting cookies to other trackers)
- Pervasive mechanisms to install cookie-like information that survives cache clearing

#### Location Metadata

Geographical location can infer sensitive information (points of interest, home/work locations, demographics, future movements).

Defence approaches:
1. **Cryptographic techniques** for private location-based queries (Homomorphic encryption, private equality testing, private threshold set intersection)
2. **Obfuscation techniques**: hiding location, perturbation (reporting different location), generalisation (reduced precision), dummy locations

---

## 5.2 Privacy as Control

Broader notion of privacy: from concealment to the **ability to control what happens with revealed information**. Two major concerns:
1. Enable users to express how disclosed data should be used
2. Enable organisations to define and enforce policies preventing misuse

> [!warning] Trust Requirement
> Control-oriented technologies inherently trust the service provider to correctly enforce user-established policies and not abuse collected data. Providing users with control tools can also reduce risk perception and increase risk-taking (Acquisti et al.).

### 5.2.1 Support for Privacy Settings Configuration

Privacy settings controls are often **barely usable** by individuals, causing misconfiguration and unintended data disclosure (see also [[Human Factors]] Knowledge Area, Chapter 4).

Approaches to simplify configuration:
- **Expert-defined policies** — difficult to generalise from targeted groups to general population; may over-restrict sharing
- **Machine-learning inference** from social graph — requires knowledge of social graph (itself a privacy concern)
- **Generic user-base inference** — may discriminate against groups with specific privacy requirements (activists, public figures); may perpetuate data biases
- **Crowdsourcing** — more flexible but still influenced by majority votes; may not suit non-mainstream users

### 5.2.2 Support for Privacy Policy Negotiation

Automating communication of user preferences:

- **[[P3P]] (Platform for Privacy Preferences Project)** — W3C standard allowing websites to encode privacy policies in machine-readable format. Browsers can compare site policy with user's preferences (APPEL). However, P3P has **no enforcement mechanism**.
- **Purpose-based access control** and **sticky policies** — specify allowed uses of collected information and verify compliance with cryptographic mechanisms

### 5.2.3 Support for Privacy Policy Interpretability

Privacy policies are often long, verbose, and contain legal terminology.

Two approaches:
1. **Expert-driven** — trusted experts label, analyse, and provide reasons for existing policies
2. **Automated** — [[Polisis]], a machine-learning-based framework enabling users to ask questions about natural language privacy policies, offering visual representations of data types collected, purposes, and sharing practices

---

## 5.3 Privacy as Transparency

Transparency mechanisms analyse users' online activities to provide feedback or run audits — happening **after** data disclosure, so providers are trusted.

### 5.3.1 Feedback-Based Transparency

- **Privacy mirrors** — show users their "digital selves" (how others see their data). Adopted by Facebook for profile visibility checks.
- **Visual cues** — highlight access permissions on shared data (e.g., colour-coded profile fields)
- **Privacy nudges** — immediate feedback when performing online actions (e.g., "this post is public"). Can use ML to analyse photos/text providing feedback like "this photo is very explicit."
  - Drawback: users may feel monitored or perceive advice as paternalistic

### 5.3.2 Audit-Based Transparency

Enable verification that no abuse has taken place through secure logging:

- **Formal methods** to derive auditing specifications from policies — guarantees minimal but sufficient logs; limited expressiveness for modern systems
- **Cryptography and distributed ledgers** — highly secure logs without sharing private information; no single party can modify the log
  - **UnLynx**: entities share sensitive data and perform computations without entrusting protection to any single entity; all actions logged in distributed ledger with verifiable cryptographic primitives and zero-knowledge proofs

---

## 5.4 Privacy Technologies and Democratic Values

Privacy protection is crucial for underpinning democratic values. Citing Daniel Solove: *"Part of what makes a society a good place in which to live is the extent to which it allows people freedom from the intrusiveness of others. A society without privacy protection would be suffocating."*

Episodes like the Facebook–Cambridge Analytica case highlight the importance of securing data from unintended access to protect citizens from interference and manipulation.

### 5.4.1 Privacy Technologies as Support for Democratic Political Systems

#### Electronic Voting (eVoting)

Goals of eVoting schemes:
- **Ballot secrecy**: adversary cannot determine which candidate a user voted for
- **Universal verifiability**: external observer can verify all votes are counted and tally is correct
- **Individual verifiability** (weaker): each voter can verify their vote was correctly tallied
- **Eligibility verifiability**: all votes cast by unique eligible voters

Technologies used:
1. **Mix networks with verifiable shuffles** — votes passed through multiple mixes (different authorities), each proving in zero-knowledge that mixing is correct and random
2. **Blind signatures** — authorised entity blindly signs votes; users submit via anonymous channels
3. **Homomorphic encryption** — encrypted bulletin board where votes are added and randomised; zero-knowledge proofs ensure correct operations

**Coercion resistance**: prevent forced voting through fake credentials (coerced votes ignored) or re-voting (cancel coerced choice).

#### Anonymous Petitions

Privacy technologies, particularly anonymous credentials, enable secure and privacy-preserving petition systems:
- Citizens register to obtain anonymous signing keys with relevant attributes
- At signing time, prove eligibility without revealing identity
- Advanced properties (double-signing detection) prevent abuse
- Modern approaches use distributed ledgers for threshold issuance and verification, removing central trusted parties

### 5.4.2 Censorship Resistance and Freedom of Speech

Censorship systems attempt to impose particular distribution of content — preventing publishing or access to content.

#### Data Publishing Censorship Resistance

- **Eternity Service** (Anderson): distribute file copies across servers in different jurisdictions; Encryption prevents selective denial of service; anonymous Authentication protects both users and services
- **Freenet**: peer-to-peer system for publishing, replicating, and retrieving data with anonymity for authors and readers. Nodes store encrypted content with deniability (can claim unawareness of content). Files located by key (hash).
  - Vulnerabilities: passive attacker with sufficient nodes can re-identify requesters; Bayesian inference can detect request initiators
- **Tangler**: files split into small blocks across servers; blocks contain parts of many documents ("tangled" via secret sharing). Censor deleting target file causes collateral damage to allowed files.

#### Data Access Censorship Resistance

Four approaches:
1. **Mimicking** — access to censored data looks like accessing allowed data (e.g., Skype call, innocuous webpage). Vulnerable to active probing.
2. **Tunnelling** — tunnel censored communication through an uncensored service (e.g., cloud services), making blocking costly.
3. **Embedding** — hide communication inside content (photo/video). Makes communications unobservable and deniable.
4. **Hiding destination** — relay censored traffic through intermediate nodes:
   - **Tor bridges**: Tor relays whose IPs are not public; traffic disguised with **pluggable transports**
   - **Decoy routing / refraction networking**: client directs traffic to benign destination with undetectable signal; cooperating router deflects to censored site

---

## 5.5 Privacy Engineering

"Privacy by design" is popular among policymakers but rarely addresses actual design, implementation, and integration processes.

### Primary Design Goals

1. **Minimise trust**: limit reliance on other entities' expected behaviour regarding sensitive data
   - e.g., distributed trust in mix-based eVoting with verifiable shuffles
2. **Minimise risk**: limit likelihood and impact of privacy breach
   - e.g., in Tor, compromising one relay does not provide sensitive information

### Design Strategies

| Strategy | Description |
|----------|-------------|
| **Minimise Collection** | Limit capture and storage of data whenever possible |
| **Minimise Disclosure** | Constrain information flow to parties other than the data subject |
| **Minimise Replication** | Limit entities where data are stored/processed in the clear |
| **Minimise Centralisation** | Avoid single points of failure for privacy properties |
| **Minimise Linkability** | Limit adversary's capability to link data |
| **Minimise Retention** | Limit time information is stored |

### Choosing Privacy Technologies

1. **Identify data flows to minimise** — those moving data to entities to whom data do not relate
2. **Identify minimal set of data** that needs to be transferred

Strategies to minimise unnecessary information flow:
1. **Keep data local** — compute on user side, transmit only results (with zero-knowledge proofs for correctness)
2. **Encrypt the data** — send only encrypted version
3. **Use privacy-preserving cryptographic protocols** — e.g., anonymous credentials, private information retrieval
4. **Obfuscate the data** — perturb locally, send perturbed version
5. **Anonymise the data** — remove identifiable information, send via anonymous channel

For systems that cannot avoid collecting user data, use control (Section 5.2) and transparency (Section 5.3) technologies to minimise breach risk and impact.

### Privacy Evaluation

A systematic evaluation involves:
1. **Model the privacy-preserving mechanism** as a probabilistic transformation
2. **Establish the threat model** — what adversary sees and prior knowledge
3. **Compute what adversary can learn** — probability distribution analysis or inference techniques (e.g., [[Machine Learning]])
4. **Apply privacy metrics** to capture the adversary's inference capability

> [!important] Cryptographic vs. Obfuscation-Based
> Cryptographic techniques are validated through formal proofs; obfuscation-based techniques require empirical analysis to validate the desired privacy level.

---

## 5.6 Conclusions

Protecting privacy is not limited to guaranteeing Confidentiality of information. It also requires:
- Means to help users understand the extent to which their information is available online
- Mechanisms to enable users to exercise control over this information

**Three privacy conceptions** with corresponding technologies have been described:
1. **Confidentiality** — hiding information from adversaries
2. **Control** — enabling users to decide what information to expose
3. **Transparency** — informing users about data exposure and access

Preserving privacy and online rights is essential to support **democratic societies**. The deployment of privacy technologies is key to allow free access to content and freedom of speech, and to prevent any entity from gaining disproportionate information that could damage democratic values.

---

## Cross-Reference of Topics

| Section | Topic | Key References |
|---------|-------|----------------|
| 5.1 | Privacy as Confidentiality | — |
| 5.1.1 | Data Confidentiality | [[Encryption]], [[Homomorphic Encryption]], [[MultiParty Computation]], [[Private Set Intersection]], [[Zero-Knowledge Proofs]] |
| 5.1.2 | Metadata Confidentiality | [[Tor]], [[Mix Networks]], [[Device Fingerprinting]], [[Location Privacy]] |
| 5.2 | Privacy as Control | [[P3P]], [[Polisis]], [[Sticky Policies]] |
| 5.3 | Privacy as Transparency | [[Privacy Nudges]], [[Distributed Ledgers]], [[Secure Logging]] |
| 5.4 | Privacy Technologies and Democratic Values | [[eVoting]], [[Censorship Resistance]], [[Freenet]], [[Tangler]] |
| 5.5 | Privacy Engineering | [[Privacy by Design]], [[Threat Modelling]], [[Privacy Metrics]] |

## See Also

- [[Law & Regulation]] (CyBOK Chapter 3) — European Data Protection Legislation, GDPR
- [[Human Factors]] (CyBOK Chapter 4) — usability impact on security and privacy
- [[Applied Cryptography]] (CyBOK Chapter 10) — cryptographic primitives underlying privacy technologies
- [[Adversarial Behaviours]] (CyBOK Chapter 7) — adversarial exploitation of privacy systems
- [[Risk Management & Governance]] (CyBOK Chapter 2) — organisational risk and privacy governance
- [[Data Protection]] — GDPR, data subject rights, lawful bases for processing
- [[Online Rights]] — freedom of speech, digital rights, censorship resistance
