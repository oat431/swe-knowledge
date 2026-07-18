---
title: "Search and Constraint Satisfaction"
tags:
  - search
  - csp
  - heuristics
  - artificial-intelligence
source: "Russell & Norvig Ch 3-6"
---

# Search and Constraint Satisfaction

## 1. Problem Formulation (Ch 3.1)

A **well-defined problem** consists of five components:

| Component | Description |
|---|---|
| **Initial state** | Starting configuration (e.g., `In(Arad)`) |
| **Actions** | Legal moves in each state: `ACTIONS(s)` → set of actions |
| **Transition model** | `RESULT(s, a)` → resulting state |
| **Goal test** | Determines if a state is a goal |
| **Path cost** | Numeric cost of a path (sum of non-negative step costs) |

- **State space**: All states reachable from the initial state by any sequence of actions; forms a directed graph
- **Solution**: An action sequence from initial state to a goal state; an **optimal solution** has the lowest path cost
- **Abstraction**: Removing detail while retaining validity — the choice of abstraction level is critical

---

## 2. Search Algorithms Overview

### 2.1 Tree Search vs Graph Search

- **Tree-SEARCH**: Explores paths systematically; may revisit states (redundant paths)
- **GRAPH-SEARCH**: Maintains an **explored set** (closed list) to avoid revisiting states; frontier separates explored from unexplored regions

### 2.2 Evaluation Criteria

| Criterion | Meaning |
|---|---|
| **Completeness** | Guaranteed to find a solution if one exists? |
| **Optimality** | Finds the optimal (lowest-cost) solution? |
| **Time complexity** | Number of nodes generated |
| **Space complexity** | Maximum nodes stored in memory |

Key parameters: **b** = branching factor, **d** = depth of shallowest solution, **m** = maximum depth

---

## 3. Uninformed (Blind) Search Strategies (Ch 3.4)

| Strategy | Data Structure | Complete? | Optimal? | Time | Space |
|---|---|---|---|---|---|
| **Breadth-First (BFS)** | FIFO queue | Yes (if b finite) | Yes (unit costs) | O(b^d) | O(b^d) |
| **Uniform-Cost (UCS)** | Priority queue (by g(n)) | Yes (if step ≥ ε) | Yes | O(b^(1+⌊C*/ε⌋)) | O(b^(1+⌊C*/ε⌋)) |
| **Depth-First (DFS)** | LIFO queue (stack) | No (tree) / Yes (finite graph) | No | O(b^m) | O(bm) |
| **Depth-Limited** | DFS with limit ℓ | No (if ℓ < d) | No | O(b^ℓ) | O(b^ℓ) |
| **Iterative Deepening (IDS)** | Repeated depth-limited | Yes | Yes (unit costs) | O(b^d) | O(bd) |
| **Bidirectional** | Two frontiers | Yes | Yes | O(b^(d/2)) | O(b^(d/2)) |

**Key insights:**
- **BFS** memory is the bottleneck — exponential space
- **DFS** uses linear space but can get stuck in infinite paths
- **IDS** combines BFS optimality with DFS space efficiency; the preferred uninformed method when depth is unknown
- **Backtracking search** (DFS variant): generates one successor at a time, uses O(m) memory — foundation for CSP solvers

---

## 4. Informed (Heuristic) Search Strategies (Ch 3.5)

### 4.1 Heuristic Function

$$h(n) = \text{estimated cost of cheapest path from state at } n \text{ to a goal}$$

- h(goal) = 0
- Heuristics are the primary way to inject domain knowledge into search

### 4.2 Greedy Best-First Search

- **Evaluation**: $f(n) = h(n)$
- Expands the node closest to the goal (estimated)
- **Not optimal**, not complete (tree version)
- Often efficient in practice

### 4.3 A* Search

$$f(n) = g(n) + h(n)$$

- g(n) = actual cost from start to n; h(n) = estimated cost from n to goal
- **Admissible heuristic**: never overestimates the true cost → guarantees optimality for tree search
- **Consistent (monotonic) heuristic**: $h(n) \leq c(n, a, n') + h(n')$ → guarantees optimality for graph search; f-values are non-decreasing along any path
- Every consistent heuristic is admissible
- A* expands all nodes with f(n) < C*; may expand some nodes with f(n) = C*
- **Optimally efficient** among algorithms using the same heuristic
- **Drawback**: Exponential space complexity — keeps all generated nodes in memory

**Effective branching factor** b*: if A* generates N nodes at solution depth d, then $N + 1 = 1 + b^* + (b^*)^2 + \ldots + (b^*)^d$. A good heuristic has b* close to 1.

### 4.4 Memory-Bounded Variants

| Algorithm | Key Idea |
|---|---|
| **IDA*** | Iterative deepening on f-cost instead of depth; O(bd) space |
| **RBFS** | Recursive best-first; linear space; backs up f-values when unwinding |
| **SMA*** | Uses all available memory; drops worst leaf when full; backs up values |

### 4.5 Designing Good Heuristics

1. **Relaxed problems**: Remove constraints → the optimal solution cost of the relaxed problem is an admissible heuristic for the original
   - Example (8-puzzle): "tile can move anywhere" → h₁ (misplaced tiles); "tile can move to adjacent square (even occupied)" → h₂ (Manhattan distance)
2. **Composite heuristic**: $h(n) = \max\{h_1(n), \ldots, h_m(n)\}$ — dominates all components
3. **Pattern databases**: Precompute exact solution costs for subproblems; lookup during search
4. **Disjoint pattern databases**: Count only moves affecting specific tile groups → additive admissible heuristics
5. **Learning from experience**: Use features + ML to predict h(n)

---

## 5. Beyond Classical Search (Ch 4)

### 5.1 Local Search Algorithms

For problems where only the **solution state** matters (not the path):

| Algorithm | Description |
|---|---|
| **Hill Climbing** | Move to best neighbor; gets stuck at local maxima, ridges, plateaux |
| **Random-Restart Hill Climbing** | Repeated random starts; trivially complete with probability → 1 |
| **Simulated Annealing** | Accepts downhill moves with probability e^(ΔE/T); T decreases over time; finds global optimum with probability → 1 if cooled slowly enough |
| **Local Beam Search** | Keep k states; expand all, keep k best successors; information shared between threads |
| **Genetic Algorithms** | Population-based; **selection** (fitness-proportional), **crossover** (combine parents), **mutation** (random change) |

**State-space landscape**: "location" = state, "elevation" = objective function value. Goal: find global maximum/minimum.

**Continuous spaces**: Use gradient ∇f for steepest ascent; Newton-Raphson method uses Hessian matrix; line search adjusts step size α.

### 5.2 Nondeterministic Actions (Ch 4.3)

- **Contingency plans** (strategies): Solutions are trees, not sequences — contain `if-then-else` based on observed outcomes
- **AND-OR search trees**: OR nodes = agent's choices; AND nodes = environment's outcomes
- Solution = subtree with goal at every leaf, one action at each OR node, every outcome at each AND node
- **Cyclic solutions**: Labels allow plans with loops (e.g., "keep trying Right until it works")

### 5.3 Partial Observability (Ch 4.4)

- **Belief state**: Set of possible physical states the agent might be in
- **Sensorless (conformant) problems**: Search in belief-state space; solution is an action sequence that works for ALL states in the initial belief state
- **Partial observability**: Three-stage transition:
  1. **Prediction**: b̂ = PREDICT(b, a)
  2. **Observation prediction**: POSSIBLE-PERCEPTS(b̂)
  3. **Update**: b_o = UPDATE(b̂, o) — subset of b̂ consistent with percept o
- **Recursive state estimator**: b' = UPDATE(PREDICT(b, a), o)
- Solution: conditional plan tested against belief state (not actual state)

### 5.4 Online Search (Ch 4.5)

- Agent interleaves computation and action in unknown environments
- **Online DFS**: Builds map via exploration; backtracks physically; traverses each link twice in worst case
- **LRTA*** (Learning Real-Time A*): Updates heuristic estimates H(s) from experience; "optimism under uncertainty" — assumes untried actions lead to goal

---

## 6. Adversarial Search — Game Playing (Ch 5)

### 6.1 Game Formulation

A game = search problem with adversarial element:

| Component | Description |
|---|---|
| S₀ | Initial state |
| PLAYER(s) | Whose turn |
| ACTIONS(s) | Legal moves |
| RESULT(s, a) | Transition model |
| TERMINAL-TEST(s) | Is the game over? |
| UTILITY(s, p) | Payoff for player p at terminal state |

**Zero-sum games**: Total payoff is constant; one player's gain is the other's loss.

### 6.2 Minimax Algorithm

$$\text{MINIMAX}(s) = \begin{cases} \text{UTILITY}(s) & \text{if TERMINAL-TEST}(s) \\ \max_a \text{MINIMAX}(\text{RESULT}(s,a)) & \text{if PLAYER}(s) = \text{MAX} \\ \min_a \text{MINIMAX}(\text{RESULT}(s,a)) & \text{if PLAYER}(s) = \text{MIN} \end{cases}$$

- Complete depth-first exploration of game tree
- Time: O(b^m), Space: O(bm) or O(m)
- Assumes optimal play from both sides

**Multiplayer games**: Replace single value with vector of values; each player maximizes their own component.

### 6.3 Alpha-Beta Pruning

- Same result as minimax, but prunes branches that cannot influence the decision
- **α** = best (highest) value found so far for MAX along the path
- **β** = best (lowest) value found so far for MIN along the path
- Prune when current node's value is worse than α or β
- **With optimal move ordering**: examines O(b^(m/2)) nodes — effectively doubles search depth
- **Move ordering heuristics**: killer moves, iterative deepening, transposition tables
- **Transposition table**: Hash table of previously evaluated positions (handles different move sequences reaching same state)

### 6.4 Imperfect Real-Time Decisions

When full search is infeasible:

- **Heuristic evaluation function** EVAL(s): Estimates utility of non-terminal states
- **Cutoff test**: Decides when to stop searching and apply EVAL
- **H-MINIMAX**: Minimax with EVAL at cutoff depth

**Evaluation function design:**
- Must order terminal states correctly (win > draw > loss)
- Commonly: **weighted linear function** $\text{EVAL}(s) = \sum w_i f_i(s)$
- Features: material value (pawn=1, knight=3, rook=5, queen=9), pawn structure, king safety
- Nonlinear combinations for feature interactions (e.g., bishop pair bonus)

**Quiescence search**: Extend search in non-quiescent positions (e.g., pending captures)
**Horizon effect**: Delaying inevitable losses beyond search depth; mitigated by **singular extensions**
**Forward pruning**: PROBCUT uses statistics to prune nodes probably outside (α, β) window

### 6.5 Stochastic Games (Ch 5.5)

- Games with chance elements (dice) → **chance nodes** in game tree
- **Expectiminimax**: Average over chance outcomes, weighted by probability

$$\text{EXPECTIMINIMAX}(s) = \begin{cases} \text{UTILITY}(s) & \text{if terminal} \\ \max_a \text{EXPECTIMINIMAX}(\text{RESULT}(s,a)) & \text{if MAX} \\ \min_a \text{EXPECTIMINIMAX}(\text{RESULT}(s,a)) & \text{if MIN} \\ \sum_r P(r) \cdot \text{EXPECTIMINIMAX}(\text{RESULT}(s,r)) & \text{if CHANCE} \end{cases}$$

- Evaluation function must be a **positive linear transformation** of expected utility
- Alpha-beta pruning can be adapted for chance nodes with bounded utility values
- **Monte Carlo simulation (rollouts)**: Play thousands of random games from a position; win % ≈ position value

### 6.6 Partially Observable Games (Ch 5.6)

- **Kriegspiel** (partially observable chess): Pieces invisible; referee announces checks/captures
- **Belief state**: Set of all possible board states given percept history
- **Guaranteed checkmate**: Strategy works for every possible board state in belief state
- **Probabilistic checkmate**: Works with probability 1 (or 1-ε) via randomized play
- **Card games** (bridge, poker): Averaging over clairvoyance — solve each possible deal as fully observable, average results; approximate via Monte Carlo sampling
- Optimal play in partially observable games requires **randomization** (information hiding)

---

## 7. Constraint Satisfaction Problems (Ch 6)

### 7.1 CSP Definition

A CSP consists of three components:

| Component | Description |
|---|---|
| **X** | Set of variables {X₁, ..., Xₙ} |
| **D** | Set of domains {D₁, ..., Dₙ} |
| **C** | Set of constraints specifying allowable value combinations |

- **Assignment**: Values assigned to some/all variables
- **Consistent (legal)**: No constraint violated
- **Complete**: Every variable assigned
- **Solution**: Consistent + complete assignment
- **Constraint graph**: Nodes = variables, links = binary constraints

### 7.2 Types of Constraints

| Type | Description |
|---|---|
| **Unary** | Restricts one variable (e.g., SA ≠ green) |
| **Binary** | Relates two variables (e.g., SA ≠ WA) |
| **Higher-order** | Ternary, n-ary (e.g., Between(X,Y,Z)) |
| **Global** | Arbitrary number of variables (e.g., Alldiff) |
| **Preference** | Soft constraints for optimization (COP) |

- **Alldiff**: All variables must have distinct values (Sudoku, cryptarithmetic)
- **Resource (Atmost)**: Sum constraint with upper bound
- Every finite-domain CSP can be reduced to binary constraints

### 7.3 Constraint Propagation — Inference (Ch 6.2)

#### Node Consistency
A variable is **node-consistent** if all domain values satisfy its unary constraints. Always achievable by simple elimination.

#### Arc Consistency (AC-3)

**Xᵢ is arc-consistent with respect to Xⱼ** if for every value in Dᵢ, there exists a value in Dⱼ satisfying the binary constraint on (Xᵢ, Xⱼ).

**AC-3 algorithm:**
1. Queue all arcs
2. Pop arc (Xᵢ, Xⱼ); call REVISE to remove inconsistent values from Dᵢ
3. If Dᵢ changed, re-add all arcs (Xₖ, Xᵢ) to queue
4. If any domain becomes empty → no solution
5. Complexity: O(cd³) where c = number of constraints, d = max domain size

#### Path Consistency
A set {Xᵢ, Xⱼ} is **path-consistent** with respect to Xₘ if every consistent assignment to {Xᵢ, Xⱼ} can be extended to Xₘ. Strengthens binary constraints using implicit constraints from triples.

#### K-Consistency
- **k-consistent**: For any consistent assignment to k-1 variables, a consistent value exists for any kth variable
- 1-consistency = node consistency; 2-consistency = arc consistency; 3-consistency ≈ path consistency
- **Strongly k-consistent**: k-consistent AND (k-1)-consistent AND ... AND 1-consistent
- Strongly n-consistent CSP with n variables → solvable in O(n²d) without backtracking
- Cost: Establishing n-consistency is exponential in n

### 7.4 Global Constraint Propagation

**Alldiff**: If m variables share n possible values and m > n → inconsistent. Remove singleton values iteratively.

**Resource (Atmost)**: Check sum of minimums; delete values inconsistent with other domains' minimums.

**Bounds propagation**: For continuous/large integer domains, maintain [lower, upper] bounds instead of explicit value sets. Enforce **bounds consistency**.

### 7.5 Sudoku as CSP

- 81 variables (squares), domain {1,...,9}
- 27 Alldiff constraints: 9 rows + 9 columns + 9 boxes
- AC-3 solves easy puzzles; harder puzzles need path consistency or specialized strategies (e.g., "naked triples")
- Demonstrates the power of CSP formalism: define constraints, apply general-purpose solvers

### 7.6 Backtracking Search for CSPs

- **Commutativity**: Order of variable assignments doesn't matter → consider one variable per level
- Naive branching: n·d at root → (n-1)·d next → ... → n!·d^n leaves (vs. d^n actual assignments)
- With commutativity restriction: only d^n leaves

**Key improvements over naive backtracking:**

1. **Variable ordering**: Most Constrained Variable (MRV / minimum remaining values) — pick variable with smallest domain
2. **Value ordering**: Least Constraining Value — pick value that rules out fewest choices for neighbors
3. **Forward checking**: After assigning a variable, remove inconsistent values from neighboring domains
4. **Arc consistency during search (MAC)**: Run AC-3 after each assignment; propagates constraints deeply
5. **Intelligent backtracking**: Backjump to the variable that caused the conflict (not just the previous one)

### 7.7 Local Search for CSPs

- Start with a complete (possibly inconsistent) assignment
- Repeatedly select a conflicted variable and change its value to minimize conflicts
- **Min-conflicts heuristic**: Choose value that violates the fewest constraints
- Extremely effective for large CSPs (e.g., n-queens with millions of queens solvable in minutes)

---

## Summary

| Domain | Key Algorithms | Core Idea |
|---|---|---|
| **Uninformed Search** | BFS, UCS, DFS, IDS, Bidirectional | Systematic exploration without domain knowledge |
| **Informed Search** | Greedy Best-First, A*, IDA*, RBFS, SMA* | Heuristic h(n) guides search toward goal |
| **Local Search** | Hill Climbing, Simulated Annealing, GA | Memory-efficient optimization in large spaces |
| **Nondeterministic** | AND-OR Search | Contingency plans for unpredictable outcomes |
| **Partial Observability** | Belief-State Search | Track set of possible states |
| **Online Search** | LRTA*, Online DFS | Interleave search and action in unknown environments |
| **Adversarial** | Minimax, Alpha-Beta, Expectiminimax | Optimal play against opponents |
| **CSP** | AC-3, Backtracking + Propagation, Min-Conflicts | Exploit constraint structure to prune search space |


## Related

- [[AI Overview]] — All AI topics
- [[01_AI_Foundations]] — Agent types and rationality
- [[03_Logic_and_Reasoning]] — Logical problem-solving
- [[04_Uncertainty_and_Decisions]] — Decision-making under uncertainty
