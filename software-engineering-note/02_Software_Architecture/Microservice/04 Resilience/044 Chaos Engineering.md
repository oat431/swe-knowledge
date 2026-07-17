---
tags:
  - architecture
  - microservices
  - sre
  - programming
---

# 04 Chaos Engineering

Hoping your system handles failure isn't a strategy. Chaos engineering deliberately injects controlled failures to prove resilience works — before a real incident proves it doesn't.

## The Scientific Method

Chaos engineering follows a disciplined loop — not random destruction:

1. **Define steady state** — normal system behavior measured by SLIs (latency, error rate, throughput)
2. **Hypothesize** — "the system tolerates X failure without breaching SLOs"
3. **Inject failure** — introduce the fault in a controlled way
4. **Observe** — watch dashboards, alerts, and user-facing metrics
5. **Verify or disprove** — did steady state hold?
6. **Fix what broke** — if the hypothesis failed, harden the system and re-run

> If you can't measure steady state, you can't do chaos engineering. Observability comes first.

## Steady State Hypothesis

Use **business metrics**, not infrastructure metrics. The user doesn't care about CPU — they care about whether their order went through.

| ✅ Good Steady State | ❌ Bad Steady State |
|---|---|
| Order completion rate stays above 99% | CPU stays below 80% |
| p99 latency < 500ms for checkout | No pod restarts |
| Login success rate > 99.5% | Memory usage < 70% |
| Payment processing continues within 2s | Disk I/O stays normal |

Business metrics prove the system **works for users**. Infra metrics are symptoms, not outcomes.

## Types of Failure Injection

| Failure Type | What You Inject | What You Verify |
|---|---|---|
| Pod/container kill | Terminate random pods | Auto-restart, circuit breakers activate |
| Network latency/partition | Add delay or drop packets between services | Timeouts fire, retries work correctly |
| Dependency failure | Kill a downstream service or database | Fallbacks trigger, graceful degradation |
| Resource exhaustion | Starve CPU, memory, or disk | Resource limits hold, backpressure engages |
| DNS failure | Block or corrupt DNS resolution | DNS caching works, retry with backoff |

## Blast Radius

**Start small. Widen progressively.**

```
Stage 1: One pod in staging         → safe to learn
Stage 2: One pod in production      → real traffic, minimal impact
Stage 3: One availability zone      → test redundancy across AZs
Stage 4: Regional failure           → only after stages 1-3 pass consistently
```

Never start with "kill everything in production." Progressive widening builds confidence and limits damage when experiments reveal real weaknesses.

## Tools

| Tool | Type | Notes |
|---|---|---|
| Litmus Chaos | K8s native, CNCF | Declarative experiments as CRDs, broad fault library |
| Chaos Monkey | Netflix OSS | Random instance termination, the OG chaos tool |
| Gremlin | SaaS, enterprise | UI-driven, team collaboration, attack scheduling |
| Chaos Mesh | K8s, CNCF Sandbox | Fine-grained fault injection, time skew, I/O faults |
| AWS FIS | Managed (AWS) | Native integration with EC2, ECS, EKS, RDS |

## Game Days

Scheduled chaos experiments with the **whole team watching**. Not a surprise — a practice drill.

**How to run a Game Day:**

1. Pick a hypothesis to test (e.g., "checkout survives payment-service failure")
2. Brief the team — everyone knows what's happening and when
3. Inject the failure
4. Practice incident response in real-time
5. Document findings — what worked, what didn't
6. Create action items to fix weaknesses found
7. Re-run after fixes to verify

Game Days build muscle memory. When a real incident hits at 3 AM, the response is automatic.

## Rules

- **Always have a kill switch** — abort the experiment instantly if impact exceeds expectations
- **Start in non-production** — prove the experiment is safe before touching prod
- **Inform stakeholders** — no surprise chaos in production, ever
- **Automate experiments in CI** — chaos as code, run on every deploy
- **Never chaos-test without monitoring in place** — if you can't observe it, you can't learn from it

## Sources

- [Principles of Chaos Engineering](https://principlesofchaos.org)
- [Netflix - Chaos Monkey](https://netflix.github.io/chaosmonkey/)
- [CNCF Litmus](https://litmuschaos.io)
