---
tags: [aaa, authentication, authorisation, accountability, cyber-security, cybok]
---

# 13 — Authentication, Authorisation & Accountability

> *Source: CyBOK v1.1 by NCSC, Chapter 14 — Authentication, Authorisation & Accountability*

## Purpose

AAA forms the foundation of access control in cyber security. Authentication verifies identity, authorisation determines what an authenticated entity is permitted to do, and accountability ensures actions can be traced to responsible entities.

## Authentication

### Authentication Factors
- **Knowledge** — Something you know (passwords, PINs, security questions)
- **Possession** — Something you have (tokens, smart cards, mobile devices)
- **Inherence** — Something you are (biometrics: fingerprint, face, iris, voice)
- **Location / Context** — Where you are or what you're doing

### Multi-Factor Authentication (MFA)
Combining two or more independent factors. Increases security by requiring an attacker to compromise multiple channels.

### Authentication Protocols
- Password-based: PAP, CHAP, SRP
- Challenge-response protocols
- Kerberos (ticket-based)
- Public-key based (PKI certificates)
- FIDO2 / WebAuthn (passwordless)

## Authorisation

### Access Control Models
- **DAC** (Discretionary Access Control) — Resource owners control access; Unix file permissions
- **MAC** (Mandatory Access Control) — System-enforced policy based on security labels; SELinux
- **RBAC** (Role-Based Access Control) — Permissions assigned to roles, users assigned to roles
- **ABAC** (Attribute-Based Access Control) — Policy based on user, resource, and environment attributes; XACML
- **Capability-Based** — Unforgeable tokens granting specific rights; capability systems

### Principle of Least Privilege
Entities should be granted only the minimum permissions necessary to perform their function.

## Accountability

### Audit and Logging
Recording security-relevant events: authentication attempts, access decisions, privilege changes. Requirements: completeness, integrity, non-repudiation.

### Audit Trail Analysis
Automated and manual review of logs to detect policy violations, security incidents, and anomalies.

## Related Chapters

- [[01_Risk_Management_and_Governance]] — Governance of access control policies
- [[02_Law_and_Regulation]] — Legal requirements for authentication and audit
- [[10_Operating_Systems_and_Virtualisation]] — OS-level access control (SELinux, capabilities)
