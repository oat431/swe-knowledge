---
document_type: Commit Messages / Changelog
version: "2.0"
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

> Standardized commit messages and auto-generated changelogs. Good commit messages are **documentation** — "fix bug" is useless. "fix(auth): resolve token refresh race condition when user has multiple sessions" is useful.

## 2. Commit Message Format

Based on [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <description>

[optional body — explains WHY, not WHAT]

[optional footer(s)]
```

### Types

| Type | Description | Example |
|------|-----------|---------|
| `feat` | New feature | `feat(discover): add self-preservation config` |
| `fix` | Bug fix | `fix(gate): resolve NPE on missing JWT claims` |
| `docs` | Documentation only | `docs(readme): add Docker setup instructions` |
| `style` | Formatting, whitespace (no logic change) | `style: fix indentation in build.gradle` |
| `refactor` | Code restructuring (no feature/fix) | `refactor(guard): extract realm config to JSON` |
| `test` | Add or update tests | `test(discover): add standalone mode verification` |
| `chore` | Build, tooling, deps | `chore(deps): bump Spring Boot to 4.1.0` |
| `perf` | Performance improvement | `perf(gate): cache JWKS with 5min TTL` |
| `ci` | CI/CD changes | `ci: add Gradle build to GitHub Actions` |
| `build` | Build system or external deps | `build: configure Checkstyle plugin` |
| `revert` | Revert a previous commit | `revert: feat(discover): add clustering support` |

### Breaking Changes

Append `!` after type/scope, or add `BREAKING CHANGE:` footer:

```
feat(api)!: change registration response format

BREAKING CHANGE: Registration endpoint now returns JSON by default.
Clients must set Accept: application/xml for legacy XML format.
```

## 3. Scopes — For Multi-Service Platforms

Define scopes per service or cross-cutting concern. For a microservice platform:

| Scope | Applies To | Example |
|-------|-----------|---------|
| `[service-name]` | Individual microservice | `feat(discover): configure standalone mode` |
| `docker` | Container config | `chore(docker): add healthcheck to compose` |
| `nginx` | Reverse proxy config | `feat(nginx): route new subdomain` |
| `deps` | Cross-cutting dependencies | `chore(deps): update Spring Cloud BOM` |
| `docs` | Cross-cutting documentation | `docs: update platform README` |
| `ci` | CI/CD pipelines | `ci: add multi-stack build matrix` |
| `infra` | Infrastructure (DB, cache, network) | `feat(infra): add MongoDB 8 container` |

## 4. Commit Guidelines

| Rule | Rationale |
|------|----------|
| Use imperative mood | "add" not "added" — matches `git merge` default |
| First line ≤ 72 characters | Readable in `git log --oneline` |
| Body explains WHY, not WHAT | Code shows *what* changed; commit explains *why* |
| Reference issues/tickets | `Closes #123` or `Refs PAN-42` |
| One logical change per commit | Atomic commits → easy to revert, bisect |
| No WIP commits on main | Squash before merge |

## 5. Changelog Generation

### Tooling by Stack

| Stack | Tool | Command |
|-------|------|---------|
| Any (git-based) | [git-cliff](https://git-cliff.org/) | `git cliff -o CHANGELOG.md` |
| Node.js | [standard-version](https://github.com/conventional-changelog/standard-version) | `npx standard-version` |
| Node.js | [commitlint](https://commitlint.js.org/) | `npx commitlint --from HEAD~1` |
| Java/Gradle | [semantic-release](https://semantic-release.gitbook.io/) | CI-based, auto on merge |
| Go | [svu](https://github.com/caarlos0/svu) | `svu next` |

## 6. Changelog Format

```markdown
# Changelog

## [0.1.0] - 2026-07-23

### Features
- **service:** description of feature ([#1](link))
- **service:** another feature

### Bug Fixes
- **service:** description of fix

### Documentation
- **service:** description of doc change

### Chores
- **deps:** description of dependency update

## [0.0.1] - 2026-07-22

### Chores
- initial project skeleton
```

## 7. Real-World Commit Examples

Good commits from a real Eureka Server project:

```bash
# Feature with scope
git commit -m "feat(discover): configure standalone mode with self-preservation

Standalone mode prevents Eureka from self-registering or
fetching its own registry. Self-preservation keeps the registry
intact during transient network hiccups in the homelab environment.

- register-with-eureka: false
- fetch-registry: false
- enable-self-preservation: true
- renewal-percent-threshold: 0.85
- eviction-interval-timer-in-ms: 5000"

# Test commit
git commit -m "test(discover): verify no self-registration in standalone mode"

# Documentation
git commit -m "docs(discover): rewrite README with architecture and setup guide"

# Docker
git commit -m "feat(docker): add multi-stage Dockerfile with ZGC flags"
```

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Coding-Standards]] | Code standards these commits follow |
| [[README-Developer-Guide]] | Contribution guide |
| [[Build-Scripts]] | Build process triggered by commits |

---

> **Template Standard:** Based on SWEBOK v4, Conventional Commits v1.0
> **Usage:** Every commit tells a story. "fix bug" is a wasted commit. Define scopes for your project's services and cross-cutting concerns. Generate changelogs automatically — never write them by hand. The body explains WHY — code shows what changed.
