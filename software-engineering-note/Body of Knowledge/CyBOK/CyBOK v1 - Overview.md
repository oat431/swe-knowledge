---
tags: [overview, cyber-security, cybok]
---

# CyBOK v1.1 — Cyber Security Body of Knowledge

> **The Cyber Security Body of Knowledge (CyBOK) v1.1**
> National Cyber Security Centre (NCSC), UK Government, July 2021
> 21 Knowledge Areas | ~1,067 pages | https://www.cybok.org/

## What Is This?

The **Cyber Security Body of Knowledge (CyBOK)** is a comprehensive reference mapping the foundations of cyber security. Commissioned by the UK National Cyber Security Centre (NCSC) and developed through extensive community engagement with academia, industry, and government, CyBOK defines what constitutes the core knowledge of the cyber security discipline.

Version 1.1 adds two new Knowledge Areas (Applied Cryptography and Formal Methods for Security) and includes major revisions to Network Security and Risk Management & Governance. It serves as a foundation for curriculum development, professional certification (CISSP, IISP), job descriptions, and research framing.

## The 21 Knowledge Areas

| # | Knowledge Area | Focus |
|---|---|---|
| 1 | [[01_Risk_Management_and_Governance]] | Risk concepts, governance frameworks, security policy, culture |
| 2 | [[02_Law_and_Regulation]] | GDPR, computer misuse, IP, regulatory compliance, liability |
| 3 | [[03_Human_Factors]] | Usable security, human behaviour, security UX |
| 4 | [[04_Privacy_and_Online_Rights]] | Privacy concepts, online rights, data protection |
| 5 | [[05_Malware_and_Attack_Technologies]] | Malware types, attack vectors, detection |
| 6 | [[06_Adversarial_Behaviours]] | Attacker models, motivations, tactics, techniques |
| 7 | [[07_Security_Operations_and_Incident_Management]] | SOC, SIEM, incident response, threat intelligence |
| 8 | [[08_Forensics]] | Digital forensics, evidence handling, investigation |
| 9 | [[09_Software_Security]] | Secure coding, vulnerabilities, static/dynamic analysis |
| 10 | [[10_Operating_Systems_and_Virtualisation]] | OS security, virtualisation, hypervisors |
| 11 | [[11_Distributed_Systems_Security]] | Consensus, blockchain, distributed system threats |
| 12 | [[12_Formal_Methods_for_Security]] | Formal verification, model checking, theorem proving |
| 13 | [[13_Authentication_Authorisation_Accountability]] | AAA, authentication methods, access control models |
| 14 | [[14_Secure_Software_Lifecycle]] | SSDLC, DevSecOps, security in requirements/design/test |
| 15 | [[15_Web_and_Mobile_Security]] | Web attacks, mobile platform security |
| 16 | [[16_Applied_Cryptography]] | Symmetric/asymmetric crypto, hashing, PKI, protocols |
| 17 | [[17_Network_Security]] | Network attacks, firewalls, IDS/IPS, VPN, protocol security |
| 18 | [[18_Hardware_Security]] | TPM, HSM, side-channel attacks, secure hardware |
| 19 | [[19_Cyber_Physical_Systems_Security]] | CPS/IoT, SCADA, industrial control security |
| 20 | [[20_Physical_Layer_and_Telecommunications_Security]] | Physical layer attacks, telecom security |

Plus [[00_Introduction_to_CyBOK]] for the cyber security definition and how to deploy CyBOK knowledge.

## How to Use This Vault

Each Knowledge Area follows a consistent SWEBOK-style reference format:
1. **Purpose** — What the KA covers and why it matters
2. **Core Content** — Key concepts, models, techniques, and principles
3. **Related Chapters** — [[wikilinks]] connecting across CyBOK

Start with [[00_Introduction_to_CyBOK]] for the big picture. Then explore by need — the [[wikilinks]] pull you naturally into related areas.

## Reading Paths

- **Security generalist:** [[01_Risk_Management_and_Governance|Risk & Governance]] → [[02_Law_and_Regulation|Law]] → [[03_Human_Factors|Human Factors]] → [[07_Security_Operations_and_Incident_Management|SecOps]]
- **Security engineer:** [[09_Software_Security|Software Security]] → [[14_Secure_Software_Lifecycle|SSDLC]] → [[17_Network_Security|Network]] → [[10_Operating_Systems_and_Virtualisation|OS]]
- **Security architect:** [[13_Authentication_Authorisation_Accountability|AAA]] → [[16_Applied_Cryptography|Crypto]] → [[11_Distributed_Systems_Security|Distributed Systems]] → [[18_Hardware_Security|Hardware]]
- **Offensive/defensive:** [[05_Malware_and_Attack_Technologies|Malware]] → [[06_Adversarial_Behaviours|Adversarial]] → [[15_Web_and_Mobile_Security|Web & Mobile]]
- **Emerging/advanced:** [[19_Cyber_Physical_Systems_Security|CPS/IoT]] → [[12_Formal_Methods_for_Security|Formal Methods]] → [[20_Physical_Layer_and_Telecommunications_Security|Physical Layer]]

## Related

- [[../Body of Knowledge - Overview|Body of Knowledge — Overview]] — All five BOKs
- [[../SWEBOK/SWEBOK v4 - Overview|SWEBOK v4]] — Software Engineering
- [[../SEBoK/SEBoK v2 - Overview|SEBoK v2]] — Systems Engineering
