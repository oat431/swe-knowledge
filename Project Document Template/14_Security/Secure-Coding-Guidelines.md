---
document_type: Secure Coding Guidelines
version: "1.0"
status: Draft
author: "[Security Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [secure-coding, owasp, cert, cyberok, swebok]
standard_ref:
  - CyBOK v1 — Secure Software Engineering
  - SWEBOK v4 — Security
  - OWASP Top 10
  - CERT Secure Coding Standards
---

# Secure Coding Guidelines

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Defines security-specific coding rules beyond general standards — preventing common vulnerabilities at the code level.

## 2. OWASP Top 10 Mitigations

| # | OWASP Risk | Guideline | Implementation |
|---|-----------|----------|---------------|
| A01 | [Broken Access Control] | [Always verify permissions server-side] | [RBAC middleware on every endpoint] |
| A02 | [Cryptographic Failures] | [Use established libraries, never roll your own] | [bcrypt, AES-256, TLS 1.3] |
| A03 | [Injection] | [Use parameterized queries, never concatenate SQL] | [ORM + prepared statements] |
| A04 | [Insecure Design] | [Threat model before coding] | [[Threat-Model]] |
| A05 | [Security Misconfiguration] | [No default credentials, disable debug in prod] | [Config management] |
| A06 | [Vulnerable Components] | [Scan dependencies, update regularly] | [npm audit, Snyk] |
| A07 | [Auth Failures] | [MFA, rate limiting, secure session management] | [Auth service] |
| A08 | [Data Integrity Failures] | [Validate all inputs, verify integrity] | [Schema validation] |
| A09 | [Logging Failures] | [Log security events, no sensitive data in logs] | [Structured logging] |
| A10 | [SSRF] | [Validate and whitelist URLs] | [URL validation] |

## 3. Coding Rules

### 3.1 Input Validation

| Rule | Implementation | Example |
|------|---------------|---------|
| [Validate all inputs server-side] | [Zod schema validation] | `z.object({ email: z.string().email() })` |
| [Whitelist allowed characters] | [Regex patterns] | `z.string().regex(/^[a-zA-Z0-9]+$/)` |
| [Reject oversized inputs] | [Length limits] | `z.string().max(1000)` |
| [Validate file uploads] | [Type + size checks] | `allowedTypes: ['pdf', 'jpg']` |

### 3.2 Output Encoding

| Rule | Implementation | Example |
|------|---------------|---------|
| [Encode output for context] | [React auto-escape] | JSX auto-escapes by default |
| [Set Content-Type headers] | [Express middleware] | `res.setHeader('Content-Type', 'application/json')` |
| [Use CSP headers] | [Helmet middleware] | `helmet.contentSecurityPolicy()` |

### 3.3 Authentication & Session

| Rule | Implementation | Example |
|------|---------------|---------|
| [Hash passwords with bcrypt] | [bcrypt library] | `bcrypt.hash(password, 12)` |
| [Use secure session cookies] | [Express session] | `{ httpOnly: true, secure: true, sameSite: 'strict' }` |
| [Implement MFA] | [TOTP library] | Speakeasy / otplib |
| [Rate limit auth endpoints] | [Express middleware] | `rateLimit({ windowMs: 15*60*1000, max: 5 })` |

### 3.4 Database

| Rule | Implementation | Example |
|------|---------------|---------|
| [Use parameterized queries] | [ORM / prepared statements] | `db.query('SELECT * FROM users WHERE id = $1', [id])` |
| [Never concatenate SQL] | [Code review check] | ❌ `db.query('SELECT * FROM users WHERE id = ' + id)` |
| [Encrypt sensitive columns] | [Application-level encryption] | [KMS + field encryption] |

### 3.5 Secrets Management

| Rule | Implementation | Example |
|------|---------------|---------|
| [Never hardcode secrets] | [Pre-commit hook] | [detect-secrets / git-secrets] |
| [Use secret manager] | [Vault / AWS Secrets Manager] | [Environment injection] |
| [Rotate secrets regularly] | [Automated rotation] | [Quarterly rotation] |

### 3.6 Error Handling

| Rule | Implementation | Example |
|------|---------------|---------|
| [Never expose stack traces] | [Global error handler] | [Generic error messages to client] |
| [Log errors server-side] | [Structured logging] | [Winston/Pino with context] |
| [Use generic error messages] | [Error mapping] | ["Something went wrong" not "SQL error at line 42"] |

## 4. Code Review Security Checklist

| # | Check | Status |
|---|-------|--------|
| 1 | [All inputs validated] | ☐ |
| 2 | [Parameterized queries used] | ☐ |
| 3 | [No hardcoded secrets] | ☐ |
| 4 | [Authentication required] | ☐ |
| 5 | [Authorization checked] | ☐ |
| 6 | [Sensitive data not logged] | ☐ |
| 7 | [Error messages generic] | ☐ |
| 8 | [HTTPS enforced] | ☐ |
| 9 | [CORS configured correctly] | ☐ |
| 10 | [Rate limiting applied] | ☐ |

## 5. Language-Specific Guidelines

### TypeScript / Node.js

| Guideline | Implementation |
|----------|---------------|
| [Use TypeScript strict mode] | `tsconfig.json: "strict": true` |
| [Validate with Zod] | `z.object({...}).parse(input)` |
| [Use Helmet for security headers] | `app.use(helmet())` |
| [Use express-rate-limit] | `app.use(rateLimit({...}))` |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Coding-Standards]] | General coding standards |
| [[Threat-Model]] | Threats being mitigated |
| [[SAST-Report]] | Static analysis findings |

---

> **Template Standard:** Based on CyBOK v1, OWASP, CERT
> **Usage:** These guidelines are *mandatory* for all code. Code review enforces them. SAST automates checking.
