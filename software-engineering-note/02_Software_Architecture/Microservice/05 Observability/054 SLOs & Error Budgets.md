---
tags:
  - architecture
  - microservices
  - sre
  - programming
---

# 05 SLOs & Error Budgets

SLOs replace vague reliability targets with measurable contracts. Instead of "the system should be fast", you get "99.9% of requests complete in < 200ms." When you violate this, you know exactly how much budget you've burned.

---

## SLI → SLO → SLA Hierarchy

| Layer | What it is | Example |
|-------|-----------|---------|
| **SLI** (Service Level Indicator) | The metric you measure | Request latency p99, availability % |
| **SLO** (Service Level Objective) | The target you aim for | 99.9% availability over 30 days |
| **SLA** (Service Level Agreement) | The contract with consequences | Refund if SLO missed for 2+ months |

> **Rule:** SLO should always be stricter than SLA. The gap is your buffer — it gives you time to react before breaching a contractual obligation.

---

## Common SLIs for Microservices

| SLI | Formula | Good threshold |
|-----|---------|---------------|
| Availability | successful requests / total requests | 99.9% |
| Latency (p50) | median response time | < 50ms |
| Latency (p95) | 95th percentile response time | < 150ms |
| Latency (p99) | 99th percentile response time | < 500ms |
| Error rate | 5xx responses / total responses | < 0.1% |
| Throughput | requests per second sustained | depends on capacity plan |

---

## Error Budget

```
Error Budget = 100% - SLO target
```

Your error budget is the **allowed unreliability**. It's not a target to hit — it's a budget to spend on deployments, experiments, and inevitable failures.

| SLO Target | Error Budget | Downtime/month | Downtime/year |
|-----------|-------------|----------------|---------------|
| 99% | 1% | 7.2 hours | 3.65 days |
| 99.5% | 0.5% | 3.6 hours | 1.83 days |
| 99.9% | 0.1% | 43.2 minutes | 8.76 hours |
| 99.95% | 0.05% | 21.6 minutes | 4.38 hours |
| 99.99% | 0.01% | 4.3 minutes | 52.6 minutes |

---

## Burn Rate Alerting

**Burn rate** = how fast you're consuming your error budget relative to the window.

- Burn rate **1** → consuming budget at exactly the sustainable pace (budget hits 0 at window end)
- Burn rate **> 1** → unsustainable, budget will exhaust before window ends
- Burn rate **< 1** → under budget, healthy

### Multi-Window Burn Rate Alerts

| Burn Rate | Window | Severity | Action |
|-----------|--------|----------|--------|
| 14.4x | 1 hour (5min short) | Critical | **Page on-call** |
| 6x | 6 hours (30min short) | High | **Page on-call** |
| 3x | 1 day (2hr short) | Medium | **Create ticket** |
| 1x | 3 days (full window) | Low | **Review next sprint** |

> Multi-window burn rates are more actionable than static threshold alerts because they account for both speed and duration of the problem.

---

## Error Budget Policy

What happens when budget is exhausted:

1. **Freeze feature releases** — no new deployments until budget recovers
2. **Focus on reliability work** — all engineering effort goes to stability
3. **Conduct postmortem** — identify what consumed the budget
4. **Resume features** — only when budget has recovered to a safe margin (e.g., > 50%)

The policy must be agreed upon by engineering AND product leadership before incidents happen. It's a pre-negotiated social contract.

---

## Practical Example: Order Service

| SLI | Target (SLO) | Window | Error Budget |
|-----|-------------|--------|-------------|
| Availability (2xx) | 99.9% | 30 days | 43.2 min downtime |
| Latency p99 | < 300ms | 30 days | 0.1% requests can exceed |
| Error rate (5xx) | < 0.1% | 30 days | ~4,320 errors per 4.32M requests |
| Correctness (order placed = order stored) | 99.99% | 30 days | 4.3 min of inconsistency |

If a bad deploy causes 20 minutes of 500s → you've burned **46% of your availability budget**. One more incident of similar size and you're frozen.

---

## Sources

- [Google SRE Book — Service Level Objectives](https://sre.google/sre-book/service-level-objectives/)
- [Datadog SLO Documentation](https://docs.datadoghq.com/service_management/service_level_objectives/)
- Alex Hidalgo — *Implementing Service Level Objectives* (O'Reilly)
