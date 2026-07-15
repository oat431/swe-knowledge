---
title: Advanced Data Structures
tags:
  - b-tree
  - fibonacci-heap
  - veb-tree
  - disjoint-set
  - data-structures
  - clrs
source: CLRS Chapters 18–21
---

# Advanced Data Structures

> These four data structures push beyond the $O(\lg n)$ barrier (or reach it in specialized ways) by exploiting the **structure of the problem** — disk locality, lazy consolidation, recursive decomposition, or union-by-rank with path compression.

---

## Chapter 18: B-Trees

### Motivation: Data Structures on Disk

When data lives on **secondary storage** (magnetic disks), the cost model changes entirely:
- **Disk access time** (8–11 ms) dwarfs **CPU time** (~50 ns for memory access)
- Information is read/written in **pages** (2¹¹–2¹⁴ bytes)
- Goal: **minimize disk I/O**, even at the cost of extra CPU work

B-trees are designed so that each node fills an entire disk page, maximizing information per access.

### B-Tree Definition

A **B-tree** $T$ with minimum degree $t \geq 2$ satisfies:

| Property | Description |
|---|---|
| **Keys per node** | Every non-root node has $\lceil t \rceil - 1$ to $2t - 1$ keys |
| **Root** | Has at least 1 key (if non-empty) |
| **Children** | Internal node with $k$ keys has $k + 1$ children |
| **Leaf structure** | All leaves at the **same depth** $h$ |
| **Key ordering** | Keys within a node are sorted; keys separate child subtrees |

When $t = 2$, every internal node has 2–4 children → **2-3-4 tree**.

### Height Bound

**Theorem 18.1.** For an $n$-key B-tree of height $h$ and minimum degree $t$:

$$h \leq \log_t \frac{n+1}{2}$$

A B-tree with branching factor 1001 and height 2 stores **over 1 billion keys** — only 2 disk accesses needed (root cached in memory).

### Operations

#### Search — `B-TREE-SEARCH(x, k)`
- Multi-way branching at each node: $(x.n + 1)$-way decision
- **Time:** $O(h)$ disk accesses, $O(th)$ CPU = $O(t \log_t n)$ CPU
- Use **binary search within nodes** to reduce CPU to $O(\lg n)$

#### Insertion — `B-TREE-INSERT(T, k)`
- **Single top-down pass** — never backtracks
- **Key trick:** Split any full node **proactively** as you descend
- If root is full: create new root, split old root → tree grows **at the top**

**Split operation** (`B-TREE-SPLIT-CHILD`):
1. Full node $y$ (with $2t - 1$ keys) splits around median key
2. Median moves up to parent
3. Two halves become separate children
- **Time:** $O(t)$ CPU, $O(1)$ disk ops per split

**Insertion flow:**
1. If root is full → split root first
2. Descend, splitting any full child encountered along the way
3. Insert into the (guaranteed non-full) leaf

#### Deletion — `B-TREE-DELETE(T, k)`
More complex than insertion — handles multiple cases:

| Case | Condition | Action |
|---|---|---|
| 1 | Key in leaf | Simple delete |
| 2a | Key in internal node, predecessor child has $\geq t$ keys | Replace with predecessor |
| 2b | Key in internal node, successor child has $\geq t$ keys | Replace with successor |
| 2c | Key in internal node, both children have $t-1$ keys | Merge children, delete from merged node |
| 3a | Descending to child with $t-1$ keys, sibling has $\geq t$ keys | Rotate key from sibling |
| 3b | Descending to child with $t-1$ keys, both siblings have $t-1$ keys | Merge with sibling |

- **Time:** $O(h)$ disk accesses = $O(\log_t n)$

### Practical Notes

- **Typical branching factors:** 50–2000 (depending on key size vs. page size)
- **B⁺-tree variant:** Satellite data only in leaves; internal nodes store only keys + pointers → maximizes branching factor
- **B\*-tree variant:** Internal nodes at least $\frac{2}{3}$ full (instead of $\frac{1}{2}$)

---

## Chapter 19: Fibonacci Heaps

### Motivation

Fibonacci heaps achieve **optimal amortized bounds** for priority queue operations, especially **`DECREASE-KEY` in $O(1)$** — critical for graph algorithms like Dijkstra's and Prim's.

| Operation | Binary Heap | Fibonacci Heap |
|---|---|---|
| `MAKE-HEAP` | $\Theta(1)$ | $\Theta(1)$ |
| `INSERT` | $\Theta(\lg n)$ | $\Theta(1)$ |
| `MINIMUM` | $\Theta(1)$ | $\Theta(1)$ |
| `EXTRACT-MIN` | $\Theta(\lg n)$ | $O(\lg n)$ |
| `UNION` | $\Theta(n)$ | $\Theta(1)$ |
| `DECREASE-KEY` | $\Theta(\lg n)$ | $\Theta(1)$ |
| `DELETE` | $\Theta(\lg n)$ | $O(\lg n)$ |

> Fibonacci heaps are predominantly of **theoretical interest** — constant factors and complexity make binary/k-ary heaps preferable in practice.

### Structure

A Fibonacci heap is a **collection of min-heap-ordered rooted trees** with a lazy structure.

**Node attributes:**
- `key`, `degree` (number of children)
- `parent`, `child` (pointer to any child)
- `left`, `right` (circular doubly-linked sibling list)
- `mark` — TRUE if node has lost a child **since becoming a child of its current parent**

**Heap attributes:**
- `min` — pointer to minimum node
- `n` — total number of nodes

### Potential Function

$$\Phi(H) = t(H) + 2m(H)$$

where $t(H)$ = number of trees, $m(H)$ = number of marked nodes.

### Mergeable Heap Operations

#### `FIB-HEAP-INSERT(H, x)` — $O(1)$ amortized
- Create single-node tree, add to root list
- Update `min` if needed
- Amortized cost: $O(1) + 1 = O(1)$

#### `FIB-HEAP-UNION(H₁, H₂)` — $O(1)$ amortized
- Concatenate root lists, update `min`
- $\Delta\Phi = 0$, amortized cost = actual cost = $O(1)$

#### `FIB-HEAP-EXTRACT-MIN(H)` — $O(\lg n)$ amortized
1. Remove min node, add its children to root list
2. **CONSOLIDATE:** Link trees of equal degree until at most one tree per degree

**Consolidation:**
- Use auxiliary array $A[0..D(n)]$ indexed by degree
- For each root $w$ in root list:
  - While another root $y$ has same degree as $x$ (starting at $w$):
    - Link $y$ to $x$ (smaller key becomes parent)
  - Place $x$ in $A[\text{degree}]$
- Rebuild root list from array

**Amortized cost:** $O(D(n)) = O(\lg n)$

### Decrease Key and Delete

#### `FIB-HEAP-DECREASE-KEY(H, x, k)` — $O(1)$ amortized
If min-heap order violated:
1. **CUT** $x$ from its parent → make $x$ a root
2. **CASCADING-CUT** up the tree:
   - If parent is unmarked → mark it (first child lost)
   - If parent is already marked → cut parent, recurse (second child lost)

**The mark bit** enforces: a node can lose **at most one child** before being cut from its parent. This limits tree degradation.

**Amortized analysis:**
- Each cascading cut: unmarks a node (−2 potential) and creates a new root (+1)
- Net: each cut "pays for itself" through potential reduction
- Amortized cost: $O(1)$

#### `FIB-HEAP-DELETE(H, x)` — $O(\lg n)$ amortized
1. Decrease key to $-\infty$ → node becomes minimum
2. Extract minimum

### Maximum Degree Bound

**Theorem:** $D(n) = O(\lg n)$ where $D(n)$ is the maximum degree of any node in an $n$-node Fibonacci heap.

**Key insight:** If node $x$ has degree $k$, then $\text{size}(x) \geq F_{k+2}$ (Fibonacci number).

**Proof sketch:**
- Children linked in order $y_1, y_2, \ldots, y_k$
- $y_i.\text{degree} \geq i - 2$ (each child lost at most one subtree before being cut)
- Size bound follows from Fibonacci recurrence: $\text{size}(x) \geq 2 + \sum_{i=2}^{k} F_i = F_{k+2}$

Since $F_k \approx \phi^k / \sqrt{5}$ where $\phi = (1+\sqrt{5})/2$:

$$n \geq F_{k+2} \geq \phi^k \implies k \leq \log_\phi n$$

This is why the structure is called a **Fibonacci** heap.

### When to Use Fibonacci Heaps

- **Graph algorithms** with many `DECREASE-KEY` calls: Dijkstra's ($O((V+E)\lg V)$ → $O(V \lg V + E)$), Prim's MST
- **Dense graphs** where $|E| \gg |V|$
- Not practical for small inputs or when `EXTRACT-MIN` dominates

---

## Chapter 20: van Emde Boas Trees

### Motivation

Priority queue operations in $O(\lg \lg u)$ time when keys are **integers in range $\{0, 1, \ldots, u-1\}$**. This beats the $\Omega(\lg n)$ comparison-based lower bound by exploiting integer structure.

> **Prerequisite:** Universe size $u$ must be known upfront and be an exact power of 2.

### Key Idea: Recursive Square-Root Decomposition

Split the $u$-bit universe into clusters:
- $\lceil\sqrt{u}\rceil$ clusters, each of size $\lfloor\sqrt{u}\rfloor$
- **Summary** structure tracks which clusters are non-empty
- Recurse on both clusters and summary

### Helper Functions

```
high(x) = ⌊x / ⌊√u⌋⌋     → which cluster
low(x)  = x mod ⌊√u⌋      → position within cluster
index(x, y) = x · ⌊√u⌋ + y  → reconstruct value
```

### Proto van Emde Boas Structure

**Base case:** $u = 2$ — a 2-bit array $A[0..1]$

**Recursive case ($u > 2$):**
- `summary`: proto-vEB($\lceil\sqrt{u}\rceil$) — tracks non-empty clusters
- `cluster[0..⌈√u⌉−1]`: array of proto-vEB($\lfloor\sqrt{u}\rfloor$) pointers

**Problem:** Most operations make **two** recursive calls → $T(u) = 2T(\sqrt{u}) + O(1) = O(\lg u)$, not $O(\lg \lg u)$.

### van Emde Boas Tree (Full Version)

**Key enhancement:** Store `min` and `max` explicitly in each node.

**Rules:**
- `min` stores the minimum element but it is **NOT** stored in any cluster
- `max` stores the maximum (may also appear in a cluster if $|S| \geq 2$)

**This breaks the two-call pattern:**
- `MINIMUM`/`MAXIMUM`: Return `min`/`max` in $O(1)$ — no recursion
- `SUCCESSOR`: Check cluster's `max` to decide if successor is local → one recursive call
- `INSERT` into empty tree: Just set `min` and `max` — no recursion
- `DELETE` from single-element tree: Clear `min` and `max` — no recursion

**Result:** $T(u) = T(\lceil\sqrt{u}\rceil) + O(1) = O(\lg \lg u)$

### Operations Summary

| Operation | Time | Technique |
|---|---|---|
| `MINIMUM` / `MAXIMUM` | $O(1)$ | Direct attribute access |
| `MEMBER` | $O(\lg \lg u)$ | Single recursive descent |
| `SUCCESSOR` / `PREDECESSOR` | $O(\lg \lg u)$ | One recursive call (cluster or summary) |
| `INSERT` | $O(\lg \lg u)$ | One recursive call (only if cluster non-empty) |
| `DELETE` | $O(\lg \lg u)$ | One recursive call (mutually exclusive cases) |

### INSERT Detail

```
VEB-TREE-INSERT(V, x):
    if V.min == NIL:
        VEB-EMPTY-TREE-INSERT(V, x)   // O(1)
    else if x < V.min:
        swap x with V.min              // new min not stored in clusters
    if V.u > 2:
        if cluster[high(x)] is empty:
            INSERT into V.summary       // mark cluster non-empty
            EMPTY-TREE-INSERT(cluster[high(x)], low(x))
        else:
            INSERT into cluster[high(x)]  // single recursive call
    if x > V.max: V.max = x
```

### DELETE Detail

The key trick: when deleting `min`, find the actual minimum in the clusters to replace it.

```
VEB-TREE-DELETE(V, x):
    if V.min == V.max:               // single element
        V.min = V.max = NIL
    elif V.u == 2:                    // base case
        set remaining element as min/max
    elif x == V.min:
        first_cluster = MINIMUM(V.summary)
        x = index(first_cluster, MINIMUM(cluster[first_cluster]))
        V.min = x                     // new min from clusters
        // fall through to delete x from its cluster
    DELETE from cluster[high(x)]
    if cluster now empty:
        DELETE from V.summary
    update V.max if needed
```

**Critical observation:** The two recursive calls (lines 13 and 15) are **mutually exclusive** — if the cluster becomes empty (triggering summary update), the first call was $O(1)$ because it deleted the sole element.

### Space and Construction

- **Space:** $O(u)$ — pre-allocated for all possible universe values
- **Construction:** $O(u)$ time (vs. $O(1)$ for red-black trees)
- Trade-off: expensive initialization pays off only with many operations

### Reduced-Space Variant (RS-vEB)

Replace the `cluster` array with a **hash table** (dynamic table):
- Only stores pointers to non-empty clusters
- **Space:** $O(n)$ (where $n$ = actual elements stored)
- Operations remain $O(\lg \lg u)$ expected amortized time
- **y-fast tries** achieve $O(n)$ space with $O(\lg \lg u)$ worst-case queries

---

## Chapter 21: Disjoint Sets

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
