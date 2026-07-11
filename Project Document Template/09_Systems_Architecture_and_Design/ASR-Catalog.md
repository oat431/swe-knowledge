---
document_type: ASR Catalog
version: "1.0"
status: Draft
author: "[Author Name]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
architect: "[Solution Architect]"
classification: "Internal / Confidential"
tags: [asr, architecturally-significant, requirements, swebok, iso-42010]
standard_ref:
  - SWEBOK v4 — Architecture
  - ISO/IEC/IEEE 42010 — Architecture Description
---

# ASR Catalog (Architecturally Significant Requirements)

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> This catalog identifies and tracks Architecturally Significant Requirements (ASRs) — requirements that have a profound effect on the architecture. Not all requirements drive architecture; this catalog focuses on those that do.

## 2. What Makes a Requirement Architecturally Significant?

| Criterion | Description | Example |
|----------|-------------|---------|
| [Quality Attribute] | [NFR that constrains architecture] | [99.9% availability] |
| [Structural] | [Affects component decomposition] | [Independent deployment] |
| [Integration] | [External system connectivity] | [Must integrate with ERP] |
| [Constraint] | [Technology or organizational limitation] | [Must use existing cloud] |
| [High Risk] | [Requirement with significant risk] | [Data migration 500K records] |
| [High Business Value] | [Critical business function] | [Auto-approval engine] |

## 3. ASR Register

| ASR ID | Requirement | Category | Priority | Architecture Impact | Design Decision |
|--------|-------------|----------|----------|-------------------|----------------|
| ASR-001 | [System shall support 100 concurrent users] | Quality — Performance | 🔴 | [Auto-scaling, load balancing, caching] | ADR-001 |
| ASR-002 | [System shall be 99.9% available] | Quality — Availability | 🔴 | [Multi-AZ, health checks, failover] | ADR-007 |
| ASR-003 | [System shall process requests in <1 hour] | Quality — Performance | 🔴 | [Async processing, workflow engine] | ADR-005 |
| ASR-004 | [System shall integrate with ERP via REST API] | Integration | 🔴 | [Integration service, API gateway] | ADR-001 |
| ASR-005 | [System shall maintain complete audit trail] | Constraint — Compliance | 🔴 | [Append-only audit log, all actions logged] | ADR-002 |
| ASR-006 | [System shall encrypt data at rest and in transit] | Quality — Security | 🔴 | [KMS, TLS 1.3, encrypted storage] | ADR-006 |
| ASR-007 | [System shall support zero-downtime deployments] | Quality — Availability | 🔴 | [Blue-green deployment, container orchestration] | ADR-007 |
| ASR-008 | [System shall auto-approve eligible requests] | Functional — Critical | 🔴 | [Rules engine, workflow service] | ADR-001 |
| ASR-009 | [System shall use existing cloud provider] | Constraint — Technical | 🔴 | [Cloud-native services, IaC] | — |
| ASR-010 | [Data must remain in-country] | Constraint — Legal | 🔴 | [Local cloud region, data residency controls] | — |
| ASR-011 | [System shall support 10x volume growth] | Quality — Scalability | 🟡 | [Horizontal scaling, stateless services] | ADR-001 |
| ASR-012 | [System shall support daily deployments] | Quality — Maintainability | 🟡 | [CI/CD pipeline, microservices] | ADR-007 |

## 4. ASR to Architecture Mapping

| ASR | Architecture Component | Design Pattern | Trade-off |
|-----|----------------------|---------------|----------|
| ASR-001 | [Load Balancer, Auto-scaling] | [Horizontal scaling] | [Cost vs performance] |
| ASR-002 | [Multi-AZ, Health checks] | [Redundancy] | [Cost vs availability] |
| ASR-003 | [Workflow Engine, Async] | [Event-driven] | [Complexity vs performance] |
| ASR-004 | [Integration Service, API GW] | [Anti-corruption layer] | [Coupling vs integration] |
| ASR-005 | [Audit Service, Append-only log] | [Event sourcing] | [Storage cost vs compliance] |
| ASR-006 | [KMS, TLS, Encryption] | [Defense in depth] | [Performance vs security] |
| ASR-007 | [Containers, K8s, Blue-green] | [Immutable infrastructure] | [Complexity vs availability] |
| ASR-008 | [Rules Engine, Workflow] | [Strategy pattern] | [Flexibility vs complexity] |

## 5. ASR Quality Attribute Scenarios

| ASR | Quality Attribute | Scenario | Response Measure |
|-----|------------------|---------|-----------------|
| ASR-001 | Performance | [100 concurrent users submit requests] | [Response <2s at 95th percentile] |
| ASR-002 | Availability | [Single AZ failure] | [Service continues, <1 min failover] |
| ASR-003 | Performance | [Request submitted with all valid data] | [Decision within 1 hour] |
| ASR-007 | Availability | [New version deployed] | [Zero downtime, zero dropped requests] |
| ASR-011 | Scalability | [Volume increases 10x] | [Auto-scale, same SLA] |

## 6. ASR Traceability

| ASR | Business Req | System Req | Design Element | Test Case |
|-----|-------------|-----------|---------------|-----------|
| ASR-001 | NFR-003 | SYRS-004 | Load Balancer | PERF-TC-001 |
| ASR-002 | NFR-010 | SYRS-006 | Multi-AZ | REL-TC-001 |
| ASR-003 | BR-04 | SYRS-003 | Workflow Engine | FR-TC-103 |
| ASR-004 | BR-02 | SYRS-009 | Integration Svc | INT-TC-001 |
| ASR-005 | BR-07 | SYRS-006 | Audit Service | SEC-TC-005 |
| ASR-006 | NFR-012,013 | SYRS-012,013 | KMS, TLS | SEC-TC-006 |
| ASR-007 | NFR-013 | SYRS-007 | K8s, Blue-green | REL-TC-003 |
| ASR-008 | BR-04 | SYRS-003 | Rules Engine | FR-TC-103 |

## 7. ASR Risk Assessment

| ASR | Risk | Probability | Impact | Mitigation |
|-----|------|-----------|--------|-----------|
| ASR-002 | [99.9% availability hard to achieve] | Medium | High | [Multi-AZ, extensive testing] |
| ASR-003 | [<1 hour processing dependent on external systems] | Medium | High | [Timeout handling, fallback] |
| ASR-004 | [ERP API unstable] | Medium | High | [POC, fallback to batch] |
| ASR-011 | [10x scaling may require architecture changes] | Low | High | [Design for horizontal scaling from start] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[SRS]] | All requirements including ASRs |
| [[Software Architecture Document]] | Architecture driven by ASRs |
| [[Architecture Decision Records]] | Decisions driven by ASRs |
| [[Trade Study Reports]] | Analysis of ASR-driven alternatives |
| [[Nonfunctional Requirements Catalog]] | NFRs that become ASRs |

---

> **Template Standard:** Based on SWEBOK v4, ISO/IEC/IEEE 42010
> **Usage:** Not all requirements are architecturally significant. ASRs are the ones that *shape* the architecture — they determine component structure, technology choices, and quality attributes. Review this catalog at every architecture review.
