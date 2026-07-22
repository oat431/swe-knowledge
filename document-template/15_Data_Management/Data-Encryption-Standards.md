---
document_type: Data Encryption Standards
version: "1.0"
status: Draft
author: "[Security Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Confidential"
tags: [encryption, cryptography, dmbok, iso-27001]
standard_ref:
  - DMBOK v2 — Data Security
  - ISO/IEC 27001:2022 — A.10 Cryptography
---

# Data Encryption Standards

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines encryption standards for data at rest, in transit, and in use — protecting data confidentiality.

## 2. Encryption at Rest

| Data Classification | Encryption | Algorithm | Key Management |
|--------------------|-----------|----------|---------------|
| 🔴 L1 Restricted | ✅ Required | [AES-256-GCM] | [KMS, auto-rotate annually] |
| 🟡 L2 Confidential | ✅ Required | [AES-256-GCM] | [KMS, auto-rotate annually] |
| 🟢 L3 Internal | 🟡 Recommended | [AES-256] | [KMS] |
| ⚪ L4 Public | ❌ Not needed | — | — |

## 3. Encryption in Transit

| Connection | Protocol | Certificate | Configuration |
|-----------|---------|-------------|-------------|
| [Client → Load Balancer] | [TLS 1.3] | [Cloud-managed] | [HSTS, OCSP stapling] |
| [Load Balancer → Services] | [TLS 1.3] | [Internal cert] | [Full strict] |
| [Services → Database] | [TLS 1.2+] | [Cloud-managed] | [Required] |
| [Services → Cache] | [TLS 1.2+] | [Cloud-managed] | [Required] |
| [Services → External APIs] | [TLS 1.2+] | [Vendor cert] | [Certificate pinning] |

## 4. Key Management

| Aspect | Standard | Implementation |
|--------|---------|---------------|
| [Key storage] | [HSM-backed] | [AWS KMS / Azure Key Vault] |
| [Key rotation] | [Annual for data keys] | [Auto-rotation] |
| [Key access] | [Least privilege] | [IAM policies] |
| [Key audit] | [All key operations logged] | [CloudTrail / Azure Monitor] |
| [Key recovery] | [Backup keys in secure storage] | [Manual process] |

## 5. Application-Level Encryption

| Field | Algorithm | Key | Implementation |
|-------|----------|-----|---------------|
| [Passwords] | [bcrypt, 12 rounds] | [Per-hash salt] | [Auth service] |
| [PII fields] | [AES-256-GCM] | [KMS data key] | [Application layer] |
| [API tokens] | [HMAC-SHA256] | [Secret key] | [Auth service] |
| [Session tokens] | [JWT RS256] | [RSA key pair] | [Auth service] |

## 6. Encryption Algorithms

| Use Case | Approved | Prohibited |
|---------|---------|-----------|
| [Symmetric] | [AES-256-GCM, AES-256-CBC] | [DES, 3DES, RC4] |
| [Asymmetric] | [RSA-2048+, ECDSA P-256+] | [RSA-1024, DSA] |
| [Hashing] | [SHA-256, SHA-3, bcrypt] | [MD5, SHA-1] |
| [TLS] | [TLS 1.2, TLS 1.3] | [TLS 1.0, TLS 1.1, SSL] |

## 7. Certificate Management

| Aspect | Standard | Implementation |
|--------|---------|---------------|
| [Issuance] | [CA-signed or internal CA] | [Let's Encrypt / Internal PKI] |
| [Validity] | [Maximum 1 year] | [Auto-renewal] |
| [Monitoring] | [Alert 30 days before expiry] | [Certificate manager] |
| [Revocation] | [CRL + OCSP stapling] | [CA configuration] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Data-Classification-Schema]] | Classification driving encryption |
| [[Security-Policy]] | Security standards |
| [[Data-Access-Control-Policy]] | Access controls |

---

> **Template Standard:** Based on DMBOK v2, ISO/IEC 27001:2022
> **Usage:** Encryption is *not optional* for L1/L2 data. If you can't encrypt it, you can't store it.
