---
title: "Provider Management and Model Routing"
note_type: capability-topic
capability_area: model-and-inference-operations
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - model-routing
  - provider-management
---

# Provider Management and Model Routing

> Operating a portfolio of model providers behind one routing layer — fallbacks, A/B and canary promotion, eval-gated routing, and continuous provider-health assessment — so that no single vendor is a single point of failure and every request gets a good-enough model.

## Why This Is a Senior Skill

Mid-level engineers hardcode one provider and one model, and discover the vendor's outage from their users. Senior engineers run a routing layer as the product's model-facing surface: providers and models are interchangeable entries in configuration, promotion of any new model is gated by evaluation, and degradation of any provider is absorbed by fallbacks the system exercises before it needs them. Routing is where the decisions from every other topic in this area — selection, cost, latency, capacity — are executed per request.

The failure mode is a routing layer that exists in the architecture diagram but not in production: fallbacks that have never run and health signals nobody wired up are paperwork, not resilience.

## Core Frameworks

### Routing Decision Dimensions

| Dimension | Questions It Answers |
|---|---|
| Task fit | Which model meets the quality bar for this request class? ([[01_Model_Selection_and_Benchmarks]]) |
| Cost | Which eligible model is cheapest per task? ([[03_Cost_Optimization]]) |
| Latency | Which eligible provider is currently fastest, including queueing? ([[04_Latency_Engineering]]) |
| Availability | Which providers are healthy right now? |
| Quota | Which providers still have rate-limit headroom this window? |

### Routing Strategy Comparison

| Strategy | How It Works | Maturity |
|---|---|---|
| Static pinning | One model per task class, hardcoded | Entry level; any failure is a product failure |
| Ordered fallbacks | Primary → secondary → last-resort list | Baseline; order is fixed, not adaptive |
| Rule-based routing | Route on task class, tenant, data sensitivity | Standard; explicit and auditable |
| A/B and canary | Split traffic between candidates; compare on live metrics | Required for any model change |
| Eval-gated / adaptive routing | Score candidates offline; monitor live quality per route and reweight | Mature; routing follows evidence |

### Provider Health Signals

| Signal | What It Detects |
|---|---|
| Error rate by code (429/5xx/timeouts) | Overload, outage, capacity exhaustion |
| Latency drift (p50 and p95) | Silent degradation before hard failure |
| Rate-limit headroom | Approaching quota exhaustion |
| Quality proxy (refusals, malformed outputs, eval spot-checks) | Model behavior change under the same ID |

### The Promotion Workflow

| Stage | Gate |
|---|---|
| Candidate identified | Passes offline eval suite ([[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/00_overview|Evaluation and Observability]]) |
| Shadow / dry-run | Side-by-side output comparisons on live traffic |
| Canary 5% | Live metrics within tolerance of incumbent |
| Ramp 25% → 50% → 100% | Each step gated on quality, cost, and latency deltas |
| Rollback | Pre-defined trigger thresholds and instant config revert |

## In Practice

**Build the routing layer before you need the second provider.** Migrating an application off a hardcoded provider during an outage is the worst time to introduce an abstraction. A thin routing layer — many teams use OpenRouter or LiteLLM-style gateways — makes providers interchangeable from day one, so the first fallback is a configuration change, not a re-architecture.

**Make fallbacks exercised, not theoretical.** A fallback path that has never run in production will fail exactly when you need it — it will reference a deprecated model ID or a quota you never reserved. Periodically force a small fraction of traffic through each secondary route, or run chaos drills that fail the primary, so the fallback ladder stays real.

**Gate every model promotion on evaluation, then ramp on live metrics.** No model enters traffic because a leaderboard or a sales pitch recommends it. Promotion follows the staged workflow — offline suite, shadow, canary, ramp — with pre-agreed rollback thresholds on quality, cost, and latency. An ungated swap is how a cost optimization or an "upgrade" becomes an incident.

**Route by task and tenant before routing by price.** The routing decision starts with the constraints — which models are eligible for this request class, which tenants require data residency — and only then minimizes cost or latency among the eligible. Price-first routing sends regulated or hard requests to models that fail them silently.

**Monitor provider health continuously and act on degradation, not just outage.** Providers degrade before they fail: rising 429s, drifting p95s, and retuned model behavior all precede an incident. Wire health signals into the routing layer so it sheds traffic from a degrading provider automatically, and into your alerts so a human investigates the drift.

**Treat every routing decision as data to feed the next one.** Log which model and provider served each request, with cost, latency, and quality proxies, back into the eval pipeline. Routing strategy then improves from evidence instead of anecdotes — the same loop that turns [[computing-foundation-note/Artificial_Intelligence/12_AI_ROI_and_Roadmap|ROI tracking]] from a report into a control system.

## Practical Exercise

Build the provider-routing layer for a feature that currently uses one hardcoded provider:

1. Enumerate the request classes and their eligibility constraints (task difficulty, data residency, budget).
2. Choose a routing abstraction (gateway library or service) and define model + provider as configuration.
3. Onboard a second provider for the same or a comparable model; reserve and verify quota.
4. Implement ordered fallbacks with circuit breaking on 5xx/429/timeout signals.
5. Force a failover drill: disable the primary and confirm the product degrades gracefully.
6. Add a canary path for a candidate model with a 5% ramp and rollback thresholds.
7. Instrument per-route cost, latency, and quality proxies; review weekly and adjust weights.

## Knowledge Connections

- [[career-path/18_Applied_AI_Engineer/00_overview|Applied AI Engineer]]: path positioning
- [[01_Model_Selection_and_Benchmarks]]: task fit is the first routing dimension
- [[03_Cost_Optimization]]: cost is a routing dimension, not an afterthought
- [[04_Latency_Engineering]]: latency and provider queueing feed routing health
- [[05_Serving_Infrastructure_and_Scaling]]: rate limits and quotas the router must respect
- [[computing-foundation-note/Artificial_Intelligence/12_AI_ROI_and_Roadmap]]: the ROI loop closes on routing data
- [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/00_overview|Evaluation and Observability]]: the eval gates in the promotion workflow
