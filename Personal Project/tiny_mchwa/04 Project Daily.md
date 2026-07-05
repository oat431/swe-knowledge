---
tags: [tiny-mchwa, daily]
date: 2026-07-04
---

# tiny mchwa — Daily Log

## 2026-07-04 (Day 1) 🐜

### What happened
- Project kickoff — spec-driven development with AI
- Reviewed spec docs (00-03), checklist, DB schema
- Built MVP backend (Go + Fiber v3 + sqlx + PostgreSQL)
- Built MVP frontend (React 19 + DaisyUI + Bun + Vite)

### API (tiny-mchwa-api)
- ✅ Todolists CRUD (create, list, get, update, delete)
- ✅ Tasks CRUD (create, list, get, update, delete)
- ✅ Pagination + filtering (status, sourceService, title)
- ✅ Computed todolist status from tasks
- ✅ Standardized `{data, error, meta}` response format
- ✅ Middleware: RequestID, Recover, Helmet, CORS, Logger
- ✅ Struct validation (go-playground/validator)
- ✅ Graceful shutdown
- ✅ godotenv for .env loading
- ✅ 12 handler tests passing (mock-based, no DB needed)
- ✅ Services refactored to interfaces for testability

### Web (tiny-mchwa-web)
- ✅ Todolists page (list, create, edit, delete, pagination)
- ✅ Todolist detail page (tasks list, create, edit, delete, status toggle)
- ✅ TanStack Query for data fetching
- ✅ URL params for pagination + filters
- ✅ 404 page, ErrorBoundary, page titles
- ✅ Niramit font
- ✅ Vite proxy to API at localhost:8005

### PRs & Issues

**API PRs:**
- #1 feat: mvp polish (validation, CORS, lint, Air) — merged
- #6 feat: tasks CRUD + computed status — merged
- #7 feat: add tests + refactor services to interfaces — merged

**API Issues:**
- #2 production readiness (migrations, Dockerfile, health check) — pending
- #5 auth + observability (JWT, rate limiter, Swagger, OTel) — pending

**Web PRs:**
- #6 404 page, error handling, page titles — merged
- #7 URL params for pagination + filters — merged
- #8 TanStack Query for data fetching — merged

**Web Issues:**
- #1 Vitest + React Testing Library tests — pending
- #4 accessibility improvements — pending

### Config
- API port: 8005
- Web port: 3300
- DB: `tiny_mchwa` on PostgreSQL (todolists + tasks tables)
- Auth: hardcoded UUID for MVP (Keycloak later)

### Next steps
- API: production readiness (#2), then auth (#5)
- Web: tests (#1), accessibility (#4)
- Frontend: axios migration when Keycloak lands

---

## Related
- [[00 Project Overview]]
- [[01 Data Model]]
- [[02 API Spec]]
- [[03 Status Logic]]
