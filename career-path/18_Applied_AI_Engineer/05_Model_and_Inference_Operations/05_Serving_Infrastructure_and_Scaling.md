---
title: "Serving Infrastructure and Scaling"
note_type: capability-topic
capability_area: model-and-inference-operations
career_path: applied-ai-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - applied-ai
  - ai-engineering
  - inference-serving
  - scaling
---

# Serving Infrastructure and Scaling

> Building and operating the layer that actually runs model inference — GPU serving stacks, autoscaling, rate limits, and queueing — so that capacity, cost, and latency behave predictably as traffic grows.

## Why This Is a Senior Skill

Mid-level engineers deploy one model to one endpoint and scale it by hand when it falls over. Senior engineers build a serving platform: they reason in throughput and concurrency per GPU rather than per server, they choose autoscaling signals that match how inference load actually behaves, and they design rate limits and queues as explicit protection mechanisms. They also know where their responsibility ends — this area owns serving mechanics, while SLO definition and incident response live with [[career-path/07_SRE_and_Platform_Engineer/00_overview|SRE and Platform Engineer]], and monitoring tooling with [[career-path/18_Applied_AI_Engineer/03_Evaluation_and_Observability/00_overview|Evaluation and Observability]].

The failure mode is scaling the wrong resource: adding GPUs to a fleet that is bound by queue policy or KV-cache memory spends money without buying latency. Capacity is a diagnosis, not a shopping list.

## Core Frameworks

### The Serving Stack, Layer by Layer

| Layer | Responsibility | Example Tooling |
|---|---|---|
| Gateway / router | Auth, rate limits, routing, retries | API gateways; OpenRouter-style aggregators |
| Inference engine | Batching, KV-cache management, model execution | vLLM, TensorRT-LLM |
| Model weights + runtime | The model artifact and its container | Docker images with pinned model snapshots |
| Hardware | GPUs and their memory/bandwidth budget | H100/A100-class GPUs; smaller GPUs for small models |

### Capacity Vocabulary

| Term | Meaning | Why You Must Know It |
|---|---|---|
| Throughput (tokens/s per GPU) | Steady-state generation capacity | The unit for capacity planning |
| KV-cache size | Memory per concurrent sequence | The binding constraint, not the model weights |
| Max concurrent sequences | Sequences a GPU can hold at once | Latency explodes when exceeded |
| Batch size / max batch tokens | How much work a GPU does per step | Trades throughput against latency |

### Autoscaling Signals

| Signal | Strength | Weakness |
|---|---|---|
| Request rate (RPS) | Simple, predictable | Ignores request-size mix |
| Queue depth | Responds to actual backlog | Lags; already too late by definition |
| GPU utilization | Tracks compute directly | High utilization ≠ available KV-cache memory |
| Concurrent sequences | Tracks the true bottleneck | Requires engine-specific metrics |
| Token throughput | Best overall for generation workloads | Needs engine instrumentation |

### Rate Limiting and Queueing

| Mechanism | What It Protects | Trade-off |
|---|---|---|
| Per-user rate limits | Fairness; abuse containment | Legitimate bursts get rejected |
| Global admission control | The GPU fleet itself | Visible 429s/queueing under load |
| Priority queues | Interactive traffic over batch | Complex; batch starvation risk |
| Backpressure to callers | Upstream systems | Requires caller cooperation |
| Burst-to-API overflow | Your cost ceiling (see [[02_API_vs_Self_Hosted_Tradeoffs]]) | Cost spikes on overflow |

## In Practice

**Plan capacity in concurrent sequences and KV-cache memory, not requests per second.** A GPU's limit is not CPU-bound request handling; it is how many sequences' KV-caches fit in memory. Two workloads with the same RPS but different context lengths need very different fleets. Model the memory per sequence first, then derive RPS capacity from it.

**Scale on a signal that leads the failure, not one that lags it.** Queue depth confirms you are already late; concurrent-sequence headroom tells you before the queue forms. Instrument the serving engine's own metrics — sequences active, KV-cache utilization, prefill queue — and drive the autoscaler from the constraint that actually binds.

**Separate interactive and batch traffic.** Mixing a latency-critical chat endpoint with batch summarization jobs on the same fleet lets batch traffic eat the KV-cache and destroy interactive latency. Give them different pools, or at minimum priority queues with hard admission control on the interactive path.

**Use rate limits as a design tool, not an emergency brake.** Decide per product surface what a legitimate user can consume, and enforce it before the fleet so one abusive or buggy client cannot degrade everyone. The limit should produce a fast, explainable rejection or queue position — never a timeout that burns the caller's patience.

**Treat provider APIs as one more capacity source with its own limits.** When you self-host, API overflow absorbs bursts ([[02_API_vs_Self_Hosted_Tradeoffs]]); when you are API-only, the provider's rate limits and queueing are your capacity plan. Either way, rate-limit errors must be a first-class, budgeted event in the routing layer — see [[06_Provider_Management_and_Model_Routing]].

**Own the deployment path for weights and engine versions with release rigor.** Model snapshots, inference-engine versions, and GPU driver/kernel combinations all interact in ways that change throughput and even output determinism. Version them together, stage changes through canary, and roll back atomically — inference serving is release engineering, not a file upload.

## Practical Exercise

Right-size and protect one self-hosted serving deployment:

1. Profile the workload: RPS, average and p95 context length, output length.
2. Compute KV-cache bytes per sequence for your model, and the maximum concurrent sequences per GPU.
3. Derive the GPU count for your peak concurrency with 30% headroom.
4. Choose an autoscaling signal (concurrent sequences or token throughput) and configure min/max replicas.
5. Add per-user and global rate limits with an explicit rejected-request experience.
6. Load-test to peak; verify p95 latency and that interactive traffic survives a batch-traffic spike.
7. Write the runbook: scale-up/down triggers, overflow-to-API threshold, rollback steps.

## Knowledge Connections

- [[career-path/07_SRE_and_Platform_Engineer/00_overview|SRE and Platform Engineer]]: SLO definition and incident response
- [[career-path/09_Data_and_ML_Engineer/00_overview|Data and ML Engineer]]: classic MLOps for trained models
- [[career-path/18_Applied_AI_Engineer/00_overview|Applied AI Engineer]]: path positioning
- [[02_API_vs_Self_Hosted_Tradeoffs]]: overflow-to-API is a capacity decision
- [[04_Latency_Engineering]]: the latency budgets that serving must hold
- [[06_Provider_Management_and_Model_Routing]]: rate limits and health signals execute in the router
