---
title: "26 Maximum Flow"
tags:
  - max-flow
  - ford-fulkerson
  - edmonds-karp
  - min-cut
  - graph-algorithms
  - clrs
source: CLRS
---

# 26 · Maximum Flow

> Find the maximum flow from source to sink. The Max-Flow Min-Cut theorem states: maximum flow value = minimum cut capacity.

### Flow Network Definition

A **flow network** $G = (V, E)$ is a directed graph where:
- Each edge $(u, v)$ has a nonnegative **capacity** $c(u, v) \geq 0$
- Distinguished **source** $s$ and **sink** $t$
- Every vertex lies on some path from $s$ to $t$

**Flow** $f: V \times V \to \mathbb{R}$ satisfies:
1. **Capacity constraint:** $0 \leq f(u, v) \leq c(u, v)$ for all $u, v$
2. **Flow conservation:** $\sum_{v} f(v, u) = \sum_{v} f(u, v)$ for all $u \neq s, t$

**Flow value:** $|f| = \sum_{v \in V} f(s, v) - \sum_{v \in V} f(v, s)$

### Ford-Fulkerson Method

**Core idea:** Iteratively find **augmenting paths** in the **residual network** and push flow along them.

```
FORD-FULKERSON-METHOD(G, s, t)
1  initialize flow f to 0
2  while there exists an augmenting path p in the residual network G_f
3      augment flow f along p
4  return f
```

### Key Concepts

#### Residual Network

Given flow $f$, the residual network $G_f = (V, E_f)$ has edges with **residual capacity**:

$$c_f(u, v) = \begin{cases} c(u, v) - f(u, v) & \text{if } (u, v) \in E \\ f(v, u) & \text{if } (v, u) \in E \\ 0 & \text{otherwise} \end{cases}$$

**Reverse edges** allow algorithms to "undo" flow decisions.

#### Augmenting Path

A path from $s$ to $t$ in $G_f$. The **bottleneck** capacity of path $p$ is:
$$c_f(p) = \min\{c_f(u, v) : (u, v) \text{ is on } p\}$$

#### Augmenting Flow Along a Path

```
AUGMENT(f, p)
1  c_f(p) = min{c_f(u, v) : (u, v) ∈ p}
2  for each edge (u, v) ∈ p
3      if (u, v) ∈ E
4          f(u, v) = f(u, v) + c_f(p)      // forward edge: add flow
5      else
6          f(v, u) = f(v, u) - c_f(p)      // backward edge: subtract flow
```

### Max-Flow Min-Cut Theorem

A **cut** $(S, T)$ partitions $V$ with $s \in S$, $t \in T$.

**Net flow across cut:** $f(S, T) = \sum_{u \in S} \sum_{v \in T} f(u, v) - \sum_{u \in S} \sum_{v \in T} f(v, u)$

**Capacity of cut:** $c(S, T) = \sum_{u \in S} \sum_{v \in T} c(u, v)$

> **Max-Flow Min-Cut Theorem:** The following are equivalent:
> 1. $f$ is a maximum flow
> 2. The residual network $G_f$ contains no augmenting path
> 3. $|f| = c(S, T)$ for some cut $(S, T)$

**Corollary:** Maximum flow value = minimum cut capacity.

### Basic Ford-Fulkerson Algorithm

Uses BFS to find augmenting paths (also known as **Edmonds-Karp** when using BFS).

```
EDMONDS-KARP(G, s, t)
1  initialize flow f to 0
2  while true
3      BFS in G_f to find shortest augmenting path p
4      if p doesn't exist
5          return f
6      AUGMENT(f, p)
```

| Property | Value |
|---|---|
| **Time** | $O(VE^2)$ |
| **Augmenting paths** | At most $O(VE)$ |
| **BFS per iteration** | $O(E)$ |

**Why BFS?** Shortest augmenting paths guarantee that the number of augmentations is bounded by $O(VE)$.

### Bipartite Matching (§26.3)

**Maximum bipartite matching** reduces to maximum flow:
1. Add source $s$ connected to all left vertices (capacity 1)
2. Add sink $t$ connected from all right vertices (capacity 1)
3. Original edges have capacity 1
4. Maximum flow = maximum matching size

| Property | Value |
|---|---|
| **Time** | $O(VE)$ using Edmonds-Karp |

### Push-Relabel Method (§26.4)

Alternative to augmenting-path methods. Maintains a **preflow** (allows excess flow at vertices) and uses local operations.

**Key concepts:**
- **Preflow:** Satisfies capacity constraint and $f(V, u) \geq 0$ for all $u \neq s$ (excess allowed)
- **Height function** $h: V \to \mathbb{N}$: $h(s) = |V|$, $h(t) = 0$
- **Push:** Send flow from a vertex with excess to a neighbor with lower height
- **Relabel:** Raise height of a vertex with excess but no eligible neighbor

```
PUSH(u, v)           // applicable when u has excess, h(u) = h(v) + 1
1  Δ = min(u.e, c_f(u, v))
2  send Δ flow from u to v

RELABEL(u)           // applicable when u has excess, no eligible neighbor
1  h(u) = 1 + min{h(v) : (u, v) ∈ E_f}
```

### Relabel-to-Front (§26.5)

A specific, efficient implementation of push-relabel using a neighbor list.

| Property | Value |
|---|---|
| **Time** | $O(V^3)$ |
| **Space** | $O(V + E)$ |

### Max-Flow Algorithm Summary

| Algorithm | Time | Method |
|---|---|---|
| Ford-Fulkerson (BFS / Edmonds-Karp) | $O(VE^2)$ | Shortest augmenting path |
| Push-relabel (generic) | $O(V^2 E)$ | Preflow + local ops |
| Relabel-to-front | $O(V^3)$ | Push-relabel + ordering |

---

## Cross-Chapter Connections

| Concept | MST (Ch23) | Shortest Path (Ch24–25) | Max Flow (Ch26) |
|---|---|---|---|
| **Greedy** | Kruskal, Prim | Dijkstra | — |
| **Dynamic programming** | — | Floyd-Warshall | — |
| **Relaxation** | — | Core operation | — |
| **Graph type** | Undirected | Directed | Directed |
| **Optimality** | Cut property | Relaxation theorem | Max-flow min-cut |
| **Union-Find** | Kruskal | — | — |
| **BFS** | — | Unweighted SP | Edmonds-Karp |

### Common Techniques
- **Greedy:** Kruskal, Prim, Dijkstra — locally optimal → globally optimal with right invariant
- **Relaxation:** Progressive improvement of estimates (all SP algorithms)
- **Augmenting paths:** Ford-Fulkerson method — find path → push flow → repeat
- **Cut-based arguments:** MST cut property, max-flow min-cut theorem

---

## Practice Problems

1. **MST Uniqueness:** Prove that if all edge weights are distinct, the MST is unique.
2. **Second-best MST:** Find the MST with second-lowest weight in $O(V^2)$ time.
3. **Negative cycle detection:** Modify Bellman-Ford to print the negative cycle.
4. **Floyd-Warshall path reconstruction:** Modify to store predecessors and reconstruct all-pairs shortest paths.
5. **Min-cut extraction:** Given a max flow, extract the actual min-cut $(S, T)$.
6. **Edge-disjoint paths:** Show that the maximum number of edge-disjoint $s$-$t$ paths equals the max flow (with unit capacities).

---

*Source: CLRS, Introduction to Algorithms, Chapters 23–26.*

## Related

- [[Algorithm Overview]] — Practical graph algorithms (BFS, DFS, topological sort)
- [[01 Graphs]] — Graph representations and basic traversals
- [[02 Greedy Algorithms]] — Greedy approach used in MST (Kruskal's, Prim's)
- [[02_Advanced_Data_Structures]] — Disjoint sets for Kruskal's, Fibonacci heaps for Prim's
- [[01_Amortized_Analysis]] — Amortized analysis of priority queue operations


---

## Hands-On Exercises

### Exercise 4: Ford-Fulkerson Max Flow — Trace Augmenting Paths
Given flow network:
```
     S --16--> A --10--> T
     |          ↑        ↑
     13         4        20
     ↓          |        |
     B --9--→ C --7--→ T
```

More precisely:
- S→A: 16, S→B: 13
- A→C: 4, A→T: 10
- B→C: 9, B→T: 14 (wait, let me redo)

Actually use: S→A:10, S→B:10, A→B:2, A→T:10, B→T:10.

1. Find augmenting paths using BFS (Edmonds-Karp).
2. For each path, find bottleneck capacity and augment.
3. Draw the residual network after each augmentation.
4. What is the max flow? Identify the min-cut (S, T).

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 6 | [Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/) (LC 778) | 🔴 Hard | Dijkstra's / Union-Find |
| 7 | [Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/) (LC 332) | 🔴 Hard | Eulerian Path (graph theory) |
| 8 | **Implement Edmonds-Karp Max Flow** | 🔴 Code | Ford-Fulkerson + BFS |

### Assignment Guidelines
- **Problem 3** is a coding project — implement Edmonds-Karp and test on flow networks.
- **Target time:** 30 min per Hard, 45 min for Max Flow project.