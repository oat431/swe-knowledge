---
title: Data & Messaging Checklist
checklist_type: microservice-domain
version: 2.0
status: active
scope: data ownership, messaging, events, brokers, and distributed workflows
last_updated: 2026-08-06
---

# Data & Messaging Checklist

> Tick every box before data flows between services in production. Wrong data architecture is the hardest thing to fix later. Deep reference: [[031 Database per Service]], [[033 Saga Pattern]], [[023 Event-Driven Architecture]].

---

## Database per Service

- [ ] **Each service owns its data boundary** — no other service directly reads or writes its owned schema → [[031 Database per Service]]
- [ ] Shared database or transitional integration is an explicit exception with a named owner, access boundary, migration plan, and expiry; direct cross-service schema access is not the normal design
- [ ] Data accessed only through service API — the service IS the data gatekeeper
- [ ] Polyglot persistence OK — order service uses PostgreSQL, search service uses Elasticsearch
- [ ] Schema changes: backward-compatible, versioned migrations (Flyway/Liquibase) → [[03 Migration Backup & Scaling]]

---

## Transactions

- [ ] **No distributed transactions (2PC/XA)** — they don't scale → [[033 Saga Pattern]]
- [ ] Saga pattern for multi-service operations: sequence of local transactions + compensating actions
- [ ] Choreography: services publish events, other services react. Decentralized, harder to trace
- [ ] Orchestration: saga orchestrator coordinates steps. Centralized, easier to understand
- [ ] Every saga step has a compensating action or an explicitly documented non-compensatable outcome
- [ ] Sagas are eventually consistent — accept it. Don't fight it with distributed locks
- [ ] Saga timeout, retry, stuck-saga detection, manual reconciliation, and idempotent compensation are defined

---

## Event-Driven Communication

- [ ] **Async where synchronous isn't needed** → [[023 Event-Driven Architecture]]
- [ ] Events: "OrderPlaced", "PaymentProcessed", "ShipmentCreated" — past tense, domain language
- [ ] Commands: "PlaceOrder", "ProcessPayment" — imperative, directed at a specific service
- [ ] Events are facts — immutable after publication. Define retention, compaction, redaction, and deletion policy for the physical store
- [ ] Schema evolution policy is explicit: additive changes first, consumer compatibility verified, deprecation window provided before removal or rename
- [ ] Event versioning: include schema version in event envelope

---

## Message Broker

- [ ] **Broker chosen with documented reasoning** → [[024 Messaging Patterns]]
- [ ] RabbitMQ: mature, flexible routing, good for most use cases
- [ ] Kafka: high throughput, persistent log, replay capability. Good for event sourcing
- [ ] Cloud: SQS/SNS (AWS), Pub/Sub (GCP). Zero ops, cloud lock-in
- [ ] Broker availability model is documented — quorum/replication or managed-service guarantees, failure domains, and recovery procedure. For RabbitMQ 4.x, evaluate quorum queues or streams rather than relying on removed classic queue mirroring.
- [ ] Broker authentication, authorization, encryption, retention, backup, and restore are configured

---

## Message Reliability

- [ ] **Delivery guarantee selected** — at-most-once, at-least-once, effectively-once, or a bounded Kafka transactional consume-process-produce workflow → [[024 Messaging Patterns]]
- [ ] Exactly-once is not claimed unless the complete processing boundary is defined; external side effects still use idempotency or deduplication
- [ ] Publisher confirms: producer knows message was accepted by broker
- [ ] Consumer acknowledgements: manual ack after successful processing, not auto-ack
- [ ] Dead Letter Queue (DLQ): failed messages after N retries → inspect, fix, replay

---

## Idempotency

- [ ] **Consumers of at-least-once or replayable messages are idempotent** — duplicate message ≠ duplicate effect → [[024 Messaging Patterns]]
- [ ] Idempotency key: unique per operation, stored with result. Replay returns cached result
- [ ] Database unique constraint: natural idempotency (INSERT IGNORE, ON CONFLICT DO NOTHING)
- [ ] Deduplication: check if event ID already processed before acting

---

## Ordering & Concurrency

- [ ] **Ordering guarantee understood** — does your broker preserve order, and at what scope? → [[024 Messaging Patterns]]
- [ ] Kafka: order per partition. Same key → same partition → ordered
- [ ] RabbitMQ: order per queue, but concurrent consumers may reorder
- [ ] Out-of-order handling: detect via sequence numbers, buffer and reorder, or design for commutativity
- [ ] Competing consumers: multiple instances processing same queue. OK for independent messages

---

## Event Sourcing (If Used)

- [ ] **Events are the source of truth only when event sourcing is intentionally selected** — current state is derived from the event stream → [[034 CQRS & Event Sourcing]]
- [ ] Domain events are immutable after publication. Physical retention, compaction, redaction, and legal deletion policies are explicitly defined.
- [ ] Snapshots: periodic state snapshot to avoid replaying entire history
- [ ] Schema evolution: upcast old events to new schema on read
- [ ] Event sourcing + CQRS: write model (events) separate from read model (projections)

---

## Data Consistency Patterns

- [ ] **Transactional outbox** — write business data and the event record in one DB transaction → [[031 Database per Service]]
- [ ] Outbox table: `INSERT INTO outbox (event_type, payload)`. Separate process publishes to broker
- [ ] Change Data Capture (CDC): Debezium tails DB transaction log → publishes to Kafka. No app code
- [ ] Outbox relay retries safely, is idempotent, exposes attempt/status timestamps, and alerts on outbox growth
- [ ] Compensating transactions for saga recovery — not automatic, must be designed and reconciled

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

- [ ] Each service owns its data boundary — no unapproved direct cross-service schema access
- [ ] No distributed transactions (2PC) unless explicitly justified — sagas or eventual consistency instead
- [ ] Events are facts, immutable, append-only
- [ ] Consumers are idempotent wherever delivery, replay, or retry can duplicate messages
- [ ] Dead letter queue configured — failed messages not silently dropped
- [ ] Schema evolution planned — backward-compatible additions only
- [ ] Outbox pattern or CDC for reliable event publishing
- [ ] Broker security, HA/recovery, retention, lag, DLQ, replay, and restore are tested

## Decision, Evidence & Exceptions

- Service/data ownership map:
- Broker and delivery guarantee:
- Event/schema compatibility policy:
- Saga/workflow recovery policy:
- Outbox/CDC design:
- Retention, replay, and PII deletion policy:
- Evidence links (schema tests, broker failover, DLQ replay, restore test):
- N/A items and reason:
- Exceptions, approver, and review/expiry date:

---

## Sources

- [[031 Database per Service]] — database ownership and polyglot persistence
- [[033 Saga Pattern]] — distributed transactions via saga
- [[023 Event-Driven Architecture]] — event-driven communication
- [[024 Messaging Patterns]] — message reliability, ordering, idempotency
- [[034 CQRS & Event Sourcing]] — event sourcing and CQRS
- [[Microservice Launch]] — system-wide launch checklist
- RabbitMQ quorum queues — https://www.rabbitmq.com/docs/quorum-queues
- Apache Kafka design and delivery semantics — https://kafka.apache.org/43/design/design/
- AWS transactional outbox — https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/transactional-outbox.html
- AWS saga orchestration — https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/saga-orchestration.html
