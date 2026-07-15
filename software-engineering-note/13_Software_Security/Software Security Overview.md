---
tags:
  - overview
  - swebok
  - software-security
  - cybersecurity
  - threat-modeling
  - secure-coding
  - devsecops
---

# Software Security — Overview

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 13 — Software Security *(NEW in v4)*
> **Purpose:** Protect software systems from malicious attacks, unauthorized access, and security vulnerabilities throughout the development lifecycle.

## What Is This?

Software Security is the discipline of building software that is resistant to attack, that protects the confidentiality, integrity, and availability (CIA triad) of data and services, and that fails safely when compromised. Security is not a feature to be added at the end of development — it must be designed in from the start, tested throughout, and maintained continuously as new threats emerge. SWEBOK v4 elevates Software Security to a full Knowledge Area (new in this edition), reflecting the reality that security is now a fundamental quality attribute for virtually all software systems.

The threat landscape has expanded dramatically: cloud-native architectures, APIs, mobile platforms, IoT devices, supply chain dependencies, and AI systems each introduce unique attack surfaces. The cost of security failures is severe — data breaches, ransomware, regulatory fines, and loss of user trust. Yet security is often under-resourced because its value is measured in incidents that *didn't* happen.

Modern security engineering takes a "shift left" approach: integrating security practices into every phase of the development lifecycle rather than relying on a final security review. Threat modeling during design, secure coding during construction, security testing during verification, and vulnerability management during operations form a continuous security discipline. DevSecOps operationalizes this by embedding security automation into CI/CD pipelines.

## The 6 Topic Areas

### 1. [[Security Fundamentals]]
- CIA triad: Confidentiality, Integrity, Availability
- Security principles: least privilege, defense in depth, fail-safe defaults, separation of duties, economy of mechanism
- Authentication vs. authorization vs. accountability
- Cryptographic foundations: symmetric/asymmetric encryption, hashing, digital signatures, PKI
- Security architecture patterns: zero trust, perimeter defense, micro-segmentation
- **Book:** *Security Engineering, 3rd Ed.* — Ross Anderson

### 2. [[Threat Modeling]]
- STRIDE model: Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege
- Attack trees and attack libraries
- Data flow diagrams with trust boundaries
- DREAD risk rating: Damage, Reproducibility, Exploitability, Affected Users, Discoverability
- Threat modeling as a design activity: when, who, how
- Tools: Microsoft Threat Modeling Tool, OWASP Threat Dragon, pytm
- **Book:** *Threat Modeling: Designing for Security* — Adam Shostack

### 3. [[Secure Coding Practices]]
- Input validation and output encoding (prevent injection attacks)
- Memory safety: buffer overflows, use-after-free, format string vulnerabilities
- Authentication and session management best practices
- Secure error handling and logging (don't leak information)
- Language-specific concerns: C/C++ memory safety, JavaScript prototype pollution, Python deserialization
- OWASP guidelines and secure coding standards (CERT, CWE)
- **Book:** *Secure Coding in C and C++, 2nd Ed.* — Robert Seacord

### 4. [[Security Testing]]
- Static Application Security Testing (SAST): analyzing source code for vulnerabilities
- Dynamic Application Security Testing (DAST): testing running applications with attack payloads
- Interactive Application Security Testing (IAST): combining SAST and DAST
- Penetration testing: manual, expert-driven attack simulation
- Fuzz testing: automated input mutation to find crashes and unexpected behavior
- Security regression testing: ensuring fixes stay fixed
- **Book:** *The Web Application Hacker's Handbook, 2nd Ed.* — Stuttard & Pinto

### 5. [[Vulnerability Management]]
- CVE (Common Vulnerabilities and Exposures): standardized vulnerability identifiers
- CWE (Common Weakness Enumeration): cataloging software weakness types
- CVSS (Common Vulnerability Scoring System): severity scoring
- Vulnerability disclosure: responsible disclosure, bug bounties, coordinated vulnerability disclosure
- Dependency scanning and Software Composition Analysis (SCA)
- Patch management and vulnerability remediation prioritization
- **Book:** *The Tangled Web* — Michał Zalewski

### 6. [[DevSecOps]]
- Security as code: automated security checks in CI/CD pipelines
- Infrastructure security: container scanning, IaC policy enforcement, secrets management
- Runtime security: WAF, RASP, intrusion detection
- Security champions: embedding security expertise in development teams
- Compliance as code: automated policy enforcement (Open Policy Agent, Sentinel)
- Security metrics: vulnerability density, mean time to remediate, security debt
- **Book:** *Security Engineering, 3rd Ed.* — Ross Anderson

## Recommended Books (Priority Order)

| # | Book | Author(s) | Pages | Priority |
|---|------|-----------|:-----:|:--------:|
| 1 | *Security Engineering, 3rd Ed.* (2020) | Ross Anderson | 1200 | 🔴 Essential |
| 2 | *The Web Application Hacker's Handbook, 2nd Ed.* (2011) | Stuttard & Pinto | 912 | 🔴 Essential |
| 3 | *Threat Modeling: Designing for Security* (2014) | Adam Shostack | 624 | 🟡 Recommended |
| 4 | *Secure Coding in C and C++, 2nd Ed.* (2013) | Robert Seacord | 432 | 🟡 Recommended |
| 5 | *The Tangled Web* (2011) | Michał Zalewski | 320 | 🟢 Supplementary |

## Relationship to Other KAs

- **[[Software Quality Overview|Software Quality]]** — Security is a quality attribute. Security defects are a category of quality defects. Related cybersecurity notes exist in `programming-note/Cybersecurity/`.
- **[[Software Requirements Overview|Software Requirements]]** — Security requirements (authentication, authorization, encryption, audit logging) must be elicited alongside functional requirements.
- **[[Software Architecture Overview|Software Architecture]]** — Security architecture (zero trust, defense in depth, secure communication patterns) is a foundational design concern.
- **[[Software Testing Overview|Software Testing]]** — Security testing (SAST, DAST, penetration testing, fuzzing) is a specialized testing discipline.
- **[[Software Engineering Operations Overview|Software Engineering Operations]]** — DevSecOps integrates security into operational pipelines. Runtime security monitoring is an operations concern.
- **[[Software Construction Overview|Software Construction]]** — Secure coding practices are construction practices. Input validation, memory safety, and error handling are coded at construction time.

## Related
- [[SWEBOK v4 - Overview]]
- [[12_Software_Quality/Software Quality Overview|Software Quality Overview]]
- [[05_Software_Testing/Software Testing Overview|Software Testing Overview]]
