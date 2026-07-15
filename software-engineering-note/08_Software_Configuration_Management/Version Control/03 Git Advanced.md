---
tags:
  - git
  - version-control
  - advanced
  - programming
---

# Git Advanced

Once you've mastered [[01 Git Fundamentals|fundamentals]] and picked a [[02 Git Workflows|workflow]], these advanced techniques will save you hours of debugging and keep your repo history clean. These are the tools that separate everyday Git users from power users.

---

## Interactive Rebase

Rewrite, reorder, squash, or edit commits before sharing them. This is the #1 tool for cleaning up messy local history.

```bash
# Rebase the last 5 commits interactively
git rebase -i HEAD~5
```

### In the Editor

```
pick abc1234 feat: add login form
pick def5678 fix: typo in login form
pick ghi9012 feat: add validation
pick jkl3456 fix: forgot password field
pick mno7890 wip: debug logging
```

Change `pick` to one of:

| Command | Effect |
|---------|--------|
| `pick` | Keep the commit as-is |
| `reword` | Keep changes, edit the commit message |
| `squash` | Merge into previous commit, combine messages |
| `fixup` | Merge into previous commit, discard this message |
| `edit` | Pause at this commit (amend it) |
| `drop` | Remove the commit entirely |

### Example: Squash Messy Commits

```
pick abc1234 feat: add login form
fixup def5678 fix: typo in login form
fixup jkl3456 fix: forgot password field
pick ghi9012 feat: add validation
drop mno7890 wip: debug logging
```

Result: 2 clean commits instead of 5 messy ones.

✅ **Good:** `git rebase -i` to clean up commits before opening a PR.
❌ **Bad:** `git rebase -i` on commits that are already pushed and shared with others.

---

## Git Bisect

Binary search through your commit history to find which commit introduced a bug.

```bash
# Start bisecting
git bisect start

# Mark current commit as bad (bug exists)
git bisect bad

# Mark a known good commit
git bisect good v1.0.0

# Git checks out a middle commit — test it, then mark:
git bisect good   # bug is NOT here
git bisect bad    # bug IS here

# Repeat until Git finds the exact commit
# Result: abc1234 is the first bad commit

# End the bisect session
git bisect reset
```

### Automate with a Script

```bash
# Run a test script that exits 0 (good) or 1 (bad)
git bisect start v2.0.0 v1.0.0
git bisect run npm test
```

This is incredibly powerful for large codebases — finding a regression in 1000+ commits takes ~10 steps.

---

## Git Reflog

The reflog records every move of `HEAD` — even commits that seem "lost" after a bad reset or rebase.

```bash
# View the reflog
git reflog

# Output example:
# abc1234 HEAD@{0}: reset: moving to HEAD~3
# def5678 HEAD@{1}: commit: feat: add search
# ghi9012 HEAD@{2}: commit: feat: add login
```

### Recover a "Lost" Commit

```bash
# Oh no, I did a bad reset and lost my work!
git reset --hard HEAD~3

# Find the lost commit in reflog
git reflog
# def5678 HEAD@{1}: commit: feat: add search

# Recover it
git reset --hard def5678
```

### Create a Branch from a Reflog Entry

```bash
# Recover a specific state as a new branch
git branch recovered-work HEAD@{2}
```

> 💡 Reflog is local only and entries expire (default: 90 days for reachable, 30 days for unreachable commits).

---

## Git Hooks

Hooks are scripts that run automatically at specific Git events. They live in `.git/hooks/`.

### Common Hooks

| Hook | When It Runs | Use Case |
|------|--------------|----------|
| `pre-commit` | Before `git commit` | Linting, formatting, tests |
| `commit-msg` | After writing commit message | Enforce commit message format |
| `pre-push` | Before `git push` | Run full test suite |
| `post-merge` | After `git merge` | Reinstall dependencies |
| `pre-rebase` | Before `git rebase` | Warn about rebasing shared branches |

### Example: pre-commit Hook

```bash
#!/bin/bash
# .git/hooks/pre-commit

# Run linter on staged files
npm run lint -- --fix
if [ $? -ne 0 ]; then
  echo "❌ Linting failed. Fix errors before committing."
  exit 1
fi

echo "✅ Lint passed."
```

> Remember to make it executable:
> ```bash
> chmod +x .git/hooks/pre-commit
> ```

### Husky (JavaScript Projects)

[Husky](https://typicode.github.io/husky/) manages Git hooks as part of your project, so they're shared with the team.

```bash
# Install husky
npm install husky --save-dev

# Initialize
npx husky init

# Add a hook
npx husky add .husky/pre-commit "npm run lint"
npx husky add .husky/commit-msg 'npx commitlint --edit $1'
```

❌ **Bad:** Putting hook scripts in `.git/hooks/` — they're not tracked by Git and not shared.
✅ **Good:** Using Husky (or similar) so hooks are in the repo and consistent across the team.

---

## Submodules vs Monorepo

| Aspect | Submodules | Monorepo |
|--------|-----------|----------|
| Structure | Multiple repos linked together | Single repo, multiple packages |
| Versioning | Each repo versioned independently | Shared versioning |
| CI/CD | Separate pipelines | Unified pipeline |
| Code sharing | Via package managers or submodule refs | Direct imports |
| Git complexity | High (submodule gotchas) | Low |
| Repo size | Small per repo | Can grow very large |

### Submodule Basics

```bash
# Add a submodule
git submodule add https://github.com/user/lib.git libs/lib

# Clone a repo with submodules
git clone --recurse-submodules https://github.com/user/project.git

# Update submodules
git submodule update --remote

# Initialize submodules after clone
git submodule init && git submodule update
```

⚠️ Common pitfall: Forgetting to update submodules after pulling — leads to stale dependencies.

❌ **Bad:** Using submodules for tightly coupled code that changes together.
✅ **Good:** Using submodules for stable, independently versioned libraries.

---

## Git Internals (Brief)

Understanding the object model helps when troubleshooting advanced issues.

### Object Types

| Type | Contains | Example |
|------|----------|---------|
| **blob** | File content (no filename) | The contents of `main.py` |
| **tree** | Directory listing (filenames → blobs) | Maps `main.py` → blob hash |
| **commit** | Snapshot pointer (tree + parent + metadata) | Points to root tree |
| **tag** | Annotated tag object | Points to a commit |

### How They Connect

```
commit (abc123)
  ├── tree (root)
  │   ├── blob: main.py
  │   ├── blob: README.md
  │   └── tree: src/
  │       └── blob: utils.py
  └── parent: commit (def456)
```

### HEAD and Refs

```bash
# HEAD is a pointer to the current branch or commit
cat .git/HEAD
# ref: refs/heads/main

# Branches are just files containing a commit hash
cat .git/refs/heads/main
# abc1234def5678...

# Inspect objects directly
git cat-file -t abc1234    # type: commit
git cat-file -p abc1234    # pretty-print contents
git cat-file -s abc1234    # size in bytes
```

---

## Useful Aliases

Add these to `~/.gitconfig`:

| Alias | Command | What It Does |
|-------|---------|--------------|
| `git lg` | `log --oneline --graph --all` | Visual commit graph |
| `git st` | `status --short` | Compact status |
| `git co` | `checkout` | Shorter checkout |
| `git br` | `branch` | Shorter branch |
| `git ci` | `commit` | Shorter commit |
| `git unstage` | `reset HEAD --` | Unstage a file |
| `git last` | `log -1 HEAD --stat` | Show last commit with stats |
| `git amend` | `commit --amend --no-edit` | Amend last commit without editing message |

```bash
# Set aliases via CLI
git config --global alias.lg "log --oneline --graph --all"
git config --global alias.st "status --short"
git config --global alias.unstage "reset HEAD --"

# Or edit ~/.gitconfig directly
[alias]
    lg = log --oneline --graph --all
    st = status --short
    co = checkout
    unstage = reset HEAD --
    last = log -1 HEAD --stat
    amend = commit --amend --no-edit
```

---

## Common Disasters & Recovery

| Disaster | Recovery Command | What It Does |
|----------|-----------------|--------------|
| Bad commit (already pushed) | `git revert abc1234` | Creates a new commit that undoes the bad one |
| Bad commit (local, not pushed) | `git reset HEAD~1` | Moves HEAD back, keeps changes unstaged |
| Want to erase all evidence | `git reset --hard HEAD~1` | Moves HEAD back, **destroys** changes |
| Need a destroyed commit back | `git reflog` → `git reset --hard abc1234` | Recover via reflog |
| Committed to wrong branch | `git reset HEAD~1` → `git switch correct` → `git commit` | Move the commit |
| Messy merge, want to abort | `git merge --abort` | Undo an in-progress merge |
| Messy rebase, want to abort | `git rebase --abort` | Undo an in-progress rebase |

### Reset vs Revert

| Command | Effect | Safe for Shared History? |
|---------|--------|--------------------------|
| `git reset` | Moves branch pointer (rewrites history) | ❌ No |
| `git revert` | Creates new commit that undoes changes | ✅ Yes |

❌ **Bad:** `git reset --hard` on a shared branch — others will have divergent history.
✅ **Good:** `git revert` on shared branches — creates a clean undo commit everyone can pull.

```bash
# Revert a specific commit
git revert abc1234

# Revert a merge commit (specify parent)
git revert -m 1 merge-commit-hash

# Revert multiple commits
git revert abc1234..def5678
```

---

## Sources

- [Pro Git — Git Internals](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects)
- [Pro Git — Rewriting History](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)
- [Git Bisect Documentation](https://git-scm.com/docs/git-bisect)
- [Husky Documentation](https://typicode.github.io/husky/)
- [Git Submodules Guide](https://git-scm.com/book/en/v2/Git-Tools-Submodules)
