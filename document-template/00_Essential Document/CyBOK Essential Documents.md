---
tags: [overview, cyber-security, cybok, essential-documents]
---

# CyBOK — Essential Documents by Knowledge Area

> **Source:** [[CyBOK v1 - Overview|CyBOK v1.1 (NCSC, 2021)]] — 21 Knowledge Areas
> Organized by security domain: Governance → Offensive → Defensive → Software → Infrastructure → Advanced
>
> ⚠️ **This is a document-only extract.** For the full body of knowledge, see:
> `F:\projects\orlita_md\software-engineering-note\Body of Knowledge\CyBOK\`

**Priority Legend:**
| Icon | Level | Meaning |
|---|---|---|
| 🔴 | **Must Have** | Essential for virtually all security programs; skipping creates significant risk |
| 🟡 | **Nice to Have** | Recommended for medium+ maturity; adds substantial value |
| 🟢 | **Optional** | Situational — depends on domain, scale, regulatory needs, or threat profile |

---

## 1. Governance & Risk

> **Owner:** CISO / Risk Manager

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Risk Assessment Report** | Identified threats, vulnerabilities, likelihood, impact, and risk levels per NIST SP 800-30 or ISO/IEC 27005 | 🔴 Must Have | ISO/IEC 27005, NIST SP 800-30 |
| **Risk Register** | Living document of all identified risks with owners, mitigation strategies, and status | 🔴 Must Have | ISO 31000, ISO/IEC 27001:2022 |
| **Risk Treatment Plan** | Decisions for each risk: accept, avoid, mitigate, share, or transfer | 🔴 Must Have | ISO/IEC 27005 |
| **Security Policy** | Enactable rules governing security behaviour, responsibilities, and consequences | 🔴 Must Have | ISO/IEC 27001:2022 |
| **Information Security Management System (ISMS)** | Documented systematic approach to managing security, including scope, policy, risk assessment, controls | 🔴 Must Have | ISO/IEC 27001:2022 |
| **Statement of Applicability (SoA)** | List of ISO 27001 controls selected with justification for inclusions/exclusions | 🔴 Must Have | ISO/IEC 27001:2022 |
| **Business Continuity Plan (BCP)** | Procedures for maintaining critical operations during and after disruptions | 🔴 Must Have | ISO 22301, ISO/IEC 27035 |
| **Incident Response Plan** | Defined phases: prepare, detect, assess, respond, learn per ISO/IEC 27035 | 🔴 Must Have | ISO/IEC 27035 |
| **Vulnerability Management Report** | Prioritized vulnerabilities with patching status, risk decisions, and justifications | 🔴 Must Have | ISO/IEC 27002 |
| **Security Metrics Dashboard** | Consistently measured KPIs: training completion, patch latency, incident rates | 🔴 Must Have | ISO/IEC 27004 |
| **Compliance Assessment Report** | Evaluation against GDPR, PCI-DSS, HIPAA, NIS Directive, and other applicable regulations | 🔴 Must Have | GDPR, PCI-DSS, HIPAA |
| **Security Awareness Training Records** | Evidence of employee security education and phishing simulation results | 🔴 Must Have | ISO/IEC 27002 |
| **Third-Party Risk Assessment** | Security evaluation of vendors, suppliers, and partners | 🟡 Nice to Have | ISO/IEC 27036 |
| **Data Protection Impact Assessment (DPIA)** | Privacy risk assessment for processing personal data | 🔴 Must Have | GDPR (Art. 35) |
| **Privacy Policy & Notice** | Public-facing and internal privacy documentation | 🔴 Must Have | GDPR, ISO/IEC 27701 |
| **Security Culture Assessment** | Evaluation of organizational security behaviours, attitudes, and Just Culture maturity | 🟡 Nice to Have | — |

## 2. Threat Intelligence & Adversary Analysis

> **Owner:** Threat Intelligence Analyst / SOC Lead

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Threat Model** | Structured analysis of attack surface: STRIDE, attack trees, or system-driven methods (STAMP/STPA-Sec) | 🔴 Must Have | — |
| **Cyber Threat Intelligence (CTI) Report** | Tactical, operational, and strategic threat intelligence: IoCs, TTPs, threat actor profiles | 🔴 Must Have | ISO/IEC 27002 |
| **Malware Analysis Report** | Static and dynamic analysis findings for identified malware samples | 🟡 Nice to Have | — |
| **Adversary Emulation Plan** | MITRE ATT&CK-aligned simulation of adversary TTPs to test defences | 🟡 Nice to Have | MITRE ATT&CK |
| **Attack Surface Analysis** | Comprehensive inventory of exposed assets, services, and entry points | 🔴 Must Have | — |
| **Digital Forensics Report** | Chain-of-custody-preserving investigation findings for legal or internal use | 🔴 Must Have | ISO/IEC 27037, ISO/IEC 27042 |

## 3. Security Operations

> **Owner:** SOC Manager / SecOps Lead

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **SOC Runbook / Playbook** | Standard operating procedures for alert triage, escalation, and incident handling | 🔴 Must Have | ISO/IEC 27035 |
| **SIEM Configuration & Rules** | Correlation rules, alert thresholds, data source integrations | 🔴 Must Have | — |
| **IDS/IDPS Sensor Deployment Plan** | Sensor placement, monitored zones, capture configuration | 🔴 Must Have | — |
| **Security Monitoring Dashboard** | Real-time telemetry: alerts, trends, asset health, threat feeds | 🔴 Must Have | — |
| **Incident Report** | Post-incident documentation: timeline, root cause, impact, lessons learned | 🔴 Must Have | ISO/IEC 27035 |
| **Post-Incident Review (Post-Mortem)** | Blameless analysis of incident handling effectiveness and process improvements | 🔴 Must Have | ISO/IEC 27035 |
| **SOAR Automation Playbooks** | Automated response workflows for common incident types | 🟡 Nice to Have | — |
| **Forensic Evidence Log** | Chain-of-custody records, evidence handling procedures | 🔴 Must Have | ISO/IEC 27037 |

## 4. Secure Software Development

> **Owner:** AppSec Engineer / DevSecOps Lead

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Security Requirements Specification** | Derived from threat modeling and compliance needs | 🔴 Must Have | ISO/IEC 27001:2022 |
| **Abuse / Misuse Cases** | Negative scenarios: how the system could be attacked | 🟡 Nice to Have | — |
| **Threat Model (Application)** | STRIDE, attack trees per application or feature | 🔴 Must Have | — |
| **Secure Design Review Report** | Architecture and design evaluation for security flaws | 🔴 Must Have | ISO/IEC 27002 |
| **Secure Coding Guidelines** | Language-specific standards: CERT, OWASP, MISRA | 🔴 Must Have | CERT, OWASP ASVS |
| **SAST Report** (Static Analysis) | Source code vulnerability scan results | 🔴 Must Have | ISO/IEC 27001:2022 |
| **SCA Report** (Software Composition Analysis) | Third-party dependency vulnerability scan results | 🔴 Must Have | ISO/IEC 5230 (OpenChain) |
| **DAST Report** (Dynamic Analysis) | Runtime penetration and fuzzing results | 🔴 Must Have | ISO/IEC 27001:2022 |
| **Penetration Test Report** | Controlled attack simulation findings | 🔴 Must Have | ISO/IEC 27001:2022 |
| **Vulnerability Disclosure Report** | Coordinated disclosure process documentation | 🟡 Nice to Have | ISO/IEC 29147 |
| **DevSecOps Pipeline Configuration** | Security automation in CI/CD: SAST, SCA, image scanning, policy-as-code | 🔴 Must Have | — |
| **SSDLC Process Documentation** | Security gates and activities at each SDLC phase | 🔴 Must Have | ISO/IEC 27034 |

## 5. Infrastructure & Network Security

> **Owner:** Network Security Engineer / Infrastructure Security Lead

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Network Security Architecture** | Network segmentation, DMZ design, firewall rules, VPN topology | 🔴 Must Have | ISO/IEC 27033 |
| **Firewall & ACL Configuration** | Rule sets, change management, review schedule | 🔴 Must Have | — |
| **IDS/IDPS Signature Management** | Detection signatures, update procedures, false positive tuning | 🔴 Must Have | — |
| **DNS Security Configuration** | DNSSEC deployment, domain blacklisting, DNS monitoring | 🔴 Must Have | — |
| **TLS/SSL Certificate Inventory** | Certificate lifecycle: issuance, renewal, revocation | 🔴 Must Have | — |
| **VPN & Remote Access Policy** | Secure remote access standards and configuration | 🔴 Must Have | ISO/IEC 27002 |
| **Web Application Firewall (WAF) Rules** | OWASP Top 10 protections, custom rules, rate limiting | 🔴 Must Have | — |
| **Hardware Security Module (HSM) Configuration** | Key management, cryptographic operations, tamper protection | 🔴 Must Have | FIPS 140-2/3 |
| **Cloud Security Architecture** | IAM, network controls, encryption, logging for cloud environments | 🔴 Must Have | ISO/IEC 27017, CSA CCM |
| **Container Security Baseline** | Image scanning, runtime security, least-privilege, admission controls | 🔴 Must Have | — |
| **OS Hardening Standard** | Baseline configuration for OS security: CIS Benchmarks, STIGs | 🔴 Must Have | CIS Benchmarks |
| **Physical Security Assessment** | Physical and environmental controls protecting infrastructure | 🔴 Must Have | ISO/IEC 27002 |

## 6. Identity, Cryptography & Advanced

> **Owner:** Security Architect / Cryptography Engineer

| Document | Description | Priority | ISO/IEEE Reference |
|---|---|---|---|
| **Access Control Policy** | Models: RBAC, ABAC, MAC; least privilege, separation of duties | 🔴 Must Have | ISO/IEC 29146 |
| **Authentication Standard** | MFA requirements, password policy, biometric standards | 🔴 Must Have | NIST SP 800-63, ISO/IEC 24760 |
| **PKI & Certificate Policy** | Certificate authority hierarchy, key lifecycle, revocation procedures | 🔴 Must Have | ISO/IEC 9594-8 (X.509) |
| **Cryptographic Standards** | Approved algorithms, key lengths, protocols, and deprecated methods | 🔴 Must Have | NIST SP 800-57, ISO/IEC 18033 |
| **Key Management Plan** | Key generation, distribution, rotation, storage, and destruction | 🔴 Must Have | NIST SP 800-57, ISO/IEC 11770 |
| **CPS Security Assessment** | Security evaluation of cyber-physical / ICS / SCADA / IoT systems | 🟡 Nice to Have | IEC 62443 |
| **Formal Verification Report** | Mathematical proof of security properties for critical components | 🟢 Optional | — |
| **Hardware Security Evaluation** | Side-channel analysis, fault injection testing, TPM/HSM validation | 🟢 Optional | ISO/IEC 15408 (Common Criteria) |

---

## Related

- [[SWEBOK Essential Documents]] — Software engineering documents by SDLC phase
- [[PMBOK Essential Documents]] — Project management documents by phase
- [[BABOK Essential Documents]] — Business analysis documents by KA
- [[DMBOK Essential Documents]] — Data management documents by KA
- [[CyBOK v1 - Overview]] — Full CyBOK knowledge base
