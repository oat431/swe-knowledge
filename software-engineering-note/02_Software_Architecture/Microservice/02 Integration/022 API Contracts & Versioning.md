---
tags:
  - architecture
  - microservices
  - programming
---

# 02 API Contracts & Versioning

In microservices, service boundaries are API contracts. Breaking a contract breaks every consumer. Contract-first development and explicit versioning prevent silent breakage.

---

## Contract-First Development

Define the interface **BEFORE** implementation. The contract is the agreement between teams.

1. Write the schema (OpenAPI, Protobuf, AsyncAPI)
2. Review with consumers
3. Generate server stubs and client SDKs
4. Implement behind the contract

> The contract is the product. Code is the implementation detail.

---

## Contract Formats

| Format | Use Case | Pros | Cons |
|--------|----------|------|------|
| **OpenAPI** | REST APIs | Human-readable, rich tooling, wide adoption | Verbose for complex models |
| **Protobuf** | gRPC services | Strongly typed, compact binary, code-gen | Less human-readable, steeper learning curve |
| **AsyncAPI** | Event-driven / messaging | Mirrors OpenAPI for async, channel-aware | Younger ecosystem, fewer tools |
| **JSON Schema** | Payload validation | Language-agnostic, composable | No endpoint/transport semantics |

---

## Versioning Strategies

| Strategy | Example | Pros | Cons |
|----------|---------|------|------|
| **URL path** | `/v1/orders` | Explicit, easy routing, cacheable | URL pollution |
| **Header** | `Accept-Version: v2` | Clean URLs | Hidden, harder to test in browser |
| **Content negotiation** | `Accept: application/vnd.api.v2+json` | RESTful purist | Complex, poor discoverability |

**Recommendation:** URL path versioning for simplicity. Everyone can see the version. Route at the gateway level.

---

## Backward Compatibility Rules

### ✅ Safe Changes (Additive)

- New **optional** field in response
- New endpoint
- New enum value (if consumers ignore unknown)
- Wider input validation (accept more)

### ❌ Breaking Changes

- Removing a field
- Renaming a field
- Changing a field's type
- Making an optional field required
- Narrowing input validation

### The Rule

> Only make **additive** changes. If you must break → create a **new version**.

---

## Consumer-Driven Contracts (Pact)

Consumer defines what it expects. Provider verifies it can fulfill those expectations. Breaking changes caught in **CI before deploy**.

```
┌──────────┐         ┌───────────┐         ┌──────────┐
│ Consumer │──────▶  │ Pact File │  ◀──────│ Provider │
│  (test)  │ writes  │ (contract)│ verifies │  (test)  │
└──────────┘         └───────────┘         └──────────┘
```

**Flow:**
1. Consumer writes a test defining expected request/response
2. Pact generates a contract file (JSON)
3. Provider runs verification against the pact file
4. Both run in CI → broken contract = failed build

---

## Schema Evolution for Events

Events are **immutable facts**. You can't ask consumers to replay history.

| Rule | Reason |
|------|--------|
| Never delete fields | Old consumers will break |
| Add fields as optional | Forward compatibility |
| Use **Avro + Schema Registry** | Enforces compatibility checks on publish |
| Default values for new fields | Backward compatibility |

**Compatibility modes (Schema Registry):**
- `BACKWARD` — new schema can read old data
- `FORWARD` — old schema can read new data
- `FULL` — both directions safe

---

## Deprecation Strategy

Never surprise consumers. Follow the lifecycle:

```
Announce → Sunset Header → Migration Period → Remove
```

1. **Announce** — document deprecation, notify consumers
2. **Sunset Header** — `Sunset: Sat, 01 Mar 2025 00:00:00 GMT` in responses
3. **Migration Period** — run old + new versions in parallel, provide migration guide
4. **Remove** — only after confirming zero traffic on old version

> Postel's Law: Be conservative in what you send, liberal in what you accept.

---

## Sources

- [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Pact Documentation](https://docs.pact.io/)
- [Postel's Law (Robustness Principle)](https://en.wikipedia.org/wiki/Robustness_principle)
