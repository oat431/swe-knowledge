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

> **Source:** SWEBOK v4 Chapter 13 *(NEW in v4)*
> **Purpose:** Build security into software throughout the entire development lifecycle — from requirements and design through construction, testing, and maintenance.

## What Is This?

Software Security is the engineering discipline of building software that resists attack, protects the confidentiality, integrity, and availability (CIA triad) of data and services, and fails safely when compromised. SWEBOK v4 elevates this to a full Knowledge Area, reflecting the reality that security is now a fundamental quality attribute for virtually all software systems. The core principle: build security in from the start — don't bolt it on afterward.

The threat landscape has expanded dramatically with cloud-native architectures, APIs, IoT devices, supply chain dependencies, and AI/ML systems each introducing unique attack surfaces. Modern security engineering takes a "shift left" approach: threat modeling during design, secure coding during construction, security testing during verification, and vulnerability management during operations. DevSecOps operationalizes this by embedding security automation into CI/CD pipelines.

Organizational governance matters too. The SSE-CMM measures process capability, ISO/IEC 27001 establishes Information Security Management Systems, and the Common Criteria (ISO/IEC 15408) provides evaluation frameworks. In Agile contexts, security teams must become enablers rather than blockers — working iteratively, automating checks, and integrating with development workflows.

## Knowledge Areas

### Software Security Fundamentals
- CIA triad (Confidentiality, Integrity, Availability) plus authenticity, accountability, non-repudiation
- Software security as a product quality characteristic per ISO/IEC 25010:2023
- Cybersecurity scope: protecting people and organizations from social engineering, hacking, malware, spyware

### Security Management and Organization
- SSE-CMM for measuring organizational security process capability
- ISO/IEC 27001:2022 for establishing an Information Security Management System (ISMS)
- Adapting security practices to Agile: security as enabler, automation, involvement with developers

### Software Security Engineering and Processes
- Secure Development Life Cycle (SDLC) as a spiral model treating security holistically
- DevSecOps: integrating security into CI/CD culture with automation and platform design
- Common Criteria (ISO/IEC 15408:2022) for evaluating IT product security functionality and assurance

### Security Engineering for Software Systems
- Security requirements: misuse/abuse cases, threat actors, risk assessment
- Security design: access control, cryptography, threat modeling, security patterns
- Secure construction: CERT Top 10 practices (input validation, least privilege, defense in depth)
- Security testing: static analysis, penetration testing, fuzzing, vulnerability assessment (CVE, CWE, CVSS)

### Software Security Tools
- Source code analyzers detecting injection flaws, buffer overflows, insecure library usage
- Binary analysis tools examining compiled code for hidden vulnerabilities
- Penetration testing and fuzzing tools for dynamic operational assessment

### Domain-Specific Software Security
- Container and cloud security: forgotten assets, outsourced physical controls
- IoT security: massive endpoint attack surface, device-to-device communication hardening
- Machine learning security: model poisoning attacks on training data, evasion attacks on inputs

## My Notes

### Security Engineering (Anderson)
- [[01_Security_Fundamentals]] — Security framework, opponent types, psychology/usability, security economics
- [[02_Protocols_and_Cryptography]] — Authentication protocols, symmetric/asymmetric crypto, TLS 1.3
- [[03_Access_Control_and_Architecture]] — ACLs/capabilities, OS security, MLS, distributed systems, inference
- [[04_Network_Attack_and_Defence]] — Network attacks, DDoS, DNS, firewalls, IDS, malware, botnets
- [[05_Secure_Development_and_Assurance]] — Secure SDLC, threat modeling, assurance, Common Criteria

### Applied Security
- [[Cybersecurity/]]

## Relationship to Other KAs

- **[[Software Quality Overview|Software Quality]]** — Security is a quality attribute; security defects are quality defects.
- **[[Software Requirements Overview|Software Requirements]]** — Security requirements (authentication, authorization, encryption) must be elicited alongside functional requirements.
- **[[Software Architecture Overview|Software Architecture]]** — Security architecture (zero trust, defense in depth) is a foundational design concern.
- **[[Software Testing Overview|Software Testing]]** — Security testing (SAST, DAST, penetration testing, fuzzing) is a specialized testing discipline.
- **[[Software Engineering Operations Overview|Software Engineering Operations]]** — DevSecOps integrates security into operational pipelines; runtime security monitoring is an operations concern.
- **[[Software Construction Overview|Software Construction]]** — Secure coding practices (input validation, memory safety, error handling) are construction activities.

---

## SWEBOK v4 Coverage Map

> **Source:** [[SWEBOK v4 - Overview|SWEBOK v4]] Chapter 13 *(NEW in v4)* | **Last analyzed:** 2026-07-21 | **Coverage:** ~85% (updated)

| # | SWEBOK Topic | Status | Vault File(s) | Notes |
|---|---|---|---|---|
| 1 | Security Fundamentals | ✅ | `01_Security_Fundamentals.md` (18 KB) | CIA triad, policy/mechanism/assurance framework |
| 2 | Security Management & Organization | ✅ | `08_Security_Management_and_Governance.md` (27 KB) | SSE-CMM, ISO 27001, NIST CSF, SOC 2, PCI DSS, shift-left |
| 3 | Security Engineering & Processes | ✅ | `05_Secure_Development_and_Assurance.md` (21 KB) | Secure SDLC, DevSecOps, Common Criteria |
| 4 | Security Engineering for Systems | ✅ | `02`, `03` | Crypto, TLS, ACLs, distributed systems — excellent depth |
| 5 | Software Security Tools | ✅ | `04`, `07_Vulnerability_Management.md` (21 KB) | CVE/CWE/CAPEC/CVSS, scanning tools, pen testing, bug bounty |
| 6 | Domain-Specific Security | ✅ | `06_Domain_Specific_Security.md` (25 KB) | Cloud, IoT, ML, mobile, API security |

### Gaps to Fill

| Priority | Gap | SWEBOK Topic | What's Missing |
|----------|-----|-------------|----------------|
| 🔴 High | Domain-Specific Security | Domain-Specific | Container/cloud security, IoT security, ML security (poisoning, evasion) |
| 🔴 High | Vulnerability Management | Security Engineering | CVE, CWE, CAPEC, CVSS taxonomies and scoring |
| 🟡 Medium | Security Management | Management & Org | SSE-CMM, ISO 27001 ISMS organizational governance |
| 🟡 Medium | CERT Secure Coding Practices | Security Engineering | CERT Top 10: input validation, least privilege, defense in depth |
| 🟢 Low | Security Testing Techniques | Tools | SAST, DAST, fuzzing, web app scanners (cross-covered by Ch.05) |
