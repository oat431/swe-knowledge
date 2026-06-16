# Fluffy Mouton — Migration Plan: Monolith Auth → OAuth + Gateway

> **Target:** Migrate Fluffy Mouton (URL Shortener) from monolithic local-JWT authentication to Keycloak OAuth2, routed through Flowero Gate.  
> **Source:** `F:\projects\fluffy-mouton\fluffy-mouton-api`  
> **References:** [Flowero Guard](../Flowero%20Guard.md) · [Flowero Gate](../Flowero%20Gate.md)  
> **Estimated effort:** ~2 hours (execution), ~4-6 hours (with testing and cleanup)  
> **Date:** 2026-06-17

---

## Table of Contents

1. [Current State Audit](#1-current-state-audit)
2. [Target Architecture](#2-target-architecture)
3. [Changes Summary](#3-changes-summary)
4. [Phase 1 — Keycloak Client Registration](#4-phase-1--keycloak-client-registration)
5. [Phase 2 — Replace JWT Middleware](#5-phase-2--replace-jwt-middleware)
6. [Phase 3 — Remove Monolith Auth](#6-phase-3--remove-monolith-auth)
7. [Phase 4 — Adapt Short-Link Controllers](#7-phase-4--adapt-short-link-controllers)
8. [Phase 5 — Configuration & Environment](#8-phase-5--configuration--environment)
9. [Phase 6 — Docker & Networking](#9-phase-6--docker--networking)
10. [Phase 7 — Gateway Route Registration](#10-phase-7--gateway-route-registration)
11. [Phase 8 — Testing & Verification](#11-phase-8--testing--verification)
12. [Phase 9 — Cleanup](#12-phase-9--cleanup)
13. [Risk Assessment](#13-risk-assessment)

---

## 1. Current State Audit

### 1.1 Architecture (Before)

```
Browser → http://localhost:8080/api/v1/short-link/
              │
              ▼
     ┌─────────────────────┐
     │  Fluffy Mouton API  │
     │  (Go Fiber v3)      │
     │                     │
     │  ┌───────────────┐  │
     │  │ JWTMiddleware  │  │  — validates local HS256 JWT
     │  │ → c.Locals     │  │  — extracts auth_id (UUID)
     │  │   ("auth_id")  │  │
     │  └───────┬───────┘  │
     │          │          │
     │  ┌───────▼───────┐  │
     │  │ Auth Module   │  │  — Register, Login, Revoke
     │  │ (local DB)    │  │  — Email verification
     │  │               │  │  — Refresh token mgmt
     │  └───────────────┘  │
     │                     │
     │  ┌───────────────┐  │
     │  │ ShortLink     │  │  — CRUD by auth_id
     │  │ Controller    │  │  — Redirect
     │  └───────────────┘  │
     │                     │
     │  Local PostgreSQL   │
     │  tb_auth            │
     │  tb_refresh_token   │
     │  tb_verify_token    │
     │  tb_short_link      │
     └─────────────────────┘
```

### 1.2 Auth Inventory

| Component | File | Role |
|-----------|------|------|
| JWT generation | `pkg/utils/jwt_util.go` | HS256 sign with `SECRET` env var |
| JWT middleware | `internal/middleware/jwt_middleware.go` | Validates token, sets `c.Locals("auth_id")` |
| Auth controller | `internal/controller/auth_controller.go` | Register, Login, Revoke, Detail, VerifyEmail |
| Auth service | `internal/service/auth_service.go` | Business logic for all auth ops |
| Auth repository | `internal/repository/auth_repository.go` | CRUD on `tb_auth` |
| Auth model | `internal/model/auth.go` | username, email, password, is_verified |
| Refresh token model | `internal/model/refresh_token.go` | Token, expires_at, revoked |
| Verify token model | `internal/model/verify_token.go` | Token, expires_at |
| Refresh token repo | `internal/repository/refresh_token.go` | Save/revoke refresh tokens |
| Verify token repo | `internal/repository/verify_token_repository.go` | Save/find/delete verify tokens |
| Email service | `internal/service/email_service.go` | SMTP for verification emails |
| Password utils | `pkg/utils/password_util.go` | bcrypt hash/compare |

### 1.3 Short-Link Auth Dependency

All short-link endpoints use `JWTMiddleware` and read `c.Locals("auth_id")` as `ownBy`:

```
GET    /api/v1/short-link/           ← auth_id → GetAllShortLinks(ownBy)
POST   /api/v1/short-link/random     ← auth_id → CreateRandomShortLink(url, ownBy)
POST   /api/v1/short-link/custom     ← auth_id → CreateCustomShortLink(url, custom, ownBy)
PUT    /api/v1/short-link/:id        ← auth_id → UpdateShortLink(id, url, ownBy)
DELETE /api/v1/short-link/:id        ← auth_id → DeleteShortLink(id, ownBy)
```

### 1.4 Current Deployment

- Docker Compose with external `postgres-network`
- Exposes port 8080 directly (no gateway)
- CORS: `localhost:3000`, `localhost:5173`
- `.env` variables: `PORT`, `DB_*`, `SECRET`, `SMTP_*`

---

## 2. Target Architecture

### 2.1 Architecture (After)

```
Browser → https://gateway.panomete.com/api/v1/short/**
              │
              ▼
     ┌─────────────────────────────────────┐
     │         Nginx (Edge)              │
     │     TLS termination                 │
     └──────────────┬──────────────────────┘
                    │
                    ▼
     ┌─────────────────────────────────────┐
     │        Flowero Gate                 │
     │   Route: /api/v1/short/**           │
     │          → lb://shortlink-service   │
     │                                     │
     │   Filters:                          │
     │   • StripPrefix=1                   │
     │   • CircuitBreaker                  │
     │   • RequestRateLimiter              │
     │                                     │
     │   GlobalFilters:                    │
     │   • JWT Claim Header Filter         │
     │     → X-User-Id: <keycloak sub>     │
     │     → X-User-Name: <username>       │
     │     → X-User-Roles: ROLE_USER,...   │
     │   • Authorization stripped           │
     └──────────────┬──────────────────────┘
                    │
                    ▼
     ┌─────────────────────────────────────┐
     │    Fluffy Mouton API (Go Fiber)     │
     │                                     │
     │  ┌───────────────────────────┐      │
     │  │ OAuth Middleware           │      │
     │  │ • Path 1: Trust headers    │      │
     │  │   (X-User-Id)              │      │
     │  │ • Path 2: Validate JWT     │      │
     │  │   (JWKS from Keycloak)     │      │
     │  │ → c.Locals("user_id")      │      │
     │  └───────────┬───────────────┘      │
     │              │                      │
     │  ┌───────────▼───────────────┐      │
     │  │ ShortLink Controller       │      │
     │  │ (unchanged business logic) │      │
     │  └───────────────────────────┘      │
     │                                     │
     │  Local PostgreSQL                   │
     │  tb_short_link (only)               │
     │  (tb_auth, tb_refresh_token,        │
     │   tb_verify_token are DELETED)      │
     └─────────────────────────────────────┘
```

### 2.2 Key Design Decisions

| Decision                                                    | Rationale                                                                                                                                                                                      |     |
| ----------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- |
| **Dual-mode middleware (headers + JWKS)**                   | Defense-in-depth. Service validates JWT independently via Keycloak JWKS, but also trusts `X-User-Id` headers from Gateway for convenience. Works even if accidentally exposed without gateway. |     |
| **Remove ALL auth endpoints**                               | Keycloak handles register/login/revoke/verify-email. These endpoints serve no purpose in the new architecture.                                                                                 |     |
| **Keep `ownBy` as UUID**                                    | Keycloak `sub` claim is already a UUID. Compatible with existing `own_by` column type.                                                                                                         |     |
| **Remove `tb_auth`, `tb_refresh_token`, `tb_verify_token`** | User management moves to Keycloak. These tables become dead weight.                                                                                                                            |     |
| **Route path: `/short` not `/short-link`**                  | Aligned with gateway convention (`/api/v1/short/**`). Shorter, cleaner.                                                                                                                        |     |

---

## 3. Changes Summary

| Category | What Changes | Files Affected |
|----------|-------------|----------------|
| **ADD** | Keycloak JWT validation (JWKS) | `pkg/utils/keycloak_jwt.go` (new) |
| **ADD** | OAuth middleware (dual-mode) | `internal/middleware/oauth_middleware.go` (new) |
| **ADD** | Go dependency: `github.com/MicahParks/keyfunc/v3` | `go.mod` |
| **MODIFY** | `main.go` — Initialize JWKS on startup | `cmd/api/main.go` |
| **MODIFY** | Bootstrap container (remove auth deps) | `internal/bootstrap/api_container.go` |
| **MODIFY** | Middleware wiring + remove CORS | `internal/router/main_router.go` |
| **MODIFY** | Short-link router (swap middleware, rename group) | `internal/router/short_link_router.go` |
| **MODIFY** | Short-link controller (`auth_id` → `user_id`) | `internal/controller/short_link_controller.go` |
| **MODIFY** | Docker Compose networking + env vars | `docker-compose.*.yml` |
| **MODIFY** | Environment config | `.env.example`, `.env` |
| **REMOVE** | Entire auth module (15+ files) | See [Section 6.1](#61-files-to-delete) |
| **REMOVE** | Old JWT middleware | `internal/middleware/jwt_middleware.go` |
| **REMOVE** | Old JWT utils | `pkg/utils/jwt_util.go` |
| **REMOVE** | Password utils | `pkg/utils/password_util.go` (and test) |
| **REMOVE** | Email config | `internal/config/email_config.go` |

---

## 4. Phase 1 — Keycloak Client Registration

### 4.1 Create Client in Keycloak

1. Log into Keycloak Admin: `https://auth.panomete.com/admin`
2. Navigate to **flowerogate** realm → **Clients** → **Create**
3. Configure:

```
Client ID:      service-shortlink
Name:           Fluffy Mouton (URL Shortener)
Type:           OpenID Connect
Access Type:    Confidential
Service accounts roles: Enabled  ← Required: enables client credentials grant
```

4. **Save** → go to **Credentials** tab → copy **Client Secret**

### 4.2 Store the Secret

Add to your flowerogate `.env` file:

```bash
# Fluffy Mouton
SHORTLINK_SERVICE_SECRET=<copied-client-secret>
```

### 4.3 Assign Roles

Short-link service currently has no role-based restrictions (any authenticated user can access). If you want admin-only features later, assign `ROLE_ADMIN` to specific users via Keycloak → Users → Role Mapping.

---

## 5. Phase 2 — Replace JWT Middleware

### 5.1 Add Keycloak JWT Validation Library

```bash
cd F:\projects\fluffy-mouton\fluffy-mouton-api
go get github.com/MicahParks/keyfunc/v3
```

### 5.2 Create: `pkg/utils/keycloak_jwt.go`

```go
package utils

import (
	"context"
	"fmt"
	"os"
	"sync"
	"time"

	"github.com/MicahParks/keyfunc/v3"
	"github.com/golang-jwt/jwt/v5"
)

var (
	jwksOnce   sync.Once
	jwksKeySet *keyfunc.Keyfunc
	jwksErr    error
)

// InitJWKS fetches the Keycloak JWKS endpoint once and caches the public keys.
// Must be called once during application startup.
func InitJWKS() error {
	jwksOnce.Do(func() {
		issuerURL := os.Getenv("OAUTH_ISSUER_URI")
		jwksURL := issuerURL + "/protocol/openid-connect/certs"

		jwksKeySet, jwksErr = keyfunc.NewDefaultCtx(
			context.Background(),
			[]string{jwksURL},
			keyfunc.Options{
				RefreshInterval:  time.Hour,       // Refresh keys every hour
				RefreshErrorHandler: func(err error) {
					fmt.Printf("JWKS refresh error: %v\n", err)
				},
			},
		)
	})
	return jwksErr
}

// ValidateKeycloakJWT validates a JWT against the cached Keycloak JWKS key set.
func ValidateKeycloakJWT(ctx context.Context, tokenString string) (*KeycloakClaims, error) {
	if jwksKeySet == nil {
		return nil, fmt.Errorf("JWKS not initialized: call InitJWKS() first")
	}

	token, err := jwt.ParseWithClaims(
		tokenString,
		&KeycloakClaims{},
		jwksKeySet.Keyfunc,
		jwt.WithLeeway(5*time.Second), // Allow 5s clock skew
		jwt.WithValidMethods([]string{"RS256"}),
		jwt.WithIssuer(os.Getenv("OAUTH_ISSUER_URI")),
	)
	if err != nil {
		return nil, fmt.Errorf("jwt validation failed: %w", err)
	}

	claims, ok := token.Claims.(*KeycloakClaims)
	if !ok || !token.Valid {
		return nil, fmt.Errorf("invalid token claims")
	}

	return claims, nil
}

// KeycloakClaims represents the claims we extract from a Keycloak JWT.
type KeycloakClaims struct {
	jwt.RegisteredClaims

	// User identity
	PreferredUsername string `json:"preferred_username"`
	Email             string `json:"email"`
	EmailVerified     bool   `json:"email_verified"`

	// Realm roles
	RealmAccess RealmAccess `json:"realm_access"`
}

// RealmAccess contains realm-level roles.
type RealmAccess struct {
	Roles []string `json:"roles"`
}

// HasRole checks if the JWT contains a specific realm role.
func (c *KeycloakClaims) HasRole(role string) bool {
	for _, r := range c.RealmAccess.Roles {
		if r == role {
			return true
		}
	}
	return false
}
```

### 5.3 Create: `internal/middleware/oauth_middleware.go`

This middleware supports **two auth paths**:
- **Path 1 — Behind Gateway:** Trust `X-User-Id` header (gateway already validated JWT)
- **Path 2 — Direct access:** Validate Keycloak JWT from `Authorization` header via JWKS

```go
package middleware

import (
	"context"
	"oat431/fluffy-mouton/pkg/common"
	"oat431/fluffy-mouton/pkg/utils"
	"strings"

	"github.com/gofiber/fiber/v3"
	"github.com/google/uuid"
)

// OAuthMiddleware validates requests through either:
//  1. Gateway headers (X-User-Id) — when behind Flowero Gate
//  2. Direct Keycloak JWT — for direct/internal service access
//
// Sets c.Locals("user_id") (uuid.UUID), c.Locals("user_name") (string),
// and c.Locals("user_roles") ([]string).
func OAuthMiddleware(c fiber.Ctx) error {
	// ---- Path 1: Behind Gateway (trust X-User-Id header) ----
	userIDHeader := c.Get("X-User-Id")
	if userIDHeader != "" {
		userID, err := uuid.Parse(userIDHeader)
		if err != nil {
			return c.Status(fiber.StatusUnauthorized).JSON(common.ResponseDTO[any]{
				Status: common.ERROR,
				Error: &common.ResponseDTOError{
					HttpCode:  fiber.StatusUnauthorized,
					ErrorCode: "UNAUTHORIZED",
					Message:   "Invalid X-User-Id header format",
				},
			})
		}

		c.Locals("user_id", userID)
		c.Locals("user_name", c.Get("X-User-Name", ""))

		// Parse roles from comma-separated header
		rolesHeader := c.Get("X-User-Roles", "")
		var roles []string
		if rolesHeader != "" {
			roles = strings.Split(rolesHeader, ",")
		}
		c.Locals("user_roles", roles)

		return c.Next()
	}

	// ---- Path 2: Direct JWT validation ----
	authHeader := c.Get("Authorization")
	if authHeader == "" {
		return c.Status(fiber.StatusUnauthorized).JSON(common.ResponseDTO[any]{
			Status: common.ERROR,
			Error: &common.ResponseDTOError{
				HttpCode:  fiber.StatusUnauthorized,
				ErrorCode: "UNAUTHORIZED",
				Message:   "Missing Authorization header or X-User-Id",
			},
		})
	}

	parts := strings.Split(authHeader, " ")
	if len(parts) != 2 || !strings.EqualFold(parts[0], "bearer") {
		return c.Status(fiber.StatusUnauthorized).JSON(common.ResponseDTO[any]{
			Status: common.ERROR,
			Error: &common.ResponseDTOError{
				HttpCode:  fiber.StatusUnauthorized,
				ErrorCode: "UNAUTHORIZED",
				Message:   "Invalid Authorization header format",
			},
		})
	}

	claims, err := utils.ValidateKeycloakJWT(context.Background(), parts[1])
	if err != nil {
		return c.Status(fiber.StatusUnauthorized).JSON(common.ResponseDTO[any]{
			Status: common.ERROR,
			Error: &common.ResponseDTOError{
				HttpCode:  fiber.StatusUnauthorized,
				ErrorCode: "UNAUTHORIZED",
				Message:   "Invalid or expired token: " + err.Error(),
			},
		})
	}

	userID, err := uuid.Parse(claims.Subject)
	if err != nil {
		return c.Status(fiber.StatusUnauthorized).JSON(common.ResponseDTO[any]{
			Status: common.ERROR,
			Error: &common.ResponseDTOError{
				HttpCode:  fiber.StatusUnauthorized,
				ErrorCode: "UNAUTHORIZED",
				Message:   "Invalid user ID in token",
			},
		})
	}

	c.Locals("user_id", userID)
	c.Locals("user_name", claims.PreferredUsername)
	c.Locals("user_roles", claims.RealmAccess.Roles)

	return c.Next()
}

// RequireRole returns a middleware that checks for a specific role.
func RequireRole(role string) fiber.Handler {
	return func(c fiber.Ctx) error {
		roles, ok := c.Locals("user_roles").([]string)
		if !ok {
			return c.Status(fiber.StatusForbidden).JSON(common.ResponseDTO[any]{
				Status: common.ERROR,
				Error: &common.ResponseDTOError{
					HttpCode:  fiber.StatusForbidden,
					ErrorCode: "FORBIDDEN",
					Message:   "Access denied",
				},
			})
		}
		for _, r := range roles {
			if r == role {
				return c.Next()
			}
		}
		return c.Status(fiber.StatusForbidden).JSON(common.ResponseDTO[any]{
			Status: common.ERROR,
			Error: &common.ResponseDTOError{
				HttpCode:  fiber.StatusForbidden,
				ErrorCode: "FORBIDDEN",
				Message:   "Insufficient permissions",
			},
		})
	}
}

// OptionalAuth attempts to extract user info but does NOT reject
// unauthenticated requests. Useful for public endpoints.
func OptionalAuth(c fiber.Ctx) error {
	userIDHeader := c.Get("X-User-Id")
	if userIDHeader != "" {
		if userID, err := uuid.Parse(userIDHeader); err == nil {
			c.Locals("user_id", userID)
		}
	}
	return c.Next()
}
```

### 5.4 Update `cmd/api/main.go` — Initialize JWKS on startup

```go
func main() {
	config.LoadEnvConfig()

	// Initialize Keycloak JWKS key set (fetches public keys on startup)
	if err := utils.InitJWKS(); err != nil {
		log.Printf("WARNING: Failed to initialize JWKS: %v", err)
		log.Println("JWT validation via Authorization header will fail. Gateway header mode will still work.")
	}

	// ... rest of main unchanged ...
}
```

---

## 6. Phase 3 — Remove Monolith Auth

### 6.1 Files to DELETE

```
internal/controller/auth_controller.go
internal/service/auth_service.go
internal/service/email_service.go
internal/repository/auth_repository.go
internal/repository/refresh_token.go
internal/repository/verify_token_repository.go
internal/model/auth.go
internal/model/refresh_token.go
internal/model/verify_token.go
internal/router/auth_router.go
internal/payload/request/login_request.go
internal/payload/request/register_request.go
internal/payload/response/auth_response.go
internal/payload/response/jwt_response.go
internal/middleware/jwt_middleware.go
internal/config/email_config.go
pkg/utils/jwt_util.go
pkg/utils/password_util.go
pkg/utils/password_util_test.go
pkg/utils/verify_token.go
```

### 6.2 Files to MODIFY

#### `internal/bootstrap/api_container.go`

```go
package bootstrap

import (
	"oat431/fluffy-mouton/internal/controller"
	"oat431/fluffy-mouton/internal/repository"
	"oat431/fluffy-mouton/internal/service"

	"github.com/gofiber/fiber/v3/log"
	"github.com/jmoiron/sqlx"
)

type APIContainer struct {
	ShortLinkController *controller.ShortLinkController
	RedirectController  *controller.RedirectController
}

func NewAPIContainer(db *sqlx.DB) (*APIContainer, error) {
	log.Info("Registering Application Repository")
	shortLinkRepository := repository.NewShortLinkRepository(db)

	log.Info("Registering Application Service")
	shortLinkService, err := service.NewShortLinkService(shortLinkRepository)
	if err != nil {
		return nil, err
	}

	log.Info("Registering Application Controller")
	shortLinkController, err := controller.NewShortLinkController(shortLinkService)
	if err != nil {
		return nil, err
	}
	redirectController, err := controller.NewRedirectController(shortLinkService)
	if err != nil {
		return nil, err
	}

	log.Info("Registered All API")
	return &APIContainer{
		ShortLinkController: shortLinkController,
		RedirectController:  redirectController,
	}, nil
}
```

#### `internal/router/main_router.go`

```go
package router

import (
	"oat431/fluffy-mouton/internal/bootstrap"
	"oat431/fluffy-mouton/internal/middleware"

	"github.com/gofiber/fiber/v3"
	"github.com/gofiber/fiber/v3/log"
	"github.com/jmoiron/sqlx"
)

func init() {
	log.Info("Initializing routes...")
}

func SetupRoutes(app *fiber.App, apiContainer *bootstrap.APIContainer, db *sqlx.DB) {
	// Order matters: recover must be first to catch panics everywhere
	app.Use(middleware.Recover)
	app.Use(middleware.RequestID)
	app.Use(middleware.GlobalLogger)

	// CORS is now handled by Flowero Gate.
	// Remove the local CORS middleware — or keep for local dev only.

	api := app.Group("/api")
	v1 := api.Group("/v1")

	RegisterHealthRoutes(v1, db)
	RegisterShortLinkRoutes(v1, apiContainer.ShortLinkController)
	RegisterRedirectRoutes(v1, apiContainer.RedirectController)

	// Note: Auth routes are REMOVED. Keycloak handles all auth.
}
```

#### `internal/router/short_link_router.go`

```go
package router

import (
	"oat431/fluffy-mouton/internal/controller"
	"oat431/fluffy-mouton/internal/middleware"
	"oat431/fluffy-mouton/internal/payload/request"

	"github.com/gofiber/fiber/v3"
)

func RegisterShortLinkRoutes(router fiber.Router, controller *controller.ShortLinkController) {
	route := router.Group("/short")

	// All short-link endpoints require authentication
	route.Get("/",
		middleware.OAuthMiddleware,
		controller.GetAllShortLinks,
	)
	route.Post("/random",
		middleware.Validate[request.ShortLinkRequest],
		middleware.OAuthMiddleware,
		controller.CreateRandomShortLink,
	)
	route.Post("/custom",
		middleware.Validate[request.ShortLinkRequest],
		middleware.OAuthMiddleware,
		controller.CreateCustomShortLink,
	)
	route.Put("/:id",
		middleware.Validate[request.UpdateShortLinkRequest],
		middleware.OAuthMiddleware,
		controller.UpdateShortLink,
	)
	route.Delete("/:id",
		middleware.OAuthMiddleware,
		controller.DeleteShortLink,
	)
}
```

> **⚠️ Important:** The route path changes from `/short-link` to `/short` to align with the gateway path `/api/v1/short/**`. Update any frontend code that uses `/short-link`.

---

## 7. Phase 4 — Adapt Short-Link Controllers

### 7.1 `internal/controller/short_link_controller.go`

Change every occurrence of:

```go
// OLD
ownBy := c.Locals("auth_id").(uuid.UUID)

// NEW
ownBy := c.Locals("user_id").(uuid.UUID)
```

That's it. The `own_by` column in `tb_short_link` already uses `uuid.UUID` type, and Keycloak's `sub` claim is also a UUID. **No database schema change needed.**

### 7.2 `internal/controller/redirect_controller.go`

The redirect endpoint is public (no auth required). No changes needed.

---

## 8. Phase 5 — Configuration & Environment

### 8.1 `.env.example` — Before

```bash
PORT=
DB_PORT=
DB_USER=
DB_PASSWORD=
DB_NAME=
DB_HOST=
SECRET=
SMTP_HOST=
SMTP_PORT=
SMTP_USER=
SMTP_PASS=
```

### 8.2 `.env.example` — After

```bash
PORT=8080
DB_PORT=5432
DB_USER=
DB_PASSWORD=
DB_NAME=fluffy_mouton
DB_HOST=
OAUTH_ISSUER_URI=https://auth.panomete.com/realms/flowerogate
```

**Removed:** `SECRET`, `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASS`  
**Added:** `OAUTH_ISSUER_URI`

### 8.3 `internal/config/env_config.go`

No changes needed — it just loads `.env`. New variable is picked up automatically.

---

## 9. Phase 6 — Docker & Networking

### 9.1 Dockerfile — No Changes

The Dockerfile is fine as-is. Healthcheck uses `/api/v1/health/check` which still exists.

### 9.2 `docker-compose.physical-db.yml` — After

```yaml
services:
  api:
    build:
      context: .
      dockerfile: Dockerfile
    image: fluffy-mouton-api:latest
    container_name: shortlink-service
    env_file:
      - .env
    environment:
      PORT: ${PORT:-8080}
      DB_HOST: ${DB_HOST}
      DB_PORT: ${DB_PORT:-5432}
      DB_USER: ${DB_USER}
      DB_PASSWORD: ${DB_PASSWORD}
      DB_NAME: ${DB_NAME}
      OAUTH_ISSUER_URI: ${OAUTH_ISSUER_URI}
    ports:
      - "${PORT:-8080}:${PORT:-8080}"
    volumes:
      - ./.env:/app/.env:ro
    restart: unless-stopped
    healthcheck:
      test: ["CMD-SHELL", "wget -qO- http://127.0.0.1:${PORT:-8080}/api/v1/health/check || exit 1"]
      interval: 30s
      timeout: 5s
      retries: 3
      start_period: 20s
    networks:
      - microservices-net

networks:
  microservices-net:
    external: true
    name: microservices-net
```

**Changes:**
- `container_name`: `fluffy-mouton-api` → `shortlink-service` (matches `lb://` name in gateway)
- Network: `postgres-network` → `microservices-net`
- Added `OAUTH_ISSUER_URI` env var
- Removed `SECRET` and `SMTP_*` env vars

---

## 10. Phase 7 — Gateway Route Registration

Add this route to Flowero Gate's `application.yml`:

```yaml
# ---- Shortlink Service (Fluffy Mouton) ----
- id: route-shortlink
  uri: lb://shortlink-service
  order: 0
  predicates:
    - Path=/api/v1/short/**
  filters:
    - StripPrefix=1
    - name: CircuitBreaker
      args:
        name: shortlinkCircuitBreaker
        fallbackUri: forward:/fallback/shortlink
    - name: RequestRateLimiter
      args:
        redis-rate-limiter.replenishRate: 100
        redis-rate-limiter.burstCapacity: 200
        redis-rate-limiter.requestedTokens: 1
```

Add circuit breaker config:

```yaml
resilience4j:
  circuitbreaker:
    instances:
      shortlinkCircuitBreaker:
        slidingWindowSize: 10
        failureRateThreshold: 50
        waitDurationInOpenState: 10000
        permittedNumberOfCallsInHalfOpenState: 3
```

Add fallback handler in Flowero Gate:

```java
@GetMapping("/fallback/shortlink")
public Mono<ResponseEntity<Map<String, Object>>> shortlinkFallback() {
    return Mono.just(ResponseEntity
        .status(HttpStatus.SERVICE_UNAVAILABLE)
        .body(Map.of(
            "error", "service_unavailable",
            "message", "Shortlink service is temporarily unavailable."
        )));
}
```

**After gateway changes:** Restart Flowero Gate (or hit `/actuator/gateway/refresh` if using dynamic routes).

---

## 11. Phase 8 — Testing & Verification

### 11.1 Pre-Migration Checklist

- [ ] Keycloak client `service-shortlink` created with confidential access
- [ ] Client secret stored in flowerogate `.env`
- [ ] `OAUTH_ISSUER_URI` configured in Fluffy Mouton `.env`
- [ ] Gateway route added for `/api/v1/short/**`
- [ ] `shortlink-service` container on `microservices-net`
- [ ] Database `tb_short_link` still exists (no schema changes)

### 11.2 Test Cases

#### Connect via Gateway (with Keycloak JWT)

```bash
TOKEN=*** -s -X POST https://auth.panomete.com/realms/flowerogate/protocol/openid-connect/token \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials" \
  -d "client_id=service-shortlink" \
  -d "client_secret=$SHORTLINK_SERVICE_SECRET" \
  | jq -r '.access_token')

curl -s https://gateway.panomete.com/api/v1/short/ \
  -H "Authorization: Bearer $TOKEN" | jq
# Expected: 200 OK with data array
```

#### Test Direct Service Access

```bash
curl -s http://localhost:8080/api/v1/short/ \
  -H "Authorization: Bearer $TOKEN" | jq
# Expected: 200 OK — JWT validated via JWKS
```

#### Test Unauthenticated

```bash
curl -s http://localhost:8080/api/v1/short/ | jq
# Expected: 401 with "Missing Authorization header or X-User-Id"
```

#### Test Gateway Rate Limiting

```bash
for i in $(seq 1 201); do
  curl -s -o /dev/null -w "%{http_code}\n" \
    -H "Authorization: Bearer $TOKEN" \
    https://gateway.panomete.com/api/v1/short/
done
# Expected: First 200 = 200, 201st = 429
```

#### Test Gateway Circuit Breaker

```bash
docker stop shortlink-service
curl -s https://gateway.panomete.com/api/v1/short/ \
  -H "Authorization: Bearer $TOKEN" | jq
# Expected: 503 with "service_unavailable"
```

### 11.3 Integration Test Script

```bash
#!/bin/bash
# test_fluffy_mouton_migration.sh

BASE="https://gateway.panomete.com/api/v1/short"
AUTH_URL="https://auth.panomete.com/realms/flowerogate/protocol/openid-connect/token"

echo "=== Fluffy Mouton Migration Test ==="

echo -n "Getting token... "
TOKEN=*** -sf -X POST "$AUTH_URL" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials" \
  -d "client_id=service-shortlink" \
  -d "client_secret=$SHORTLINK_SERVICE_SECRET" \
  | jq -r '.access_token')
[ -z "$TOKEN" ] || [ "$TOKEN" = "null" ] && echo "FAILED" && exit 1
echo "OK"

echo -n "Test 1 - Unauthenticated... "
[ "$(curl -s -o /dev/null -w "%{http_code}" "$BASE/")" = "401" ] && echo "PASS" || echo "FAIL"

echo -n "Test 2 - Authenticated... "
[ "$(curl -s -o /dev/null -w "%{http_code}" -H "Authorization: Bearer $TOKEN" "$BASE/")" = "200" ] && echo "PASS" || echo "FAIL"

echo -n "Test 3 - Create short link... "
RESP=*** -sf -X POST "$BASE/random" -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" -d '{"url":"https://example.com/test"}')
LINK_ID=$(echo "$RESP" | jq -r '.data.id')
SHORT=$(echo "$RESP" | jq -r '.data.short_link')
[ -n "$LINK_ID" ] && [ "$LINK_ID" != "null" ] && echo "PASS ($SHORT)" || echo "FAIL"

echo -n "Test 4 - Update... "
[ "$(curl -s -o /dev/null -w "%{http_code}" -X PUT "$BASE/$LINK_ID" \
  -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/json" \
  -d '{"url":"https://example.com/updated"}')" = "200" ] && echo "PASS" || echo "FAIL"

echo -n "Test 5 - Redirect... "
[ "$(curl -s -o /dev/null -w "%{http_code}" "https://gateway.panomete.com/api/v1/r/$SHORT")" = "301" ] && echo "PASS" || echo "FAIL"

echo -n "Test 6 - Delete... "
[ "$(curl -s -o /dev/null -w "%{http_code}" -X DELETE "$BASE/$LINK_ID" \
  -H "Authorization: Bearer $TOKEN")" = "200" ] && echo "PASS" || echo "FAIL"

echo -n "Test 7 - Health check... "
[ "$(curl -s -o /dev/null -w "%{http_code}" "https://gateway.panomete.com/api/v1/health/check")" = "200" ] && echo "PASS" || echo "FAIL"

echo "=== All Tests Complete ==="
```

---

## 12. Phase 9 — Cleanup

### 12.1 Database Cleanup

```sql
-- ⚠️ RUN ONLY AFTER VERIFICATION ⚠️
DROP TABLE IF EXISTS tb_refresh_token;
DROP TABLE IF EXISTS tb_verify_token;
DROP TABLE IF EXISTS tb_auth;
```

**Keep `tb_short_link`** — it contains user data.

### 12.2 Go Module Cleanup

```bash
go mod tidy
```

Removes unused dependencies (`golang.org/x/crypto` bcrypt, etc.)

### 12.3 Update `API_DOCUMENTATION.md`

- Remove all auth endpoint sections
- Update authentication section: OAuth2 via Keycloak
- Update CORS section: handled by Flowero Gate
- Update code examples: Keycloak token acquisition
- Update base URL: `https://gateway.panomete.com`

### 12.4 Update `data/db_fluffy_mouton.sql`

Remove `tb_auth`, `tb_refresh_token`, `tb_verify_token` table creation. Keep `tb_short_link`.

---

## 13. Risk Assessment

### Risks

| Risk | Severity | Mitigation |
|------|----------|------------|
| Keycloak JWKS endpoint unreachable | High | `InitJWKS()` failure logs warning; gateway header mode still works. Monitor Keycloak health. |
| `sub` claim format changes | Medium | Keycloak UUID `sub` is stable. If Keycloak is reconfigured, migration needed. |
| `own_by` in existing short links references old `auth_id` UUIDs | **High** | Existing `own_by` values are old local UUIDs. After migration, new `own_by` values are Keycloak UUIDs. **Old short links will have orphaned ownership.** |
| Frontend hardcodes `/short-link` path | Medium | Route changed to `/short` to match gateway. Update frontend or add alias route. |
| Email verification gap | Low | Keycloak handles email verification. Verify it's enabled in Keycloak. |
| Loss of user data | Medium | Old auth table dropped. Users re-register in Keycloak. |

### ⚠️ Critical Data Migration Concern

**The biggest risk:** Existing short links have `own_by` set to old local `tb_auth` UUIDs. After migration, user identity comes from Keycloak UUIDs — these are **different UUIDs**.

**Options:**

| Option | Effort | Description |
|--------|--------|-------------|
| **A: Truncate & restart** | Minimal | Clear `tb_short_link`. Users create new short links under their Keycloak identity. Simplest. |
| **B: User mapping table** | Medium | Create `tb_user_mapping(local_auth_id, keycloak_sub)` during migration. Update `own_by` with a JOIN. Requires one-time script. |
| **C: Coexistence period** | High | Keep both auth systems running. New users via Keycloak, old users via local auth. Sunset after N months. |

**Recommendation: Option A** — Fluffy Mouton is still in development (`🛠️` status). The number of existing short links is likely small. Truncate and let users recreate.

If production data exists and is important, use **Option B**.

### Option B Migration Script (if needed)

```sql
-- 1. Create mapping table
CREATE TABLE tb_user_mapping (
    local_auth_id UUID PRIMARY KEY,
    keycloak_sub UUID NOT NULL UNIQUE,
    username VARCHAR(255),
    migrated_at TIMESTAMP DEFAULT NOW()
);

-- 2. Insert mappings (match users by username/email — manual or scripted)
INSERT INTO tb_user_mapping (local_auth_id, keycloak_sub, username)
SELECT a.id, 'keycloak-uuid-here', a.username
FROM tb_auth a
WHERE a.username = 'panomete';
-- (repeat for each user)

-- 3. Update short link ownership
UPDATE tb_short_link sl
SET own_by = tm.keycloak_sub
FROM tb_user_mapping tm
WHERE sl.own_by = tm.local_auth_id;

-- 4. Verify
SELECT own_by, COUNT(*) FROM tb_short_link GROUP BY own_by;

-- 5. After verification, drop old tables
DROP TABLE tb_user_mapping;
DROP TABLE tb_auth;
DROP TABLE tb_refresh_token;
DROP TABLE tb_verify_token;
```

---

## Execution Order (Recommended)

```
Phase 1: Keycloak client registration         │  5 min  │
Phase 5: Update .env                          │  2 min  │
Phase 2: Create keycloak_jwt.go               │ 5 min  │
Phase 2: Create oauth_middleware.go            │ 5 min  │
Phase 2: Update main.go (InitJWKS)            │  2 min  │
Phase 4: Update short_link_controller.go      │  5 min  │ (auth_id → user_id)
Phase 3: Update main_router.go                │  3 min  │
Phase 3: Update short_link_router.go          │  3 min  │
Phase 3: Update api_container.go              │  5 min  │
Phase 3: Remove old auth files                │  2 min  │
Phase 2: go mod tidy                          │  1 min  │
Phase 6: Update docker-compose                │  5 min  │
Phase 7: Add gateway route                    │ 10 min  │
Phase 13: Data migration decision             │ 5 min  │ (truncate or map)
─────────────────────────────────────────────────────────
Phase 8: Deploy & test                        │ 20 min  │
Phase 9: Cleanup                              │  5 min  │
Phase 9: Update API docs                      │ 5 min  │
─────────────────────────────────────────────────────────
                                    Total:   ~2 hours   │
```

---

## References

- [Flowero Guard — Section 8: Downstream Service Integration](../Flowero%20Guard.md#8-downstream-service-integration)
- [Flowero Guard — Section 10: Integration Checklist](../Flowero%20Guard.md#10-integration-checklist-for-new-services)
- [Flowero Gate — Section 13: Integration Checklist](../Flowero%20Gate.md#13-integration-checklist-for-new-services)
- [github.com/MicahParks/keyfunc](https://github.com/MicahParks/keyfunc) — Go JWKS client library

---

*Plan created: 2026-06-17*
