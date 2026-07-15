---
tags:
  - programming
  - fundamental
  - logging
---

# Logging & Structured Logging

Logs are your eyes in production. Good logging means you can diagnose issues in minutes; bad logging means you're flying blind. Structured logging — writing logs as machine-parseable data rather than free-text strings — is the modern standard for any system that runs at scale.

---

## Log Levels

Use levels consistently. Every log statement should have a clear purpose.

| Level | When to Use | Example |
|-------|-------------|---------|
| **TRACE** | Extremely detailed, usually off in production | Method entry/exit, loop iterations |
| **DEBUG** | Diagnostic info useful during development | Variable values, SQL queries, cache hits |
| **INFO** | Significant business events | Order created, user logged in, payment processed |
| **WARN** | Unexpected but recoverable situations | Retry attempt, deprecated API call, config fallback |
| **ERROR** | Failures that need attention | Unhandled exception, external service down, data corruption |

### Level Selection Rules

- **INFO** is the default for business events — this is what ops teams watch
- **DEBUG** should be safe to turn on in production without flooding logs
- **ERROR** means something is broken and needs human investigation
- **WARN** means something unexpected happened but the system recovered
- If you're not sure, start with DEBUG and promote to INFO after review

---

## ❌ Bad Logging Patterns

### Logging Sensitive Data

```java
// ❌ NEVER log passwords, tokens, PII
log.info("User login: email={}, password={}", email, password);
log.debug("Auth token: {}", bearerToken);
log.info("Credit card: {}", cardNumber);
```

```java
// ✅ Mask or omit sensitive data
log.info("User login: email={}", email);
log.debug("Auth token present: {}", bearerToken != null);
```

### Too Verbose / No Context

```java
// ❌ Meaningless
log.info("Processing...");
log.info("Done");
log.info("Error occurred");

// ❌ Dumping entire objects
log.info("Order: {}", order.toString());  // 200 lines of nested fields
```

### Log Level Abuse

```java
// ❌ Using ERROR for non-errors
log.error("User preferences not set, using defaults");  // This is a WARN at most

// ❌ Using INFO for debug details
log.info("Entering method calculateTotal()");  // This is TRACE or DEBUG

// ❌ Using DEBUG for important business events
log.debug("Payment of $500 processed for order #1234");  // This should be INFO
```

---

## ✅ Good Logging Patterns

### Structured Logging (JSON)

Instead of free-text lines, log structured data that machines can parse and query.

```java
// ❌ Unstructured — hard to query
log.info("User john@example.com placed order #12345 for $299.99");

// ✅ Structured — queryable fields
log.info("Order placed", Map.of(
    "orderId", "12345",
    "userId", "john@example.com",
    "amount", 299.99,
    "currency", "USD"
));
```

Output:
```json
{
  "timestamp": "2024-01-15T14:30:00Z",
  "level": "INFO",
  "service": "order-service",
  "message": "Order placed",
  "orderId": "12345",
  "userId": "john@example.com",
  "amount": 299.99,
  "currency": "USD",
  "traceId": "abc-123-def"
}
```

### What Every Log Entry Needs

| Field | Purpose | Example |
|-------|---------|---------|
| `timestamp` | When it happened | `2024-01-15T14:30:00.123Z` |
| `level` | Severity | `INFO` |
| `service` | Which service | `order-service` |
| `traceId` | Distributed trace link | `abc-123-def-456` |
| `userId` | Who triggered it | `user-789` |
| `message` | Human-readable summary | `Order placed successfully` |
| `duration` | How long (for performance) | `245ms` |
| `error` | Stack trace (if applicable) | `"Connection refused"` |

---

## Structured Logging with SLF4J + Logback

### Dependencies (Maven)

```xml
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>2.0.9</version>
</dependency>
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.4.14</version>
</dependency>
<!-- JSON encoder -->
<dependency>
    <groupId>net.logstash.logback</groupId>
    <artifactId>logstash-logback-encoder</artifactId>
    <version>7.4</version>
</dependency>
```

### Logback Config (logback-spring.xml)

```xml
<configuration>
  <appender name="JSON" class="ch.qos.logback.core.ConsoleAppender">
    <encoder class="net.logstash.logback.encoder.LogstashEncoder">
      <includeMdcKeyName>traceId</includeMdcKeyName>
      <includeMdcKeyName>userId</includeMdcKeyName>
      <fieldNames>
        <timestamp>timestamp</timestamp>
        <message>message</message>
      </fieldNames>
    </encoder>
  </appender>

  <root level="INFO">
    <appender-ref ref="JSON" />
  </root>
</configuration>
```

### TypeScript (Node.js) — Winston

```typescript
import winston from 'winston';

const logger = winston.createLogger({
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.json()
  ),
  defaultMeta: { service: 'order-service' },
  transports: [new winston.transports.Console()]
});

// Usage
logger.info('Order placed', {
  orderId: '12345',
  userId: 'user-789',
  amount: 299.99
});
```

---

## Correlation IDs (Distributed Tracing)

In microservices, a single request spans multiple services. A `traceId` ties all logs together.

### How It Flows

```
[Client] → [API Gateway] → [Order Service] → [Payment Service] → [Inventory Service]
   traceId=abc-123  traceId=abc-123   traceId=abc-123   traceId=abc-123
```

### MDC (Mapped Diagnostic Context)

```java
// In a filter/interceptor — set once per request
MDC.put("traceId", request.getHeader("X-Trace-Id"));
MDC.put("userId", getCurrentUserId());

try {
    // All log statements in this thread automatically include traceId
    log.info("Processing order");
    orderService.create(order);
} finally {
    MDC.clear();  // Always clean up
}
```

### Spring Boot — Automatic Trace Propagation

```yaml
# application.yml
management:
  tracing:
    sampling:
      probability: 1.0  # 100% in dev, 1-10% in prod
  zipkin:
    tracing:
      endpoint: http://zipkin:9411/api/v2/spans
```

Spring Boot 3 + Micrometer Tracing automatically:
- Generates `traceId` and `spanId`
- Injects them into MDC
- Propagates them to outgoing HTTP calls

For deeper distributed tracing concepts, see [[05 Distributed Tracing]].

---

## Log Aggregation

In production, logs go to centralized systems — not just stdout.

| Tool | Type | Notes |
|------|------|-------|
| **ELK Stack** | Self-hosted | Elasticsearch + Logstash + Kibana — full control |
| **Datadog** | SaaS | Great UI, alerting, APM integration |
| **Grafana Loki** | Self-hosted | Lightweight, label-based, pairs with Grafana |
| **AWS CloudWatch** | Cloud-native | Automatic for Lambda/ECS/EKS |
| **Splunk** | Enterprise | Powerful search, expensive |

### What to Send to Aggregation

- ✅ All WARN and ERROR logs
- ✅ All INFO business events
- ✅ Request/response summaries (with sanitized bodies)
- ❌ TRACE/DEBUG — keep local or sample at 1%

---

## Spring Boot Logging Config

```yaml
# application.yml
logging:
  level:
    root: INFO
    com.mycompany: DEBUG          # Your code at DEBUG
    org.springframework: WARN     # Framework noise down
    org.hibernate.SQL: DEBUG      # SQL queries when needed
  pattern:
    console: "%d{ISO8601} [%thread] %-5level %logger{36} - %msg%n"
  file:
    name: logs/application.log
```

### Per-Environment Config

| Environment | Root Level | Your Code | Hibernate |
|-------------|------------|-----------|-----------|
| Development | DEBUG | DEBUG | DEBUG |
| Staging | INFO | INFO | WARN |
| Production | WARN | INFO | ERROR |

---

## Logging Checklist

- [ ] Every log entry has a clear level (not just INFO for everything)
- [ ] No sensitive data in logs (passwords, tokens, PII)
- [ ] Structured format (JSON) for production logs
- [ ] Correlation ID (`traceId`) on every entry
- [ ] Business events logged at INFO (order created, payment processed)
- [ ] Errors include context (what failed, why, relevant IDs)
- [ ] Log levels configurable per environment
- [ ] No string concatenation in log calls — use parameterized messages

---

**Sources:** SLF4J manual; Logback documentation; Spring Boot reference (logging section); Datadog logging best practices
