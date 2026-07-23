---
document_type: Coding Standards / Style Guide
version: "2.0"
status: Draft
author: "[Technical Lead]"
created: "[YYYY-MM-DD]"
last_updated: "[YYYY-MM-DD]"
project_name: "[Project Name]"
project_id: "[Project-ID]"
classification: "Internal / Confidential"
tags: [coding-standards, style-guide, java, typescript, go, linting, swebok]
standard_ref:
  - SWEBOK v4 — Construction
  - Google Java Style Guide
  - Effective Go
  - Airbnb JavaScript Style Guide
---

# Coding Standards / Style Guide

> **Project:** [Project Name]
> **Version:** [X.Y] | **Status:** [Draft | Under Review | Approved | Baselined]
> **Last Updated:** [YYYY-MM-DD]

---

## 1. Purpose

> Coding standards ensure consistent, readable, maintainable code across all services — regardless of language or framework. Standards are **enforced**, not suggested. If linting fails in CI, the PR doesn't merge.

## 2. Language-Specific Standards

Pick the section matching your service's language.

### 2.1 Java / Spring Boot

| Aspect | Standard | Tool |
|--------|---------|------|
| Java Version | 25 LTS | `java.toolchain` in Gradle |
| Build System | Gradle (wrapper committed) | `./gradlew` |
| Style Guide | Google Java Style Guide | Checkstyle |
| Formatting | 4 spaces, 120 char line width | Spotless (Gradle plugin) |
| Naming — Classes | PascalCase | `FlowerodiscoveryApplication` |
| Naming — Methods/Vars | camelCase | `restTemplate`, `getForEntity()` |
| Naming — Constants | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT` |
| Naming — Packages | lowercase, reversed domain | `panomete.flowerodiscovery` |
| Null Handling | `Optional<T>` for returns, `@Nullable` for params | Checker Framework |
| Lombok | **Avoid** — explicit code > magic | — |
| Records | Prefer over POJOs for DTOs | Java 16+ `record` |
| Dependency Injection | Constructor injection (not field) | `@Autowired` on constructor |
| Exception Handling | Catch specific, not `Exception` | Custom exceptions extend `RuntimeException` |
| Imports | No wildcard imports | `import ... .RestTemplate` not `import ... .*` |

**Example — Good vs Bad:**

```java
// ✅ Good — constructor injection, record DTO, clear names
@SpringBootApplication
@EnableEurekaServer
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}

// ✅ Good — clear test names, AssertJ assertions
@Test
void eurekaDashboardIsReachable() {
    ResponseEntity<String> response = restTemplate.getForEntity(
            url("/"), String.class);
    assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
    assertThat(response.getBody()).contains("Instances currently registered with Eureka");
}

// ❌ Bad — field injection, vague names
@Autowired
private EurekaServer e;   // field injection + meaningless name
```

**Gradle conventions:**
- Use `build.gradle` (Groovy DSL) for consistency
- Pin versions via BOM (`dependencyManagement` block) for Spring Cloud
- Java toolchain, not `sourceCompatibility`
- Test task uses `useJUnitPlatform()`

**application.yaml conventions:**
- YAML over `.properties` — hierarchical, readable
- kebab-case for all keys and values (`flowero-discover`, not `floweroDiscover`)
- Group by domain: spring → server → eureka → management → logging
- Every non-obvious value has an inline comment explaining WHY

### 2.2 TypeScript / Node.js

| Aspect | Standard | Tool |
|--------|---------|------|
| TypeScript | Strict mode (`strict: true`) | `tsc --noEmit` |
| Runtime | Node.js 20 LTS | `.nvmrc` |
| Style Guide | Airbnb or Standard | ESLint |
| Formatting | 2 spaces, single quotes, trailing commas | Prettier |
| Naming — Files | kebab-case | `request-service.ts` |
| Naming — Classes | PascalCase | `RequestService` |
| Naming — Vars/Functions | camelCase | `createRequest()` |
| Naming — Constants | UPPER_SNAKE | `MAX_RETRY_COUNT` |
| Null Handling | Optional chaining `?.`, nullish coalescing `??` | — |
| `any` | **Never** — use `unknown` or generics | ESLint: `no-explicit-any` |
| Return Types | Always explicit | ESLint: `explicit-function-return-type` |
| Interfaces vs Types | Interface for objects, Type for unions | — |
| Error Handling | Custom error classes, never `throw "string"` | — |

**Example:**

```typescript
// ✅ Good
interface CreateRequestDTO {
  type: RequestType;
  amount: number;
  description?: string;
}

async function createRequest(dto: CreateRequestDTO): Promise<Request> {
  const validated = validateDTO(dto);
  const request = await requestRepository.save(validated);
  return request;
}

// ❌ Bad
async function createRequest(dto: any): Promise<any> {
  return await db.query("INSERT INTO requests...");
}
```

### 2.3 Go

| Aspect | Standard | Tool |
|--------|---------|------|
| Go Version | Latest stable (1.23+) | `go.mod` |
| Style Guide | [Effective Go](https://go.dev/doc/effective_go) | `gofmt`, `golangci-lint` |
| Formatting | `gofmt` default (tabs, no semicolons) | `gofmt -w .` |
| Naming — Packages | lowercase, single word | `package handlers` |
| Naming — Exported | PascalCase | `func NewService()` |
| Naming — Unexported | camelCase | `func validateInput()` |
| Naming — Acronyms | All caps or all lower | `HTTPServer` or `httpServer` |
| Error Handling | Always check, wrap with context | `fmt.Errorf("...: %w", err)` |
| Interfaces | Small (1-3 methods), defined at consumer | — |
| Project Layout | [Standard Go Project Layout](https://github.com/golang-standards/project-layout) | `cmd/`, `internal/`, `pkg/` |

**Example:**

```go
// ✅ Good
func (s *Service) CreatePost(ctx context.Context, req CreatePostRequest) (*Post, error) {
    if err := req.Validate(); err != nil {
        return nil, fmt.Errorf("validating request: %w", err)
    }
    post, err := s.repo.Insert(ctx, req.toModel())
    if err != nil {
        return nil, fmt.Errorf("inserting post: %w", err)
    }
    return post, nil
}

// ❌ Bad
func (s *Service) CreatePost(req interface{}) interface{} {
    return s.repo.Insert(req)  // no error handling, no types, no context
}
```

## 3. Universal Rules (All Languages)

| Rule | Rationale |
|------|----------|
| **No hardcoded secrets** | Credentials, API keys → env vars or secret manager |
| **No commented-out code** | Delete it — git history preserves it |
| **No magic numbers** | Extract to named constants |
| **Functions do one thing** | If you need "and" in the function name, split it |
| **Tests cover happy path + edge cases + error paths** | Error paths are as important as success paths |
| **Meaningful names** | `createRequest()` not `cr()`, `retryCount` not `r` |
| **Early returns** | Flatten nested ifs — return early on error |
| **Log at the right level** | INFO for business events, DEBUG for details, ERROR for failures |

## 4. Project Structure Conventions

### Java / Spring Boot

```
src/
├── main/
│   ├── java/[domain]/[service]/
│   │   └── Application.java       # @SpringBootApplication entry point
│   └── resources/
│       ├── application.yml         # Default config
│       ├── application-dev.yml     # Dev overrides
│       └── application-prod.yml    # Prod overrides
└── test/
    └── java/[domain]/[service]/
        └── ApplicationTests.java   # Integration tests
```

### TypeScript / Node.js

```
src/
├── app/           # Use cases, commands, queries
├── domain/        # Entities, value objects, events
├── infra/         # Persistence, messaging, external APIs
└── interfaces/    # HTTP handlers, middleware, DTOs
```

### Go

```
cmd/[service]/     # Main entry point
internal/          # Private application code
  ├── handlers/    # HTTP handlers
  ├── service/     # Business logic
  └── repository/  # Data access
pkg/               # Shared libraries (public API)
```

## 5. Linting & CI Enforcement

| Language | Linter | Formatter | CI Command |
|----------|--------|-----------|-----------|
| Java | Checkstyle / SpotBugs | Spotless | `./gradlew check` |
| TypeScript | ESLint | Prettier | `npm run lint && npm run format --check` |
| Go | golangci-lint | gofmt | `golangci-lint run && gofmt -d .` |

All linting **must pass in CI** before merge. Configure as pre-commit hooks:
- **Node.js:** Husky + lint-staged
- **Go:** pre-commit hook running `gofmt`
- **Java:** Spotless Gradle plugin with `spotlessCheck`

---

## Related Documents

| Document | Relationship |
|----------|-------------|
| [[README-Developer-Guide]] | Project setup and contribution |
| [[Code-Review-Records]] | Standards enforced during review |
| [[Secure-Coding-Guidelines]] | Security-specific coding rules |

---

> **Template Standard:** Based on SWEBOK v4, Google Java Style Guide, Effective Go, Airbnb JS Style Guide
> **Usage:** Pick the language section matching your service. Standards are **enforced in CI** — linting failure = no merge. Standards without enforcement are just suggestions. The universal rules apply to every language equally.
