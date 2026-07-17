---
document_type: Nonfunctional Requirements Catalog
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
ba_owner: "[Business Analyst Name]"
tech_lead: "[Technical Lead Name]"
classification: "Internal / Confidential"
tags: [nfr, non-functional, quality-attributes, swebok, iso-25010]
standard_ref:
  - SWEBOK v4 — Requirements
  - ISO/IEC 25010 — SQuaRE (Quality Model)
  - ISO/IEC/IEEE 29148 — Requirements Engineering
---

# Nonfunctional Requirements Catalog

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | Archived]
> **Last Updated:** [YYYY-MM-DD]

---

## Document Control

| Field | Value |
|-------|-------|
| Document Owner | [Name / Role] |
| Technical Lead | [Name / Role] |
| Business Analyst | [Name / Role] |

### Revision History

| Version | Date | Author | Change Description |
|---------|------|--------|--------------------|
| 0.1 | [YYYY-MM-DD] | [Name] | Initial draft |
| 1.0 | [YYYY-MM-DD] | [Name] | Approved version |

### Approvals

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Technical Lead | | | |
| Solution Architect | | | |
| BA Lead | | | |

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Quality Model](#2-quality-model)
3. [Performance Requirements](#3-performance-requirements)
4. [Availability & Reliability](#4-availability--reliability)
5. [Security Requirements](#5-security-requirements)
6. [Usability Requirements](#6-usability-requirements)
7. [Scalability & Maintainability](#7-scalability--maintainability)
8. [Compatibility & Portability](#8-compatibility--portability)
9. [Compliance Requirements](#9-compliance-requirements)
10. [NFR Traceability](#10-nfr-traceability)

---

## 1. Executive Summary

| Field | Detail |
|-------|--------|
| Total NFRs | [X] |
| Quality Categories | [8 per ISO 25010] |
| Critical NFRs | [X — must be verified before go-live] |
| Source Standards | [ISO 25010 (SQuaRE), OWASP, NIST, WCAG] |

---

## 2. Quality Model

### 2.1 ISO 25010 Quality Characteristics

```mermaid
mindmap
  root((Software<br>Quality))
    Performance
      Time Behavior
      Resource Utilization
      Capacity
    Reliability
      Maturity
      Availability
      Fault Tolerance
      Recoverability
    Security
      Confidentiality
      Integrity
      Non-Repudiation
      Accountability
      Authenticity
    Usability
      Learnability
      Operability
      User Error Protection
      Accessibility
    Maintainability
      Modularity
      Reusability
      Analysability
      Modifiability
      Testability
    Compatibility
      Co-existence
      Interoperability
```

---

## 3. Performance Requirements

| ID | Requirement | Metric | Target | Threshold | Measurement | Priority |
|----|-------------|--------|--------|-----------|-------------|----------|
| PERF-001 | [Page load time] | [Response time] | [<1.5s] | [<3s] | [95th percentile] | 🔴 |
| PERF-002 | [API response time] | [Response time] | [<500ms] | [<1s] | [95th percentile] | 🔴 |
| PERF-003 | [Database query time] | [Response time] | [<200ms] | [<500ms] | [95th percentile] | 🟡 |
| PERF-004 | [Request processing time] | [End-to-end] | [<1 hour] | [<4 hours] | [Average] | 🔴 |
| PERF-005 | [Report generation time] | [Duration] | [<10s] | [<30s] | [Standard report] | 🟡 |
| PERF-006 | [Concurrent users] | [Count] | [100] | [50] | [Without degradation] | 🔴 |
| PERF-007 | [Transaction throughput] | [Requests/hour] | [200] | [100] | [Sustained] | 🟡 |

### Performance Testing Strategy

| Test Type | Tool | Frequency | Pass Criteria |
|-----------|------|-----------|--------------|
| [Load test] | [k6 / JMeter] | [Pre-release] | [All PERF targets met] |
| [Stress test] | [k6 / JMeter] | [Quarterly] | [Graceful degradation] |
| [Endurance test] | [k6 / JMeter] | [Pre-go-live] | [No memory leaks, stable performance] |

---

## 4. Availability & Reliability

| ID | Requirement | Metric | Target | Threshold | Measurement | Priority |
|----|-------------|--------|--------|-----------|-------------|----------|
| REL-001 | [System availability] | [Uptime %] | [99.9%] | [99.5%] | [Monthly] | 🔴 |
| REL-002 | [Recovery Time Objective] | [Duration] | [1 hour] | [4 hours] | [DR test] | 🔴 |
| REL-003 | [Recovery Point Objective] | [Data loss] | [15 minutes] | [1 hour] | [DR test] | 🔴 |
| REL-004 | [Mean Time Between Failures] | [Duration] | [720 hours] | [168 hours] | [Production metrics] | 🟡 |
| REL-005 | [Mean Time To Repair] | [Duration] | [30 minutes] | [2 hours] | [Incident logs] | 🟡 |
| REL-006 | [Planned maintenance window] | [Duration] | [2 hours/month] | [4 hours/month] | [Maintenance log] | 🟡 |
| REL-007 | [Zero-downtime deployment] | [Boolean] | [Yes] | [N/A] | [Deployment process] | 🟡 |

### High Availability Architecture

| Component | Strategy | Failover Time |
|-----------|---------|--------------|
| [Application] | [Multi-instance, load balanced] | [<30 seconds] |
| [Database] | [Primary-replica, auto-failover] | [<60 seconds] |
| [Cache] | [Cluster mode, auto-failover] | [<10 seconds] |
| [File Storage] | [Cross-region replication] | [Automatic] |

---

## 5. Security Requirements

| ID | Requirement | Category | Standard | Priority |
|----|-------------|----------|---------|----------|
| SEC-001 | [MFA for admin users] | Authentication | [OWASP] | 🔴 |
| SEC-002 | [Password policy — 12+ chars, complexity] | Authentication | [NIST SP 800-63B] | 🔴 |
| SEC-003 | [Session timeout — 30 min inactivity] | Session Mgmt | [OWASP] | 🔴 |
| SEC-004 | [RBAC — role-based access control] | Authorization | [ISO 27001] | 🔴 |
| SEC-005 | [Least privilege principle] | Authorization | [ISO 27001] | 🔴 |
| SEC-006 | [Data encryption at rest — AES-256] | Cryptography | [NIST SP 800-57] | 🔴 |
| SEC-007 | [Data encryption in transit — TLS 1.3] | Cryptography | [NIST SP 800-52] | 🔴 |
| SEC-008 | [Audit trail — user, action, timestamp, IP] | Audit | [ISO 27001] | 🔴 |
| SEC-009 | [Input validation — SQL injection, XSS] | Input Validation | [OWASP Top 10] | 🔴 |
| SEC-010 | [API rate limiting — 100 req/min/user] | API Security | [OWASP API] | 🟡 |
| SEC-011 | [CSRF protection] | Web Security | [OWASP] | 🔴 |
| SEC-012 | [Security headers — CSP, HSTS, X-Frame] | Web Security | [OWASP] | 🟡 |
| SEC-013 | [Vulnerability scanning — monthly] | Testing | [OWASP] | 🟡 |
| SEC-014 | [Penetration testing — annual] | Testing | [OWASP] | 🟡 |

---

## 6. Usability Requirements

| ID | Requirement | Metric | Target | Measurement | Priority |
|----|-------------|--------|--------|-------------|----------|
| USA-001 | [Customer task completion time] | [Duration] | [<5 min] | [First-time user test] | 🔴 |
| USA-002 | [Operations task completion time] | [Duration] | [<3 min] | [Trained user test] | 🔴 |
| USA-003 | [User error rate] | [%] | [<5%] | [Usability test] | 🟡 |
| USA-004 | [User satisfaction score] | [SUS score] | [≥80] | [Post-task survey] | 🟡 |
| USA-005 | [Training time — customer] | [Duration] | [<15 min] | [Self-service guide] | 🟡 |
| USA-006 | [Training time — operations] | [Duration] | [<2 days] | [Classroom + hands-on] | 🟡 |
| USA-007 | [Accessibility — WCAG 2.1 AA] | [Conformance] | [AA] | [Automated + manual] | 🔴 |
| USA-008 | [Responsive design] | [Devices] | [Desktop, tablet, mobile] | [Cross-device test] | 🔴 |
| USA-009 | [Browser support] | [Browsers] | [Chrome, Firefox, Safari, Edge — latest 2] | [Cross-browser test] | 🔴 |
| USA-010 | [Multi-language support] | [Languages] | [English, Thai] | [Localization test] | 🟡 |

---

## 7. Scalability & Maintainability

| ID | Requirement | Metric | Target | Measurement | Priority |
|----|-------------|--------|--------|-------------|----------|
| SMA-001 | [Horizontal scaling] | [Capacity] | [10x current volume] | [Load test] | 🟡 |
| SMA-002 | [Vertical scaling] | [Resources] | [Auto-scale CPU/memory] | [Config test] | 🟡 |
| SMA-003 | [Code complexity] | [Cyclomatic] | [<10 per function] | [Static analysis] | 🟡 |
| SMA-004 | [Test coverage] | [Code coverage] | [≥80%] | [Coverage report] | 🟡 |
| SMA-005 | [Deployment frequency] | [Frequency] | [Daily, zero-downtime] | [CI/CD metrics] | 🟡 |
| SMA-006 | [API versioning] | [Compatibility] | [Backward compatible 2 versions] | [API contract test] | 🟡 |
| SMA-007 | [Log structured format] | [Format] | [JSON, with correlation ID] | [Log review] | 🟡 |
| SMA-008 | [Documentation currency] | [Currency] | [Updated within 1 sprint] | [Doc review] | 🟢 |

---

## 8. Compatibility & Portability

| ID | Requirement | Category | Target | Priority |
|----|-------------|----------|--------|----------|
| COM-001 | [ERP integration] | Interoperability | [REST API, bidirectional] | 🔴 |
| COM-002 | [Payment gateway integration] | Interoperability | [REST API, outbound] | 🔴 |
| COM-003 | [Email/SMS service integration] | Interoperability | [REST API, outbound] | 🟡 |
| COM-004 | [Data export — CSV, Excel, PDF] | Portability | [Standard formats] | 🟡 |
| COM-005 | [Data import — CSV, Excel] | Portability | [Standard formats] | 🟡 |
| COM-006 | [Cloud provider portability] | Portability | [Containerized, no vendor-specific services] | 🟢 |

---

## 9. Compliance Requirements

| ID | Requirement | Regulation | Evidence | Priority |
|----|-------------|-----------|---------|----------|
| CMP-001 | [GDPR compliance] | [GDPR] | [Privacy impact assessment, consent management] | 🔴 |
| CMP-002 | [Data retention — 7 years] | [Industry regulation] | [Automated retention policy] | 🔴 |
| CMP-003 | [Audit trail — all actions] | [ISO 27001] | [Immutable audit log] | 🔴 |
| CMP-004 | [Data sovereignty — in-country] | [Local law] | [Hosting location proof] | 🔴 |
| CMP-005 | [Accessibility — WCAG 2.1 AA] | [ISO/IEC 40500] | [Accessibility audit report] | 🔴 |
| CMP-006 | [PCI-DSS — if payment processing] | [PCI-DSS] | [PCI compliance report] | 🔴 |

---

## 10. NFR Traceability

| NFR ID | Quality Characteristic | SRS Requirement | Design Element | Test Case |
|--------|----------------------|----------------|---------------|-----------|
| PERF-001 | Performance | NFR-001 | [CDN, caching] | PERF-TC-001 |
| REL-001 | Reliability | NFR-010 | [HA architecture] | REL-TC-001 |
| SEC-001 | Security | SEC-001 | [Auth module] | SEC-TC-001 |
| USA-001 | Usability | NFR-020 | [Portal UX] | USA-TC-001 |
| SMA-001 | Maintainability | NFR-030 | [Containerized arch] | SMA-TC-001 |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Software-Requirements-Specification]] | NFRs are part of the SRS |
| [[System-Requirements-Specification]] | System-level NFRs |
| [[Business-Requirements]] | Business NFRs elaborated here |
| [[Architecture-Decision-Records]] | ADRs capture NFR-driven decisions |
| [[Usability-Test-Plan]] | NFRs drive performance, security, usability testing |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC 25010 (SQuaRE), ISO/IEC/IEEE 29148
> **Usage:** This catalog provides *measurable* quality requirements. Each NFR must have a metric, target, threshold, and measurement method. "The system shall be fast" is not an NFR — "Page load time shall be <2 seconds at 95th percentile" is.
