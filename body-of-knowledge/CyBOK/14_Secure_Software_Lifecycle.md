---
tags: [secure-sdlc, devsecops, cyber-security, cybok]
---

# 14 — Secure Software Lifecycle

> *Source: CyBOK v1.1 by NCSC, Chapter 15 — Secure Software Lifecycle*

## Purpose

The Secure Software Development Lifecycle (SSDLC) integrates security activities into every phase of software development — from requirements through design, implementation, testing, deployment, and maintenance. Moving security "left" in the lifecycle prevents vulnerabilities rather than patching them post-release.

## Security in Requirements
- Security requirements elicitation (derived from threat modelling and compliance)
- Abuse/misuse cases alongside use cases
- Regulatory and compliance requirements mapping (GDPR, PCI-DSS, HIPAA)
- Security acceptance criteria

## Security in Design
- Secure architecture patterns (defense in depth, least privilege, secure defaults)
- Threat modelling (STRIDE, attack trees)
- Security design reviews
- Interface security specification

## Security in Implementation
- Secure coding standards (CERT, OWASP, MISRA)
- Static Application Security Testing (SAST)
- Software Composition Analysis (SCA) — third-party dependency scanning
- Code review for security

## Security in Testing
- Dynamic Application Security Testing (DAST)
- Penetration testing
- Fuzz testing
- Security regression testing
- Vulnerability scanning

## Security in Deployment & Operations
- Secure configuration management
- Runtime application self-protection (RASP)
- Patch management
- Incident response integration
- Continuous security monitoring

## DevSecOps
Integrating security into DevOps pipelines — automation of security checks in CI/CD, infrastructure-as-code security scanning, container image scanning, and security as code.

## Related Chapters

- [[09_Software_Security]] — Vulnerability categories and detection
- [[12_Formal_Methods_for_Security]] — Formal verification in the SDLC
- [[07_Security_Operations_and_Incident_Management]] — Post-deployment security operations
