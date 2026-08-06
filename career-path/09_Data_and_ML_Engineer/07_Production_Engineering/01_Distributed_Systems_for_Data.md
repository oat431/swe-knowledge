---
title: "Distributed Systems for Data"
note_type: capability-topic
capability_area: production-engineering
career_path: data-and-ml-engineer
prerequisite:
  - "[[00_overview]]"
tags:
  - career-path
  - data-engineering
  - distributed-systems
  - CAP-theorem
  - consistency
---

# Distributed Systems for Data

> Designing data systems that handle partitioning, replication, and consistency trade-offs appropriate for the use case.

## Why This Is a Senior Skill

Mid-level engineers deploy databases with default settings. Senior engineers choose partitioning strategies that match access patterns, select consistency models that align with business requirements, and understand the failure modes of distributed consensus.

The senior challenge is that distributed systems have no perfect answers: every choice trades one property for another. The right choice depends on the specific use case, not on theoretical ideals.

## Core Frameworks

### CAP Theorem Application

| System Type | CAP Choice | Use Case | Example |
|-------------|-----------|----------|---------|
| CA: Consistent and Available | Sacrifice partition tolerance | Single datacenter, no network partitions | Traditional RDBMS |
| CP: Consistent and Partition-tolerant | Sacrifice availability during partitions | Financial transactions, inventory | ZooKeeper, etcd, HBase |
| AP: Available and Partition-tolerant | Sacrifice consistency during partitions | Analytics, logs, social feeds | Cassandra, DynamoDB, Riak |

### Partitioning Strategies

| Strategy | How It Works | Best For | Risk |
|----------|-------------|----------|------|
| Range partitioning | Keys in contiguous ranges go to same node | Range queries, time-series data | Hot spots if access is skewed |
| Hash partitioning | Hash of key determines node | Uniform distribution, key-value lookups | No range queries |
| Directory partitioning | Lookup table maps keys to nodes | Flexible, dynamic rebalancing | Directory is a bottleneck |
| Composite partitioning | Partition by multiple keys | Multi-dimensional queries | Complex routing logic |

### Consistency Models

| Model | Guarantee | Latency | Use Case |
|-------|-----------|---------|----------|
| Strong: linearizable | All reads see the latest write | Highest | Financial balances, inventory counts |
| Sequential | All nodes see writes in the same order | High | Event sourcing, audit logs |
| Causal | Causally related writes are ordered | Medium | Social feeds, collaborative editing |
| Eventual | All nodes eventually converge | Lowest | Analytics, counters, logs |
| Read-your-writes | A client sees its own writes immediately | Low-medium | User profiles, session state |

## In Practice

**Choose CP for financial systems, AP for analytics.** A bank account balance must be consistent: you cannot allow double-spending during a network partition. An analytics dashboard can tolerate stale data for minutes: availability matters more than instant consistency.

**Partition by access pattern, not by data size.** A 10TB table with uniform access needs more partitions than a 10TB table accessed only by a few keys. Analyze query patterns before choosing a partitioning strategy.

**Use read-your-writes consistency for user-facing systems.** When a user updates their profile, they expect to see the update immediately, even if other users see stale data for seconds. This is weaker than strong consistency but sufficient for most user interactions.

**Understand your consensus algorithm.** If you use ZooKeeper or etcd, understand how leader election works, what happens during a partition, and how many node failures the cluster can tolerate. A 3-node cluster tolerates 1 failure. A 5-node cluster tolerates 2 failures but has higher write latency.

**Plan for split-brain scenarios.** In an AP system, both sides of a partition may accept writes. When the partition heals, conflicts must be resolved. Choose a conflict resolution strategy: last-write-wins, merge, or application-level resolution. Document the choice and test it.

## Practical Exercise

Design a distributed data system for a multi-region e-commerce platform:
1. Choose consistency models for: product catalog, inventory, shopping cart, order history
2. Design a partitioning strategy for the product catalog: what is the partition key?
3. Calculate replication requirements: how many node failures can you tolerate?
4. Design conflict resolution for the shopping cart: what happens when a user updates from two devices simultaneously?
5. Write a failure scenario: a network partition isolates one region. What happens to each data type?

## Knowledge Connections

- [[software-engineering-note/06_Software_Engineering_Operations/Software Engineering Operations Overview]]: distributed systems fundamentals
- [[02_Scaling_and_Performance_Tuning]]: partitioning affects query performance
- [[03_Reliability_and_Fault_Tolerance]]: consistency choices affect failure behavior
- [[01_Data_Architecture/00_overview|Data Architecture]]: distributed systems are an architectural choice
- [[03_Data_Integration_and_Interoperability/00_overview|Data Integration]]: consistency across integrated systems

## Common Pitfalls

- Choosing strong consistency when eventual suffices: 10x latency and cost for no business benefit
- Partitioning by data size instead of access pattern: uniform data does not mean uniform queries
- Ignoring split-brain resolution: both sides of a partition accept writes, conflicts discovered in production
- Underestimating consensus overhead: a 5-node cluster has higher write latency than a 3-node cluster
- Assuming the database handles everything: distributed systems require application-level conflict resolution

## Key Takeaways

- CAP is a trade-off: choose based on business requirements, not technical preference
- Partition by access pattern: uniform data size does not mean uniform access
- Read-your-writes consistency is sufficient for most user-facing systems and cheaper than strong consistency
- Understand your consensus algorithm: leader election and failure tolerance are not magic
- Plan for split-brain: conflict resolution must be designed and tested, not discovered in production
- Senior engineers match consistency cost to business value: strong consistency is expensive and not always necessary
