---
tags:
  - git
  - version-control
  - overview
  - programming
---

# Version Control Overview

Version control tracks every change to your codebase, enabling safe collaboration and effortless rollback. It's the foundation of modern software development — if you're not using it, start today.

---

## Why Version Control Matters

- **History** — every change is recorded with who, what, when, and why
- **Collaboration** — multiple developers work on the same codebase without stepping on each other
- **Experimentation** — try new features in branches without risking the main code
- **Rollback** — undo mistakes by reverting to any previous state
- **Accountability** — clear audit trail for code reviews and compliance

---

## When to Use What

| Situation | Tool / Approach | Notes |
|-----------|-----------------|-------|
| Any code project | Git | Industry standard, use it by default |
| Large binary files (assets, models) | [[03 Git Advanced#Submodules vs Monorepo\|Git LFS]] | Don't store binaries directly in Git |
| Monorepo with many teams | Trunk-Based + feature flags | See [[02 Git Workflows#Trunk-Based Development]] |
| Release-heavy product (libraries, SDKs) | GitFlow | See [[02 Git Workflows#GitFlow]] |
| Small team, continuous deployment | GitHub Flow | See [[02 Git Workflows#GitHub Flow]] |
| Solo hobby project | Git + GitHub/GitLab | Still use version control — always |

---

## Topic Files

| File | What's Covered |
|------|----------------|
| [[01 Git Fundamentals]] | Commits, branches, merge, rebase, stash, .gitignore, conflicts |
| [[02 Git Workflows]] | GitFlow, GitHub Flow, Trunk-Based, PR conventions, commit messages |
| [[03 Git Advanced]] | Interactive rebase, bisect, reflog, hooks, internals, disaster recovery |

---

## Common Mistakes

❌ **Bad:** Starting a project without `git init` — "I'll add version control later."
✅ **Good:** Initialize Git on day one, even for personal scripts.

❌ **Bad:** Committing directly to `main` with vague messages like `update`.
✅ **Good:** Use feature branches and descriptive commits ([Conventional Commits](https://www.conventionalcommits.org/)). See [[02 Git Workflows#Commit Message Conventions]].

---

## Quick Reference

```bash
# Start a new repo
git init

# Clone an existing repo
git clone https://github.com/user/repo.git

# Stage, commit, push
git add .
git commit -m "feat: initial commit"
git push origin main
```

> 💡 For a deep dive into these commands, see [[01 Git Fundamentals]].

---

## Sources

- [Pro Git Book — Scott Chacon & Ben Straub](https://git-scm.com/book/en/v2)
- [Git Official Documentation](https://git-scm.com/doc)
- [GitHub Guides](https://guides.github.com/)
