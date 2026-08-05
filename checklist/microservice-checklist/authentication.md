---
title: Authentication & OAuth2 Checklist
checklist_type: microservice-domain
version: 2.0
status: active
scope: identity, tokens, sessions, and authorization foundations
last_updated: 2026-08-06
---

# Authentication & OAuth2 Checklist

> Tick every box before your auth system hits production. Authentication is the gatekeeper — get it wrong and nothing else matters. Deep reference: [[microservice-infrastructure]] Part 3, [[03 Authentication]].

---

## Identity Provider

- [ ] **Build vs Adopt: NEVER build your own OAuth2/OIDC server** → [[03 Authentication]]
- [ ] Keycloak (self-hosted, free), Auth0/Okta (SaaS), AWS Cognito (AWS-native)
- [ ] One identity provider for all services — one user store, one login experience
- [ ] Auth server is a dedicated service — NOT embedded in an application
- [ ] Realm/tenant per environment (dev, staging, prod) — isolated user data

---

## Grant Types — Which One When

- [ ] **Authorization Code + PKCE** — user-facing apps (SPA, mobile, server-rendered). Most secure → [[microservice-infrastructure]]
- [ ] **Client Credentials** — service-to-service. No user involved. Service authenticates as itself
- [ ] **Refresh Token** — get new access tokens without re-login. Rotate on each use
- [ ] **Device Code** — input-constrained devices (TV, CLI, IoT)
- [ ] ❌ **Implicit** — deprecated. Never use. Tokens in URL = exposed
- [ ] ❌ **Password** — deprecated. Never use. User gives credentials to client

---

## Token Management

- [ ] **Access tokens: short-lived (≤15 min)** — RS256 or ES256 (asymmetric) → [[03 Authentication]]
- [ ] **Refresh tokens: long-lived (days/weeks), rotated on each use**
- [ ] Token rotation: each refresh invalidates old refresh token. Stolen token detected on next legitimate refresh
- [ ] **Token profile documented** — Required claims for this issuer and token type are defined and validated. Validate `iss`, `sub`, `aud`, and `exp` where required; use `iat` and `jti` when the token profile or replay policy requires them.
- [ ] JWT stored securely: httpOnly Secure SameSite cookie (browser), Keychain/Keystore (mobile), memory (SPA access token)
- [ ] NEVER in localStorage — XSS can read it

---

## Token Validation

- [ ] **Every resource server validates JWT independently** — not just trusting gateway headers → [[03 API Security]]
- [ ] Validate: signature, issuer, audience, expiry, not-before
- [ ] **Issuer discovery** — Resource servers use the configured issuer metadata at `/.well-known/openid-configuration` and use its authoritative `jwks_uri`; the JWKS path is not hardcoded as a universal convention.
- [ ] JWKS cached per service — fetch on startup where appropriate, refresh safely on key rotation, retain old keys until tokens signed with them can no longer be valid
- [ ] Reject: expired tokens (401), invalid signature (401), wrong audience (401), insufficient scope (403)
- [ ] `alg: none` rejected by default — do not configure validation to accept it
- [ ] Algorithm and key-type allowlists are explicit; unexpected algorithms, issuers, key IDs, and key types are rejected

---

## Key Management

- [ ] **Signing algorithm policy** — Prefer asymmetric signing when multiple resource servers validate tokens. If HMAC is selected, document key-sharing boundaries, rotation, audience separation, and verifier trust assumptions.
- [ ] Private signing key is held only by the issuer; public verification keys are published through the issuer's configured JWKS endpoint
- [ ] Key rotation: publish new key in JWKS, keep old key until all tokens signed with it expire
- [ ] Keys stored securely — never in source code, never in config files. Vault or K8s Secrets

---

## Architecture Patterns

- [ ] **Pattern chosen: gateway-only, every-service, or introspection** → [[microservice-infrastructure]]
- [ ] If gateway-derived claims are used, the internal propagation mechanism is integrity-protected and accepted only from the trusted gateway; unsigned caller-controlled headers are never the sole identity proof
- [ ] Every service validates JWT independently where the token model supports it — defense in depth and the preferred production default
- [ ] Token introspection (opaque tokens) — only if immediate revocation is critical. Adds per-request latency
- [ ] Pattern documented with reasoning

---

## Scopes & Permissions

- [ ] **Scopes encoded in JWT claims** — `scope: "read:users write:orders"` → [[03 Authorization & Rate Limiting]]
- [ ] Service validates scope before processing — `read:users` required for GET, `write:users` for POST
- [ ] Roles in JWT — `roles: ["admin", "user"]`. Checked at gateway or service level
- [ ] Fine-grained auth (permission per resource) in service, not in token — token carries identity + roles
- [ ] Least privilege: clients request minimum scopes needed

---

## Login Security

- [ ] **Rate limit login endpoint** — 5 attempts per username per 15 minutes → [[03 Authentication]]
- [ ] Account lockout — temporary (15 min) after N failures, not permanent
- [ ] MFA for sensitive operations — admin, billing, user data export
- [ ] Password policy follows the applicable identity standard: minimum length, compromised-password screening, rate limiting, and secure password hashing are documented. For single-factor passwords, align with the current organizational/NIST policy rather than an 8-character default.
- [ ] Brute force protection: rate limit + account lockout + CAPTCHA after threshold

---

## Session & Logout

- [ ] **Session model documented** — Browser sessions, bearer-token APIs, and machine-to-machine clients have explicitly different lifecycle rules where applicable
- [ ] Logout: invalidate the refresh token/session and clear the session cookie where used
- [ ] Single logout (SLO): propagate logout to all services if using sessions
- [ ] Idle timeout: expire session after N minutes of inactivity
- [ ] Absolute timeout: expire session after N hours regardless of activity
- [ ] Access-token revocation strategy documented — short expiry, introspection, denylist, sender-constrained tokens, or another justified mechanism
- [ ] Refresh-token reuse detection and response are tested

---

## Multi-Tenancy

- [ ] **If multi-tenant: isolated realms/tenants** in auth server → [[microservice-infrastructure]]
- [ ] Users from Tenant A cannot see Tenant B data — enforced at auth + application layers
- [ ] Tenant context in JWT claims — `tenant_id: "org-abc"`
- [ ] Social login per tenant — Google, GitHub via auth server identity brokering

---

## Security

- [ ] **Auth server behind TLS** — proper hostname, valid certificate → [[03 Network & TLS]]
- [ ] Auth server database backed up regularly — it IS your user store
- [ ] No secrets in client-side code — client secrets live on the server
- [ ] Redirect URIs whitelisted — exact match, not wildcard. Prevents open redirect
- [ ] PKCE required for all Authorization Code flows — even with client secret
- [ ] CORS on auth server restricted to known origins
- [ ] Login events logged: who, when, from where, success/failure → audit trail
- [ ] Issuer outage and JWKS outage behavior is defined; services fail closed for authorization while avoiding unnecessary restart storms

---

## Testing

- [ ] Valid token → 200. Expired token → 401. Missing token → 401. Invalid signature → 401
- [ ] Wrong scope → 403. Wrong role → 403. Wrong audience → 401
- [ ] Refresh token rotation works — old refresh token invalidated after use
- [ ] Key rotation: old tokens still validate, new tokens signed with new key
- [ ] Rate limit: 6 rapid login attempts → 429 on 6th
- [ ] MFA flow tested end-to-end
- [ ] Password reset flow: token single-use, short expiry, tied to user

---

## Decision, Evidence & Exceptions

- Identity provider and issuer:
- User-facing grant types:
- Service-to-service authentication:
- Token profile and algorithm allowlist:
- JWKS discovery and rotation policy:
- Access-token and refresh-token lifecycle:
- Session/revocation model:
- Evidence links (configuration, tests, rotation drill, audit review):
- N/A items and reason:
- Exceptions, approver, and review/expiry date:

## Quick Sanity Check

- [ ] Auth server is dedicated, not embedded
- [ ] Authorization Code + PKCE for all user-facing flows — never implicit
- [ ] Access tokens ≤15 min, RS256/ES256, all required claims present
- [ ] Refresh tokens rotated on each use
- [ ] Issuer metadata exposes the authoritative `jwks_uri`, and services cache/refresh keys safely
- [ ] Every service validates JWT independently (or documented exception)
- [ ] Login rate-limited + audit logged
- [ ] MFA for admin/sensitive operations
- [ ] Auth server database backed up
- [ ] No secrets in client-side code
- [ ] `alg: none` and all non-allowlisted algorithms rejected — verified in validation config

---

## Sources

- [[03 Authentication]] — JWT, password storage, session security
- [[03 API Security]] — API-level auth patterns
- [[microservice-infrastructure]] — full infrastructure reference (Part 3)
- OAuth2 — https://oauth.net/2/
- OIDC — https://openid.net/connect/
- OAuth 2.0 Security Best Current Practice — https://datatracker.ietf.org/doc/rfc9700/
- JSON Web Token Best Current Practices — https://datatracker.ietf.org/doc/html/rfc8725
- OpenID Connect Discovery — https://openid.net/specs/openid-connect-discovery-1_0.html
- NIST Digital Identity Guidelines — https://pages.nist.gov/800-63-4/sp800-63b/authenticators/
