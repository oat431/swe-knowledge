---
title: "Advanced Graph Algorithms"
tags: [graph-algorithms, mst, shortest-path, max-flow, clrs]
created: 2026-07-15
source: "CLRS Chapters 23–26"
---

# 03 · Advanced Graph Algorithms

> Covers CLRS Chapters 23–26: Minimum Spanning Trees, Single-Source Shortest Paths, All-Pairs Shortest Paths, and Maximum Flow.

---

## Chapter 23 · Minimum Spanning Trees

### Problem Definition

Given a connected, undirected graph $G = (V, E)$ with a weight function $w: E \to \mathbb{R}$, find a **minimum spanning tree (MST)** — a spanning tree of minimum total weight.

**Key property:** A spanning tree connects all $|V|$ vertices with exactly $|V| - 1$ edges.

### Generic MST Algorithm

```
GENERIC-MST(G, w)
1  A = ∅
2  while A does not form a spanning tree
3      find an edge (u, v) that is safe for A
4      A = A ∪ {(u, v)}
5  return A
```

An edge $(u, v)$ is **safe** for $A$ if adding it to $A$ maintains the invariant that $A$ is a subset of some MST.

### Cut Property

A **cut** $(S, V - S)$ is a partition of vertices. An edge $(u, v)$ **crosses** the cut if one endpoint is in $S$ and the other in $V - S$.

> **Theorem (Cut Property):** For any cut $(S, V - S)$ of $G$, if edge $(u, v)$ is a light edge crossing the cut, then $(u, v)$ is safe for $A$.

A **light edge** crossing a cut is one with minimum weight among all edges crossing that cut.

### Kruskal's Algorithm

**Strategy:** Greedy — process edges in order of increasing weight; add each edge if it connects two different components.

```
KRUSKAL(G, w)
1  A = ∅
2  for each vertex v ∈ V
3      MAKE-SET(v)
4  sort edges of E by weight w (nondecreasing)
5  for each edge (u, v) ∈ E, in sorted order
6      if FIND-SET(u) ≠ FIND-SET(v)
7          A = A ∪ {(u, v)}
8          UNION(u, v)
9  return A
```

**Data structure:** Disjoint-set (Union-Find) with union by rank and path compression.

| Complexity | Value |
|---|---|
| **Time** | $O(E \log E)$ = $O(E \log V)$ |
| **Bottleneck** | Sorting edges |

### Prim's Algorithm

**Strategy:** Greedy — grow a single tree from an arbitrary start vertex, always adding the cheapest edge connecting the tree to a new vertex. (Similar to Dijkstra's algorithm.)

```
PRIM(G, w, r)
1  for each u ∈ V
2      u.key = ∞
3      u.π = NIL
4  r.key = 0
5  Q = V                           // min-priority queue, keyed on key
6  while Q ≠ ∅
7      u = EXTRACT-MIN(Q)
8      for each v ∈ Adj[u]
9          if v ∈ Q and w(u, v) < v.key
10             v.π = u
11             v.key = w(u, v)
```

| Implementation | Time |
|---|---|
| Binary min-heap | $O(E \log V)$ |
| Fibonacci heap | $O(E + V \log V)$ |

### Kruskal vs. Prim

| Feature | Kruskal | Prim |
|---|---|---|
| Strategy | Edge-centric (sort all edges) | Vertex-centric (grow from root) |
| Data structure | Union-Find | Priority queue |
| Best for | Sparse graphs | Dense graphs |
| Graph type | Undirected, connected | Undirected, connected |

---

## Chapter 24 · Single-Source Shortest Paths

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

## Chapter 25 · All-Pairs Shortest Paths

### Problem Definition

Find shortest paths between **every pair** of vertices. Output: $n \times n$ matrix $D = (d_{ij})$ where $d_{ij} = \delta(i, j)$.

**Naive approach:** Run single-source algorithm $|V|$ times.

| Single-Source Algorithm | Total Time |
|---|---|
| Dijkstra (Fibonacci heap) | $O(VE + V^2 \log V)$ |
| Bellman-Ford | $O(V^2 E)$ = $O(V^4)$ dense |

### Matrix Multiplication Approach (§25.1–25.2)

Shortest paths can be computed via repeated matrix multiplication in the **min-plus semiring**:

$$d_{ij}^{(m)} = \min_{1 \leq k \leq n} \{d_{ik}^{(m-1)} + w_{kj}\}$$

- After $m$ multiplications: shortest paths using at most $m$ edges
- After $|V| - 1$ multiplications: all shortest paths (no negative cycles)
- **Repeated squaring** variant: $O(V^3 \log V)$

### Floyd-Warshall Algorithm

Dynamic programming approach: considers **intermediate vertices** explicitly.

**Idea:** Let $d_{ij}^{(k)}$ = shortest path from $i$ to $j$ using only vertices $\{1, 2, \ldots, k\}$ as intermediate vertices.

**Recurrence:**
$$d_{ij}^{(k)} = \begin{cases} w_{ij} & \text{if } k = 0 \\ \min\left(d_{ij}^{(k-1)},\; d_{ik}^{(k-1)} + d_{kj}^{(k-1)}\right) & \text{if } k \geq 1 \end{cases}$$

```
FLOYD-WARSHALL(W)
1  n = W.rows
2  D⁰ = W
3  for k = 1 to n
4      let D^(k) = (d_ij^(k)) be a new n × n matrix
5      for i = 1 to n
6          for j = 1 to n
7              d_ij^(k) = min(d_ij^(k-1), d_ik^(k-1) + d_kj^(k-1))
8  return D^(n)
```

| Property | Value |
|---|---|
| **Time** | $\Theta(V^3)$ |
| **Space** | $\Theta(V^2)$ (in-place) |
| **Negative weights** | ✅ |
| **Negative cycle detection** | ✅ (check diagonal) |

**Key insight:** The $k$-th iteration asks "Is vertex $k$ useful as an intermediate on the path from $i$ to $j$?"

### Transitive Closure (§25.2)

Use Floyd-Warshall with boolean OR/AND instead of min/+ to compute reachability:

$$t_{ij}^{(k)} = t_{ij}^{(k-1)} \lor (t_{ik}^{(k-1)} \land t_{kj}^{(k-1)})$$

### Johnson's Algorithm (§25.3)

For **sparse graphs** — combines reweighting with Dijkstra.

**Steps:**
1. Create $G'$ by adding new vertex $s$ with zero-weight edges to all vertices
2. Run Bellman-Ford from $s$ to compute $h(v) = \delta(s, v)$ for all $v$
3. If negative cycle detected → report and stop
4. **Reweight** edges: $\hat{w}(u, v) = w(u, v) + h(u) - h(v)$ (all nonnegative!)
5. Run Dijkstra from each vertex with reweighted edges
6. Recover original shortest-path weights: $\delta(u, v) = \hat{\delta}(u, v) - h(u) + h(v)$

| Property | Value |
|---|---|
| **Time** | $O(V^2 \log V + VE)$ |
| **Best for** | Sparse graphs with negative weights |

### All-Pairs Algorithm Summary

| Algorithm | Time | Negative Weights? | Best For |
|---|---|---|---|
| Repeated Dijkstra | $O(VE + V^2 \log V)$ | ❌ | Nonnegative weights |
| Floyd-Warshall | $\Theta(V^3)$ | ✅ | Dense graphs |
| Johnson's | $O(V^2 \log V + VE)$ | ✅ | Sparse graphs |
| Matrix multiplication | $O(V^3 \log V)$ | ✅ (no neg. cycles) | Theoretical |

---

## Chapter 26 · Maximum Flow

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
