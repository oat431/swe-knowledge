---
document_type: Version Description Document
version: "1.0"
status: Draft
author: "[Configuration Manager]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [version-description, release, swebok]
standard_ref:
  - SWEBOK v4 — Configuration Management
---

# Version Description Document

> **Project:** [Project Name]
> **Version:** [X.Y.Z] | **Status:** [Draft | Under Review | Approved]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Describes a specific software version — what's included, how to install, and what changed.

## 2. Version Information

| Field | Detail |
|-------|--------|
| [Version] | [X.Y.Z] |
| [Release Date] | [YYYY-MM-DD] |
| [Release Type] | [Major / Minor / Patch] |
| [Baseline] | [BL-XXX] |
| [Previous Version] | [X.Y.Z] |

## 3. Version Contents

| CI | Version | Location | Checksum |
|----|---------|---------|---------|
| [Source Code] | [v1.2.0] | [Git tag: v1.2.0] | [SHA: abc123] |
| [Docker Image] | [v1.2.0] | [ghcr.io/org/project:v1.2.0] | [digest: sha256:xyz] |
| [Database Schema] | [v1.2] | [migrations/v1.2/] | — |
| [Configuration] | [v1.2.0] | [config/v1.2.0/] | — |
| [Documentation] | [v1.2.0] | [docs/v1.2.0/] | — |

## 4. Changes from Previous Version

| Type | Description | MR | Impact |
|------|-----------|-----|--------|
| [Feature] | [Added document preview] | [MR-001] | [User-facing] |
| [Feature] | [Added push notifications] | [MR-005] | [User-facing] |
| [Fix] | [Fixed mobile upload] | [MR-002] | [Bug fix] |
| [Fix] | [Fixed token refresh] | [MR-003] | [Bug fix] |
| [Maintenance] | [Updated dependencies] | — | [Internal] |

## 5. Installation / Upgrade

| Step | Action | Command | Notes |
|------|--------|---------|-------|
| 1 | [Pull Docker image] | [docker pull ghcr.io/org/project:v1.2.0] | — |
| 2 | [Run migrations] | [npm run db:migrate] | [Reversible] |
| 3 | [Deploy application] | [kubectl apply -f deployment.yaml] | [Zero downtime] |
| 4 | [Verify deployment] | [npm run test:smoke] | [Must pass] |

## 6. Known Issues

| # | Issue | Workaround | Fix Planned |
|---|-------|-----------|------------|
| 1 | [Date filter on dashboard] | [Manual date entry] | [v1.2.1] |

## 7. Compatibility

| Component | Compatible Versions | Notes |
|---------|-------------------|-------|
| [Database] | [PostgreSQL 14+] | [Schema compatible] |
| [Browser] | [Chrome 90+, Firefox 90+, Safari 15+] | [ES2020 target] |
| [API] | [v1] | [Backward compatible] |

## 8. Rollback Instructions

| Step | Action | Command |
|------|--------|---------|
| 1 | [Revert Docker image] | [kubectl rollout undo deployment/app] |
| 2 | [Revert database] | [npm run db:rollback] |
| 3 | [Verify rollback] | [npm run test:smoke] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Release-Notes]] | User-facing release notes |
| [[Baseline-Records]] | Baseline this version belongs to |
| [[Deployment-Plan]] | Deployment procedures |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** The VDD is the *technical* version description. Release Notes are for users; VDD is for engineers.
