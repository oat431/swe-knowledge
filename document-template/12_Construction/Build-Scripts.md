---
document_type: Build Scripts
version: "2.0"
status: Draft
author: "[Technical Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [build-scripts, ci-cd, automation, gradle, npm, go, docker, swebok]
standard_ref:
  - SWEBOK v4 — Construction
  - 12-Factor App (Build, Release, Run)
---

# Build Scripts

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Active]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Documents the build pipeline — scripts, stages, and automation that transform source code into deployable artifacts. The build pipeline **IS the quality gate**. If it's not in the pipeline, it doesn't happen.

## 2. Build Pipeline (Universal)

Every project follows the same pipeline stages, regardless of language:

```mermaid
flowchart LR
    CODE[Source Code] --> LINT[Lint / Format]
    LINT --> COMPILE[Compile / Type-Check]
    COMPILE --> TEST[Test]
    TEST --> PACKAGE[Package Artifact]
    PACKAGE --> IMAGE[Docker Image]
    IMAGE --> PUSH[Push to Registry]
    PUSH --> DEPLOY[Deploy]

    style CODE fill:#2196F3,color:#fff
    style LINT fill:#FF9800,color:#fff
    style COMPILE fill:#FF9800,color:#fff
    style TEST fill:#4CAF50,color:#fff
    style PACKAGE fill:#9C27B0,color:#fff
    style IMAGE fill:#607D8B,color:#fff
    style DEPLOY fill:#4CAF50,color:#fff
```

## 3. Tech-Stack Variants

### 3.1 Java / Gradle / Spring Boot

| Command | Purpose | Output |
|---------|---------|--------|
| `./gradlew build` | Full pipeline: compile → test → bootJar | `build/libs/*.jar` |
| `./gradlew build -x test` | Quick assemble, skip tests | JAR only |
| `./gradlew compileJava` | Syntax check only | `build/classes/` |
| `./gradlew test` | Run all tests (JUnit 5) | `build/reports/tests/` |
| `./gradlew bootJar` | Package fat JAR | `build/libs/*.jar` |
| `./gradlew bootRun` | Start embedded server locally | Service on port |
| `./gradlew clean` | Remove build artifacts | Deletes `build/` |
| `./gradlew dependencies` | Print dependency tree | stdout |
| `./gradlew dependencyCheckAnalyze` | OWASP vulnerability scan | `build/reports/dependency-check/` |

**Gradle Wrapper:** Always use `./gradlew` (committed to VCS). Never rely on system Gradle.

```bash
# Generate wrapper (once per project)
gradle wrapper --gradle-version 9.5.1
```

**Key files committed to VCS:**
| File | Purpose |
|------|---------|
| `build.gradle` | Dependencies, plugins, tasks |
| `settings.gradle` | Project name, module structure |
| `gradlew` / `gradlew.bat` | Wrapper scripts |
| `gradle/wrapper/gradle-wrapper.jar` | Wrapper binary |
| `gradle/wrapper/gradle-wrapper.properties` | Wrapper version pin |

**Build phases (from `./gradlew build`):**
```
1. compileJava         — Compile main sources
2. processResources    — Copy config files
3. classes             — Done compiling
4. compileTestJava     — Compile test sources
5. testClasses         — Done compiling tests
6. test                — Run JUnit 5 tests
7. bootJar             — Package fat JAR (embedded server)
8. jar                 — Package plain JAR (no server)
9. check               — All verification done
10. build              — BUILD SUCCESSFUL
```

### 3.2 Node.js / TypeScript / npm

| Command | Purpose |
|---------|---------|
| `npm run dev` | Start development server with hot reload |
| `npm run build` | Compile TypeScript → JavaScript |
| `npm test` | Run all tests (Jest / Vitest) |
| `npm run test:unit` | Unit tests only |
| `npm run test:integration` | Integration tests |
| `npm run lint` | Run ESLint |
| `npm run format` | Run Prettier |
| `npm run type-check` | Run `tsc --noEmit` |

**Key files committed to VCS:** `package.json`, `package-lock.json`, `tsconfig.json`

### 3.3 Go

| Command | Purpose |
|---------|---------|
| `go build -o bin/app ./cmd/app` | Compile binary |
| `go test ./...` | Run all tests |
| `go test -v ./...` | Detailed test output |
| `go test -cover ./...` | Coverage report |
| `golangci-lint run` | Run linter |
| `gofmt -w .` | Auto-format |
| `go mod tidy` | Clean up dependencies |

**Key files committed to VCS:** `go.mod`, `go.sum`

## 4. Docker Multi-Stage Build

Universal pattern — separate build and runtime stages for smaller images:

```dockerfile
# ---- Stage 1: Build ----
FROM eclipse-temurin:25-jdk-noble AS build    # Java
# FROM node:20-alpine AS build                 # Node.js
# FROM golang:1.23-alpine AS build             # Go
WORKDIR /app

# Copy dependency manifests first (cache layer)
COPY build.gradle settings.gradle gradlew ./   # Java
# COPY package*.json ./                         # Node.js
# COPY go.mod go.sum ./                         # Go
RUN ./gradlew dependencies --no-daemon -q || true

# Copy source and build
COPY src/ src/
RUN ./gradlew bootJar --no-daemon

# ---- Stage 2: Runtime ----
FROM eclipse-temurin:25-jre-noble
# FROM node:20-alpine                           # Node.js
# FROM alpine:3.20                              # Go
WORKDIR /app

# Create non-root user
RUN groupadd -r app && useradd -r -g app app

COPY --from=build /app/build/libs/*.jar app.jar
EXPOSE 8080
USER app

ENTRYPOINT ["java", "-Xms128m", "-Xmx192m", "-XX:+UseZGC", "-jar", "app.jar"]
```

**Key principles:**
- Dependency manifests copied first → cached unless they change
- Source copied last → fast incremental builds
- Non-root user in runtime stage
- Explicit JVM flags (`-Xms`, `-Xmx`)
- Multi-stage → final image is small (no JDK, no build tools in runtime)

## 5. CI/CD Pipeline (GitHub Actions — Multi-Stack)

```yaml
name: CI/CD

on:
  push:
    branches: [main, dev]
  pull_request:
    branches: [main]

jobs:
  # ---- Java Service ----
  build-java:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          java-version: '25'
          distribution: 'temurin'
      - run: ./gradlew build --no-daemon
      - run: docker build -t ${{ github.repository }}:${{ github.sha }} .

  # ---- Node.js Service ----
  build-node:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm run lint && npm test -- --coverage

  # ---- Go Service ----
  build-go:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-go@v5
        with:
          go-version: '1.23'
      - run: go test ./...
      - run: go build -o bin/app ./cmd/app
```

## 6. Build Artifacts

| Artifact | Format | Location | Retention |
|---------|--------|---------|----------|
| Fat JAR (Spring Boot) | `.jar` | `build/libs/` | Per build |
| Plain JAR | `.jar` | `build/libs/` | Per build |
| Go Binary | Executable | `bin/` or `dist/` | Per release |
| Docker Image | Docker | Container Registry | Tagged by SHA |
| Test Report | HTML/JUnit XML | `build/reports/tests/` | CI artifacts |
| Coverage Report | HTML/XML | `build/reports/coverage/` | CI artifacts |

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[README-Developer-Guide]] | Build instructions for developers |
| [[Dependency-Manifest]] | Dependencies being built |
| [[Commit-Messages-Changelog]] | Commit standards |
| [[Deployment-Plan]] | What happens after build |

---

> **Template Standard:** Based on SWEBOK v4, 12-Factor App (Build, Release, Run)
> **Usage:** Pick the variant matching your tech stack. The pipeline stages are universal — lint → compile → test → package → image → deploy. The Gradle wrapper is your build entry point for Java; never use system Gradle. Automate everything in CI.
