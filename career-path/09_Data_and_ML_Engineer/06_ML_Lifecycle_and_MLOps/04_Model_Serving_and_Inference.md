---
title: "Model Serving and Inference"
note_type: capability-topic
capability_area: ml-lifecycle-and-mlops
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - model-serving
  - inference
  - mlops
---

# Model Serving and Inference

> Deploying trained models to production in a pattern that matches latency, throughput, and freshness requirements.

## Why This Is a Senior Skill

Mid-level engineers deploy a model to an endpoint. Senior engineers choose between batch, real-time, and streaming serving patterns based on the use case, design deployment strategies that minimize risk during model updates, and manage model serialization across framework versions.

The senior challenge is matching the serving pattern to the business requirement without over-engineering. Real-time serving costs 10x more than batch and is not always necessary.

## Core Frameworks

### Serving Pattern Decision Matrix

| Pattern | Latency | Throughput | Freshness | Cost | Best For |
|---------|---------|-----------|-----------|------|---------|
| Batch prediction | Hours | Very high | Low | Low | Daily recommendations, nightly scoring |
| Real-time REST | 10-100ms | Medium-high | High | Medium | Fraud detection, personalization |
| Streaming inference | Sub-second | High | Very high | High | Real-time anomaly detection |
| Edge deployment | Sub-millisecond | Low-medium | Variable | Variable | Mobile, IoT, low-connectivity |
| Embedded in application | Sub-millisecond | Application-bound | High | Low | Simple models, feature flags |

### Deployment Strategy Comparison

| Strategy | Risk | Rollback Speed | Traffic Split | Complexity |
|----------|------|---------------|---------------|-----------|
| Blue-green | Medium: full cutover | Fast: switch traffic | 0% or 100% | Low |
| Canary | Low: gradual increase | Fast: shift traffic back | 1%, 5%, 25%, 100% | Medium |
| A/B test | Low: controlled comparison | Medium: analyze and switch | 50/50 or configured | High: requires experiment framework |
| Shadow | Very low: no user impact | Not applicable: shadow only | 100% to both, one is shadow | Medium: double compute |
| Rolling update | Medium: mixed versions | Slow: must roll back all instances | Gradual instance replacement | Low |

### Model Serialization Options

| Format | Framework | Size | Portability | Inference Speed |
|--------|-----------|------|-------------|----------------|
| Pickle | Python/PyTorch | Large | Low: Python-only | Baseline |
| ONNX | Cross-framework | Medium | High | Optimized |
| TorchScript | PyTorch | Medium | Medium | Optimized |
| SavedModel | TensorFlow | Large | Medium: TF ecosystem | Optimized |
| TensorRT | NVIDIA GPU | Medium | Low: NVIDIA only | Fastest on GPU |
| PMML | Legacy/Java | Large | High: Java ecosystem | Baseline |

## In Practice

**Start with batch unless you need real-time.** Most ML use cases can tolerate hours of staleness. Batch prediction on a schedule is simpler, cheaper, and easier to debug. Move to real-time only when the business case demands it.

**Canary deployments are the default for production models.** Never replace a production model in one step. Route 1% of traffic to the new model, monitor for 24 hours, increase to 5%, then 25%, then 100%. If metrics degrade at any step, roll back immediately.

**Shadow test before cutting over.** Run the new model in shadow mode alongside the current model. Compare predictions without exposing users to the new model. This catches correctness issues without user impact.

**Optimize inference cost separately from accuracy.** A model that is 0.5% more accurate but 10x more expensive to serve may not be worth it. Profile inference latency and cost before committing to a serving pattern. Quantization and distillation can reduce cost with minimal accuracy loss.

**Handle model dependencies explicitly.** Models depend on feature stores, embedding tables, and external APIs. If a dependency is unavailable, the model cannot serve. Design graceful degradation: return a default prediction or cached result rather than failing the request.

## Practical Exercise

Design a serving strategy for a model you are working on:
1. Choose a serving pattern and justify it against the decision matrix
2. Design the deployment strategy: how do you roll out a new model version safely?
3. Choose a serialization format and benchmark inference latency
4. Define rollback criteria: what metrics trigger an automatic rollback?
5. Design graceful degradation: what happens when a dependency is unavailable?
6. Estimate serving cost at expected traffic volume

## Knowledge Connections

- [[computing-foundation-note/Artificial_Intelligence/AI Overview]]: model deployment fundamentals
- [[02_Feature_Engineering_and_Feature_Store]]: online feature serving for real-time inference
- [[03_Model_Training_and_Evaluation]]: evaluation informs serving decisions
- [[05_ML_Monitoring_and_Drift]]: monitoring deployed models for degradation
- [[07_Production_Engineering/00_overview|Production Engineering]]: infrastructure for model serving

## Common Pitfalls

- Defaulting to real-time serving when batch suffices: 10x cost increase for no business benefit
- No rollback plan: a bad model in production with no way to revert within minutes
- Ignoring model dependency failures: feature store down means model cannot serve predictions
- Serialization format mismatch: model trained in PyTorch, served with TensorFlow without conversion testing
- Cold start latency ignored: first request after deployment takes 10x longer than steady-state

## Key Takeaways

- Batch serving is the default: move to real-time only when the business case demands it
- Canary deployments with gradual traffic increase are the standard for production model updates
- Shadow testing catches correctness issues without user impact before cutover
- Inference cost is a first-class concern: profile it before committing to a serving pattern
- Model dependencies must have fallback paths: graceful degradation beats hard failures
- Senior engineers match serving pattern to business requirement, not to technical preference
