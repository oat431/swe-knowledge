---
title: "21 Disjoint Sets"
tags:
  - disjoint-set
  - union-find
  - data-structures
  - clrs
source: CLRS
---

# 21 · Disjoint Sets (Union-Find)

> Maintains disjoint dynamic sets with near-O(1) operations. Union by rank + path compression yields O(m·α(n)) — one of the most efficient data structures known.

### The Disjoint-Set Data Structure

Maintains a collection $\{S_1, S_2, \ldots, S_k\}$ of **disjoint dynamic sets**. Each set has a **representative** — a distinguished member.

### Operations

| Operation | Description |
|---|---|
| `MAKE-SET(x)` | Create new set $\{x\}$ with $x$ as representative |
| `UNION(x, y)` | Replace sets containing $x$ and $y$ with their union |
| `FIND-SET(x)` | Return representative of the set containing $x$ |

> Applications: connected components, Kruskal's MST, unification in logic programming.

### Linked-List Representation

Each set = doubly-linked list with head pointing to representative.

| Operation | Time |
|---|---|
| `MAKE-SET` | $O(1)$ |
| `FIND-SET` | $O(1)$ |
| `UNION` | $O(\min(n_1, n_2))$ — append shorter list |

**Simple weighted-union heuristic:** Always append the shorter list to the longer one.

**Lemma:** With weighted union, $m$ operations on $n$ elements take $O(m + n \lg n)$ time.

### Disjoint-Set Forests

Each set is a **rooted tree**. The representative is the root.

```
MAKE-SET(x):
    x.p = x
    x.rank = 0

FIND-SET(x):
    while x != x.p:
        x = x.p
    return x

UNION(x, y):
    LINK(FIND-SET(x), FIND-SET(y))

LINK(x, y):
    if x.rank > y.rank:
        y.p = x
    else:
        x.p = y
        if x.rank == y.rank:
            y.rank = y.rank + 1
```

### Two Heuristics

#### Union by Rank

Attach the **shorter tree** under the root of the **taller tree**.

- Rank is an upper bound on tree height
- When ranks are equal, increment the winner's rank
- **Effect:** Prevents tall, skinny trees

#### Path Compression

During `FIND-SET`, make every node on the path point **directly to the root**.

```
FIND-SET(x):         // with path compression
    if x != x.p:
        x.p = FIND-SET(x.p)
    return x.p
```

- Does not change ranks (ranks are upper bounds, not exact heights)
- **Effect:** Flattens tree structure dramatically

### Analysis: Inverse Ackermann Function

With **both** union by rank and path compression:

**Theorem 21.14 (Tarjan).** $m$ operations on $n$ elements take:

$$O(m \cdot \alpha(n))$$

where $\alpha(n)$ is the **inverse Ackermann function**.

#### Ackermann Function

$$A_k(j) = \begin{cases} j + 1 & \text{if } k = 0 \\ A_{k-1}^{(j+1)}(j) & \text{if } k \geq 1 \end{cases}$$

where $A_{k-1}^{(j+1)}$ denotes $j + 1$ applications of $A_{k-1}$.

| $k$ | $A_k(j)$ | Growth |
|---|---|---|
| 0 | $j + 1$ | Linear |
| 1 | $2j + 3$ | Linear |
| 2 | $2^{j+3}(j+2) - 3$ | Exponential |
| 3 | Tower of exponentials | Incomprehensible |

#### Inverse Ackermann

$$\alpha(n) = \min\{k : A_k(1) \geq n\}$$

- $\alpha(n) \leq 4$ for any practical $n$ (even $n = 2^{65536}$)
- **Effectively constant** in practice

### Key Lemma: Bounding the Ranks

**Lemma 21.7.** Every node has rank $\leq \lfloor \lg n \rfloor$.

**Proof:** A node of rank $r$ has $\geq 2^r$ descendants (proven by induction on rank using union by rank).

**Lemma 21.12.** For each integer $k = 0, 1, \ldots, \lfloor\lg n^*\rfloor$, the number of nodes with rank $r$ that are the root of a "type-2" (non-root) block is at most $n / A_k(1)$.

This rank-based partitioning, combined with the Ackermann function's explosive growth, yields the $O(m \alpha(n))$ bound.

### Practical Summary

| Representation | $m$ ops, $n$ elements |
|---|---|
| Linked list (no heuristic) | $O(mn)$ |
| Linked list + weighted union | $O(m + n \lg n)$ |
| Forest + union by rank | $O(m \lg n)$ |
| Forest + path compression | $O(m \lg n)$ |
| **Forest + union by rank + path compression** | **$O(m \cdot \alpha(n))$** |

> The combination of union by rank and path compression is **one of the most efficient data structures known** — nearly $O(1)$ per operation.

---

## Cross-Chapter Connections

| Structure | Key Insight | Amortized Bound | Application |
|---|---|---|---|
| **B-tree** | Node = disk page; high branching factor | Worst-case $O(\log_t n)$ | Databases, file systems |
| **Fibonacci heap** | Lazy consolidation; mark bits limit child loss | $O(1)$ for INSERT, DECREASE-KEY | Dijkstra's, Prim's MST |
| **vEB tree** | Recursive square-root decomposition; explicit min/max | $O(\lg \lg u)$ worst-case | Integer priority queues |
| **Disjoint sets** | Union by rank + path compression | $O(\alpha(n))$ per op | Kruskal's, connected components |

### The Amortized Analysis Thread

All four structures rely on **amortized analysis** (Ch 17):
- **B-trees:** Aggregate — $O(1)$ amortized splits per insertion
- **Fibonacci heaps:** Potential method — $\Phi = t(H) + 2m(H)$
- **vEB trees:** Not amortized — genuine $O(\lg \lg u)$ worst-case
- **Disjoint sets:** Accounting via Ackermann — nearly $O(1)$ per operation

---

*Source: CLRS, Chapters 18–21 (4th Edition)*

## Related

- [[Algorithm Overview]] — Practical data structures (trees, heaps, hash tables, graphs)
- [[01_Amortized_Analysis]] — Amortized analysis of B-tree and Fibonacci heap operations
- [[03_Advanced_Graph_Algorithms]] — Disjoint sets used in Kruskal's MST algorithm
- [[01 Trees & BSTs]] — Binary search trees, the simpler cousin of B-trees
- [[01 Heaps & Priority Queues]] — Binary heaps, the simpler cousin of Fibonacci heaps

---

## Hands-On Exercises

### Exercise 1: Disjoint Sets — Implement Union-Find
Implement the disjoint-set forest with **union by rank** and **path compression** from the note.

```java
class UnionFind {
    int[] parent, rank;

    UnionFind(int n) {
        parent = new int[n];
        rank = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i;
    }

    int find(int x) {
        // TODO: Path compression — make every node point to root
        if (parent[x] != x) parent[x] = find(parent[x]);
        return parent[x];
    }

    void union(int x, int y) {
        // TODO: Union by rank — attach shorter tree under taller
        int px = find(x), py = find(y);
        if (px == py) return;
        if (rank[px] < rank[py]) parent[px] = py;
        else if (rank[px] > rank[py]) parent[py] = px;
        else { parent[py] = px; rank[px]++; }
    }
}
```

**Test:** Create 6 elements {0,1,2,3,4,5}. Perform: `union(0,1)`, `union(2,3)`, `union(4,5)`, `union(1,3)`, `union(3,5)`. Verify all have the same root. Trace the tree structure after each union.

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 1 | [Number of Connected Components](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/) (LC 323) | 🟡 Medium | Union-Find |
| 2 | [Redundant Connection](https://leetcode.com/problems/redundant-connection/) (LC 684) | 🟡 Medium | Union-Find (cycle detection) |
| 3 | [Accounts Merge](https://leetcode.com/problems/accounts-merge/) (LC 721) | 🟡 Medium | Union-Find |
| 4 | [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/) (LC 130) | 🟡 Medium | Union-Find |
| 5 | [Number of Islands II](https://leetcode.com/problems/number-of-islands-ii/) (LC 305) | 🔴 Hard | Dynamic Union-Find |
| 6 | [Minimize Malware Spread](https://leetcode.com/problems/minimize-malware-spread/) (LC 924) | 🔴 Hard | Union-Find |

### Assignment Guidelines
- **Start** with 1–2 (basics), then 3–6 (applications).
- **Key insight:** Connected components, merge groups, cycle detection → Union-Find.
- **Target time:** 15 min per Medium, 25 min per Hard.