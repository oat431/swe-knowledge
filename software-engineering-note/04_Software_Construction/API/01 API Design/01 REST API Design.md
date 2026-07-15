---
tags:
- api
- programming
- protocols
---

# 01 REST API Design

REST is the dominant API architecture on the web. When done right, it's simple, cacheable, and scalable. When done wrong, it's a mess of inconsistent URLs and broken conventions.

---

## The Six Constraints of REST

| Constraint | What It Means |
|-----------|--------------|
| **Client-Server** | UI is separate from data storage. Client and server evolve independently. |
| **Stateless** | Each request contains all info needed. No server-side session. |
| **Cacheable** | Responses declare whether they can be cached. `Cache-Control` headers. |
| **Uniform Interface** | Resources identified by URIs. Self-descriptive messages. HATEOAS. |
| **Layered System** | Client doesn't know if it's talking to the server or an intermediary. |
| **Code on Demand** (optional) | Server can send executable code (JavaScript) to the client. |

---

## Resource Naming

> **Nouns, not verbs. Collections are plural.**

| ❌ | ✅ |
|----|-----|
| `GET /getOrders` | `GET /orders` |
| `POST /createUser` | `POST /users` |
| `GET /getOrderById?id=123` | `GET /orders/123` |
| `POST /deleteOrder` | `DELETE /orders/123` |

---

## HTTP Methods

| Method | Action | Idempotent? | Example |
|--------|--------|:----------:|---------|
| **GET** | Read | ✅ | `GET /orders/123` |
| **POST** | Create | ❌ | `POST /orders` |
| **PUT** | Replace (full update) | ✅ | `PUT /orders/123` |
| **PATCH** | Partial update | ❌ | `PATCH /orders/123` |
| **DELETE** | Delete | ✅ | `DELETE /orders/123` |
| **HEAD** | Like GET, no body | ✅ | `HEAD /orders/123` |
| **OPTIONS** | What methods are allowed | ✅ | `OPTIONS /orders` |

---

## Status Codes

| Code | Meaning | When |
|:----:|---------|------|
| **200** | OK | Successful GET, PUT, PATCH |
| **201** | Created | Successful POST — include `Location` header |
| **204** | No Content | Successful DELETE |
| **400** | Bad Request | Invalid input, validation error |
| **401** | Unauthorized | Missing or invalid credentials |
| **403** | Forbidden | Authenticated but not allowed |
| **404** | Not Found | Resource doesn't exist |
| **409** | Conflict | Duplicate, version conflict |
| **422** | Unprocessable | Semantic error in request body |
| **429** | Too Many Requests | Rate limited |
| **500** | Internal Server Error | Something broke on the server |

---

## Versioning

| Strategy | Example | Pros/Cons |
|----------|---------|-----------|
| **URL path** | `/v1/orders` | Simple, visible. "Violates" REST (URLs identify resources, not versions). |
| **Header** | `Accept: application/vnd.api+json;version=1` | Clean URLs. Harder to test in browser. |
| **Query param** | `/orders?version=1` | Simple. Pollutes query params. |
| **Content negotiation** | `Accept: application/vnd.myapp.v1+json` | Most RESTful. Most complex. |

> **Most teams use URL path versioning.** It's the simplest and clients understand it instantly.

---

## Pagination

```json
// Request: GET /orders?page=2&limit=50

// Response:
{
  "data": [...],
  "meta": {
    "page": 2,
    "limit": 50,
    "total": 1247,
    "totalPages": 25,
    "next": "/orders?page=3&limit=50",
    "prev": "/orders?page=1&limit=50"
  }
}
```

| Pattern | When |
|---------|------|
| **Offset-based** (`page`/`limit`) | Simple, common. Inconsistent if data changes during pagination. |
| **Cursor-based** (`after`/`before`) | Stable. Better for real-time data (Twitter, Slack). |

---

## Error Response Format

Status codes tell the client *what* went wrong. A consistent error body tells them *why* and *how to fix it*.

### Standard Error Structure

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Request validation failed",
    "status": 422,
    "timestamp": "2026-07-11T10:30:00Z",
    "path": "/api/orders",
    "details": [
      {
        "field": "quantity",
        "message": "Must be greater than 0",
        "rejectedValue": -1
      },
      {
        "field": "productId",
        "message": "Product not found",
        "rejectedValue": "nonexistent-id"
      }
    ],
    "traceId": "abc-123-def"
  }
}
```

### What Every Error Response Needs

| Field | Why |
|-------|-----|
| `code` | Machine-readable error type — client can switch on it |
| `message` | Human-readable explanation |
| `status` | HTTP status code (redundant but convenient for logging) |
| `timestamp` | When it happened — critical for debugging logs |
| `path` | Which endpoint failed |
| `details` | Field-level validation errors (400/422 only) |
| `traceId` | Correlation ID for distributed tracing |

### ❌ vs ✅

```json
// ❌ Useless error
{ "error": "Something went wrong" }

// ❌ Leaks internals
{ "error": "SQLException: duplicate key value violates unique constraint \"orders_pkey\"" }

// ✅ Safe, actionable
{
  "error": {
    "code": "DUPLICATE_ORDER",
    "message": "An order with this reference already exists",
    "status": 409
  }
}
```

### Spring Boot Implementation

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErrorResponse> handleValidation(MethodArgumentNotValidException ex) {
        List<ErrorDetail> details = ex.getBindingResult().getFieldErrors().stream()
            .map(fe -> new ErrorDetail(fe.getField(), fe.getDefaultMessage(), fe.getRejectedValue()))
            .toList();

        ErrorResponse error = ErrorResponse.builder()
            .code("VALIDATION_ERROR")
            .message("Request validation failed")
            .status(422)
            .details(details)
            .timestamp(Instant.now())
            .build();

        return ResponseEntity.status(422).body(error);
    }

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleNotFound(ResourceNotFoundException ex) {
        ErrorResponse error = ErrorResponse.builder()
            .code("NOT_FOUND")
            .message(ex.getMessage())
            .status(404)
            .timestamp(Instant.now())
            .build();

        return ResponseEntity.status(404).body(error);
    }
}
```

---

## CORS (Cross-Origin Resource Sharing)

CORS is the browser's security mechanism that blocks requests from `http://localhost:3000` to `http://localhost:8080` unless the server explicitly allows it. Every frontend-backend setup hits this.

### How It Works

```
Browser (http://localhost:3000)
  → GET http://localhost:8080/api/orders
  → Browser checks: is localhost:3000 allowed to call localhost:8080?
  → Server responds with: Access-Control-Allow-Origin: http://localhost:3000
  → If missing or wrong → request BLOCKED by browser
```

### Simple vs Preflight Requests

| Type | When | Method |
|------|------|--------|
| **Simple** | GET/POST/HEAD with standard headers | Sent directly |
| **Preflight** | PUT, DELETE, custom headers, JSON content-type | Browser sends `OPTIONS` first, then the real request |

### Spring Boot Configuration

```java
// ✅ Global CORS config
@Configuration
public class CorsConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/api/**")
            .allowedOrigins("http://localhost:3000", "https://app.example.com")
            .allowedMethods("GET", "POST", "PUT", "PATCH", "DELETE")
            .allowedHeaders("Authorization", "Content-Type")
            .exposedHeaders("X-RateLimit-Remaining", "X-Request-Id")
            .allowCredentials(true)
            .maxAge(3600);  // Cache preflight for 1 hour
    }
}
```

```java
// ❌ NEVER do this in production
.addAllowedOrigin("*")  // allows any website to call your API
```

### Common CORS Pitfalls

| Problem | Cause | Fix |
|---------|-------|-----|
| `*` with credentials | Browsers block `Access-Control-Allow-Origin: *` when `withCredentials: true` | List specific origins |
| Missing headers on error responses | CORS headers not set on 4xx/5xx responses | Add CORS filter before auth filter |
| Proxy not forwarding headers | Nginx/reverse proxy strips CORS headers | Configure proxy to pass `Origin` header through |

---

## Idempotency Keys

Network failures cause retries. If `POST /orders` is retried, you get duplicate orders. Idempotency keys solve this.

### How It Works

```
Client generates UUID: idempotency-key: abc-123
  → POST /orders (idempotency-key: abc-123, body: {...})
  → Server creates order, stores key → order-789
  → Network timeout, client retries
  → POST /orders (idempotency-key: abc-123, body: {...})
  → Server sees key already exists → returns order-789 (no duplicate!)
```

### Implementation

```java
@PostMapping("/orders")
public ResponseEntity<Order> createOrder(
        @RequestBody CreateOrderRequest request,
        @RequestHeader("Idempotency-Key") String idempotencyKey) {

    // Check if we've seen this key before
    Optional<Order> existing = idempotencyStore.find(idempotencyKey);
    if (existing.isPresent()) {
        return ResponseEntity.ok(existing.get());  // Return same result
    }

    // Create the order
    Order order = orderService.create(request);

    // Store the key → result mapping (expire after 24h)
    idempotencyStore.save(idempotencyKey, order, Duration.ofHours(24));

    return ResponseEntity.status(201).body(order);
}
```

| Aspect | Details |
|--------|--------|
| **Key format** | UUID v4 (client-generated) |
| **Header name** | `Idempotency-Key` (Stripe's convention, widely adopted) |
| **Storage** | Redis, database, or in-memory cache |
| **TTL** | 24–48 hours (matches max retry window) |
| **Scope** | Per-user, per-endpoint |

> **When to use:** Any state-changing endpoint where duplicates are harmful — payments, orders, transfers. Not needed for GET (already idempotent) or truly idempotent PUT/DELETE.

---

## HATEOAS (Hypermedia as the Engine of Application State)

The API response includes links to related actions. The client navigates the API by following links — no hardcoded URLs.

```json
{
  "id": 123,
  "status": "shipped",
  "_links": {
    "self": { "href": "/orders/123" },
    "cancel": { "href": "/orders/123/cancel" },
    "customer": { "href": "/customers/456" }
  }
}
```

> HATEOAS is the most violated REST constraint. Most APIs skip it. It's powerful but adds complexity. Include it only if clients genuinely benefit from discoverability.

---

## Sources

- Fielding, Roy. *Architectural Styles*, 2000.
- Microsoft REST API Guidelines — https://github.com/microsoft/api-guidelines
- RFC 7231 — HTTP/1.1 Semantics and Content
- Stripe API — https://stripe.com/docs/api (gold standard for error responses and idempotency)
