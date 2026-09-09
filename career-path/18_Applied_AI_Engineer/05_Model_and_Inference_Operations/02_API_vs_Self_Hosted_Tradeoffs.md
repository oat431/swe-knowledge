---
title: "API vs Self-Hosted Tradeoffs"
note_type: capability-topic
capability_area: model-and-inference-operations
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - self-hosting
  - unit-economics
---

# API vs Self-Hosted Tradeoffs

> Deciding whether to rent intelligence through a provider API or run open-weight models on your own infrastructure, on explicit unit economics plus control, privacy, and capability requirements.

## Why This Is a Senior Skill

Mid-level engineers pick a side on ideology — "APIs are overpriced" or "self-hosting is an ops tax" — and apply it everywhere. Senior engineers compute the decision per workload and re-compute it as prices and hardware move: they know the utilization at which a GPU fleet beats API pricing, they price the operational burden honestly, and they design hybrid estates where both modes coexist behind one routing layer ([[06_Provider_Management_and_Model_Routing]]).

The failure mode is deciding once and never revisiting: prices, open-weight releases, and your own traffic mix all move, and a decision frozen in time becomes wrong quietly. The answer should carry an expiry date and a re-evaluation trigger.

## Core Frameworks

### The Full Comparison Ledger

| Dimension | Provider API | Self-Hosted (Open Weights) |
|---|---|---|
| Cost structure | Per token, zero at idle | Fixed (hardware) + variable (power, ops) |
| Cost at low volume | Cheapest option | Wasteful — GPUs idle |
| Cost at sustained high volume | Linear with traffic | Can break even and win beyond a utilization point |
| Control over model behavior | Parameters only; providers change models underneath you | Full: quantization, sampling, system prompts, fine-tuning |
| Privacy / data residency | Data leaves your boundary (varies by provider policy) | Data stays in your VPC or on-prem |
| Model availability | Frontier and reasoning models often API-only | Only open-weight models; usually one generation behind |
| Operational burden | Provider carries capacity, upgrades, scaling | You own GPUs, drivers, serving stack, upgrades, on-call |
| Procurement | Usage-based, no capital commitment | Upfront capital or long-term instance reservations |

### When Each Mode Wins

| Signal | Winner | Why |
|---|---|---|
| Low or spiky volume, early product | API | You pay only for what you use; zero idle cost |
| Frontier capability is the product | API | The strongest models are largely closed or impractical to self-host |
| Sustained high volume, simple task | Self-hosted | Unit cost per token drops below API price at scale |
| Strict data residency or regulatory isolation | Self-hosted | Tokens never leave your boundary |
| Fine-tuning or sampling control needed | Self-hosted | Open weights are the prerequisite |
| Small team, no infrastructure appetite | API | Ops burden has real people cost |

### The Unit-Economics Check

| Step | Computation |
|---|---|
| 1. Workload profile | Requests/s at peak, tokens per request, context-length mix |
| 2. GPU requirement | Concurrent capacity from peak tokens/s divided by per-GPU throughput |
| 3. Cost per token, self-hosted | Fleet cost (amortized) divided by tokens actually served — not theoretical peak |
| 4. Compare | API $/token vs self-hosted $/token at your real utilization |
| 5. Re-check triggers | Price cuts, new open-weight releases, traffic growth — re-run quarterly |

The trap in this table is comparing API cost against theoretical GPU throughput. Real utilization often sits at 30–50%; the comparison must use tokens actually served.

### Hybrid Patterns

| Pattern | Description | Use When |
|---|---|---|
| API-first, self-host later | Start on API; move volume to self-hosted once the utilization math flips | Uncertain growth |
| Split by task | Small open model for volume tasks, API for hard tasks | Mixed workload |
| Split by tenant | Regulated tenants self-hosted, rest on API | Data-residency requirements |
| Overflow / burst | Self-hosted base capacity, API absorbs peaks and failures | Predictable base load with spikes |

## In Practice

**Decide on unit economics at realistic utilization, not sticker prices.** Compute the cost per token of a GPU fleet at 40% utilization, including power, ops time, and reserve capacity, and compare it with API pricing at your actual volume mix. If the self-hosted number only wins at 90% utilization, you will never achieve — you are buying an ops team in order to pay more.

**Price the operational burden as a line item, not as "free engineering time".** A GPU fleet means driver upgrades, serving-stack maintenance, capacity planning, and on-call. Translate that into person-weeks per quarter and put it in the comparison; pretending it is zero is how teams end up self-hosting a model they barely use.

**Use the quiet-model-update policy as a control criterion.** API providers retune and swap model versions without notice, which changes behavior and invalidates your evals. If behavioral determinism matters to the product, self-hosting a pinned weight snapshot is the only way to get it — the API side mitigates through version pinning and regression gates instead.

**Let privacy and data residency override pure economics.** If customer data cannot leave your boundary — contractually, regulatorily, or reputationally — the API is not an option regardless of price. Classify workloads by data sensitivity first; only run the economics on workloads eligible for either mode.

**Design the system so the mode is a routing decision, not an architecture.** A routing layer that can send a request to either a provider API or your own endpoint ([[06_Provider_Management_and_Model_Routing]]) makes the API/self-hosted choice reversible and lets you migrate workload class by class, measuring as you go. Hard-coding either mode is the expensive way to be wrong.

## Practical Exercise

Build the buy-vs-build case for one workload:

1. Profile the workload: peak requests/s, average and p95 tokens per request, context-length mix.
2. Estimate the GPU fleet needed at your peak, and its monthly amortized cost.
3. Compute the self-hosted cost per token at 30%, 50%, and 80% utilization.
4. Get current API pricing for a comparable open-weight model (most providers host open models too).
5. Add the ops burden: on-call rotation, upgrade cadence, capacity-planning effort per quarter.
6. Decide per workload class; document the assumptions and the trigger points for re-evaluation.
7. If the answer is hybrid, define which task class moves first and under what success metric.

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/12_AI_ROI_and_Roadmap]]: TCO and build-vs-buy framing
- [[career-path/18_Applied_AI_Engineer/00_overview|Applied AI Engineer]]: path positioning
- [[01_Model_Selection_and_Benchmarks]]: which model classes are even self-hostable
- [[05_Serving_Infrastructure_and_Scaling]]: the stack you must run if you self-host
- [[06_Provider_Management_and_Model_Routing]]: the layer that makes both modes interchangeable
