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

### AC-[XX]: [Abuse Case Title]

| Field | Detail |
|-------|--------|
| **ID** | [AC-XX] |
| **Title** | [What the attacker achieves] |
| **Actor** | [Attacker / Misuser type] |
| **Goal** | [Attacker's goal] |
| **Precondition** | [Condition that must hold] |
| **Attack Steps** | [1. Step one<br>2. Step two<br>3. Step three] |
| **Impact** | [Business/security impact] |
| **Mitigation** | [Controls that prevent or detect] |
| **Security Req** | [SEC-XXX: Derived security requirement] |

> **Repeat this format for each abuse/misuse case.**

## 4. Abuse Case Summary

| ID | Title | Impact | Mitigation | Status |
|----|-------|--------|-----------|--------|
| [AC-XX] | [Title] | [🔴 Critical / 🟡 Medium / 🟢 Low] | [Mitigation] | [Status] |

## 5. Security Requirements Derived

| Abuse Case | Security Requirement | Source |
|-----------|---------------------|--------|
| [AC-XX] | [SEC-XXX: Derived requirement] | [[Security-Requirements-Specification]] |

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