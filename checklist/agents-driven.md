# .agents Project Structure Guide

> How to organize full-stack projects with AI agent assistance.
> Last updated: 2026-06-02

---

## Two Separate Concepts

### 1. Agent Workspace (Private — `~/.openclaw/workspace/`)

The agent's "brain" — persona, memory, instructions. Does NOT contain your project code.

```
~/.openclaw/workspace/
├── AGENTS.md       ← operating instructions & rules
├── SOUL.md         ← persona, tone, boundaries
├── USER.md         ← about your human
├── IDENTITY.md     ← agent name, vibe, emoji
├── TOOLS.md        ← local tool conventions (SSH hosts, camera names, etc.)
├── HEARTBEAT.md    ← periodic check checklist
├── MEMORY.md       ← curated long-term memory (decisions, preferences)
├── memory/         ← daily raw logs
│   └── 2026-06-02.md
├── skills/         ← workspace-level skills (highest priority)
└── canvas/         ← Canvas UI files
```

Keep this in a **private git repo**. Never commit secrets.

### 2. Project `.agents/` (Committed to Project Repo)

Project-specific instructions for AI agents working on this codebase.

```
my-fullstack-app/
├── .agents/
│   ├── AGENTS.md       ← "when working on THIS project, remember..."
│   └── CONTEXT.md      ← architecture overview, conventions, tech decisions
├── backend/
├── frontend/
└── ...
```

| Layer | Purpose | Visibility |
|---|---|---|
| **Workspace** | Agent identity, memory, behavior | Private, per-machine |
| **.agents/** | Project rules, architecture, conventions | Committed to project repo |

---

## Recommended Full-Stack Project Layout

```
my-fullstack-app/
│
├── .agents/                          ← AI agent project context
│   ├── AGENTS.md                     ← rules & conventions for this project
│   └── CONTEXT.md                    ← architecture, tech stack, ADR summary
│
├── backend/                          ← backend service(s)
│   ├── cmd/
│   │   └── server/main.go           ← entry point
│   ├── internal/
│   │   ├── handler/                  ← HTTP/gRPC handlers (thin)
│   │   ├── service/                  ← business logic
│   │   ├── repository/              ← data access
│   │   ├── domain/                   ← domain models (framework-free)
│   │   └── middleware/
│   ├── migrations/                   ← versioned DB migrations
│   ├── go.mod
│   └── Dockerfile
│
├── frontend/
│   ├── web/                          ← React / Next.js or Vue / Nuxt
│   │   ├── app/                      ← App Router (Next) or pages/ (Nuxt)
│   │   ├── components/
│   │   ├── composables/              ← Vue composables (if Vue)
│   │   ├── hooks/                    ← React hooks (if React)
│   │   ├── lib/
│   │   └── package.json
│   │
│   └── mobile/                       ← React Native / Flutter (optional)
│       └── ...
│
├── shared/                           ← shared code: types, validators, constants
│   ├── types/                        ← generated from OpenAPI or hand-written
│   ├── validators/                   ← Zod/Yup schemas (shared by FE + BE)
│   └── constants/
│
├── packages/                         ← monorepo shared packages (alternative)
│   ├── shared-types/                 ← @myapp/types
│   ├── validators/                   ← @myapp/validators
│   └── config-eslint/               ← @myapp/eslint-config
│
├── batch/                            ← background jobs, ETL, workers, scripts
│   ├── jobs/
│   └── scripts/
│
├── infra/                            ← infrastructure as code
│   ├── docker/
│   ├── terraform/
│   ├── k8s/
│   └── ansible/
│
├── docker-compose.yml
├── .gitignore
└── README.md
```

---

## Monorepo Alternative (pnpm workspaces / Turborepo)

Better when frontend + backend share types and tooling heavily:

```
my-fullstack-app/
├── .agents/
│   ├── AGENTS.md
│   └── CONTEXT.md
│
├── apps/
│   ├── api/                          ← backend
│   ├── web/                          ← frontend web
│   ├── mobile/                       ← mobile app
│   └── worker/                       ← batch/background jobs
│
├── packages/
│   ├── shared-types/                 ← TypeScript types
│   ├── validators/                   ← Zod schemas
│   ├── config-eslint/               ← shared lint config
│   └── config-ts/                    ← shared tsconfig
│
├── pnpm-workspace.yaml
├── turbo.json
└── package.json
```

---

## `.agents/AGENTS.md` Template

```markdown
# AGENTS.md — [Project Name]

## Stack
- Backend: Go 1.23, PostgreSQL, Redis, Clean Architecture
- Frontend: React 19 + Next.js App Router, TanStack Query, Tailwind, shadcn/ui
- Shared: TypeScript types, Zod validators
- Infra: Docker Compose (dev), K8s (prod), Terraform

## Architecture
- Backend: handler → service → repository pattern
- Frontend: Server Components by default, "use client" only for interactivity
- API: REST with OpenAPI spec. Client generated from spec.
- Auth: JWT (15min) + refresh token via HTTP-only cookies

## Conventions
- Commits: conventional commits (feat:, fix:, chore:)
- PRs: CI must be green before merge
- Testing: Vitest (unit), Playwright (E2E), Go testing (backend)
- Forms: React Hook Form + Zod (schemas in shared/validators)
- DB: golang-migrate, migrations must be backward-compatible

## Red Lines
- Never commit .env files or secrets
- API keys in vault, never in code or config
- No `dangerouslySetInnerHTML` without DOMPurify
- DB migrations must be reversible
```

---

## `.agents/CONTEXT.md` Template

```markdown
# CONTEXT.md — Architecture & Decisions

## Why we chose this stack
- [ ] Go for backend: team expertise, performance, deploy simplicity
- [ ] Next.js App Router: SSR + RSC, streaming, built-in optimizations
- [ ] PostgreSQL: relational data, mature tooling, strong consistency
- [ ] shadcn/ui: headless, customizable, no npm dependency lock-in

## Key Architecture Decisions (ADRs)
- [ ] ADR-001: UUIDv7 for primary keys (time-ordered, distributed-friendly)
- [ ] ADR-002: Cursor-based pagination for all list endpoints
- [ ] ADR-003: TanStack Query for server state (not Redux)
- [ ] ADR-004: HTTP-only cookies for auth (not localStorage)

## External Dependencies
| Service | Purpose | Criticality |
|---|---|---|
| PostgreSQL | Primary DB | Critical |
| Redis | Cache + session store | High |
| S3-compatible | File storage | Medium |
| SendGrid | Transactional email | Low |
```

---

## Workspace vs Project: Quick Reference

| | Agent Workspace | Project .agents/ |
|---|---|---|
| **Location** | `~/.openclaw/workspace/` | `<project>/.agents/` |
| **Contains** | Persona, memory, skills | Project rules, context |
| **Git repo** | Private (personal) | Committed with project |
| **Shares with team?** | ❌ No | ✅ Yes |
| **Loaded when?** | Every session startup | When agent works on that project |
