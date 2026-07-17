---
document_type: Release Notes
version: "1.0"
status: Active
author: "[Product Manager]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Public"
tags: [release-notes, changelog, swebok]
standard_ref:
  - SWEBOK v4 — Operations
---

# Release Notes

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Communicates what changed in each release — new features, improvements, bug fixes, and known issues.

## 2. Release Template

---

### v1.2.0 — [YYYY-MM-DD]

**Release Name:** [Optional name]
**Release Type:** [Major / Minor / Patch]

#### 🚀 New Features

| # | Feature | Description | Related |
|---|---------|-------------|---------|
| 1 | [Mobile-responsive forms] | [Forms now adapt to mobile screens] | [FR-001] |
| 2 | [Real-time status tracking] | [Live status updates without refresh] | [FR-002] |
| 3 | [Push notifications] | [Browser push for status changes] | [FR-204] |

#### ✨ Improvements

| # | Improvement | Before | After |
|---|-----------|--------|-------|
| 1 | [Faster page loads] | [3.2s average] | [1.8s average] |
| 2 | [Better error messages] | [Generic errors] | [Specific, helpful messages] |
| 3 | [Keyboard shortcuts] | [None] | [A=Approve, R=Reject] |

#### 🐛 Bug Fixes

| # | Fix | Issue | Impact |
|---|-----|-------|--------|
| 1 | [Fixed upload on mobile] | [DEF-001] | [Uploads now work on iOS/Android] |
| 2 | [Fixed token refresh] | [DEF-002] | [No more forced re-login] |
| 3 | [Fixed report filters] | [DEF-003] | [Filters now apply correctly] |

#### ⚠️ Known Issues

| # | Issue | Workaround | Fix Planned |
|---|-------|-----------|------------|
| 1 | [Date filter on dashboard] | [Manual date entry] | [v1.2.1] |

#### 📦 Dependencies

| Package | Old Version | New Version | Reason |
|---------|-----------|------------|--------|
| [axios] | [1.6.0] | [1.6.2] | [Security fix] |
| [pg] | [8.11.0] | [8.11.3] | [Bug fix] |

#### 🔄 Migration Notes

| # | Action Required | Who | When |
|---|----------------|-----|------|
| 1 | [Run database migration] | [DevOps] | [Before deployment] |
| 2 | [Clear browser cache] | [Users] | [After deployment] |

#### 📊 Metrics

| Metric | Value |
|--------|-------|
| [Total changes] | [15] |
| [New features] | [3] |
| [Improvements] | [3] |
| [Bug fixes] | [3] |
| [Known issues] | [1] |

---

## 3. Release History

| Version | Date | Type | Features | Fixes |
|---------|------|------|---------|-------|
| [v1.2.0] | [YYYY-MM-DD] | [Minor] | [3] | [3] |
| [v1.1.0] | [YYYY-MM-DD] | [Minor] | [2] | [5] |
| [v1.0.0] | [YYYY-MM-DD] | [Major] | [10] | [0] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Deployment-Plan]] | Deployment procedures |
| [[Commit-Messages-Changelog]] | Commit-level changes |
| [[Test-Report]] | Quality evidence |

---

> **Template Standard:** Based on SWEBOK v4
> **Usage:** Release notes are for *everyone* — developers, PMs, users. Write them for humans, not machines.
