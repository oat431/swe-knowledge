# Improved Checklists — Overview

> These are the checklists you actually tick off before shipping. No tutorials. No version-specific trivia. No code examples. Just the items that matter.
>
> For deep knowledge on each topic, follow the `→` links to your vaults.

---

## The Checklists

| Checklist | For | Lines |
|-----------|-----|:-----:|
| [[Release]] | The shipping process: versioning → rollout → verify → rollback | ~45 items |
| [[Security]] | System-wide security: threat model → compliance | ~50 items |
| [[Database]] | The data layer: modeling, migrations, backup, performance | ~45 items |
| [[PostgreSQL]] | PostgreSQL 18: vacuum, PITR, pooling, replication | ~40 items |
| [[MongoDB]] | MongoDB 8: document modeling, replica sets, sharding | ~35 items |
| [[Valkey]] | Valkey/Redis 9: caching patterns, eviction, HA | ~30 items |
| [[QA]] | The quality strategy: test pyramid → mutation → metrics | ~40 items |
| [[AI]] | AI/LLM apps: model selection, RAG, agents, evals, guardrails | ~45 items |
| [[API Launch]] | Any backend service before production | ~45 items |
| [[Microservice Launch]] | Multi-service systems before production | ~35 items |
| [[Frontend Launch]] | Any frontend app before production | ~35 items |

---

## Where the Originals Live

The original checklists in `../` are your **reference manuals** — keep them for deep dives, framework-specific config, and migration guides. These improved versions are what you actually tick.

| Original (Reference) | Improved (Checklist) |
|---------------------|---------------------|
| `api-checklist/api.md` (319 lines) | [[API Launch]] (~45 lines) |
| `microservice-checklist/microservice-infrastructure.md` (292 lines) | [[Microservice Launch]] (~35 lines) |
| `web-checklist/web.md` (306 lines) | [[Frontend Launch]] (~35 lines) |
| `release-checklist/release.md` (170 lines) | [[Release]] (~45 lines) |
| `security-checklist/security.md` (210 lines) | [[Security]] (~50 lines) |
| `database-checklist/database.md` (190 lines) | [[Database]] (~45 lines) |
| `database-checklist/postgresql.md` (190 lines) | [[PostgreSQL]] (~40 lines) |
| `database-checklist/mongodb.md` (180 lines) | [[MongoDB]] (~35 lines) |
| `database-checklist/valkey-redis.md` (165 lines) | [[Valkey]] (~30 lines) |
| `qa-checklist/qa.md` (175 lines) | [[QA]] (~40 lines) |
| `ai-checklist/ai.md` (215 lines) | [[AI]] (~45 lines) |

---

## How to Use

1. Before shipping: open the checklist, tick every box.
2. Unticked item? Follow the `→` link to your vault for the solution.
3. Don't modify the checklist during a launch. Fix the issue, tick it, move on.
4. After launch: add any missed items back into the original reference checklists.

---

## Your Vaults

| Domain | Vault |
|--------|-------|
| API Design, REST, GraphQL, gRPC | [[API Overview]] |
| Database, SQL, NoSQL, Migrations | [[Database Overview]] |
| Microservices, Resilience, Patterns | [[Microservice Overview]] |
| Security, OWASP, Auth, TLS | [[Cybersecurity Overview]] |
| Testing, QA, CI/CD | [[QA Overview]] |
| Networking, DNS, HTTP, TCP | [[Computer Networks Overview]] |
| OS, Processes, Memory, Concurrency | [[Operating Systems Overview]] |
| Clean Code, Architecture | Clean Code / Clean Architecture |
