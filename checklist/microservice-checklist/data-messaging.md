# Data & Messaging Checklist

> Tick every box before data flows between services in production. Wrong data architecture is the hardest thing to fix later. Deep reference: [[03 Database per Service]], [[03 Saga Pattern]], [[02 Event-Driven Architecture]].

---

## Database per Service

- [ ] **Each service owns its database** — no other service accesses it directly → [[03 Database per Service]]
- [ ] Shared database: NEVER in microservices. One service's schema change breaks another
- [ ] Data accessed only through service API — the service IS the data gatekeeper
- [ ] Polyglot persistence OK — order service uses PostgreSQL, search service uses Elasticsearch
- [ ] Schema changes: backward-compatible, versioned migrations (Flyway/Liquibase) → [[03 Migration Backup & Scaling]]

---

## Transactions

- [ ] **No distributed transactions (2PC/XA)** — they don't scale → [[03 Saga Pattern]]
- [ ] Saga pattern for multi-service operations: sequence of local transactions + compensating actions
- [ ] Choreography: services publish events, other services react. Decentralized, harder to trace
- [ ] Orchestration: saga orchestrator coordinates steps. Centralized, easier to understand
- [ ] Every saga step has a compensating action — what to do if this step fails
- [ ] Sagas are eventually consistent — accept it. Don't fight it with distributed locks

---

## Event-Driven Communication

- [ ] **Async where synchronous isn't needed** → [[02 Event-Driven Architecture]]
- [ ] Events: "OrderPlaced", "PaymentProcessed", "ShipmentCreated" — past tense, domain language
- [ ] Commands: "PlaceOrder", "ProcessPayment" — imperative, directed at a specific service
- [ ] Events are facts — immutable, append-only. Never update or delete an event
- [ ] Schema evolution: add fields (backward-compatible), never remove or rename (breaking)
- [ ] Event versioning: include schema version in event envelope

---

## Message Broker

- [ ] **Broker chosen with documented reasoning** → [[02 Messaging Patterns]]
- [ ] RabbitMQ: mature, flexible routing, good for most use cases
- [ ] Kafka: high throughput, persistent log, replay capability. Good for event sourcing
- [ ] Cloud: SQS/SNS (AWS), Pub/Sub (GCP). Zero ops, cloud lock-in
- [ ] Broker is HA — clustered, mirrored queues or multi-broker

---

## Message Reliability

- [ ] **At-least-once delivery** (default) — consumers must be idempotent → [[02 Messaging Patterns]]
- [ ] Exactly-once: Kafka transactions, RabbitMQ publisher confirms + idempotent consumer
- [ ] Publisher confirms: producer knows message was accepted by broker
- [ ] Consumer acknowledgements: manual ack after successful processing, not auto-ack
- [ ] Dead Letter Queue (DLQ): failed messages after N retries → inspect, fix, replay

---

## Idempotency

- [ ] **Every consumer is idempotent** — duplicate message ≠ duplicate effect → [[02 Messaging Patterns]]
- [ ] Idempotency key: unique per operation, stored with result. Replay returns cached result
- [ ] Database unique constraint: natural idempotency (INSERT IGNORE, ON CONFLICT DO NOTHING)
- [ ] Deduplication: check if event ID already processed before acting

---

## Ordering & Concurrency

- [ ] **Ordering guarantee understood** — does your broker preserve order? → [[02 Messaging Patterns]]
- [ ] Kafka: order per partition. Same key → same partition → ordered
- [ ] RabbitMQ: order per queue, but concurrent consumers may reorder
- [ ] Out-of-order handling: detect via sequence numbers, buffer and reorder, or design for commutativity
- [ ] Competing consumers: multiple instances processing same queue. OK for independent messages

---

## Event Sourcing (If Used)

- [ ] **Events are the source of truth** — current state derived from event stream → [[03 CQRS & Event Sourcing]]
- [ ] Event store is append-only, immutable. Never update or delete events
- [ ] Snapshots: periodic state snapshot to avoid replaying entire history
- [ ] Schema evolution: upcast old events to new schema on read
- [ ] Event sourcing + CQRS: write model (events) separate from read model (projections)

---

## Data Consistency Patterns

- [ ] **Transactional outbox** — write to DB + publish event atomically → [[03 Database per Service]]
- [ ] Outbox table: `INSERT INTO outbox (event_type, payload)`. Separate process publishes to broker
- [ ] Change Data Capture (CDC): Debezium tails DB transaction log → publishes to Kafka. No app code
- [ ] Compensating transactions for saga rollback — not automatic, must be designed

---

## Testing

- [ ] Event published on state change — consumer receives it
- [ ] Consumer idempotent — duplicate event processed once
- [ ] DLQ populated after max retries — message inspectable, replayable
- [ ] Saga: all steps succeed → final state correct
- [ ] Saga: step fails → compensating actions execute → system returns to valid state
- [ ] Broker HA tested — kill one node, publishing + consuming continues

---

## Quick Sanity Check

- [ ] Each service owns its database — no shared DB access
- [ ] No distributed transactions (2PC) — sagas or eventual consistency instead
- [ ] Events are facts, immutable, append-only
- [ ] Every consumer is idempotent — duplicate messages don't break anything
- [ ] Dead letter queue configured — failed messages not silently dropped
- [ ] Schema evolution planned — backward-compatible additions only
- [ ] Outbox pattern or CDC for reliable event publishing
- [ ] Message broker is HA

---

## Sources

- [[03 Database per Service]] — database ownership and polyglot persistence
- [[03 Saga Pattern]] — distributed transactions via saga
- [[02 Event-Driven Architecture]] — event-driven communication
- [[02 Messaging Patterns]] — message reliability, ordering, idempotency
- [[03 CQRS & Event Sourcing]] — event sourcing and CQRS
- [[Microservice Launch]] — system-wide launch checklist
