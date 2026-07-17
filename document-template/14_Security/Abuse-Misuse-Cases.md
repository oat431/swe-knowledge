---
document_type: Abuse / Misuse Cases
version: "1.0"
status: Draft
author: "[Security Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [abuse-cases, misuse-cases, security-requirements, swebok]
standard_ref:
  - SWEBOK v4 — Security
---

# Abuse / Misuse Cases

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines how the system could be misused or abused — identifying security requirements through adversarial thinking.

## 2. Abuse Case vs Use Case

| Aspect | Use Case | Abuse Case |
|--------|---------|-----------|
| **Actor** | [Legitimate user] | [Attacker / Misuser] |
| **Goal** | [Achieve business objective] | [Cause harm, gain unauthorized access] |
| **Focus** | [Functionality] | [Security] |
| **Output** | [Requirements] | [Security requirements] |

## 3. Abuse Cases

### AC-001: Unauthorized Data Access

| Field | Detail |
|-------|--------|
| **ID** | [AC-001] |
| **Title** | [Access another user's data] |
| **Actor** | [Malicious user / Compromised account] |
| **Goal** | [View or modify data belonging to other users] |
| **Precondition** | [User has valid account] |
| **Attack Steps** | [1. Enumerate user IDs<br>2. Modify request ID in API call<br>3. Access unauthorized data] |
| **Impact** | [Data breach, privacy violation] |
| **Mitigation** | [RBAC, ownership validation on every request] |
| **Security Req** | [SEC-011: Users shall only access own data] |

### AC-002: Brute Force Authentication

| Field | Detail |
|-------|--------|
| **ID** | [AC-002] |
| **Title** | [Brute force login credentials] |
| **Actor** | [External attacker] |
| **Goal** | [Gain unauthorized access by guessing passwords] |
| **Precondition** | [Login endpoint accessible] |
| **Attack Steps** | [1. Automate login attempts<br>2. Try common passwords<br>3. Try credential stuffing] |
| **Impact** | [Account compromise] |
| **Mitigation** | [Rate limiting, account lockout, MFA] |
| **Security Req** | [SEC-003: Account lockout after 5 failed attempts] |

### AC-003: SQL Injection

| Field | Detail |
|-------|--------|
| **ID** | [AC-003] |
| **Title** | [Inject malicious SQL via input] |
| **Actor** | [External attacker] |
| **Goal** | [Extract or modify database data] |
| **Precondition** | [Input field accepts user input] |
| **Attack Steps** | [1. Identify injectable input<br>2. Craft SQL payload<br>3. Execute malicious query] |
| **Impact** | [Data breach, data modification] |
| **Mitigation** | [Parameterized queries, input validation, WAF] |
| **Security Req** | [SEC-031: SQL queries shall use parameterized statements] |

### AC-004: Denial of Service

| Field | Detail |
|-------|--------|
| **ID** | [AC-004] |
| **Title** | [Overwhelm system with requests] |
| **Actor** | [External attacker] |
| **Goal** | [Make system unavailable] |
| **Precondition** | [API endpoint accessible] |
| **Attack Steps** | [1. Identify high-resource endpoints<br>2. Send flood of requests<br>3. Exhaust server resources] |
| **Impact** | [Service unavailability] |
| **Mitigation** | [Rate limiting, WAF, CDN, auto-scaling] |
| **Security Req** | [SEC-051: API shall implement rate limiting] |

### AC-005: Privilege Escalation

| Field | Detail |
|-------|--------|
| **ID** | [AC-005] |
| **Title** | [Gain elevated privileges] |
| **Actor** | [Malicious user] |
| **Goal** | [Access admin functions without authorization] |
| **Precondition** | [User has standard account] |
| **Attack Steps** | [1. Identify admin endpoints<br>2. Manipulate role parameter<br>3. Access admin functions] |
| **Impact** | [Full system compromise] |
| **Mitigation** | [Server-side role validation, no client-side role claims] |
| **Security Req** | [SEC-012: Admin functions require admin role] |

### AC-006: Data Exfiltration

| Field | Detail |
|-------|--------|
| **ID** | [AC-006] |
| **Title** | [Bulk data export by authorized user] |
| **Actor** | [Malicious insider / Compromised account] |
| **Goal** | [Extract large volume of sensitive data] |
| **Precondition** | [User has data access] |
| **Attack Steps** | [1. Use legitimate export function<br>2. Export maximum allowed data<br>3. Repeat until dataset complete] |
| **Impact** | [Data breach, IP theft] |
| **Mitigation** | [Export limits, DLP monitoring, audit logging] |
| **Security Req** | [SEC-041: All data access shall be logged] |

## 4. Abuse Case Summary

| ID | Title | Impact | Mitigation | Status |
|----|-------|--------|-----------|--------|
| [AC-001] | [Unauthorized Data Access] | 🔴 Critical | [RBAC + ownership validation] | ✅ Mitigated |
| [AC-002] | [Brute Force Auth] | 🔴 Critical | [Rate limiting + lockout + MFA] | ✅ Mitigated |
| [AC-003] | [SQL Injection] | 🔴 Critical | [Parameterized queries + WAF] | ✅ Mitigated |
| [AC-004] | [Denial of Service] | 🟡 Medium | [Rate limiting + CDN + WAF] | ✅ Mitigated |
| [AC-005] | [Privilege Escalation] | 🔴 Critical | [Server-side role validation] | ✅ Mitigated |
| [AC-006] | [Data Exfiltration] | 🟡 Medium | [Export limits + DLP + audit] | ✅ Mitigated |

## 5. Security Requirements Derived

| Abuse Case | Security Requirement | Source |
|-----------|---------------------|--------|
| [AC-001] | [SEC-011: Users shall only access own data] | [[Security-Requirements-Specification]] |
| [AC-002] | [SEC-003: Account lockout after 5 failed attempts] | [[Security-Requirements-Specification]] |
| [AC-003] | [SEC-031: Parameterized queries] | [[Security-Requirements-Specification]] |
| [AC-004] | [SEC-051: Rate limiting] | [[Security-Requirements-Specification]] |
| [AC-005] | [SEC-012: Admin role required] | [[Security-Requirements-Specification]] |
| [AC-006] | [SEC-041: All data access logged] | [[Security-Requirements-Specification]] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Threat-Model]] | Threat analysis |
| [[Security-Requirements-Specification]] | Requirements derived from abuse cases |
| [[Secure-Design-Review-Report]] | Design review |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Abuse cases are *adversarial use cases*. For every use case, ask "How could this be misused?" Document the answer.
