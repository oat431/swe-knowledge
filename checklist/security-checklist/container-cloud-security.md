# Container & Cloud Security Checklist

> **Container and cloud security** covers the runtime environment: Docker images, Kubernetes policies, cloud IAM, network controls, and continuous posture monitoring.
> The master checklist ([[security]]) §7 + §13 say "harden containers, lock down cloud." This file is *how* to harden Dockerfiles, enforce K8s policies, and monitor cloud posture.
> Deep references: → [[03 Container & Cloud Security]].
> Last updated: 2026-08-07

---

## 1. Docker Image Hardening

### Dockerfile Security

- [ ] **Non-root user** — The container runs as a non-root user. `USER` directive in Dockerfile or `runAsUser` in K8s manifest:
  ```dockerfile
  RUN addgroup -S app && adduser -S app -G app
  USER app
  ```
- [ ] **Read-only root filesystem** — Where possible, mount the root FS read-only. Writable paths (`/tmp`, cache dirs) are explicit `tmpfs` mounts:
  ```yaml
  # Kubernetes pod spec
  securityContext:
    readOnlyRootFilesystem: true
  ```
- [ ] **No privileged mode** — `--privileged` gives the container host-level access. Never in production.
  ```yaml
  securityContext:
    privileged: false
    allowPrivilegeEscalation: false
  ```
- [ ] **Capabilities dropped** — Drop all Linux capabilities, add back only what's needed:
  ```yaml
  securityContext:
    runAsNonRoot: true
    capabilities:
      drop: [ALL]
      # add: [NET_BIND_SERVICE]  # only if binding port <1024
  ```
- [ ] **Minimal base image** — `alpine`, `distroless`, or `scratch` over full OS images. Fewer packages = smaller attack surface.
- [ ] **Multi-stage build** — Build tools and compilers in the build stage; only the runtime binary in the final image. No `gcc`, no `make`, no shell in the prod image.
- [ ] **No shell in prod image** — Distroless images have no shell. An attacker who gets RCE can't spawn `/bin/sh`.
- [ ] **`.dockerignore` configured** — Prevents `.git`, `.env`, test files, and local secrets from leaking into the build context.

### Image Scanning

- [ ] **Scan on build** — Trivy/Grype scans the image before it's pushed to the registry:
  ```bash
  trivy image --severity CRITICAL,HIGH --exit-code 1 myapp:latest
  ```
- [ ] **Scan on push (registry)** — Private registry (Harbor, ECR) scans the image again on push. Defense in depth — a vulnerability introduced after the local scan is caught.
- [ ] **Scan on schedule** — New CVEs are published daily. An image that was clean at build time may have a new finding next week. Re-scan scheduled images.

---

## 2. Kubernetes Security

### Pod Security

- [ ] **Pod Security Standards enforced** — Replace deprecated Pod Security Policies with Pod Security Standards (PSS). Enforce `restricted` profile for production:
  ```yaml
  # Namespace label enforces restricted PSS
  pod-security.kubernetes.io/enforce: restricted
  pod-security.kubernetes.io/audit: restricted
  pod-security.kubernetes.io/warn: restricted
  ```
- [ ] **No privilege escalation** — `allowPrivilegeEscalation: false` on every pod. Prevents `setuid` attacks inside the container.
- [ ] **`runAsNonRoot: true`** — Pod spec rejects containers that try to run as UID 0.
- [ ] **Seccomp profile** — `RuntimeDefault` or `Localhost` profile restricts the syscalls a container can make.
- [ ] **Network policies** — Default-deny ingress + egress; only required connections allowed. A compromised pod can't reach every other pod by default.

### RBAC

- [ ] **RBAC follows least privilege** — Service accounts get only the permissions they need. No `cluster-admin` for app pods. Audit with `kubectl auth can-i --list --as=system:serviceaccount:default:myapp`.
- [ ] **Service accounts per workload** — Each workload has its own service account. Not the `default` SA (which may have accumulated permissions).
- [ ] **Token auto-mount disabled if unused** — `automountServiceAccountToken: false` for pods that don't talk to the K8s API. Prevents token theft → cluster pivoting.

### Admission Controllers (Policy as Code)

- [ ] **OPA Gatekeeper or Kyverno** — Enforce policies at admission time (before a pod is created). Catches misconfiguration that CI scanning misses:
  ```yaml
  # Kyverno policy: disallow privileged containers
  apiVersion: kyverno.io/v1
  kind: ClusterPolicy
  metadata:
    name: disallow-privileged
  spec:
    rules:
      - name: privileged-containers
        match:
          resources:
            kinds: ["Pod"]
        validate:
          message: "Privileged containers are not allowed"
          pattern:
            spec:
              containers:
                - securityContext:
                    privileged: "false"
  ```
- [ ] **Common policies enforced** — Disallow privileged pods, require resource limits, require `runAsNonRoot`, disallow `latest` tag, enforce image registry allowlist.

---

## 3. Cloud IAM (AWS / GCP / Azure)

- [ ] **No root/admin for routine work** — Root account locked away (MFA, strong password, no API keys). Routine work uses IAM roles/users with scoped permissions.
- [ ] **Least privilege** — IAM policies grant only required actions on required resources. No `Action: "*", Resource: "*"`. Audit with IAM Access Analyzer (AWS) / Policy Analyzer (GCP).
- [ ] **Short-lived credentials** — STS / Workload Identity / OIDC federation over long-lived access keys. No `AWS_ACCESS_KEY_ID` in CI environment variables.
- [ ] **Access keys rotated or removed** — Long-lived keys rotated every 90 days, or removed entirely in favor of role assumption.
- [ ] **Permission boundaries** — For multi-tenant or multi-team environments, permissions boundaries cap the maximum permissions a role can have even if the policy grants more.
- [ ] **Cloud audit logging enabled** — CloudTrail (AWS), Cloud Audit Logs (GCP), Activity Log (Azure) enabled on all regions, shipped to central tamper-resistant storage.
- [ ] **Unused permissions reviewed** — IAM Access Analyzer generates least-privilege policies based on actual usage. Review quarterly.

---

## 4. Network Security

- [ ] **Network segmentation** — Public-facing load balancers in public subnets; data stores and internal services in private subnets. Strict security groups between tiers.
- [ ] **Security groups default-deny** — Start with deny-all, add only required rules. No `0.0.0.0/0` on database ports (3306, 5432, 27017, 6379, etc.).
- [ ] **Egress filtering** — Outbound traffic restricted to known destinations. NAT gateway + egress allowlist, or service mesh policies (Istio AuthorizationPolicy). Prevents data exfiltration and SSRF pivoting.
- [ ] **No internet-exposed databases** — RDS, ElastiCache, MongoDB Atlas: public access off. Security group restricted to the app tier only.
- [ ] **mTLS for service-to-service (if mesh)** — Istio/Linkerd mTLS between internal services. Automatic certificate rotation.

---

## 5. Cloud Posture Monitoring (CSPM)

- [ ] **CSPM tool running** — AWS Security Hub, GCP Security Command Center, Azure Defender for Cloud, or third-party (Wiz, Orca, Prowler). Continuous scanning for misconfigurations.
- [ ] **CIS Benchmarks checked** — Center for Internet Security benchmarks for cloud accounts and Kubernetes clusters. CSPM tools map to CIS controls.
- [ ] **Findings routed to owners** — CSPM findings don't sit in a dashboard. They route to the team that owns the resource, with SLAs (same as vulnerability remediation SLAs).
- [ ] **Drift detection** — Terraform state monitored against live infrastructure. Manual changes (console clicks) create drift; CSPM catches the security impact of drift.
- [ ] **Public resource alerts** — Immediate alert when a storage bucket, database, or service is made public. Often accidental; sometimes the first sign of a breach.

---

## 6. Runtime Security

- [ ] **Falco or runtime detection** — Falco monitors system calls at runtime. Alerts on: shell spawn in a container, unexpected network connection, file read in `/etc/shadow`, privilege escalation.
- [ ] **Immutable infrastructure** — Nodes are replaceable, not patched in place. Auto-scaling group replaces nodes with fresh images. Reduces the window for persistent backdoors.
- [ ] **Secrets at runtime** — Secrets mounted via CSI driver (External Secrets Operator, Vault Agent). Not in environment variables readable via `/proc/1/environ`.
- [ ] **Container drift detection** — If a container modifies its filesystem at runtime (writes outside expected paths), alert. May indicate compromise.

---

## 7. Anti-Patterns to Avoid

- [ ] **Running as root** — The most common Dockerfile mistake. If the container is compromised, the attacker has root inside. Drop to a non-root user.
- [ ] **`--privileged` in production** — Equivalent to root access on the host. Only for debugging, never prod.
- [ ] **No network policies** — Every pod can talk to every other pod by default. A compromised pod is a launching point to the entire cluster.
- [ ] **`default` service account with broad permissions** — Every pod in the namespace inherits those permissions. One compromise → cluster-level access.
- [ ] **Public databases** — An RDS instance with public access = `true` and `0.0.0.0/0` security group is an open door. Most common cloud breach cause.
- [ ] **Floating tags in production** — `myapp:latest` in a K8s manifest. Next deployment pulls a different image. Pin by digest in prod.
- [ ] **No image scanning** — Base images carry hundreds of CVEs. Unscanned images ship those CVEs to production.
- [ ] **CSPM findings ignored** — Dashboard turns red, nobody looks. The misconfiguration that enables a breach was visible for months.

---

## Quick Sanity Check

- [ ] Containers run as non-root, read-only root FS, capabilities dropped
- [ ] Docker image scanned (Trivy/Grype) before push and on schedule
- [ ] K8s: Pod Security Standards (restricted), network policies (default-deny)
- [ ] RBAC least privilege; per-workload service accounts; token auto-mount off when unused
- [ ] Cloud IAM: no root for routine work, least privilege, short-lived credentials
- [ ] Cloud audit logging enabled, shipped to tamper-resistant storage
- [ ] Network: private subnets for data stores, default-deny security groups, egress filtering
- [ ] CSPM tool running; findings routed to owners with SLAs
- [ ] Runtime detection (Falco) for shell spawn, unexpected connections, privilege escalation

---

## Sources

- Master checklist: [[security]] §7 + §13.
- Deep reference: [[03 Container & Cloud Security]].
- Tools: Trivy, Falco, OPA Gatekeeper, Kyverno, Prowler, AWS Security Hub, Cloud Custodian.
- Standards: CIS Benchmarks (AWS/GCP/Azure/Kubernetes), NIST SP 800-190 (Container Security), Pod Security Standards.
