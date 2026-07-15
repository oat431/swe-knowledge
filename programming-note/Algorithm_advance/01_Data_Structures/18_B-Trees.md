---
title: "18 B-Trees"
tags:
  - b-tree
  - data-structures
  - disk-based
  - clrs
source: CLRS
---

# 18 · B-Trees

> B-trees are designed for **disk-based storage**. Each node fills an entire disk page, maximizing information per access. They power databases, file systems, and every major storage engine.

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

## Hands-On Exercises

### Exercise 2: B-Tree — Trace Insertion
Insert the keys `F, S, Q, K, C, L, H, T, V, W, M, R, N, P, A, B, X, Y, D, Z, E` into a B-tree with minimum degree `t = 3` (max 5 keys per node).

1. After inserting `F, S, Q, K` — what does the root look like?
2. Insert `C` — does the root split? What is the new root?
3. Continue inserting the remaining keys. Draw the tree after every split.
4. What is the height of the final tree?

**Hint:** A node with 5 keys (full, `2t - 1 = 5`) splits on the next insertion into it. The median key moves up.

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 7 | **B-Tree Implementation** (mini-project) | 🟡 Code | B-tree insert/search/delete |

### Assignment Guidelines
- **Problem 1**: Implement B-tree insert with split for t=3.
- **Problem 2**: Trace deletion by hand.
- **Target time:** 60 min for project, 20 min per theory.