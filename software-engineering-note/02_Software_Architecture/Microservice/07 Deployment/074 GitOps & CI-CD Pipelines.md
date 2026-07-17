---
tags:
  - architecture
  - microservices
  - devops
  - programming
---

# 07 GitOps & CI/CD Pipelines

GitOps makes Git the single source of truth for what's running in production. Commit = deploy. No manual kubectl, no drift between environments.

---

## GitOps Principles

1. **Declarative config in Git** — entire desired state (manifests, Helm values, Kustomize overlays) lives in a git repo
2. **Automated reconciliation** — an agent continuously compares desired state (git) vs actual state (cluster) and applies diffs
3. **Drift detection + self-healing** — someone runs `kubectl edit` manually? Agent reverts it back to what git says
4. **Audit trail via git history** — every change is a commit with author, timestamp, and PR review. Free compliance.

---

## Push vs Pull Deployment Model

### Traditional Push (CI/CD → Cluster)

CI pipeline builds image, then `kubectl apply` or `helm upgrade` directly to the cluster.

**Problem:** CI system needs cluster credentials. One compromised pipeline = full cluster access.

### GitOps Pull Model (Cluster ← Git)

Agent lives *inside* the cluster, watches the git repo, and pulls changes in.

```
┌─────────────┐       ┌─────────────┐       ┌─────────────────┐
│  Developer  │──PR──▶│  Config Repo │◀──watches──│  GitOps Agent   │
└─────────────┘       └─────────────┘       │  (in-cluster)   │
                                             └────────┬────────┘
                                                      │ applies
                                                      ▼
                                             ┌─────────────────┐
                                             │   K8s Cluster    │
                                             └─────────────────┘
```

**Benefit:** No external system has cluster credentials. Agent pulls, never exposes the API server.

---

## ArgoCD vs Flux

| Feature | ArgoCD | Flux |
|---------|--------|------|
| UI | Built-in web UI + CLI | No UI by default (Weave GitOps optional) |
| CRDs | Application, AppProject | GitRepository, Kustomization, HelmRelease |
| Multi-tenancy | AppProject-based isolation | Namespace-scoped controllers |
| Adoption (2026) | ~77% market share | Growing, strong in platform teams |
| CNCF status | Graduated | Graduated |
| Best for | Teams wanting visibility + UI | Lightweight, composable setups |

Both are production-ready. ArgoCD wins on DX for most teams; Flux wins on minimalism.

---

## Repository Strategy

| Strategy | Pros | Cons |
|----------|------|------|
| Monorepo | Simple, atomic changes | Blast radius, slow CI |
| Polyrepo | Independent, clear ownership | Coordination overhead |
| **Hybrid** (recommended) | Balance of both | Slight complexity |

### Recommended Separation

- **App repo** — source code + Dockerfile + unit tests
- **Config repo** — K8s manifests, Helm charts, Kustomize overlays

### Flow

```
App Repo CI:  code → test → build image → push to registry → update image tag in Config Repo
Config Repo:  GitOps agent detects new commit → deploys to cluster
```

This separation means:
- App devs don't need cluster access
- Config repo PRs act as deployment gates
- Clear audit of *what* is deployed vs *how* it was built

---

## Independent Pipelines per Service

Each microservice has its own CI pipeline. Change in Service A does **not** rebuild Service B.

### Pipeline Stages

```
lint → unit test → integration test → build image → push image → update config repo → GitOps deploys
```

- Image tag = git SHA (e.g., `myapp:a1b2c3d`)
- CI updates the config repo via automated PR or direct commit to a deploy branch
- GitOps agent picks up the change within seconds

---

## Rollback

GitOps rollback is trivial:

```bash
# Revert the last deployment
git revert HEAD
git push
# Agent reconciles → previous version is live
```

No special tooling. No rollback commands. Git history IS your rollback mechanism.

ArgoCD also supports one-click rollback from UI to any previous sync.

---

## Key Rules

1. **Never manual `kubectl` in prod** — all changes through git. Period.
2. **Image tags = git SHA** — never use `:latest`. You must know exactly what's running.
3. **Config repo PRs require review** — deployment is a code change, treat it like one.
4. **One pipeline per service** — independence is the whole point of microservices.
5. **Secrets via sealed-secrets or external-secrets** — never plaintext in git.

---

## Sources

- [OpenGitOps Principles](https://opengitops.dev/)
- [ArgoCD Documentation](https://argo-cd.readthedocs.io/)
- CNCF Survey 2026 — GitOps Adoption Report
