---
title: "Cost Optimization for Data Systems"
note_type: capability-topic
capability_area: production-engineering
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - cost-optimization
  - finops
  - cloud-cost
---

# Cost Optimization for Data Systems

> Managing data system costs through architectural choices, resource right-sizing, and continuous monitoring without sacrificing reliability or performance.

## Why This Is a Senior Skill

Mid-level engineers provision resources and move on. Senior engineers understand the cost drivers of data systems, design architectures that separate compute from storage for independent scaling, and implement FinOps practices that keep costs aligned with business value.

The senior challenge is that data systems accumulate cost silently. Storage grows, queries run longer, and unused resources linger. Without active management, data infrastructure becomes the most expensive line item with the least visibility.

## Core Frameworks

### Cost Driver Analysis

| Cost Driver | Typical % of Total | Optimization Levers |
|-------------|-------------------|---------------------|
| Compute: query engines, ML training | 40-60% | Right-sizing, spot instances, auto-scaling |
| Storage: data lakes, databases | 20-30% | Lifecycle policies, compression, tiering |
| Network: data transfer, egress | 10-20% | Minimize cross-region transfers, compression |
| Managed services: orchestration, monitoring | 5-10% | Right-sizing, consolidate tools |

### Storage Tiering Strategy

| Tier | Access Pattern | Cost per GB | Retrieval Time |
|------|---------------|-------------|----------------|
| Hot: SSD, in-memory | Frequent reads and writes | Highest | Milliseconds |
| Warm: HDD, standard object storage | Occasional reads | Medium | Seconds |
| Cold: infrequent access storage | Rare reads, archival | Low | Minutes to hours |
| Glacier: deep archive | Compliance retention, disaster recovery | Lowest | Hours |

### Compute Optimization Techniques

| Technique | Savings | Trade-off |
|-----------|---------|-----------|
| Spot instances | 60-90% vs on-demand | Interruption risk, not for stateful workloads |
| Reserved instances | 30-60% vs on-demand | Commitment for 1-3 years, less flexible |
| Auto-scaling | Variable: matches demand to supply | Complexity, cold start latency |
| Compute-storage separation | Variable: scale independently | Network overhead, architecture complexity |
| Query optimization | 2-10x cost reduction | Engineering effort |

## In Practice

**Separate compute from storage.** A data warehouse that bundles compute and storage forces you to scale both together. Separating them lets you store petabytes cheaply and scale compute only when queries run. This is the single highest-leverage architectural decision for cost optimization.

**Use spot instances for fault-tolerant workloads.** Batch ETL, ML training, and data validation can tolerate interruption. Spot instances cost 60-90% less than on-demand. Design these workloads to checkpoint progress and resume after interruption.

**Implement storage lifecycle policies.** Data that was accessed daily last quarter may be accessed monthly now. Automatically move data from hot to warm to cold storage based on access patterns. Review and adjust policies quarterly.

**Monitor cost per query.** A query that scans 10TB costs 100x more than one that scans 100GB. Monitor the top 10 most expensive queries weekly and optimize them. Partition pruning, predicate pushdown, and materialized views reduce query cost directly.

**Right-size based on actual usage.** Most data systems are over-provisioned. Review CPU, memory, and storage utilization monthly. Downsize systems running at 20% utilization. Auto-scaling handles peaks without permanent over-provisioning.

## Practical Exercise

Optimize costs for a data system you manage:
1. Analyze cost breakdown: what percentage is compute, storage, network?
2. Identify the top 3 most expensive queries or pipelines
3. Apply one optimization to each: partition pruning, spot instances, or lifecycle policies
4. Estimate savings: what is the monthly cost reduction?
5. Design auto-scaling for a workload with variable demand
6. Implement a cost monitoring dashboard: track cost per query, cost per pipeline, and total monthly cost

## Knowledge Connections

- [[software-engineering-note/06_Software_Engineering_Operations/Software Engineering Operations Overview]]: cloud cost management
- [[02_Scaling_and_Performance_Tuning]]: performance optimization reduces cost
- [[01_Distributed_Systems_for_Data]]: compute-storage separation is an architectural choice
- [[01_Data_Architecture/00_overview|Data Architecture]]: architecture decisions drive cost
- [[05_CI_CD_for_Data_and_ML]]: cost testing in CI catches expensive queries

## Common Pitfalls

- Provisioning for peak and forgetting: resources sit at 20% utilization for months
- Ignoring query cost: the top 10 expensive queries often account for 50% of compute spend
- Storing everything in hot tier: data accessed once a year costs 100x more than cold storage
- Spot instances for stateful workloads: interruptions corrupt data, recovery costs more than savings
- Cost optimization as a one-time project: data grows, queries change, costs drift without continuous review

## Key Takeaways

- Compute-storage separation is the highest-leverage architectural decision for cost optimization
- Spot instances save 60-90% for fault-tolerant workloads: ETL, training, validation
- Storage lifecycle policies automatically move data to cheaper tiers based on access patterns
- Monitor cost per query: the top 10 expensive queries often account for 50% of compute cost
- Right-size based on actual utilization, not peak estimates from six months ago
- Senior engineers treat cost as a first-class metric, not an afterthought
