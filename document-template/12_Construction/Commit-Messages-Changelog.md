---
document_type: Commit Messages / Changelog
version: "1.0"
status: Active
author: "[Technical Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [commit-messages, changelog, conventional-commits, swebok]
standard_ref:
  - SWEBOK v4 — Construction
  - Conventional Commits v1.0
---

# Commit Messages / Changelog

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Standardized commit messages and auto-generated changelogs — keeping a clean, readable project history.

## 2. Commit Message Format

Based on [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

### Types

| Type | Description | Example |
|------|-----------|---------|
| [feat] | [New feature] | [feat(request): add document upload] |
| [fix] | [Bug fix] | [fix(auth): resolve token refresh issue] |
| [docs] | [Documentation] | [docs(api): update endpoint descriptions] |
| [style] | [Formatting, no code change] | [style: fix indentation] |
| [refactor] | [Code restructuring] | [refactor(service): extract validation logic] |
| [test] | [Add/update tests] | [test(request): add unit tests for approval] |
| [chore] | [Build, tooling] | [chore(deps): update dependencies] |
| [perf] | [Performance improvement] | [perf(db): add index for queries] |
| [ci] | [CI/CD changes] | [ci: add deployment pipeline] |
| [build] | [Build system changes] | [build: configure webpack] |

### Scopes

| Scope | Description |
|-------|-----------|
| [request] | [Request service] |
| [auth] | [Auth service] |
| [processing] | [Processing service] |
| [notification] | [Notification service] |
| [api] | [API layer] |
| [db] | [Database] |
| [infra] | [Infrastructure] |

### Breaking Changes

```
feat(api)!: change request response format

BREAKING CHANGE: Request response now includes nested customer object
```

## 3. Changelog Generation

```bash
# Generate changelog from commits
npm run changelog

# Generate changelog for specific version
npm run changelog -- --tag v1.0.0
```

## 4. Changelog Format

```markdown
# Changelog

## [1.2.0] - 2026-07-15

### Features
- **request:** add document upload functionality
- **auth:** implement MFA support
- **processing:** add auto-approval engine

### Bug Fixes
- **auth:** resolve token refresh issue
- **request:** fix status update race condition

### Performance
- **db:** add index for request queries

## [1.1.0] - 2026-07-01

### Features
- **api:** add pagination support
- **notification:** implement push notifications

### Bug Fixes
- **request:** fix validation error messages
```

## 5. Commit Guidelines

| Rule | Rationale |
|------|----------|
| [Use imperative mood] | ["add" not "added" — matches git default] |
| [First line < 72 chars] | [Readable in git log] |
| [Body explains why, not what] | [Code shows what; commit explains why] |
| [Reference issues] | [Closes #123] |
| [One logical change per commit] | [Atomic commits, easy revert] |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Coding-Standards]] | Code standards |
| [[README-Developer-Guide]] | Contribution guide |
| [[Build-Scripts]] | Build process |

---

> **Template Standard:** Based on SWEBOK v4, Conventional Commits
> **Usage:** Good commit messages are *documentation*. "Fix bug" is useless. "fix(auth): resolve token refresh race condition when user has multiple sessions" is useful.
