---
tags: [web-security, mobile-security, cyber-security, cybok]
---

# 15 — Web and Mobile Security

> *Source: CyBOK v1.1 by NCSC, Chapter 16 — Web & Mobile Security*

## Purpose

Web and mobile platforms represent the largest attack surface in modern computing. This KA covers the unique security challenges of browser-based applications, web services, and mobile ecosystems.

## Web Security

### Common Web Attacks
- **Injection** — SQL, command, LDAP, XPath injection
- **Cross-Site Scripting (XSS)** — Reflected, stored, DOM-based
- **Cross-Site Request Forgery (CSRF)** — Unauthorized commands from authenticated users
- **Server-Side Request Forgery (SSRF)** — Exploiting server-side fetching
- **Broken Authentication & Session Management** — Credential stuffing, session fixation
- **Security Misconfiguration** — Default credentials, verbose errors, unnecessary features

### Web Defenses
- Content Security Policy (CSP)
- Same-Origin Policy and CORS
- HTTP security headers (HSTS, X-Frame-Options)
- Input validation and output encoding
- Web Application Firewalls (WAF)

## Mobile Security

### Platform Security Models
- **Android** — Application sandboxing, permission system, SELinux, SafetyNet
- **iOS** — App sandbox, code signing, Data Protection API, Secure Enclave

### Mobile Threats
- Insecure data storage
- Weak server-side controls
- Insufficient transport layer protection
- Client-side injection
- Reverse engineering and tampering
- Malicious apps and supply chain attacks

## Related Chapters

- [[09_Software_Security]] — Vulnerability categories applicable to web/mobile
- [[14_Secure_Software_Lifecycle]] — Secure development for web/mobile apps
- [[16_Applied_Cryptography]] — TLS, certificate pinning, secure storage
