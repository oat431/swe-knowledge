---
title: "24 Single Source Shortest Paths"
tags:
  - shortest-path
  - dijkstra
  - bellman-ford
  - graph-algorithms
  - clrs
source: CLRS
---

# 24 · Single-Source Shortest Paths

> From a source vertex, find shortest paths to all others. Bellman-Ford handles negative weights; Dijkstra's is faster for non-negative weights.

### Problem Definition

Given a weighted, directed graph $G = (V, E)$ with weight function $w: E \to \mathbb{R}$ and a source vertex $s$, find the shortest path weight $\delta(s, v)$ from $s$ to every $v \in V$.

**Shortest-path weight:**
$$\delta(s, v) = \min\{w(p) : p \text{ is a path from } s \text{ to } v\}$$
$$\delta(s, v) = \infty \text{ if no path exists}$$

### Relaxation

The fundamental operation underlying all shortest-path algorithms:

```
INITIALIZE-SINGLE-SOURCE(G, s)
1  for each vertex v ∈ V
2      v.d = ∞
3      v.π = NIL
4  s.d = 0

RELAX(u, v, w)
1  if v.d > u.d + w(u, v)
2      v.d = u.d + w(u, v)
3      v.π = u
```

**Key properties maintained during relaxation:**
- **Convergence:** If $\delta(s, v) < \infty$ and $v.d$ reaches $\delta(s, v)$, it never changes
- **Path-relaxation property:** If $p = \langle v_0, v_1, \ldots, v_k \rangle$ is a shortest path from $s = v_0$ to $v_k$, and edges are relaxed in order, then $v_k.d = \delta(s, v_k)$

### Bellman-Ford Algorithm

Solves single-source shortest paths with **negative edge weights**. Detects negative-weight cycles.

```
BELLMAN-FORD(G, w, s)
1  INITIALIZE-SINGLE-SOURCE(G, s)
2  for i = 1 to |V| - 1
3      for each edge (u, v) ∈ E
4          RELAX(u, v, w)
5  for each edge (u, v) ∈ E
6      if v.d > u.d + w(u, v)
7          return FALSE          // negative-weight cycle detected
8  return TRUE
```

| Property | Value |
|---|---|
| **Time** | $O(VE)$ |
| **Handles negative weights** | ✅ |
| **Detects negative cycles** | ✅ |

**Correctness:** After $|V| - 1$ passes, $v.d = \delta(s, v)$ for all reachable $v$. The extra pass (lines 5–7) detects negative-weight cycles reachable from $s$.

**Proof sketch:** Shortest paths are simple (no cycles), so they have at most $|V| - 1$ edges. After iteration $i$, all shortest paths with $\leq i$ edges are correct.

### DAG Shortest Paths

For DAGs, process vertices in **topological order** — each edge relaxed exactly once.

```
DAG-SHORTEST-PATHS(G, w, s)
1  topologically sort G
2  INITIALIZE-SINGLE-SOURCE(G, s)
3  for each vertex u, in topological order
4      for each vertex v ∈ Adj[u]
5          RELAX(u, v, w)
```

| Property | Value |
|---|---|
| **Time** | $\Theta(V + E)$ |
| **Handles negative weights** | ✅ |
| **Handles negative cycles** | N/A (DAGs have no cycles) |

### Dijkstra's Algorithm

Solves single-source shortest paths for graphs with **nonnegative edge weights**. Uses a greedy strategy.

```
DIJKSTRA(G, w, s)
1  INITIALIZE-SINGLE-SOURCE(G, s)
2  S = ∅
3  Q = V                           // min-priority queue, keyed on d
4  while Q ≠ ∅
5      u = EXTRACT-MIN(Q)
6      S = S ∪ {u}
7      for each vertex v ∈ Adj[u]
8          RELAX(u, v, w)
```

**Invariant:** At the start of each iteration, $v.d = \delta(s, v)$ for all $v \in S$.

| Implementation | Time |
|---|---|
| Array | $O(V^2)$ |
| Binary min-heap | $O((V + E) \log V)$ |
| Fibonacci heap | $O(V \log V + E)$ |

**Greedy choice:** Always extract the vertex with minimum $d$ value — safe because all edge weights are nonnegative.

### Algorithm Comparison

| Algorithm | Graph Type | Negative Weights? | Time |
|---|---|---|---|
| Bellman-Ford | General | ✅ | $O(VE)$ |
| DAG-SP | DAG | ✅ | $\Theta(V+E)$ |
| Dijkstra | General | ❌ | $O(V \log V + E)$* |

*With Fibonacci heap.

### Difference Constraints (§24.4)

Systems of inequalities of the form $x_j - x_i \leq b_k$ can be modeled as shortest-path problems on constraint graphs. Bellman-Ford finds feasible solutions or detects infeasibility (negative cycles).

---

## Hands-On Exercises

### Exercise 2: Bellman-Ford — Trace Relaxation
Given directed graph:
```
S → A (weight 6)
S → B (weight 7)
A → C (weight 5)
A → D (weight -4)
A → B (weight 8)
B → D (weight -3)
B → E (weight 9)
D → C (weight 7)
D → E (weight -2)
E → S (weight 2)
E → C (weight -4)
```

Source: S. Trace Bellman-Ford:
1. Initialize: `d[S] = 0`, all others = ∞.
2. Run |V| - 1 = 4 iterations. In each iteration, relax ALL edges.
3. Record `d[v]` for each vertex after each iteration.
4. Run the 5th iteration (cycle detection). Does any distance change?

```java
// Table:
// Iter | d[S] | d[A] | d[B] | d[C] | d[D] | d[E]
// 0    |  0   |  ∞   |  ∞   |  ∞   |  ∞   |  ∞
// 1    |      |      |      |      |      |
// ... (fill all 4 iterations)
```

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 2 | [Network Delay Time](https://leetcode.com/problems/network-delay-time/) (LC 743) | 🟡 Medium | Dijkstra's |
| 3 | [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/) (LC 787) | 🟡 Medium | Bellman-Ford / BFS+DP |
| 4 | [Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/) (LC 1514) | 🟡 Medium | Modified Dijkstra's |
| 6 | [Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/) (LC 778) | 🔴 Hard | Dijkstra's / Union-Find |

### Assignment Guidelines
- **Problem 1**: Direct Dijkstra's application.
- **Problem 2**: Modified Bellman-Ford with k-stops constraint.
- **Problem 3**: Dijkstra's with max instead of min.
- **Target time:** 20 min per Medium.