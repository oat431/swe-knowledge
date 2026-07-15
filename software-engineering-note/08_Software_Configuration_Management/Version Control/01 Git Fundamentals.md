---
tags:
  - git
  - version-control
  - programming
---

# Git Fundamentals

Git is a distributed version control system that tracks snapshots of your project over time. Understanding its core model — commits, branches, and the three areas — is essential before you touch any [[02 Git Workflows|workflow]] or [[03 Git Advanced|advanced technique]].

---

## What Is a Commit?

A commit is a **snapshot** of your entire project at a point in time — not a diff. Each commit points to a tree (the file structure) and has metadata: author, timestamp, message, and parent commit(s).

❌ **Misconception:** "A commit stores only what changed."
✅ **Reality:** A commit references a full tree; Git deduplicates unchanged files via SHA-1 hashes internally.

```
commit → tree → blobs (files)
  ↓
parent commit(s)
```

---

## The Three Areas

| Area | Description | Key Command |
|------|-------------|-------------|
| **Working Directory** | Your actual files on disk | Edit files directly |
| **Staging Area** (Index) | Snapshot of what will go into the next commit | `git add` |
| **Repository** (`.git/`) | The full history of committed snapshots | `git commit` |

```bash
# See which area each file is in
git status
```

❌ **Bad:** Editing a file and immediately committing without staging — you might include unintended changes.
✅ **Good:** Stage specific files, review the diff, then commit.

```bash
# Review before committing
git diff              # unstaged changes
git diff --staged     # staged changes (what will be committed)
git add src/main.py   # stage specific file
git commit -m "feat: add user authentication"
```

---

## Essential Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `git init` | Create a new repo | `git init my-project` |
| `git clone` | Copy a remote repo | `git clone https://github.com/user/repo.git` |
| `git add` | Stage changes | `git add .` or `git add -p` (interactive) |
| `git commit` | Save staged snapshot | `git commit -m "fix: resolve login bug"` |
| `git status` | Show working tree state | `git status --short` |
| `git log` | View commit history | `git log --oneline --graph` |
| `git diff` | Compare changes | `git diff HEAD~3` |

---

## Branching

Branches are lightweight pointers to commits. The default branch is typically `main`.

```bash
# Create and switch to a new branch
git branch feature/login
git switch feature/login

# Shorthand: create + switch in one command
git switch -c feature/login

# List all branches
git branch -a

# Delete a branch
git branch -d feature/login
```

> `git switch` is the modern replacement for `git checkout <branch>`. Use `checkout` only for restoring files.

---

## Merge vs Rebase

Both integrate changes from one branch into another — but they work differently.

| Aspect | Merge | Rebase |
|--------|-------|--------|
| History | Preserves branch topology (merge commits) | Linear, replayed history |
| Safe? | ✅ Non-destructive | ❌ Rewrites history |
| Use on shared branches? | ✅ Yes | ❌ No |
| Use on local branches? | ✅ Yes | ✅ Yes |

### Merge

```bash
git switch main
git merge feature/login
# Creates a merge commit that combines both histories
```

✅ **Good:** Merge when integrating a finished feature into `main` or `develop`.
✅ **Good:** Merge when working with shared branches (other people see the same commits).

### Rebase

```bash
git switch feature/login
git rebase main
# Replays feature/login commits on top of main (linear history)
```

✅ **Good:** Rebase your local feature branch before merging to keep history clean.
❌ **Bad:** Rebase a branch that others have already pulled — it rewrites commit SHAs and causes conflicts.

### Golden Rule

> ❌ **Never rebase commits that have been pushed to a shared branch.**

---

## Cherry-Pick

Apply a specific commit from one branch onto your current branch.

```bash
# Pick commit abc123 onto current branch
git cherry-pick abc123

# Pick multiple commits
git cherry-pick abc123 def456

# Cherry-pick without committing (stage only)
git cherry-pick --no-commit abc123
```

**Use cases:**
- Hotfix that needs to go to both `main` and `develop`
- Backporting a fix to an older release branch

---

## Stashing

Temporarily shelve uncommitted changes to work on something else.

```bash
# Stash current changes
git stash

# Stash with a message
git stash push -m "WIP: login form"

# List all stashes
git stash list

# Apply the most recent stash (keeps it in stash list)
git stash apply

# Pop the most recent stash (removes from stash list)
git stash pop

# Apply a specific stash
git stash apply stash@{2}

# Drop a specific stash
git stash drop stash@{1}

# Clear all stashes
git stash clear
```

❌ **Bad:** Stashing changes and forgetting about them — stashes are local and not backed up.
✅ **Good:** Always add a message so you remember what each stash contains.

---

## .gitignore

Tells Git which files to ignore. Must be committed to the repo.

```bash
# .gitignore examples
node_modules/
*.log
dist/
.env
.DS_Store
__pycache__/
*.pyc
```

| Pattern | Matches |
|---------|---------|
| `*.log` | All `.log` files |
| `node_modules/` | Entire directory |
| `!important.log` | Negate — DON'T ignore this file |
| `build/**` | Everything inside `build/` |
| `*.py[cod]` | `.pyc`, `.pyo`, `.pyd` |

> 💡 Already tracked files won't be ignored. Remove them first:
> ```bash
> git rm --cached .env
> ```

---

## Conflict Resolution

Conflicts happen when two branches modify the same lines and Git can't auto-merge.

### How Conflicts Occur

```bash
# Both branches edit the same file at the same line
git switch main
# ... edit line 10 of app.py
git commit -am "change A"

git switch feature
# ... edit line 10 of app.py
git commit -am "change B"

git merge main
# CONFLICT in app.py!
```

### What the Conflict Looks Like

```
<<<<<<< HEAD
print("Hello from feature")
=======
print("Hello from main")
>>>>>>> main
```

### How to Resolve

1. Open the conflicted file
2. Choose the correct code (or combine both)
3. Remove the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
4. Stage and commit:

```bash
git add app.py
git commit -m "resolve: merge conflict in app.py"
```

### Useful Tools

```bash
# Use a visual merge tool
git mergetool

# Abort a merge if things go wrong
git merge --abort
```

✅ **Good:** Communicate with your team about which areas each person owns to minimize conflicts.
❌ **Bad:** Force-pushing over someone else's conflict resolution.

---

## Sources

- [Pro Git — Git Basics](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)
- [Pro Git — Branching](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)
- [Git SCM Documentation](https://git-scm.com/doc)
- [Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials)
