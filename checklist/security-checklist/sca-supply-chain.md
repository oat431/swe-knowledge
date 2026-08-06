# SCA & Supply Chain Security Checklist

> **Software Composition Analysis (SCA)** scans your dependencies (libraries, packages, base images) against known vulnerability databases (CVE, GHSA, OSV). **Supply chain security** goes further: provenance, signing, registries, and the CI/CD pipeline that delivers artifacts.
> The master checklist ([[security]]) §5 says "SCA in CI, SBOM generated." This file is *how* to set up the scanners, generate SBOMs, sign artifacts, and govern dependencies.
> Deep references: → [[02 Dependency & Supply Chain]].
> Last updated: 2026-08-07

---

## 1. What & Why

- [ ] **Understand the threat** — Modern apps are 80–90% open-source dependencies by line count. A single vulnerable transitive dependency is enough to compromise the whole application (Log4Shell, xz-utils, left-pad). You don't own most of your code — but you own all of the risk.
- [ ] **SCA = known vulnerabilities** — SCA compares your dependency tree against CVE/GHSA/OSV databases. It answers: "Do I have a version with a known bug?"
- [ ] **Supply chain = trust + integrity** — Who built this artifact? Is it what they claim? Can I verify it wasn't tampered? Provenance, signing, and registry trust answer these.
- [ ] **SCA ≠ SAST** — SAST reads your code for patterns. SCA reads your lockfile for versions. Both are needed.

---

## 2. Dependency Scanning (SCA)

### Tool Selection

| Tool | Type | Best For |
|---|---|---|
| **Dependabot** | GitHub-native | PRs that bump dependencies automatically |
| **Renovate** | Multi-platform | Configurable update schedules, monorepo support |
| **Trivy** | Scanner | Container images, filesystems, IaC, SBOM |
| **Snyk** | Commercial | Dashboard, monitoring, fix PRs, container + IaC |
| **Grype** | Scanner | Anchore's scanner, pairs with syft SBOM |
| **OSV-Scanner** | Scanner | Google's scanner, uses OSV database |

- [ ] **Automated dependency bot** — Dependabot or Renovate opens PRs when a new version is available. Configure for security updates (immediate) vs. version updates (weekly/monthly schedule).
  ```yaml
  # .github/dependabot.yml
  version: 2
  updates:
    - package-ecosystem: "npm"
      directory: "/"
      schedule:
        interval: "weekly"
    - package-ecosystem: "docker"
      directory: "/"
      schedule:
        interval: "weekly"
  ```
- [ ] **PR-blocking scanner in CI** — Trivy/Grype/OSV-Scanner on every PR. Fail on Critical/High with no waiver:
  ```yaml
  # .github/workflows/trivy.yml
  - name: Trivy FS Scan
    uses: aquasecurity/trivy-action@master
    with:
      scan-type: fs
      scan-ref: .
      severity: CRITICAL,HIGH
      exit-code: 1  # Fail the job
  ```
- [ ] **Container image scanning** — Trivy on the built image:
  ```bash
  trivy image myapp:latest --severity CRITICAL,HIGH --exit-code 1
  ```

### Lockfile Management

- [ ] **Lockfiles committed** — `package-lock.json`, `yarn.lock`, `go.sum`, `poetry.lock`, `Cargo.lock`. These pin exact versions including transitive deps.
- [ ] **Base images pinned by digest** — Not `node:20` (mutable tag), but `node:20@sha256:abc123...`. Tags can be moved by the registry; digests are immutable.
- [ ] **No floating `latest`** — `latest` means "whatever the registry has right now." It can change without your knowledge. Pin to a specific version or digest.

### Dependency Review in PRs

- [ ] **Dependency Review Action** — GitHub-native, shows what changed in the dependency graph on every PR:
  ```yaml
  - uses: actions/dependency-review-action@v4
    with:
      fail-on-severity: moderate
  ```
- [ ] **Lockfile diffs reviewed** — When a lockfile changes, review what was added/changed. Look for: unfamiliar package names (typosquatting), recent publish dates, low download counts, lookalike author names.

---

## 3. SBOM (Software Bill of Materials)

- [ ] **SBOM generated for every artifact** — Every container image, binary, and release package gets an SBOM. It's the ingredient list.
- [ ] **Tool: syft** —
  ```bash
  # Generate SBOM from a container image
  syft myapp:latest -o spdx-json > sbom.spdx.json

  # Generate SBOM from a directory
  syft dir:. -o cyclonedx-json > sbom.cdx.json
  ```
- [ ] **SBOM format** — SPDX or CycloneDX. Both are standard; CycloneDX is more security-focused, SPDX is more license-focused.
- [ ] **SBOM stored with the artifact** — The SBOM travels with the image/release. When a new CVE drops, you query your SBOMs: "Which artifacts contain log4j-core 2.14.0?"
- [ ] **SBOM scanned** — Feed the SBOM to Grype/Trivy for vulnerability matching: `grype sbom:./sbom.cdx.json`

---

## 4. Artifact Signing & Verification

- [ ] **Cosign signs the artifact** — Sigstore's cosign signs container images using OIDC-based keyless signing:
  ```bash
  # Sign (in CI, with OIDC)
  cosign sign --yes ghcr.io/myorg/myapp:v1.2.3

  # Verify (at deploy)
  cosign verify --certificate-identity \
    https://github.com/myorg/myapp/.github/workflows/deploy.yml@refs/tags/v1.2.3 \
    --certificate-oidc-issuer https://token.actions.githubusercontent.com \
    ghcr.io/myorg/myapp:v1.2.3
  ```
- [ ] **Signature verified at deploy** — The deployment pipeline verifies the signature before pulling the image. An unsigned or tampered image is rejected.
- [ ] **Provenance attestation (SLSA)** — Beyond signing the image, attest *how it was built*: which repo, which commit, which CI workflow, which builder. SLSA Level 2+ requires provenance.
- [ ] **Signing key protected** — If using key-based signing (not keyless), the key is in KMS/HSM, never in CI environment variables.

---

## 5. Registry Trust

- [ ] **Approved registries only** — GHCR, ECR, GCR, Artifactory. No `docker pull` from random Docker Hub accounts.
- [ ] **Private registry scans on push** — If using a private registry (Harbor, ECR + scan-on-push), every image is scanned before it's available for deploy.
- [ ] **Dependency confusion prevented** — Internal package names are scoped (e.g., `@mycompany/package-name`) or reserved on public registries. Packages resolved from the intended registry only. Attackers squat public names that exist privately → [[02 Dependency & Supply Chain]].
- [ ] **Registry access controlled** — Service accounts with read-only pull access. Push access restricted to CI. Admin access reviewed quarterly.

---

## 6. IaC Security Scanning

- [ ] **IaC scanner in CI** — Checkov, tfsec, or KICS on Terraform/CloudFormation/Kubernetes manifests:
  ```bash
  # Checkov
  checkov -d terraform/ --framework terraform

  # tfsec (now part of Trivy)
  trivy config terraform/

  # KICS
  kics scan -p terraform/ -o kics-report.json
  ```
- [ ] **Common misconfigurations caught** — Public S3 buckets, security group 0.0.0.0/0 on sensitive ports, IAM `*` permissions, unencrypted RDS, public ECR repos, privileged Kubernetes pods.
- [ ] **Policy as code (advanced)** — OPA Gatekeeper / Kyverno admission controllers enforce policies in the cluster at deploy time. Catches drift that CI scanning misses.

---

## 7. Vulnerability Remediation SLAs

- [ ] **SLAs documented and tracked** —

  | Severity | Fix Target | Notes |
  |---|---|---|
  | Critical | ≤ 7 days | Immediate if exploited (check KEV catalog) |
  | High | ≤ 30 days | |
  | Medium | ≤ 90 days | |
  | Low | Backlog priority | |

- [ ] **KEV catalog checked** — CISA's Known Exploited Vulnerabilities catalog. A CVE in KEV means it's being actively exploited — treat as Critical regardless of CVSS.
- [ ] **Exceptions require sign-off** — If a vuln can't be fixed within SLA, a risk acceptance is documented with: owner, compensating control, expiry date, review trigger.
- [ ] **SLA compliance reported** — Metrics show: % of findings fixed within SLA, aging of open findings, trend over time. Audience: engineering leadership, not just security.

---

## 8. Anti-Patterns to Avoid

- [ ] **SCA without remediation SLAs** — The scanner runs, findings accumulate, nobody fixes them. The dashboard looks red, but nothing happens. SLAs force decisions.
- [ ] **Ignoring transitive dependencies** — Your code depends on `request`, which depends on `tough-cookie`, which has a CVE. You didn't directly install `tough-cookie`, but you own the risk. SCA must scan the full tree.
- [ ] **Floating tags** — `node:20`, `python:3.12`, `alpine:latest`. These tags move. Your next build gets a different base image without your knowledge. Pin by digest.
- [ ] **No SBOM** — When Log4Shell 2.0 drops, you have no way to query "which of our services use this library?" You're manually grepping lockfiles across 50 repos. SBOM is the inventory.
- [ ] **Unsigned artifacts** — Any image in the registry can be pulled by deploy. A compromised registry or CI pipeline can push a malicious image. Signing + verification proves provenance.
- [ ] **Renovate/Dependabot PRs ignored** — The bot opens 200 PRs; the team ignores them; the backlog grows. Configure grouping (batch minor/patch), auto-merge for low-risk updates, and a schedule to avoid alert fatigue.
- [ ] **Trusting all public packages** — `npm install random-package` from an unvetted author. Typosquatting, malicious packages, and account takeovers are real supply chain vectors.

---

## Quick Sanity Check

- [ ] Dependabot/Renovate enabled, opening PRs for security updates
- [ ] Trivy/Grype/OSV-Scanner runs on every PR, fails on Critical/High
- [ ] Lockfiles committed; base images pinned by digest
- [ ] SBOM (syft) generated for every container image and stored with the artifact
- [ ] Container images signed (cosign), signatures verified at deploy
- [ ] IaC scanner (Checkov/tfsec/KICS) runs in CI on Terraform/K8s manifests
- [ ] Remediation SLAs documented (Critical ≤ 7d, High ≤ 30d)
- [ ] KEV catalog checked; exploited vulns treated as immediate
- [ ] Exceptions documented with owner, compensating control, expiry

---

## Sources

- Master checklist: [[security]] §5.
- Deep reference: [[02 Dependency & Supply Chain]].
- Tools: Dependabot, Renovate, Trivy, syft, Grype, cosign, Snyk, Checkov, tfsec, KICS.
- Standards: SLSA (Supply-chain Levels for Software Artifacts), NTIA SBOM minimum elements, CISA KEV catalog.
