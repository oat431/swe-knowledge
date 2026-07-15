---
tags:
  - git
  - version-control
  - workflows
  - programming
---

# Git Workflows

A workflow defines how your team uses branches, pull requests, and releases. Choosing the right one depends on team size, release cadence, and deployment strategy. This guide covers the three most popular approaches and the conventions that keep them running smoothly.

---

## GitFlow

A structured branching model with long-lived branches and clear roles.

### Branch Structure

| Branch | Lifetime | Purpose |
|--------|----------|---------|
| `main` | Permanent | Production-ready code, tagged releases |
| `develop` | Permanent | Integration branch for next release |
| `feature/*` | Short-lived | New features, branched from `develop` |
| `release/*` | Short-lived | Prepares a release (version bump, final fixes) |
| `hotfix/*` | Short-lived | Emergency fixes, branched from `main` |

### Flow Diagram

```
main      ─●────────────────────●─ (v1.0)──────●─ (hotfix)──●─ (v1.0.1)──
            \                  /                \           /
develop   ───●──●──●──●──●───●───●──●───────────────────────●──●──
               \      /        \  /                           \/
feature/A       ●──●──●         \/
                                 feature/B  ●──●──
                                 release/1.0  ●──●
```

### Commands

```bash
# Start a feature
git switch -c feature/login develop

# Finish a feature → merge into develop
git switch develop
git merge --no-ff feature/login
git branch -d feature/login

# Start a release
git switch -c release/1.0 develop

# Finish a release → merge into main AND develop
git switch main
git merge --no-ff release/1.0
git tag -a v1.0 -m "Release v1.0"
git switch develop
git merge --no-ff release/1.0

# Emergency hotfix
git switch -c hotfix/1.0.1 main
# ... fix ...
git switch main
git merge --no-ff hotfix/1.0.1
git tag -a v1.0.1 -m "Hotfix v1.0.1"
git switch develop
git merge --no-ff hotfix/1.0.1
```

**Best for:** Large teams, scheduled releases, libraries/SDKs, apps with multiple supported versions.

---

## GitHub Flow

A lightweight workflow: `main` is always deployable, all work happens in feature branches merged via pull requests.

### Flow

1. Create a branch from `main`
2. Make commits
3. Open a Pull Request
4. Review, discuss, approve
5. Merge into `main`
6. Deploy

```bash
# Create feature branch
git switch -c feature/add-search

# Work and commit
git add .
git commit -m "feat: add search functionality"

# Push and open PR
git push origin feature/add-search
# → Open PR on GitHub → Review → Merge → Deploy
```

✅ **Good:** Keep `main` always deployable with CI/CD checks on every PR.
❌ **Bad:** Long-lived feature branches that drift far from `main`.

**Best for:** Small-to-medium teams, continuous deployment, web applications.

---

## Trunk-Based Development

Everyone commits to `main` (the "trunk") with very short-lived branches — ideally less than a day. Incomplete features are hidden behind **feature flags**.

### Key Principles

| Principle | Description |
|-----------|-------------|
| Short-lived branches | Merge within hours, not days |
| Feature flags | Toggle incomplete features on/off in production |
| Continuous integration | Every commit is tested against `main` |
| No long-lived branches | No `develop`, no `release/*` |

### Feature Flags Example

```bash
# In your code
if feature_flags.enabled?("new-checkout")
  render_new_checkout
else
  render_old_checkout
end
```

✅ **Good:** Merge small, frequent changes to `main` and use flags to control rollout.
❌ **Bad:** Accumulating a giant branch that nobody merges for weeks.

**Best for:** Large engineering teams (Google, Facebook), continuous deployment, high-trust environments.

---

## Comparison Table

| Aspect | GitFlow | GitHub Flow | Trunk-Based |
|--------|---------|-------------|-------------|
| Complexity | High | Low | Low |
| Branches | 5 types | 2 (main + feature) | 1-2 (main + short-lived) |
| Release model | Scheduled | Continuous | Continuous |
| Best for | Libraries, versioned apps | Web apps, small teams | Large teams, CI/CD |
| Learning curve | Steep | Gentle | Moderate (needs feature flags) |
| Merge conflicts | Frequent (long-lived branches) | Rare | Very rare |

---

## Commit Message Conventions

Use [Conventional Commits](https://www.conventionalcommits.org/) for clear, machine-parseable history.

### Format

```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

### Types

| Type | When to Use | Example |
|------|-------------|---------|
| `feat` | New feature | `feat(auth): add OAuth2 login` |
| `fix` | Bug fix | `fix(api): handle null response` |
| `docs` | Documentation only | `docs(readme): update install steps` |
| `style` | Formatting, no logic change | `style: fix indentation in main.py` |
| `refactor` | Code restructuring | `refactor(db): extract query builder` |
| `test` | Adding/updating tests | `test(auth): add login validation tests` |
| `chore` | Build, CI, tooling | `chore(deps): upgrade webpack to v5` |
| `perf` | Performance improvement | `perf(api): add caching layer` |

### Breaking Changes

```
feat(api)!: change response format

BREAKING CHANGE: API responses now use camelCase instead of snake_case
```

❌ **Bad:** `git commit -m "fixed stuff"`
✅ **Good:** `fix(auth): resolve token expiration check for refresh tokens`

---

## PR (Pull Request) Conventions

### Title Format

Use the same Conventional Commits format:
```
feat(search): add full-text search with Elasticsearch
```

### Description Template

```markdown
## What
Brief description of the change.

## Why
Why this change is needed (link to issue/ticket).

## How
Key implementation details.

## Testing
How this was tested.

## Screenshots
(if UI changes)

Closes #123
```

### Review Checklist

- [ ] Code compiles and tests pass
- [ ] New code has tests
- [ ] No secrets or credentials in the diff
- [ ] Error handling is adequate
- [ ] Documentation updated if needed
- [ ] PR is small enough to review (< 400 lines ideally)

---

## Branch Naming

| Prefix | Use For | Example |
|--------|---------|---------|
| `feature/` | New features | `feature/user-profile` |
| `bugfix/` | Non-urgent bug fixes | `bugfix/date-format` |
| `hotfix/` | Urgent production fixes | `hotfix/auth-crash` |
| `release/` | Release preparation | `release/2.1.0` |
| `chore/` | Tooling, dependencies | `chore/update-deps` |

✅ **Good:** `feature/add-search-filter` — descriptive, kebab-case.
❌ **Bad:** `my-branch`, `fix`, `temp` — meaningless names.

---

## Tagging & Semantic Versioning

Use tags to mark releases. Follow [Semantic Versioning](https://semver.org/):

```
vMAJOR.MINOR.PATCH
```

| Component | When to Increment | Example |
|-----------|-------------------|---------|
| **MAJOR** | Breaking changes | `v2.0.0` |
| **MINOR** | New features (backward-compatible) | `v1.3.0` |
| **PATCH** | Bug fixes | `v1.3.1` |

```bash
# Create an annotated tag
git tag -a v1.2.0 -m "Release v1.2.0: add search feature"

# Push tags to remote
git push origin v1.2.0

# Push all tags
git push origin --tags

# List all tags
git tag -l
```

❌ **Bad:** `v1`, `release-2`, `final-final-v3`
✅ **Good:** `v1.0.0`, `v2.1.3`, `v0.1.0-beta.1`

---

## Sources

- [GitFlow — Vincent Driessen](https://nvie.com/posts/a-successful-git-branching-model/)
- [GitHub Flow — GitHub](https://docs.github.com/en/get-started/using-git/github-flow)
- [Trunk-Based Development](https://trunkbaseddevelopment.com/)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Semantic Versioning](https://semver.org/)
