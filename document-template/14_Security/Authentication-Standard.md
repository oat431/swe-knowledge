---
document_type: Authentication Standard
version: "1.0"
status: Draft
author: "[Security Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Confidential"
tags: [authentication, mfa, session, cyberok]
standard_ref:
  - CyBOK v1 — Access Control
  - NIST SP 800-63B — Digital Identity Guidelines
---

# Authentication Standard

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines authentication mechanisms, MFA requirements, and session management standards.

## 2. Authentication Methods

| Method | Use Case | Strength | Implementation |
|--------|---------|---------|---------------|
| [Password + MFA] | [Primary authentication] | [High] | [Auth service] |
| [OAuth2 / OIDC] | [Third-party SSO] | [High] | [Auth service] |
| [API Key] | [Service-to-service] | [Medium] | [API Gateway] |
| [JWT Bearer Token] | [Session management] | [High] | [Auth service] |

## 3. Password Policy

| Requirement | Standard | Enforcement |
|------------|---------|------------|
| [Minimum length] | [12 characters] | [Auth service validation] |
| [Complexity] | [Upper + lower + number + special] | [Auth service validation] |
| [Maximum age] | [90 days] | [Forced change prompt] |
| [History] | [Last 12 passwords blocked] | [Auth service check] |
| [Lockout] | [5 failed attempts → 15-min lock] | [Auth service] |
| [Bcrypt rounds] | [12] | [Hashing config] |

## 4. Multi-Factor Authentication (MFA)

| Requirement | Standard | Implementation |
|------------|---------|---------------|
| [MFA required] | [All users, all sessions] | [TOTP (Google Authenticator, Authy)] |
| [MFA backup] | [Recovery codes (8)] | [Generated at MFA setup] |
| [MFA enforcement] | [Required at first login] | [Auth service] |
| [MFA bypass] | [None — no exceptions] | [Policy] |

## 5. Session Management

| Requirement | Standard | Implementation |
|------------|---------|---------------|
| [Session token] | [JWT with RS256 signing] | [Auth service] |
| [Access token TTL] | [30 minutes] | [JWT expiry claim] |
| [Refresh token TTL] | [7 days] | [Secure cookie] |
| [Session invalidation] | [On logout, password change, MFA change] | [Token blacklist] |
| [Concurrent sessions] | [Maximum 5] | [Session tracking] |
| [Idle timeout] | [30 minutes] | [Client-side + server-side] |

## 6. Token Security

| Requirement | Standard | Implementation |
|------------|---------|---------------|
| [Token storage] | [HttpOnly secure cookie] | [Express session] |
| [Token transmission] | [HTTPS only] | [TLS enforcement] |
| [Token rotation] | [Refresh token rotated on use] | [Auth service] |
| [Token revocation] | [Blacklist on logout/security event] | [Redis blacklist] |

## 7. API Authentication

| Method | Use Case | Implementation |
|--------|---------|---------------|
| [JWT Bearer] | [User-authenticated API calls] | [Authorization header] |
| [API Key] | [Service-to-service] | [X-API-Key header] |
| [OAuth2 Client Credentials] | [Third-party integrations] | [OAuth2 flow] |

## 8. Authentication Logging

| Event | Data Logged | Retention |
|-------|-----------|----------|
| [Login success] | [User ID, timestamp, IP, user agent] | [1 year] |
| [Login failure] | [Email attempted, timestamp, IP, reason] | [1 year] |
| [MFA challenge] | [User ID, timestamp, method] | [1 year] |
| [Password change] | [User ID, timestamp, IP] | [1 year] |
| [Account lockout] | [User ID, timestamp, IP, attempts] | [1 year] |
| [Session invalidation] | [User ID, timestamp, reason] | [1 year] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Access-Control-Policy]] | Authorization framework |
| [[Security-Policy]] | Overall security policy |
| [[ISMS-Documentation]] | ISMS framework |

---

> **Template Standard:** Based on CyBOK v1, NIST SP 800-63B
> **Usage:** Authentication is the *front door*. If it's weak, everything else is irrelevant. MFA is mandatory — no exceptions.
