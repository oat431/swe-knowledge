---
title: Identity and Access Architecture
role: Software Architect
capability_area: Security Architecture
topic: Identity and Access Architecture
status: complete
created: 2026-08-22
updated: 2026-08-22
tags:
  - career-path
  - software-architect
  - identity
  - oauth
  - authorization-models
---

# Identity and Access Architecture

> **Core skill:** The architect designs how identity is established, propagated, and verified across every component — authentication architecture, authorization models, and service identity — as one system-wide scheme rather than per-service improvisation.

## Why This Matters

Identity is the control plane of a modern system. Every request carries an identity claim somewhere, and the architecture decides where that claim is proven, what it means, how it travels between services, and what it is allowed to do at each stop. Almost every lateral-movement incident is an identity architecture failure: a token that was valid anywhere in the network, a service that acted with more authority than its caller, a revocation that never propagated.

The choices are structural and they constrain everything built afterward. Single sign-on versus per-application accounts, one identity provider versus per-domain providers, tokens versus server sessions, centralized policy versus embedded rules, shared secrets versus workload identity — each decision reaches into every call path. Retrofitting an identity architecture is expensive because there is no part of the system it does not touch.

Authorization models carry the same weight. Role-based access control is simple and auditable but coarse; attribute-based access control is expressive but its policies become software in their own right; relationship-based access control answers questions the other two cannot, at the cost of a relationship store to maintain. The model determines which access questions the system can answer at all, and which it can only approximate.

## Authentication Architectures

| Approach | Flow Shape | Strength | Fits When | Cost |
|----------|------------|----------|-----------|------|
| **Local accounts** | Credentials live and are verified in each application | Simple; no shared dependency | Small internal tools with no shared user base | Credential sprawl; no central revocation |
| **Single sign-on** | One identity provider authenticates; applications trust its assertions | Central revocation, one strong authentication policy | Workforce applications | Identity provider availability becomes critical |
| **Federated identity** | External identity providers trusted via metadata; users stay in their home directory | No duplicate user management across organizations | Partner and B2B access | Trust configuration and contract management |
| **OAuth2 and OIDC** | Delegated authorization with identity layer; tokens carry scopes and claims | Standard, scoped, works for users and services | APIs, third-party access, mobile clients | Token lifecycle complexity; implementation pitfalls are well documented and common |
| **Passwordless and WebAuthn** | Hardware-bound credentials replace shared secrets | Phishing resistance | High-value user populations | Enrollment and recovery flows |

## Authorization Models Compared

| Model | Unit of Decision | Strength | Weakness | Fit |
|-------|------------------|----------|----------|-----|
| **RBAC** | Role mapped to permissions | Simple, auditable, explainable to auditors | Role explosion as organizations and resources multiply | Stable structures with coarse resources |
| **ABAC** | Attributes of subject, resource, action, and context | Fine-grained, context-aware, no role explosion | Policy complexity, testing burden, harder to explain | Regulated, dynamic, or conditional access rules |
| **ReBAC** | Relationships in a graph of subjects and objects | Answers questions like who can see this because of that | Requires a relationship store and traversal at scale | Collaborative products, hierarchies, sharing models |
| **Policy as code** | External decision points evaluate versioned policies | Testable, reviewable, consistent across services | Runtime dependency; policy language learning curve | Many services needing the same decisions |

## Service Identity

Services are principals too, and they need identity that is verifiable, scoped, and short-lived. Shared secrets in configuration are the anti-pattern the alternatives exist to remove.

| Mechanism | Identity Form | Rotation | Fit |
|-----------|---------------|----------|-----|
| Static shared secrets | Shared string per service pair | Manual, rare, risky | Legacy only; migrate away |
| Mutual TLS with managed CA | Certificate bound to workload | Automated via certificate authority | Service meshes; broad adoption |
| SPIFFE and SPIRE style identity | Workload identity document with verifiable name | Short-lived, automatic | Multi-platform, multi-cloud fleets |
| Cloud workload identity | Platform-issued role credential per workload | Automatic, minutes-scale lifetime | Cloud-native services and managed resources |

## Identity Propagation Across Services

How identity travels is a decision made once and inherited everywhere. The propagation model narrows authority as it moves: a user token presented to a gateway is exchanged for a scoped token per downstream call, or re-verified at each hop with audience restriction. Whatever the mechanism, two rules hold — identity must not widen as it travels, and every hop authorizes against local policy rather than inheriting trust. The confused deputy failure appears when one service uses its own broad identity to serve a narrowly entitled caller.

## The Identity Flow

```mermaid
flowchart LR
    ACTOR["User or workload"] --> IDP["Identity provider authenticates"]
    IDP --> TOKEN["Scoped token issued with audience"]
    TOKEN --> SERVICE["Service verifies token at its boundary"]
    SERVICE --> POLICY["Local policy decides access"]
    POLICY --> DATA["Data access with service identity"]
```

## Practical Applications

### Identity Architecture Checklist

- [ ] Every principal type — user, service, workload, third party — has a named identity source
- [ ] Every authentication flow names its token or session lifetime and its revocation mechanism
- [ ] The authorization model is explicit, and each service enforces it consistently
- [ ] Service-to-service calls carry verifiable workload identity with automatic rotation
- [ ] Identity propagation narrows scope at each hop and never widens it

### Identity Architecture Record

```markdown
## Identity Architecture Record — <system>

| Concern | Decision | Rationale | Consequence |
|---------|----------|-----------|-------------|
| User authentication | OIDC via corporate identity provider | Single revocation point; MFA enforced centrally | IdP outage blocks new sessions |
| Service identity | Mutual TLS with managed certificate authority | Short-lived certs with automatic rotation | Certificate authority is a critical dependency |
| Authorization model | RBAC for coarse access; ABAC for resource conditions | Balance explainability and fine-grained control | Policy engine must be versioned and tested |
| Token propagation | Token exchange per hop, audience restricted | Prevents confused deputy across services | Extra hop latency; exchange service availability |
```

## Common Pitfalls

| Pitfall | Why It Is a Problem | Better Approach |
|---------|---------------------|-----------------|
| **Authentication without authorization** | Proves who called, then grants whatever the service can do | Every authenticated call is authorized against local policy |
| **Long-lived bearer tokens** | A stolen token is valid everywhere until it expires — eventually | Short lifetimes, audience restriction, revocation path |
| **Per-service local identity** | Every service invents accounts and flows; policy is inconsistent | One identity provider and one token verification path |
| **Org-chart roles** | Roles mirror reporting lines and multiply without bound | Map roles to capabilities and resources, not people |
| **Confused deputy** | A privileged service performs actions a caller had no right to request | Scoped tokens per call; per-hop authorization |
| **No revocation story** | Access removal takes effect whenever each system happens to notice | Central revocation point with bounded propagation time |

## Success Indicators

- Access reviews can enumerate effective permissions for any principal from the architecture record
- A deprovisioning takes effect across all services within one bounded window
- New services adopt the identity scheme without inventing local variants
- No service-to-service call depends on static shared secrets
- Authorization decisions are logged with the identity and policy that produced them

## Related Topics

- [[02_Trust_Boundaries]]
- [[04_Security_Patterns_in_Architecture]]
- [[07_Compliance_Architecture]]
- [[career-path/08_Security_Engineer/00_overview|Security Engineer]]

## Summary

Identity architecture is the system-wide scheme for proving who is calling, deciding what they may do, and carrying that context safely between services. Authentication approaches map from local accounts to single sign-on, federation, and OAuth2 with OpenID Connect; authorization models trade simplicity against expressiveness across RBAC, ABAC, and ReBAC; service identity replaces shared secrets with short-lived, verifiable workload credentials. The architect owns the scheme and its propagation rules because every call path in the system depends on them.
