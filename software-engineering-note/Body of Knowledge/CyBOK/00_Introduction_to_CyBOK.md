---
tags: [introduction, cyber-security, cybok]
created: 2026-07-09
---

> *Source: CyBOK v1.1 by NCSC/UK Government, Chapter 1 — Introduction*

## Purpose

The Cyber Security Body of Knowledge (CyBOK) aims to codify the foundational and generally recognised knowledge on cyber security. In the same fashion as the IEEE Software Engineering Body of Knowledge (SWEBOK), CyBOK serves as a guide to the body of knowledge — mapping established knowledge from textbooks, academic research articles, technical reports, white papers, and standards, rather than replicating everything ever written on the subject. Educational programmes ranging from secondary and undergraduate education to postgraduate and continuing professional development can be built on the basis of CyBOK.

The field of cyber security suffers from fragmented foundational knowledge, making it difficult for students and educators to map coherent paths of progression. CyBOK addresses this by providing a common vocabulary, core understanding, and structured organisation of 21 Knowledge Areas (KAs) grouped into five broad categories.

---

## Cyber Security Definition

CyBOK adopts the definition from the UK National Cyber Security Strategy:

> **Cyber security** refers to the protection of information systems (hardware, software and associated infrastructure), the data on them, and the services they provide, from unauthorised access, harm or misuse. This includes harm caused intentionally by the operator of the system, or accidentally, as a result of failing to follow security procedures.

This definition expresses the breadth of coverage within the topic. Human behaviours are a crucial element — the impact of loss of information, reduced safety, and how security and privacy breaches impact trust in connected systems and infrastructures must also be considered.

A large contributor to the notion of cyber security is **Information Security**, defined by ISO 27000 as:

> Preservation of **confidentiality**, **integrity** and **availability** of information. In addition, other properties, such as **authenticity**, **accountability**, **non-repudiation**, and **reliability** can also be involved.

Related terms include Computer Security, Network Security, Information Assurance, and Systems Security. Cyber security is broader: it encompasses the cyber-physical domain where the pressing challenge is to prevent unwanted physical actions from network-connected actuators, beyond just protecting information.

---

## CyBOK Knowledge Areas Overview

The CyBOK is divided into 21 top-level Knowledge Areas (KAs), grouped into five broad categories. These categories are not entirely orthogonal — there are numerous dependencies across the KAs which are cross-referenced in the documentation and captured visually on the [CyBOK website](https://www.cybok.org).

### I. Human, Organisational & Regulatory Aspects

| # | Knowledge Area | Chapter | Description |
|---|---------------|---------|-------------|
| 1 | **Risk Management & Governance** | [[02_Risk_Management_and_Governance]] | Security management systems and organisational security controls, including standards, best practices, and approaches to risk assessment and mitigation. |
| 2 | **Law & Regulation** | [[03_Law_and_Regulation]] | International and national statutory and regulatory requirements, compliance obligations, and security ethics, including data protection and developing doctrines on cyber warfare. |
| 3 | **Human Factors** | [[04_Human_Factors]] | Usable security, social and behavioural factors impacting security, security culture and awareness as well as the impact of security controls on user behaviours. |
| 4 | **Privacy & Online Rights** | [[05_Privacy_and_Online_Rights]] | Techniques for protecting personal information, including communications, applications, and inferences from databases and data processing. Also includes systems supporting online rights touching on censorship, covertness, electronic elections, and privacy in payment and identity systems. |

### II. Attacks & Defences

| # | Knowledge Area | Chapter | Description |
|---|---------------|---------|-------------|
| 5 | **Malware & Attack Technologies** | [[06_Malware_and_Attack_Technologies]] | Technical details of exploits and distributed malicious systems, together with associated discovery and analysis approaches. |
| 6 | **Adversarial Behaviours** | [[07_Adversarial_Behaviours]] | The motivations, behaviours, and methods used by attackers, including malware supply chains, attack vectors, and money transfers. |
| 7 | **Security Operations & Incident Management** | [[08_Security_Operations_and_Incident_Management]] | The configuration, operation and maintenance of secure systems including the detection of and response to security incidents and the collection and use of threat intelligence. |
| 8 | **Forensics** | [[09_Forensics]] | The collection, analysis, and reporting of digital evidence in support of incidents or criminal events. |

### III. Systems Security

| # | Knowledge Area | Chapter | Description |
|---|---------------|---------|-------------|
| 9 | **Cryptography** | [[10_Cryptography]] | Core primitives of cryptography as presently practised and emerging algorithms, techniques for analysis of these, and the protocols that use them. |
| 10 | **Operating Systems & Virtualisation Security** | [[11_Operating_Systems_and_Virtualisation_Security]] | Operating systems protection mechanisms, implementing secure abstraction of hardware, and sharing of resources, including isolation in multi-user systems, secure virtualisation, and security in database systems. |
| 11 | **Distributed Systems Security** | [[12_Distributed_Systems_Security]] | Security mechanisms relating to larger-scale coordinated distributed systems, including aspects of secure consensus, time, event systems, peer-to-peer systems, clouds, multi-tenant data centres, and distributed ledgers. |
| 12 | **Formal Methods for Security** | [[13_Formal_Methods_for_Security]] | Formal specification, modelling and reasoning about the security of systems, software and protocols, covering the fundamental approaches, techniques and tool support. |
| 13 | **Authentication, Authorisation & Accountability** | [[14_Authentication_Authorisation_and_Accountability]] | All aspects of identity management and authentication technologies, and architectures and tools to support authorisation and accountability in both isolated and distributed systems. |

### IV. Software & Platform Security

| # | Knowledge Area | Chapter | Description |
|---|---------------|---------|-------------|
| 14 | **Software Security** | [[15_Software_Security]] | Known categories of programming errors resulting in security bugs, and techniques for avoiding these errors — both through coding practice and improved language design — and tools, techniques, and methods for detection of such errors in existing systems. |
| 15 | **Web & Mobile Security** | [[16_Web_and_Mobile_Security]] | Issues related to web applications and services distributed across devices and frameworks, including the diverse programming paradigms and protection models. |
| 16 | **Secure Software Lifecycle** | [[17_Secure_Software_Lifecycle]] | The application of security software engineering techniques in the whole systems development lifecycle resulting in software that is secure by default. |

### V. Infrastructure Security

| # | Knowledge Area | Chapter | Description |
|---|---------------|---------|-------------|
| 17 | **Applied Cryptography** | [[18_Applied_Cryptography]] | The application of cryptographic algorithms, schemes, and protocols, including issues around implementation, key management, and their use within protocols and systems. |
| 18 | **Network Security** | [[19_Network_Security]] | Security aspects of networking and telecommunication protocols, including the security of routing, network security elements, and specific cryptographic protocols used for network security. |
| 19 | **Hardware Security** | [[20_Hardware_Security]] | Security in the design, implementation, and deployment of general-purpose and specialist hardware, including trusted computing technologies and sources of randomness. |
| 20 | **Cyber-Physical Systems Security** | [[21_Cyber_Physical_Systems_Security]] | Security challenges in cyber-physical systems, such as the Internet of Things and industrial control systems, attacker models, safe-secure designs, and security of large-scale infrastructures. |
| 21 | **Physical Layer & Telecommunications Security** | [[22_Physical_Layer_and_Telecommunications_Security]] | Security concerns and limitations of the physical layer including aspects of radio frequency encodings and transmission techniques, unintended radiation, and interference. |

---

## Deploying CyBOK Knowledge to Address Security Issues

### Means and Objectives of Cyber Security

Cyber security entails protection against an adversary or against physical or random processes, implying overlap between safety and security. Core to security is **modelling adversaries**: their motives, the threats they pose, and the capabilities they may utilise.

Security controls affect **people, process, and technology**. Some focus on **prevention** of bad outcomes; others on **detection and reaction**. Selection of controls is approached through:

- **Risk Management** — see [[02_Risk_Management_and_Governance]]
- **Human Factors** — leveraging humans as a linchpin for improving security cultures, see [[04_Human_Factors]]
- **Privacy & Online Rights** — see [[05_Privacy_and_Online_Rights]]

Security also requires **vulnerability analysis**. A system without vulnerabilities would be impervious to all threats; a highly vulnerable system in benign circumstances would have no incidents. The **security assurance** domain addresses whether controls are deployed appropriately and effectively.

### Failures and Incidents

When adversaries achieve their goal, security controls have failed. Operationally, one or more failures may give rise to a **security incident**, described in terms of harm: theft or damage of information, devices, services, or networks. In the cyber-physical domain, harms to humans may arise from information loss, unintended physical action, or both.

Key KAs for incident handling:
- **Security Operations & Incident Management** — detection and reaction, see [[08_Security_Operations_and_Incident_Management]]
- **Malware & Attack Technologies** — attack vector analysis, see [[06_Malware_and_Attack_Technologies]]
- **Forensics** — post-attack analysis, see [[09_Forensics]]

A recurrent theme is the **"layer below" problem**: security controls defined within a particular abstraction may be bypassed when an adversary attacks the layer below. Examples include side-channel attacks (RF emissions, power analysis), SQL injection, and abstraction failures in formal refinement. See [[11_Operating_Systems_and_Virtualisation_Security]], [[20_Hardware_Security]], [[15_Software_Security]], [[16_Web_and_Mobile_Security]], and [[13_Formal_Methods_for_Security]].

### Risk

There is no limit in principle to the effort expended on security controls. **Risk Assessment** and **Risk Management** provide the over-arching approach to balance resources with harms and opportunities. See [[02_Risk_Management_and_Governance]].

Key elements of risk calculation:
- **Likelihood** of events — presence of vulnerabilities (known or unknown) and nature of the threat
- **Estimated impact** — consequences arising from those events
- Management responses: additional controls, accepting risk, transferring/sharing risk (e.g., insurance), or deciding not to proceed

**Security management** encompasses all actions to maintain system security during its lifetime. Functions are grouped into:
- **Physical security** — physical protection, access control, asset management (outside CyBOK scope)
- **Personnel security** — security usability, behaviour shaping, vetting, acceptable usage (see [[04_Human_Factors]], [[03_Law_and_Regulation]])
- **Information system management** — access management (see [[14_Authentication_Authorisation_and_Accountability]]), system logging (see [[08_Security_Operations_and_Incident_Management]])
- **Incident management** — security monitoring, incident detection and response (see [[08_Security_Operations_and_Incident_Management]])

---

## Principles

### Saltzer and Schroeder Principles (1975)

Eight design principles for engineering security controls, originally for secure multi-user operating systems but broadly applicable:

1. **Economy of Mechanism** — Keep security controls as simple as possible for high assurance. Underlies the notion of Trusted Computing Base (TCB).
2. **Fail-Safe Defaults** — Define operations positively in accordance with a security policy; reject all others by default.
3. **Complete Mediation** — Check all operations on all objects against the security policy.
4. **Open Design** — Security must not rely on secrecy of how the control operates (no "security by obscurity").
5. **Separation of Privilege** — Require multiple subjects to authorise an operation for higher assurance.
6. **Least Privilege** — Grant the minimum set of privileges needed for each operation.
7. **Least Common Mechanism** — Minimise sharing of resources and mechanisms between different parties.
8. **Psychological Acceptability** — Security controls should be naturally usable so users routinely and automatically apply protection.

Two further (imperfectly applicable) principles:
- **Work Factor** — Controls should require more resources to circumvent than available to the adversary.
- **Compromise Recording** — Reliable logs may be used as an alternative to preventive controls.

These principles draw on Kerckhoff's principles for cryptographic systems (open design, practical security, usability).

### NIST Principles

NIST extends Saltzer and Schroeder with three families:

| Family | Key Principles |
|--------|---------------|
| **Security Architecture & Design** | Clear Abstraction, Modularity and Layering, Partially Ordered Dependencies, Secure Evolvability |
| **Security Capability & Intrinsic Behaviours** | Least Privilege, Least Common Mechanism, Efficiently Mediated Access, Minimised Sharing, Reduced Complexity, Secure Defaults, Predicate Permission |
| **Life Cycle Security** | Hierarchical Trust, Inverse Modification Threshold, Hierarchical Protection, Trusted Communication Channels, Secure Distributed Composition, Self-Reliant Trustworthiness, Economic Security, Performance Security, Human Factored Security, Acceptable Security |

Three key security architecture strategies from NIST:
- **Reference Monitor Concept** — An abstract control sufficient to enforce system security properties.
- **Defence in Depth** — Multiple overlapping controls.
- **Isolation** — Physical or logical separation of components.

### Latent Design Conditions

From James Reason's safety-critical systems research: **latent design conditions** arise from past decisions about a system, often remaining hidden until certain events or settings align — the "Swiss Cheese model." As legacy cyber-physical systems become connected, latent insecure design conditions may be manifested. Security must now consider **safety implications** alongside information loss. See [[04_Human_Factors]].

### The Precautionary Principle

Designers must consider security and privacy implications of their choices from **conception through to decommissioning** of large-scale connected systems. Function creep over a system's lifetime and its impact on society must also be considered. See [[05_Privacy_and_Online_Rights]].

---

## Crosscutting Themes

### Security Economics

A synthesis between computer and social science combining microeconomic theory and game theory with information security. Three sub-areas:

1. **Defender economics** — Incentives of legitimate market players (ISPs, software vendors) and how they lead to optimal or sub-optimal security outcomes.
2. **Attacker economics** — Cost-benefit analyses of attackers exploiting vulnerabilities and formulating protective countermeasures.
3. **Economics of deviant security** — How cyber criminals apply security to defend their own systems against disruption (e.g., botnet resilience mechanisms, anti-forensic techniques).

Security economics is a socio-technical approach acknowledging that security is fundamentally a human problem where cost versus benefits trade-offs are key to understanding decisions of both defenders and attackers.

### Security Architecture and Lifecycle

The high-level design of a system from a security perspective, bound up with the system lifecycle from conception to decommissioning. Key steps:

1. **Review proposed use** — Identify interactions between users, data, and services.
2. **Risk assessment** — Identify high-risk interactions; consider compliance and contractual obligations.
3. **Group users and data** — Role-access requirements, formal data classification, user clearance → potential system compartments.
4. **Compartmentalise** — Enforce with network partitioning controls (see [[19_Network_Security]]).
5. **Detailed design within compartments** — Access controls (see [[14_Authentication_Authorisation_and_Accountability]]), user roles, data design.
6. **Uniform security infrastructure** — Key management, network protocols ([[19_Network_Security]]), resource management ([[12_Distributed_Systems_Security]]), user access ([[04_Human_Factors]]), intrusion detection ([[08_Security_Operations_and_Incident_Management]]).

The concepts of **"security by design"** and **"secure by default"** capture the importance of considering security throughout the entire lifecycle, including default configurations.

---

## Related Chapters

- [[02_Risk_Management_and_Governance]] — Risk management systems and organisational controls
- [[03_Law_and_Regulation]] — Legal and regulatory frameworks
- [[04_Human_Factors]] — Usable security and human behaviour
- [[05_Privacy_and_Online_Rights]] — Privacy techniques and online rights
- [[06_Malware_and_Attack_Technologies]] — Exploits and malicious systems
- [[07_Adversarial_Behaviours]] — Attacker motivations and methods
- [[08_Security_Operations_and_Incident_Management]] — Security operations and incident response
- [[09_Forensics]] — Digital evidence and forensic analysis
- [[10_Cryptography]] — Cryptographic primitives and protocols
- [[11_Operating_Systems_and_Virtualisation_Security]] — OS and virtualisation protection
- [[12_Distributed_Systems_Security]] — Distributed systems security
- [[13_Formal_Methods_for_Security]] — Formal modelling and verification
- [[14_Authentication_Authorisation_and_Accountability]] — Identity, auth, and accountability
- [[15_Software_Security]] — Software vulnerabilities and mitigations
- [[16_Web_and_Mobile_Security]] — Web and mobile platform security
- [[17_Secure_Software_Lifecycle]] — Security in the SDLC
- [[18_Applied_Cryptography]] — Cryptographic implementation and key management
- [[19_Network_Security]] — Network protocol and infrastructure security
- [[20_Hardware_Security]] — Hardware design and trusted computing
- [[21_Cyber_Physical_Systems_Security]] — CPS and IoT security
- [[22_Physical_Layer_and_Telecommunications_Security]] — Physical layer security
