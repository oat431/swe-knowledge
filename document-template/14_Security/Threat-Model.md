---
document_type: Threat Model
version: "1.0"
status: Draft
author: "[Security Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Confidential"
tags: [threat-model, stride, attack-surface, cyberok, swebok]
standard_ref:
  - CyBOK v1 — Security Risk Management
  - SWEBOK v4 — Security
  - Microsoft STRIDE
---

# Threat Model

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Structured analysis of attack surfaces, threat actors, and attack vectors — identifying what can go wrong before it does.

## 2. Threat Modeling Approach

| Aspect | Approach |
|--------|---------|
| [Method] | [STRIDE + Attack Trees] |
| [Scope] | [All user-facing interfaces, APIs, data stores] |
| [Participants] | [Security Engineer, Tech Lead, Dev Team] |
| [Tools] | [Microsoft Threat Modeling Tool / OWASP Threat Dragon] |

## 3. System Overview

```mermaid
flowchart TB
    subgraph External["External"]
        USER([User])
        ATTACKER([Attacker])
    end

    subgraph System["System"]
        WEB[Web App]
        API[API Gateway]
        SVC[Services]
        DB[(Database)]
        CACHE[(Cache)]
    end

    subgraph ThirdParty["Third Party"]
        ERP[ERP]
        PAY[Payment]
        EMAIL[Email]
    end

    USER --> WEB
    ATTACKER -.->|Attack| WEB
    WEB --> API
    API --> SVC
    SVC --> DB
    SVC --> CACHE
    SVC --> ERP
    SVC --> PAY
    SVC --> EMAIL

    style External fill:#607D8B,color:#fff
    style System fill:#2196F3,color:#fff
    style ThirdParty fill:#9C27B0,color:#fff
```

## 4. STRIDE Analysis

| Threat | Description | Component | Risk | Mitigation |
|--------|-----------|----------|------|-----------|
| **S**poofing | [Impersonate user] | [Auth] | 🔴 | [MFA, JWT validation] |
| **T**ampering | [Modify data in transit] | [API] | 🔴 | [TLS 1.3, integrity checks] |
| **R**epudiation | [Deny action] | [All] | 🟡 | [Audit logging, non-repudiation] |
| **I**nformation Disclosure | [Expose sensitive data] | [DB, API] | 🔴 | [Encryption, access controls] |
| **D**enial of Service | [Overwhelm system] | [API] | 🟡 | [Rate limiting, WAF] |
| **E**levation of Privilege | [Gain unauthorized access] | [Auth] | 🔴 | [RBAC, least privilege] |

## 5. Attack Surface Analysis

| Entry Point | Exposure | Threats | Controls |
|------------|---------|---------|---------|
| [Web Application] | [Public] | [XSS, CSRF, injection] | [WAF, CSP, input validation] |
| [REST API] | [Public] | [Injection, broken auth] | [Rate limiting, OAuth2, validation] |
| [Database] | [Internal] | [SQL injection, data theft] | [Parameterized queries, encryption] |
| [File Upload] | [Public] | [Malware, path traversal] | [Type validation, size limits, scanning] |
| [Email Service] | [External] | [Phishing, spoofing] | [SPF, DKIM, DMARC] |
| [Payment Gateway] | [External] | [Data interception] | [PCI DSS compliance, tokenization] |

## 6. Attack Trees

### Attack: Steal Customer Data

```mermaid
flowchart TD
    GOAL[Steal<br>Customer Data] --> OR1{OR}
    OR1 --> A1[SQL Injection]
    OR1 --> A2[Compromise Credentials]
    OR1 --> A3[Insider Access]
    
    A1 --> A1a[Find injectable endpoint]
    A1 --> A1b[Bypass WAF]
    
    A2 --> A2a[Phishing]
    A2 --> A2b[Credential stuffing]
    A2 --> A2c[Brute force]
    
    A3 --> A3a[Excessive access]
    A3 --> A3b[Social engineering]

    style GOAL fill:#f44336,color:#fff
    style OR1 fill:#FF9800,color:#fff
```

## 7. Threat Mitigations

| Threat | Mitigation | Implementation | Status |
|--------|----------|---------------|--------|
| [SQL Injection] | [Parameterized queries] | [ORM + prepared statements] | ✅ |
| [XSS] | [Output encoding + CSP] | [React auto-escape + CSP headers] | ✅ |
| [CSRF] | [CSRF tokens] | [SameSite cookies + tokens] | ✅ |
| [Credential Theft] | [MFA + monitoring] | [Auth service + alerting] | ✅ |
| [Data Exposure] | [Encryption + access control] | [KMS + RBAC] | ✅ |
| [DDoS] | [Rate limiting + WAF] | [API gateway + Cloud WAF] | ✅ |

## 8. Threat Model Review Schedule

| Frequency | Activity | Participants |
|----------|---------|-------------|
| [Per sprint] | [Review new features] | [Security + Dev] |
| [Quarterly] | [Full model review] | [Security + Architecture] |
| [After incident] | [Update based on findings] | [Security Team] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Risk-Assessment-Report-Security]] | Risk identification |
| [[Secure-Design-Review-Report]] | Design security review |
| [[Security-Requirements-Specification]] | Security requirements |

---

> **Template Standard:** Based on CyBOK v1, SWEBOK v4, Microsoft STRIDE
> **Usage:** Threat model *before* you build, not after you're breached. Every new feature gets threat-modeled.
