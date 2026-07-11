---
document_type: QAW Report (Quality Attribute Workshop)
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
architect: "[Solution Architect]"
classification: "Internal / Confidential"
tags: [qaw, quality-attribute, workshop, swebok]
standard_ref:
  - SWEBOK v4 — Architecture
  - ATAM — Architecture Tradeoff Analysis Method (SEI/CMU)
---

# QAW Report (Quality Attribute Workshop)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This report documents the results of a Quality Attribute Workshop (QAW) — a structured session where stakeholders articulate quality attribute scenarios that drive architectural decisions.

## 2. Workshop Summary

| Field | Detail |
|-------|--------|
| [Date] | [YYYY-MM-DD] |
| [Duration] | [X hours] |
| [Facilitator] | [Name] |
| [Participants] | [Names — architect, BA, TL, QA, sponsor, end users] |
| [Outputs] | [X quality attribute scenarios] |

## 3. Quality Attribute Scenarios

### 3.1 Performance Scenarios

| ID | Scenario | Stimulus | Response | Response Measure | Priority |
|----|---------|---------|---------|-----------------|----------|
| PERF-001 | [Normal load] | [100 concurrent users] | [System processes requests] | [<2s response at 95th percentile] | 🔴 |
| PERF-002 | [Peak load] | [120 requests in one day] | [System processes all requests] | [<5s response at 95th percentile] | 🟡 |
| PERF-003 | [Large file upload] | [User uploads 10MB document] | [File stored successfully] | [<30s upload time] | 🟡 |

### 3.2 Availability Scenarios

| ID | Scenario | Stimulus | Response | Response Measure | Priority |
|----|---------|---------|---------|-----------------|----------|
| AVAIL-001 | [Server failure] | [One server crashes] | [Traffic rerouted to healthy server] | [<30s failover, no data loss] | 🔴 |
| AVAIL-002 | [Database failure] | [Primary DB fails] | [Failover to replica] | [<60s failover, <1 min data loss] | 🔴 |
| AVAIL-003 | [Planned maintenance] | [Deployment of new version] | [Zero-downtime deployment] | [No dropped requests] | 🔴 |

### 3.3 Security Scenarios

| ID | Scenario | Stimulus | Response | Response Measure | Priority |
|----|---------|---------|---------|-----------------|----------|
| SEC-001 | [Unauthorized access attempt] | [User tries to access another's data] | [Access denied, logged] | [403 Forbidden, audit log entry] | 🔴 |
| SEC-002 | [SQL injection attempt] | [Malicious input in form field] | [Input sanitized, request blocked] | [No data breach, alert generated] | 🔴 |
| SEC-003 | [Brute force login] | [100 failed login attempts from same IP] | [Account locked, IP blocked] | [Lock after 5 attempts, block after 100] | 🔴 |

### 3.4 Scalability Scenarios

| ID | Scenario | Stimulus | Response | Response Measure | Priority |
|----|---------|---------|---------|-----------------|----------|
| SCALE-001 | [Volume growth] | [Transaction volume increases 10x] | [System auto-scales] | [Same SLA maintained] | 🟡 |
| SCALE-002 | [User growth] | [User base grows from 1K to 10K] | [System handles load] | [No performance degradation] | 🟡 |

### 3.5 Modifiability Scenarios

| ID | Scenario | Stimulus | Response | Response Measure | Priority |
|----|---------|---------|---------|-----------------|----------|
| MOD-001 | [New business rule] | [New validation rule required] | [Rule implemented and deployed] | [Within 1 sprint] | 🟡 |
| MOD-002 | [New integration] | [New external system to integrate] | [Connector built and deployed] | [Within 2 sprints] | 🟡 |
| MOD-003 | [UI change] | [New field added to form] | [Form updated and deployed] | [Within 1 sprint] | 🟡 |

## 4. Scenario Priority Matrix

| Priority | Count | Scenarios |
|----------|-------|----------|
| 🔴 Critical | [7] | PERF-001, AVAIL-001, AVAIL-002, AVAIL-003, SEC-001, SEC-002, SEC-003 |
| 🟡 Important | [6] | PERF-002, PERF-003, SCALE-001, SCALE-002, MOD-001, MOD-002, MOD-003 |
| 🟢 Desirable | [0] | — |

## 5. Architecture Implications

| Scenario | Architecture Implication | Design Decision |
|----------|------------------------|----------------|
| [PERF-001] | [Auto-scaling, caching, CDN] | ADR-001 |
| [AVAIL-001,002] | [Multi-AZ, health checks, failover] | ADR-007 |
| [AVAIL-003] | [Blue-green deployment] | ADR-007 |
| [SEC-001,002,003] | [OAuth2, input validation, rate limiting] | ADR-006 |
| [SCALE-001] | [Horizontal scaling, stateless services] | ADR-001 |
| [MOD-001] | [Rules engine, microservices] | ADR-001 |

## 6. Workshop Outcomes

| # | Outcome | Status |
|---|---------|--------|
| 1 | [13 quality attribute scenarios captured] | ✅ |
| 2 | [Scenarios prioritized by stakeholders] | ✅ |
| 3 | [Architecture implications identified] | ✅ |
| 4 | [ADRs to be created/updated] | ⏳ Pending |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[ASR Catalog]] | ASRs derived from QAW scenarios |
| [[Architecture Evaluation Report (ATAM)]] | ATAM uses QAW scenarios |
| [[Nonfunctional Requirements Catalog]] | NFRs formalized from scenarios |
| [[ADR (Architecture Decision Records)]] | Decisions driven by scenarios |

---

> **Template Standard:** Based on SWEBOK v4, ATAM (SEI/CMU)
> **Usage:** Conduct QAW *early* in the project — before architecture is designed. Stakeholders articulate what they need; architects design to meet those needs. Revisit when requirements change significantly.
