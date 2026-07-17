---
tags:
  - architecture
  - microservices
  - security
  - programming
---

# 06 Zero Trust & Network Security

In a microservice world, the network perimeter is dead. Zero trust means every request is authenticated and authorized regardless of origin.

## Traditional vs Zero Trust

| Aspect | Perimeter Security | Zero Trust |
|--------|-------------------|------------|
| Trust model | Trust inside, block outside | Never trust, always verify |
| Access control | Network-based (firewall) | Identity-based per request |
| Privilege | Broad access once inside | Least privilege, scoped tokens |
| Breach response | Detect at boundary | Assume breach, limit blast radius |
| Verification | One-time at entry | Continuous verification |

## Kubernetes Network Policies

By default, all pods can talk to all pods. This is unacceptable in zero trust.

**Principles:**
- Default deny all ingress/egress
- Explicit allow only what's needed
- Namespace isolation as a boundary

```yaml
# Default deny all ingress in a namespace
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-ingress
  namespace: payments
spec:
  podSelector: {}
  policyTypes:
    - Ingress

---
# Allow only order-service to reach payment-service
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-order-to-payment
  namespace: payments
spec:
  podSelector:
    matchLabels:
      app: payment-service
  ingress:
    - from:
        - namespaceSelector:
            matchLabels:
              name: orders
          podSelector:
            matchLabels:
              app: order-service
      ports:
        - port: 8080
```

## mTLS (Mutual TLS)

Both client and server present certificates — mutual authentication at the transport layer.

```
┌──────────────┐                    ┌──────────────┐
│  Service A   │───── mTLS ────────▶│  Service B   │
│  (client)    │                    │  (server)    │
│  presents    │◀── verify cert ───│  presents    │
│  cert A      │─── verify cert ──▶│  cert B      │
└──────────────┘                    └──────────────┘
        ▲                                   ▲
        └────── Certs issued by shared CA ──┘
```

- Service mesh (Istio/Linkerd) handles mTLS transparently — no app code changes
- Certificates auto-rotated (short-lived, ~24h)
- SPIFFE identity standard for workload identity

## Pod Security

Never run containers as root. Minimize the attack surface.

```yaml
securityContext:
  runAsNonRoot: true
  runAsUser: 1000
  readOnlyRootFilesystem: true
  allowPrivilegeEscalation: false
  capabilities:
    drop:
      - ALL
```

**Rules:**
- No privileged pods (`privileged: false`)
- Read-only filesystem — write only to mounted volumes
- Drop all Linux capabilities, add back only what's needed
- Use Pod Security Admission (restricted profile)

## Secrets Management

| Practice | Tool |
|----------|------|
| Central secret store | HashiCorp Vault |
| K8s-native sync | External Secrets Operator |
| Auto-rotation | Vault dynamic secrets |
| Never in git | `.gitignore`, pre-commit hooks, git-secrets |

**Flow:** Vault → External Secrets Operator → K8s Secret → Pod env/volume mount

Secrets should be short-lived, auto-rotated, and audited. Never hardcode. Never commit.

## Supply Chain Security

| Layer | Tool | Purpose |
|-------|------|---------|
| Image signing | cosign (Sigstore) | Verify image provenance |
| SBOM | Syft, Trivy | Know what's in your images |
| Vulnerability scan | Trivy, Grype | Catch CVEs before deploy |
| Admission control | Kyverno, OPA Gatekeeper | Block unsigned/vulnerable images |

Pipeline: Build → Scan → Sign → Store → Verify at admission → Deploy

## Implementation Summary

| Component | Provides |
|-----------|----------|
| Service Mesh (Istio/Linkerd) | mTLS, traffic encryption, identity |
| NetworkPolicy (Calico/Cilium) | L3/L4 network isolation |
| OPA / Kyverno | Policy enforcement, admission control |
| Vault + ESO | Secrets lifecycle management |
| Trivy + cosign | Supply chain verification |
| Pod Security Admission | Runtime security baseline |

## Sources

- [NIST SP 800-207 — Zero Trust Architecture](https://csrc.nist.gov/publications/detail/sp/800-207/final)
- [Kubernetes Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/)
- [Istio Security — mTLS](https://istio.io/latest/docs/concepts/security/)
- [Sigstore / cosign](https://docs.sigstore.dev/)
