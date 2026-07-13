---
document_type: As-Built Documentation
version: "1.0"
status: Draft
author: "[Systems Engineer]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal"
tags: [as-built, documentation, sebok]
standard_ref:
  - SEBoK v2 — Systems Engineering Management
---

# As-Built Documentation

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Documents the system as actually built — capturing deviations from design and final configuration.

## 2. As-Built vs Design

| Component | Design Spec | As-Built | Deviation | Rationale |
|----------|-----------|---------|-----------|----------|
| [Database] | [PostgreSQL 15] | [PostgreSQL 16] | [Version upgrade] | [Performance + security] |
| [Cache] | [Redis 6] | [Redis 7] | [Version upgrade] | [Performance] |
| [API] | [REST v1] | [REST v1] | [None] | — |
| [UI] | [React 18] | [React 18] | [None] | — |
| [Infrastructure] | [2 servers] | [3 servers] | [+1 server] | [Load requirements] |

## 3. Final Configuration

| Component | Configuration | Version | Environment |
|----------|-------------|---------|------------|
| [Application] | [Node.js + Express] | [v20 LTS] | [Production] |
| [Database] | [PostgreSQL] | [16] | [Production] |
| [Cache] | [Redis] | [7] | [Production] |
| [Search] | [Elasticsearch] | [8] | [Production] |
| [Message Queue] | [RabbitMQ] | [3.12] | [Production] |
| [Load Balancer] | [Nginx] | [1.24] | [Production] |
| [Container Runtime] | [Docker] | [24] | [Production] |

## 4. Deviation Log

| # | Component | Design | As-Built | Reason | Approved By |
|---|----------|--------|---------|--------|------------|
| 1 | [Database version] | [PostgreSQL 15] | [PostgreSQL 16] | [Security patches] | [Tech Lead] |
| 2 | [Infrastructure] | [2 servers] | [3 servers] | [Load testing showed need] | [Tech Lead] |
| 3 | [Cache version] | [Redis 6] | [Redis 7] | [Performance improvement] | [Tech Lead] |

## 5. Final Architecture Diagram

> [Link to final architecture diagram reflecting as-built state]

## 6. Configuration Files

| File | Location | Purpose |
|------|---------|---------|
| [docker-compose.yml] | [Git repository] | [Container configuration] |
| [nginx.conf] | [Git repository] | [Load balancer configuration] |
| [postgresql.conf] | [Git repository] | [Database configuration] |
| [.env.production] | [Secret manager] | [Environment variables] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[SEMP]] | SE management context |
| [[Physical-Architecture]] | Design architecture |
| [[Configuration-Management-Plan]] | Configuration management |

---

> **Template Standard:** Based on SEBoK v2
> **Usage:** As-built is *reality*. If it differs from design, document why. Future maintainers need to know what's actually deployed.
