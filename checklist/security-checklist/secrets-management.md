# Secrets Management Checklist

> **Secrets management** is the discipline of keeping keys, tokens, passwords, and certificates out of source code, logs, and unauthorized hands — and rotating them when they expire or leak.
> The master checklist ([[security]]) §4 says "no secrets in code." This file is *how* to set up the vault, scanner, and rotation practices that make that real.
> Deep references: → [[02 Secrets Management]].
> Last updated: 2026-08-07

---

## 1. Mental Model: The Secret Lifecycle

- [ ] **Understand the lifecycle** — Every secret has five stages: **Create → Distribute → Use → Rotate → Revoke**. A gap at any stage is a vulnerability.
- [ ] **Create** — Generated with a CSPRNG, sufficient entropy (≥ 128 bits), scoped to minimum permissions.
- [ ] **Distribute** — Delivered to the consumer via a secure channel (vault injection, sidecar, env var at runtime, sealed secret). Never in source, never in chat, never in email.
- [ ] **Use** — Read from the vault at runtime, not baked into the image. Access to the secret is audited.
- [ ] **Rotate** — Replaced on schedule (90 days typical) or immediately on suspected exposure. Rotation is tested, not theoretical.
- [ ] **Revoke** — When the secret is no longer needed or confirmed leaked, it is revoked and replaced. Old values are dead.

---

## 2. Local Development

- [ ] **`.env` files git-ignored** — `.env` in `.gitignore`. Never committed, ever.
- [ ] **`.env.example` committed** — Documents what variables are needed, with dummy values:
  ```bash
  # .env.example — copy to .env and fill in real values
  DATABASE_URL=postgresql://user:password@localhost:5432/dev_db
  JWT_SECRET=<generate-with: openssl rand -base64 32>
  STRIPE_SECRET_KEY=sk_test_xxxxxxxxxxxxx
  ```
- [ ] **Pre-commit hook for secrets** — gitleaks/trufflehog runs on staged files:
  ```bash
  # .pre-commit-config.yaml
  repos:
    - repo: https://github.com/gitleaks/gitleaks
      rev: v8.21.2
      hooks:
        - id: gitleaks
  ```
- [ ] **Secret values generated correctly** — `openssl rand -base64 32` for symmetric keys. Not `Math.random()`, not a typed string, not a date-based value.
- [ ] **Local secrets ≠ staging/prod secrets** — Dev uses local or dummy values. No production secrets on developer machines.

---

## 3. CI/CD Pipelines

- [ ] **Secrets stored in the CI secret store** — GitHub Encrypted Secrets, GitLab CI Variables, Jenkins Credentials. Not hardcoded in YAML.
- [ ] **Secrets masked in logs** — CI masks secret values in output. Test: deliberately echo a secret in a build step; it should appear as `::add-mask::`.
- [ ] **Fork PRs get no secrets** — Workflows triggered by `pull_request` from forks do not have access to secrets. `pull_request_target` (which *does*) must not checkout/ run untrusted code.
- [ ] **Secrets scoped to the job** — Environment-specific (prod secrets only in the deploy job, not in the build job). Use GitHub Environments / GitLab Environments to scope.
- [ ] **OIDC over long-lived keys** — For cloud deploys, use OIDC federation (GitHub Actions → AWS IAM Role, no stored AWS keys). If keys are unavoidable, rotate every 90 days and log access.
  ```yaml
  # .github/workflows/deploy.yml — OIDC auth to AWS
  permissions:
    id-token: write   # Required for OIDC
    contents: read
  jobs:
    deploy:
      runs-on: ubuntu-latest
      steps:
        - uses: aws-actions/configure-aws-credentials@v4
          with:
            role-to-assume: arn:aws:iam::123456789012:role/github-actions-deploy
            aws-region: ap-southeast-1
            # No AWS_ACCESS_KEY_ID in secrets — OIDC handles auth
  ```

---

## 4. Production: Vault / KMS / Secret Manager

### Choosing the right tool

| Context | Tool | Why |
|---|---|---|
| Kubernetes-native | External Secrets Operator + Vault/KMS | Pods get secrets as mounted files/env vars, rotated by operator |
| AWS-native | AWS Secrets Manager / Parameter Store | Native integration with Lambda, ECS, RDS |
| GCP-native | Google Secret Manager | Native integration with Cloud Run, GKE |
| Azure-native | Azure Key Vault | Native integration with App Service, AKS |
| Self-hosted / multi-cloud | HashiCorp Vault | Centralized, K8s auth method, dynamic secrets |
| Sealed secrets (GitOps) | Sealed Secrets / SOPS | Encrypted-at-rest in Git, decrypted in cluster |

- [ ] **Vault/KMS in production** — Not `.env` files. Not Kubernetes Secrets (unencrypted at rest by default).
- [ ] **Secret store per environment** — Staging and prod have separate secret stores. No shared credentials across environments.
- [ ] **Access to secrets audited** — Every read is logged with identity + timestamp. Vault audit log, CloudTrail `GetSecretValue`, etc.
- [ ] **Secrets injected at runtime** — Via sidecar (Vault Agent), init container, or CSI driver. Not baked into container images.
- [ ] **Dynamic secrets where possible** — Vault generates short-lived DB credentials on demand (hourly expiry). No long-lived DB passwords.

---

## 5. Secret Scanning (CI + Repo)

- [ ] **gitleaks in CI** — Runs on every PR and push:
  ```yaml
  # GitHub Actions
  - name: Run gitleaks
    uses: gitleaks/gitleaks-action@v2
    env:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      GITLEAKS_LICENSE: ${{ secrets.GITLEAKS_LICENSE }} # public repos are free
  ```
- [ ] **Push protection enabled** — GitHub Advanced Security / GitLab Push Rules block the commit before it lands. Stops exposure in one click.
- [ ] **Historical scan done** — `gitleaks detect --source . --report-path leak.json` on the full repo history. Past leaks don't self-heal.
- [ ] **Scan Docker images** — `trivy fs --scanners secrets ./` on the built image. Secrets baked into layers survive even if removed from the final layer.
- images.
- [ ] **Scan IaC** — Checkov, tfsec, KICS on Terraform/CloudFormation. Secrets in `terraform.tfvars` are a classic leak.

---

## 6. Rotation

- [ ] **Rotation schedule documented** — Which secrets rotate, how often, who owns it:
  ```
  | Secret Type           | Rotation Frequency | Owner         | Method              |
  |-----------------------|--------------------|---------------|---------------------|
  | DB password (app)     | 90 days            | Platform team | Vault dynamic sec   |
  | JWT signing key       | 180 days           | Auth team     | Key grace period    |
  | TLS certificate       | 60 days            | DevOps        | cert-manager auto   |
  | API key (third-party) | Per vendor policy  | Integration   | Manual + ticket     |
  | Cloud root account    | N/A (locked away)  | Security lead | Hardware MFA        |
  ```

- [ ] **Rotation is tested** — You have actually run the rotation procedure end-to-end. A rotation procedure that has never been exercised will fail during an incident.
- [ ] **Grace period for signing keys** — Old key accepted for verification during overlap window. New key used for signing immediately. Avoids breaking active sessions.
- [ ] **Automation over manual** — Vault dynamic secrets, AWS auto-rotation, cert-manager for TLS. Manual rotation is forgotten rotation.
- [ ] **Exposure triggers immediate rotation** — Suspected or confirmed leak = revoke + rotate immediately, not on next schedule. Every minute the leaked secret lives is a minute of unauthorized access.

---

## 7. Logging & Observability

- [ ] **Secrets never logged** — Structured logging with a redaction filter. Test: grep prod logs for a known test secret value.
- [ ] **Secret access logged** — Every read from the vault/KMS is audited. Access patterns baseline for anomaly detection.
- [ ] **Rotation alerts** — Alert if a secret is nearing expiry or past rotation date. Auto-rotation failure alerts to on-call.
- [ ] **Secret distribution alerts** — Unexpected reads (wrong service, new source, off-hours) trigger alerts. Could indicate compromise or misconfiguration.

---

## 8. Anti-Patterns to Avoid

- [ ] **No secrets in code** — Ever. The single most common cause of secret leaks. → [[02 Secrets Management]].
- [ ] **`.env` committed "just for staging"** — Staging secrets are still secrets. They grant access to staging data, which is often a pivot to production.
- [ ] **Symmetric algorithm with shared key** — Where asymmetric would work. If both sides must share a key, rotation requires both sides to update simultaneously.
- [ ] **Secrets in container images** — Baked into a Dockerfile `ENV` or a copied config file. Images are shared, cached, and shipped to registries — secrets inside them travel.
- [ ] **Long-lived service account tokens** — Kubernetes service account tokens, legacy API keys, "permanent" JWTs. Prefer short-lived tokens (STS, Workload Identity).
- [ ] **No rotation plan** — The secret was created two years ago. Nobody remembers who has access. Nobody knows what breaks if it's rotated. This is how leaked secrets stay valid for years.
- [ ] **Cloud root/admin key in CI** — The nuclear option. A compromised CI pipeline exfiltrates it. Use scoped IAM roles + OIDC.
- [ ] **Secrets in chat/email** — "Hey, what's the DB password?" in Slack/Teams. Use the vault, not chat.
- [ ] **Secrets re-forwarded** — A secret is forwarded from the vault to someone. Now two people can't track who has it. Send a link to the vault entry, not the value.
- [ ] **No revocation path** — The secret was created, distributed, used, and then forgotten. When it leaks, there is no documented procedure to kill it quickly.

---

## Quick Sanity Check

- [ ] `.env` git-ignored; `.env.example` committed with dummy values
- [ ] Pre-commit hook (gitleaks) runs on staged files
- [ ] CI secrets in the CI secret store, scoped to jobs, masked in logs
- [ ] Fork PRs and untrusted builds get no secrets
- [ ] Production uses Vault/KMS/Secret Manager, not `.env` or unencrypted K8s Secrets
- [ ] Secret scanning (gitleaks/trufflehog) runs in CI on every PR
- [ ] Historical scan done on the repo and Docker images
- [ ] Rotation schedule documented with owners and tested end-to-end
- [ ] No secrets in logs (verified by grep test)
- [ ] Exposed secrets are revoked immediately, not just deleted from Git

---

## Sources

- Master checklist: [[security]] §4.
- Deep reference: [[02 Secrets Management]].
- Tools: gitleaks, trufflehog, HashiCorp Vault, AWS Secrets Manager, External Secrets Operator, SOPS, Sealed Secrets.
- Templates: [[Secrets-Management]] note (if exists), [[Authentication-Standard]] template.
