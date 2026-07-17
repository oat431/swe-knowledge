---
tags:
  - architecture
  - microservices
  - programming
---

# 03 Eventual Consistency Patterns

In microservices, strong consistency across services is impractical (no distributed ACID). Eventual consistency is the default — data across services converges to a consistent state over time. The challenge: how to build systems that work correctly while data is temporarily inconsistent.

## Strong vs Eventual Consistency

| Aspect | Strong (Single DB) | Eventual (Cross-Service) |
|--------|-------------------|--------------------------|
| Guarantee | Read always returns latest write | Read may return stale data temporarily |
| Mechanism | ACID transactions, locks | Async replication, events |
| Availability | Lower (blocks on lock contention) | Higher (no cross-service coordination) |
| Independence | Services coupled to shared DB | Services fully autonomous |
| Trade-off | Immediate consistency | Availability + independence |
| Failure mode | Entire system blocks | Temporary inconsistency window |

**Rule of thumb:** Strong consistency *within* a service boundary, eventual consistency *between* services.

## Patterns for Living with Eventual Consistency

### Read-Your-Own-Writes

After a write, route that user's reads to the source of truth (not a replica/cache). Other users can tolerate stale data — the writing user cannot.

- **Implementation:** sticky sessions to primary, read-from-primary flag with short TTL, or version tokens in the client.
- **Example:** User updates profile → for the next 5s, their profile reads go to the write DB. Everyone else reads from the replica.

### Causal Consistency

If event A causes event B, all observers must see A before B. Without this, you get nonsensical orderings (reply appears before the original message).

- **Implementation:** Vector clocks, Lamport timestamps, or explicit causal ordering in event metadata.
- **Example:** `order.created` must always be processed before `order.shipped` — include a causal dependency ID.

### Monotonic Reads

Once a user sees version N of data, they must never see version < N. Prevents the "flickering" effect where data appears to go backwards.

- **Implementation:** Track the last-seen version per user/session. Route reads to replicas that are at least at that version.

## Conflict Resolution Strategies

When two services or replicas diverge, how do you reconcile?

| Strategy | How It Works | Trade-off |
|----------|-------------|-----------|
| **Last Writer Wins (LWW)** | Highest timestamp wins | Simplest; silent data loss possible |
| **Merge / CRDT** | Conflict-free replicated data types — mathematically guaranteed convergence | No data loss; limited to specific data structures (counters, sets, registers) |
| **Application-level** | Business rules decide the winner | Most flexible; most complex (e.g., highest bid wins, manual review queue) |

**When in doubt:** Use LWW for low-value data, CRDTs for counters/sets, application-level for business-critical conflicts.

## Compensation over Rollback

You can't undo a sent email. You can't un-charge a credit card. Distributed systems don't have `ROLLBACK`.

Instead: **compensating actions**.

| Failed Action | Compensation |
|---------------|-------------|
| Payment charged | Issue refund |
| Email sent | Send correction email |
| Inventory reserved | Release reservation |
| Order confirmed | Cancel order + notify user |

The **Saga pattern** coordinates multi-step workflows with compensation:
1. Each step has a corresponding compensating action.
2. If step N fails, execute compensations for steps N-1 → 1 in reverse.
3. Compensations are idempotent (safe to retry).

## UI Patterns for Eventual Consistency

The frontend must handle the inconsistency window gracefully:

| Pattern | When to Use | Example |
|---------|-------------|---------|
| **Optimistic UI** | High confidence action will succeed | Show "message sent" immediately, reconcile if it fails |
| **Polling / WebSocket** | Need to confirm backend convergence | Poll order status until `confirmed` |
| **"Processing..." states** | Action takes noticeable time | "Your transfer is being processed" with a spinner |

**Key principle:** Be honest with the user. Don't show stale data as if it's current. A "Processing..." state is better than a lie.

## When Strong Consistency IS Needed

Not everything can be eventual. Use strong consistency (DB-level locks, serializable transactions) **within the owning service** for:

- **Financial transactions** — account balance must never go negative
- **Inventory decrements** — can't sell more than you have
- **Unique constraints** — username/email uniqueness

The key: these happen *within one service's database*, not across services. You get ACID because it's a single DB.

## Sources

- Kleppmann, M. — *Designing Data-Intensive Applications* (Ch. 5, 9)
- Vogels, W. — "Eventually Consistent" (2009)
- Shapiro, M. et al. — "Conflict-Free Replicated Data Types" (2011)
