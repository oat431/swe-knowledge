---
tags: [sos, systems-of-systems, applications, systems-engineering, sebok]
---

# 12 — Systems of Systems (SoS)

> *Source: SEBoK v2, Part 4 — Systems of Systems*

## Purpose

A System of Systems (SoS) is a collection of independent systems that are integrated to achieve capabilities not possible from any individual system. SoS engineering addresses the unique challenges of architecting, developing, and managing systems where constituent systems retain operational and managerial independence while contributing to emergent capabilities.

## Architecting Approaches for SoS

SoS architecting differs from traditional system architecting because constituent systems are independently owned, managed, and operated. Key approaches include:

- **Directed SoS** — A central authority designs and manages the SoS. Constituent systems operate independently but conform to SoS-level direction.
- **Acknowledged SoS** — Constituent systems voluntarily collaborate with recognized objectives, a designated manager, and resources, while maintaining independent ownership.
- **Collaborative SoS** — Constituent systems voluntarily agree to work together toward common goals without a central authority.
- **Virtual SoS** — No central management or agreed-upon objectives; emergent behavior arises from constituent system interactions.

## Systems Engineering for SoS

SE for SoS extends traditional SE practices to account for:

- **Independence of constituents** — Each system has its own stakeholders, funding, life cycle, and evolution path
- **Emergent behavior** — Capabilities and risks arise from interactions that cannot be predicted from individual system analysis alone
- **Evolutionary development** — SoS capabilities evolve incrementally as constituent systems are added, upgraded, or retired
- **Governance across organizational boundaries** — Requires negotiation, standards, and interfaces rather than direct control

## SoS Analytic Approaches

Analysis of SoS requires methods that handle scale, heterogeneity, and emergent properties:

- Agent-based modeling and simulation
- Network analysis for interdependency mapping
- Architecture frameworks adapted for SoS (DoDAF, UAF)
- Trade-space exploration across constituent system boundaries

## SoS and Complexity

SoS exemplifies complex systems behavior: non-linear interactions, adaptation, self-organization, and emergence. Understanding complexity is essential for effective SoS engineering — traditional reductionist approaches that work for individual systems may fail to capture SoS-level behavior.

## Socio-Technical Features of SoS

SoS invariably involves both technical and human/organizational elements. Key considerations include stakeholder alignment across organizations, cultural differences between constituent system teams, contractual and legal boundaries, and the role of trust in collaborative SoS environments.

## Capability Engineering

Capability engineering focuses on delivering operational capabilities rather than individual systems. It shifts the perspective from "building a system" to "delivering a capability," considering the full DOTMLPF spectrum (Doctrine, Organization, Training, Materiel, Leadership, Personnel, Facilities). This is the dominant paradigm in defense and large-scale infrastructure SoS.

## Mission Engineering

Mission engineering extends capability engineering by focusing on specific mission outcomes. It integrates systems, operations, and tactics to achieve mission success under defined scenarios and conditions. Mission engineering emphasizes:
- Mission thread analysis
- End-to-end mission performance assessment
- Interoperability across mission partners
- Resilience under contested or degraded conditions

## Related Chapters

- [[11_Enterprise_Systems_Engineering]] — Enterprise SE shares many SoS challenges at the organizational level
- [[07_System_Definition_and_Architecture]] — Architecture methods applicable to SoS
- [[04_Systems_Models_and_Approach]] — The systems approach applied to complex engineered systems
- [[03_Systems_Science_and_Thinking]] — Complexity theory and emergence
