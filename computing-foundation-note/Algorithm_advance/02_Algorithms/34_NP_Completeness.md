---
title: "34 NP Completeness"
tags:
  - np-completeness
  - reductions
  - complexity
  - clrs
source: CLRS
---

# 34 · NP-Completeness

> NP-complete problems are the hardest problems in NP. If any NPC problem is in P, then P = NP. Established via polynomial-time reductions.

### 34.1–34.3 The Classes P, NP, and NP-Complete

#### Class P

**P** = the class of **decision problems** solvable in **polynomial time** by a deterministic Turing machine. Informally: problems with efficient algorithms.

Examples: shortest path, MST, sorting, linear programming, primality testing.

#### Class NP

**NP** = the class of decision problems where a **"yes"** instance has a **polynomial-time verifiable certificate**.

Formally: $L \in \text{NP}$ iff there exists a polynomial-time algorithm $A$ and a polynomial $p$ such that:

$$x \in L \iff \exists\, y \text{ with } |y| \leq p(|x|) \text{ such that } A(x, y) = \text{TRUE}$$

The string $y$ is the **certificate** (or witness). The verifier $A$ checks it in polynomial time.

Examples:
- **HAM-CYCLE:** certificate is the vertex sequence; verifier checks it visits all vertices and forms a cycle — $O(n)$.
- **CLIQUE:** certificate is the subset of $k$ vertices; verifier checks all $\binom{k}{2}$ edges exist — $O(k^2)$.
- **SAT:** certificate is a truth assignment; verifier evaluates the formula — $O(|\phi|)$.

**Key relationship:** $\text{P} \subseteq \text{NP}$. If you can solve a problem in polynomial time, you can certainly verify a solution in polynomial time (ignore the certificate and recompute).

**Open question:** Does $\text{P} = \text{NP}$? This is one of the seven Millennium Prize Problems. Most computer scientists believe $\text{P} \neq \text{NP}$.

#### NP-Hard and NP-Complete

- **NP-hard:** A problem $H$ is NP-hard if every problem in NP is polynomial-time reducible to $H$. (At least as hard as any NP problem, but not necessarily in NP itself.)
- **NP-complete:** A problem that is both **NP-hard** and **in NP** ($\text{NPC} = \text{NP-hard} \cap \text{NP}$).

**Implication:** If any NP-complete problem can be solved in polynomial time, then **P = NP** and every problem in NP has a polynomial-time solution.

#### Polynomial-Time Reductions

**Notation:** $A \leq_P B$ ("$A$ polynomial-time reduces to $B$") means there exists a polynomial-time computable function $f$ such that:

$$x \in A \iff f(x) \in B$$

**To show $B$ is NP-hard:** Pick a known NP-hard problem $A$ and show $A \leq_P B$.

**To show $B$ is NP-complete:** Show (1) $B \in \text{NP}$, and (2) some known NP-complete problem $A \leq_P B$.

**Transitivity:** If $A \leq_P B$ and $B \leq_P C$, then $A \leq_P C$.

---

### 34.4 NP-Completeness Proofs — The Reduction Chain

The foundational chain of reductions:

```
CIRCUIT-SAT ≤_P SAT ≤_P 3-CNF-SAT ≤_P CLIQUE
                                          ↓
3-CNF-SAT ≤_P SUBSET-SUM    CLIQUE ≤_P VERTEX-COVER
                                          ↓
                                VERTEX-COVER ≤_P HAM-CYCLE
                                          ↓
                                HAM-CYCLE ≤_P TSP
```

#### CIRCUIT-SAT ≤_P SAT (Theorem 34.8)

Transform a boolean circuit into an equivalent boolean formula. For each gate, create a variable and a clause encoding the gate's logic. The formula is satisfiable iff the circuit has a satisfying assignment.

#### SAT ≤_P 3-CNF-SAT (Theorem 34.10)

Three steps:
1. **Circuit → Formula:** Introduce variables for each gate, build a conjunction of sub-formulas.
2. **Formula → CNF:** Apply DeMorgan's laws and distributive law to push OR inward (may cause exponential blow-up, but we use the structure of step 1 to avoid this).
3. **CNF → 3-CNF:** Split long clauses using auxiliary variables $p, q$:
   - Clause with ≥ 4 literals → split into multiple 3-literal clauses
   - Clause with 2 literals $(l_1 \lor l_2)$ → $(l_1 \lor l_2 \lor p) \land (l_1 \lor l_2 \lor \bar{p})$
   - Clause with 1 literal $l$ → four clauses using $p, q$ that are satisfiable iff $l$ is true

Each step is polynomial-time computable.

---

### 34.5 NP-Complete Problems

#### CLIQUE (Theorem 34.11)

**3-CNF-SAT $\leq_P$ CLIQUE**

Given a 3-CNF formula $\phi = C_1 \land C_2 \land \cdots \land C_k$:

**Construction:** For each clause $C_r = (l_1^r \lor l_2^r \lor l_3^r)$, create three vertices (one per literal). Add edge between $v_i^r$ and $v_j^s$ iff:
- They are in **different** clauses ($r \neq s$), AND
- Their literals are **consistent** ($l_i^r$ is not the negation of $l_j^s$)

**Claim:** $\phi$ is satisfiable $\iff$ $G$ has a clique of size $k$.

- **→:** Pick one "true" literal per clause → $k$ vertices forming a clique (all consistent).
- **←:** A $k$-clique has exactly one vertex per triple (no intra-triple edges). Set those literals to 1 — no conflicts (no edges between inconsistent literals).

#### VERTEX-COVER (Theorem 34.12)

**CLIQUE $\leq_P$ VERTEX-COVER**

Given graph $G = (V, E)$ and integer $k$, construct $\bar{G} = (V, \bar{E})$ (complement graph). Output $(\bar{G},\; |V| - k)$.

**Claim:** $G$ has a $k$-clique $\iff$ $\bar{G}$ has a vertex cover of size $|V| - k$.

- **→:** If $V' \subseteq V$ is a $k$-clique in $G$, then $V - V'$ is a vertex cover in $\bar{G}$: every edge in $\bar{E}$ connects two vertices not both in $V'$ (since $V'$ is a clique in $G$, all pairs within it are edges of $G$, not $\bar{G}$).
- **←:** If $V - V'$ is a vertex cover in $\bar{G}$ of size $|V| - k$, then $V'$ is a clique in $G$: for any $u, v \in V'$, the edge $(u, v) \in \bar{E}$ would not be covered, so $(u, v) \in E$.

#### HAMILTONIAN-CYCLE (Theorem 34.13)

**VERTEX-COVER $\leq_P$ HAM-CYCLE**

Uses a **widget** construction: for each edge $(u, v) \in E$, create a 12-vertex widget $W_{uv}$ with constrained traversal paths. Selector vertices $s_1, \ldots, s_k$ choose the $k$ vertices of the cover. The construction ensures a Hamiltonian cycle exists iff $G$ has a vertex cover of size $k$.

**Widget properties:** Any Hamiltonian cycle must traverse the widget in one of three specific patterns (visiting all 12 vertices), corresponding to whether $u$, $v$, or both are in the vertex cover.

#### TSP (Theorem 34.14)

**HAM-CYCLE $\leq_P$ TSP**

Given graph $G = (V, E)$, construct a complete graph $G' = (V, V \times V)$ with edge weights:

$$w(u, v) = \begin{cases} 0 & \text{if } (u, v) \in E \\ 1 & \text{if } (u, v) \notin E \end{cases}$$

$G$ has a Hamiltonian cycle $\iff$ TSP instance $(G', 0)$ has a tour of cost 0.

#### SUBSET-SUM (Theorem 34.15)

**3-CNF-SAT $\leq_P$ SUBSET-SUM**

Construct integers in base 10 (or any base ≥ 7) encoding the formula structure:
- Variables $x_i$ → two values $v_i$ and $v_i'$ (corresponding to $x_i = 1$ and $x_i = 0$)
- Clauses $C_j$ → two slack values $s_j$ and $s_j'$ (to pad each clause-digit to target value 4)
- Target $t$: each variable-digit is 1, each clause-digit is 4

No carries between digit positions (base ≥ 7, max digit sum is 6), so digits are independent. Each variable-digit contributes 1 to the target. Each clause-digit needs 3 (from true literals) + 1 or 2 (from slack) = 4.

**Claim:** $\phi$ is satisfiable $\iff$ subset sums to $t$.

---

### 34.5 Summary of NP-Complete Problems

| Problem | Domain | Input | Question |
|---|---|---|---|
| **CIRCUIT-SAT** | Boolean logic | Boolean circuit | Satisfiable? |
| **SAT** | Boolean logic | Boolean formula | Satisfiable? |
| **3-CNF-SAT** | Boolean logic | 3-CNF formula | Satisfiable? |
| **CLIQUE** | Graphs | Graph $G$, integer $k$ | $k$-clique exists? |
| **VERTEX-COVER** | Graphs | Graph $G$, integer $k$ | Size-$k$ vertex cover? |
| **HAM-CYCLE** | Graphs | Graph $G$ | Hamiltonian cycle? |
| **TSP** | Graphs | Complete graph, bound $b$ | Tour of cost ≤ $b$? |
| **SUBSET-SUM** | Arithmetic | Set $S$, target $t$ | Subset sums to $t$? |

---

## Hands-On Exercises

### Exercise 4: NP-Completeness — Reduction Practice
Prove that **CLIQUE ≤_P INDEPENDENT-SET**:

An independent set in graph G is a subset S of vertices where no two vertices in S are connected by an edge.

1. Given a graph G and integer k, how do you construct a graph G' such that:
   - G has a k-clique ↔ G' has a k-independent set?
2. **Hint:** What is the relationship between a clique in G and an independent set in the complement graph `G̅`?
3. Write the reduction formally: describe the polynomial-time construction and prove both directions.

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 5 | **Prove: if any NPC problem is in P, then P = NP** | 🔴 Theory | NP-completeness |
| 8 | **Prove Set Cover ≤_P Dominating Set** | 🔴 Theory | NP reduction |

### Assignment Guidelines
- **Problem 1** is the most important theoretical result.
- **Problems 2–3** practice writing formal reductions.
- **Target time:** 30 min per theory problem.