---
document_type: README / Developer Guide
version: "2.0"
status: Draft
author: "[Technical Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [readme, developer-guide, onboarding, java, typescript, go, swebok]
standard_ref:
  - SWEBOK v4 — Construction
  - 12-Factor App Methodology
---

# README / Developer Guide

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | Baselined]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> The README is the **front door**. A developer must be able to clone, build, and run the service in < 5 minutes using these instructions. If they can't, the README needs work.

## 2. Project Header Template

Every README starts with a metadata table — first impression tells you what this is, what it runs on, and where it lives.

```markdown
# Service Name 🏷️

> One-line description — what it does, who it's for, why it exists.
>
> **Platform:** [Parent Platform Name] | **Phase:** [1 | 2 | N]

| Aspect | Detail |
|--------|--------|
| **Service Type** | [Foundation | Business | Infrastructure] |
| **Technology** | [Spring Cloud Eureka | Keycloak | Spring Cloud Gateway | etc.] |
| **Stack** | [Java 25 / Spring Boot 4.1 | Go | TypeScript / Node.js] |
| **Ports** | [8999 (API) · 3999 (Dashboard)] |
| **Domain** | `[service].example.com` (via reverse proxy) |
| **Database** | [None (in-memory) | PostgreSQL 18 | MongoDB 8] |
| **Mode** | [Standalone | Clustered] |
| **Dependencies** | [None | PostgreSQL | Valkey | etc.] |
```

## 3. Architecture Section

ASCII art diagram — visual overview in 30 seconds:

```markdown
## Architecture

\```
                    ┌─ REST API ─── :XXXX ─── [purpose]
  [service-name] ───┤
                    └─ Dashboard ─── :YYYY ─── [domain]
\```

### How Services Use It

1. Step one — what happens
2. Step two — what happens next
3. Step three — result
```

## 4. Quick Start — Tech-Stack Variants

Pick ONE variant based on the project's tech stack.

### Variant A: Java / Gradle / Spring Boot

```markdown
## Quick Start

### Prerequisites

- **JDK [version]** ([Eclipse Temurin](https://adoptium.net/) recommended)
- **Gradle** (wrapper included — `./gradlew`)

### Build & Run

\```bash
# Clone
git clone <repo-url>
cd <project>

# Build (compile + test + package)
./gradlew build

# Run locally
./gradlew bootRun
\```

The service starts on **port [XXXX]**.

### Verify

\```bash
# Health check
curl http://localhost:[XXXX]/actuator/health
# Expected: {"status":"UP"}
\```

### Docker

\```bash
# Build image
docker build -t [service-name] .

# Run container
docker run -p XXXX:XXXX [service-name]
\```
```

**Key conventions:**
- Use `./gradlew` (wrapper committed to VCS), never system Gradle
- `build.gradle` uses Java toolchain (not `sourceCompatibility`)
- Spring Cloud BOM in `dependencyManagement` block
- Test task uses `useJUnitPlatform()`

### Variant B: Node.js / TypeScript / npm

```markdown
## Quick Start

### Prerequisites

- Node.js v20+
- npm

### Install & Run

\```bash
npm install
cp .env.example .env
npm run dev
\```

### Verify

\```bash
curl http://localhost:[XXXX]/health
\```
```

### Variant C: Go

```markdown
## Quick Start

### Prerequisites

- Go [version]+

### Build & Run

\```bash
go mod download
go build -o bin/[service-name] ./cmd/[service-name]
./bin/[service-name]
\```

### Verify

\```bash
curl http://localhost:[XXXX]/health
\```
```

## 5. Configuration Reference

Document every non-obvious setting with its **rationale** — not just what it is, but WHY that value:

```markdown
## Configuration

Key settings in `[application.yml | .env | config.yaml]`:

| Property | Value | Why |
|----------|-------|-----|
| [prop] | [value] | [rationale — why this value, not another] |
```

## 6. API Reference

Link to the full spec, then list the most-used endpoints:

```markdown
## API Reference

See the full [[API-Specification]].

| Endpoint | Method | Purpose |
|----------|--------|---------|
| [/path] | [GET] | [description] |
| [/path] | [POST] | [description] |
| [/health] | [GET] | [Health check] |
```

## 7. Related Services

For microservice platforms, show how this service fits:

```markdown
## Related Services

| Service | How It Uses This Service |
|---------|-------------------------|
| [Service A] | [e.g., resolves routes through this registry] |
| [Service B] | [e.g., registers health for monitoring] |
```

## 8. Testing

One command to run tests, plus what they cover:

```markdown
## Testing

\```bash
# Java/Gradle
./gradlew test

# Node.js
npm test

# Go
go test ./...
\```

Tests cover:
- ✅ [test category 1]
- ✅ [test category 2]
```

## 9. Design Decisions

Link to ADRs — the README is not the place for full decision records, but it should point to them:

```markdown
## Design Decisions

| ADR | Decision |
|-----|----------|
| [ADR-ID] | [One-line summary] |

Full ADRs: [[Architecture-Decision-Records]]
```

## 10. Project Structure

Show the directory tree for quick orientation:

```markdown
## Project Structure

\```
project/
├── build.gradle / package.json / go.mod
├── Dockerfile
├── src/
│   ├── main/ ...
│   └── test/ ...
└── README.md
\```
```

## 11. README Sections Checklist

| Section | Required | Purpose |
|---------|:---:|---------|
| Project header (name + one-liner + metadata table) | ✅ | First impression — what is this? |
| Architecture diagram | ✅ | Visual overview in 30 seconds |
| Quick Start — prerequisites + build + run + verify | ✅ | Clone → running in < 5 min |
| Docker instructions | ✅ | Containerized deployment |
| Configuration reference table (with rationale) | ✅ | What every knob does and why |
| API reference / link to spec | ✅ | Contract for consumers |
| Related services | 🟡 | How this fits into the platform |
| Testing (one command) | ✅ | How to run tests |
| Design decisions (ADR links) | 🟡 | Why, not just what |
| Project structure tree | 🟡 | File map for new contributors |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[Coding-Standards]] | Code style guidelines |
| [[API-Specification]] | API reference template |
| [[Test-Plan]] | Testing strategy |
| [[Deployment-Plan]] | Deployment procedures |
| [[Architecture-Decision-Records]] | Design rationale |

---

> **Template Standard:** Based on SWEBOK v4, 12-Factor App
> **Usage:** Pick the tech-stack variant (Java/Gradle, Node.js/npm, or Go) for the Quick Start section. Every project README should follow this structure. The key insight: **a README is a contract** — if a developer can't set up in 5 minutes, fix the README. Configuration values need rationale, not just documentation.
