# tiny mchwa — Project Overview

> 🐜 Ant (Swahili) — Todo List Microservice for Homelab

---

## Purpose

A shared todo list service for the homelab ecosystem. Serves as:
- **Main platform** — personal todo lists (grocery, daily tasks)
- **Blog service** — draft ideas, content planning
- **Cookbook service** — grocery lists for recipes

One service, multiple consumers. No duplicate todo implementations across services.

---

## Tech Stack

| Layer | Choice | Notes |
|-------|--------|-------|
| Backend | Go + Fiber v3 | Go 1.22+ required |
| Frontend | React 19 + DaisyUI | MVP = backend only |
| Database | PostgreSQL | Primary data store |
| Auth | Keycloak (OAuth) | Shared homelab auth |
| Gateway | Flowero Gate | API gateway, routing |
| Testing | testify | Test-first development |

---

## Architecture Decisions

| Decision | Choice | Reason |
|----------|--------|--------|
| API Protocol | REST only | KISS — one protocol, one thing to maintain |
| Todolist status | Computed | Derived from tasks, not stored in DB |
| Task ownership | Via todolist | Tasks inherit `ownedBy` and `sourceService` from parent |
| Data isolation | Per-user | Locked to Keycloak UUID, no sharing |
| Service discovery | Via gateway | All requests through Flowero Gate |

---

## Development Order

1. **Spec** — Write requirements (this folder)
2. **Tests** — Define test cases from spec
3. **Code** — Implement to pass tests

---

## MVP Scope

**IN SCOPE:**
- CRUD todolists
- Pagination
- Filtering (status, sourceService, title)

**OUT OF SCOPE (later):**
- CRUD tasks
- Task status → todolist status computation
- Frontend
- Cross-service integration

---

## Related Documents

- [[01 Data Model]]
- [[02 API Spec]]
- [[03 Status Logic]]
- [[Web app for homelab]]
