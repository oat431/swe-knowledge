---
tags:
  - probability
  - bayesian-networks
  - mdp
  - decision-theory
  - artificial-intelligence
source: Russell & Norvig, AIMA, Ch 13–17
---

# Uncertainty and Decisions

Chapters 13–17 of Russell & Norvig develop the mathematical framework for reasoning under **uncertainty** and making **rational decisions**. The arc moves from basic probability through Bayesian networks and temporal models to utility theory, MDPs, and game theory.

---

## 1. Quantifying Uncertainty (Ch 13)

### Why Probability?

- Logical agents cannot cope with **laziness** (incomplete rules), **theoretical ignorance** (no complete domain theory), and **practical ignorance** (missing observations).
- Probability summarizes an agent's **degree of belief** relative to evidence, not a property of the world itself.
- **Decision theory = probability theory + utility theory.**

### Basic Probability

| Concept | Definition |
|---|---|
| **Prior probability** | $P(a)$ — degree of belief without evidence |
| **Conditional probability** | $P(a \mid b) = \frac{P(a \wedge b)}{P(b)}$ |
| **Product rule** | $P(a \wedge b) = P(a \mid b)\,P(b)$ |
| **Kolmogorov's axioms** | $0 \le P(\omega) \le 1$; $\sum_\omega P(\omega) = 1$ |
| **Inclusion–exclusion** | $P(a \vee b) = P(a) + P(b) - P(a \wedge b)$ |

- **Random variables** have domains (discrete or continuous); a **probability distribution** $P(X)$ gives probabilities for all values.
- **Joint distribution** $P(X_1, \dots, X_n)$ specifies the full model; **marginalization** sums out variables.
- **Normalization** avoids computing $P(e)$ directly by working with relative weights.

### Independence and Conditional Independence

- **Absolute independence**: $P(X, Y) = P(X)\,P(Y)$ — rare in practice.
- **Conditional independence**: $P(X, Y \mid Z) = P(X \mid Z)\,P(Y \mid Z)$ — much more common; enables factoring of large joint distributions.
- **Naive Bayes model**: assumes all effect variables are conditionally independent given a single cause — grows linearly, works surprisingly well.

### Bayes' Rule

$$P(\text{cause} \mid \text{effect}) = \frac{P(\text{effect} \mid \text{cause})\,P(\text{cause})}{P(\text{effect})}$$

- Converts **causal** knowledge $P(\text{effect} \mid \text{cause})$ into **diagnostic** $P(\text{cause} \mid \text{effect})$.
- Causal knowledge is more **robust** — unaffected by changes in prior probabilities.
- **General form with normalization**: $P(Y \mid X) = \alpha\, P(X \mid Y)\,P(Y)$.

### Inference from Full Joint Distribution

$$P(X \mid e) = \alpha \sum_y P(X, e, y)$$

- Scales as $O(2^n)$ — impractical for large $n$; serves as theoretical foundation.

---

## 2. Probabilistic Reasoning — Bayesian Networks (Ch 14)

### Bayesian Network Structure

A **Bayesian network** is a directed acyclic graph (DAG) where:
1. Each node = random variable (discrete or continuous).
2. Directed links encode direct influence; **no cycles**.
3. Each node has a **conditional probability table (CPT)**: $P(X_i \mid \text{Parents}(X_i))$.

**Key property**: The full joint distribution is the product of all CPTs:

$$P(x_1, \dots, x_n) = \prod_{i=1}^n P(x_i \mid \text{parents}(X_i))$$

### Compactness

- If each variable has at most $k$ parents, the network requires $O(n \cdot 2^k)$ parameters vs. $O(2^n)$ for the full joint.
- **Causal ordering** produces more compact networks than diagnostic ordering.

### Conditional Independence

- A node is **conditionally independent of its non-descendants, given its parents**.
- **Markov blanket**: parents, children, and children's parents — a node is independent of all others given its Markov blanket.
- **d-separation** provides a general topological criterion for conditional independence.

### Efficient CPT Representations

- **Deterministic nodes**: value fully determined by parents (logical, numerical).
- **Noisy-OR**: models independent inhibitory causes with $O(k)$ parameters instead of $O(2^k)$.
- **Hybrid networks** (discrete + continuous): use **linear Gaussian**, **probit**, or **logit** distributions.

### Exact Inference

| Algorithm | Approach | Complexity |
|---|---|---|
| **Enumeration** | Sum products of CPTs | $O(2^n)$ |
| **Variable elimination** | Dynamic programming; sum out variables one at a time | Depends on largest factor |
| **Clustering / Junction tree** | Merge nodes into polytree | $O(n)$ for polytrees |

- In **polytrees** (singly connected): inference is linear in network size.
- General case: **#P-hard** (strictly harder than NP-complete).

### Approximate Inference

- **Rejection sampling**: generate samples, discard inconsistent ones — exponential waste.
- **Likelihood weighting**: fix evidence, weight samples by likelihood — better but degrades with many evidence variables.
- **Gibbs sampling** (MCMC): iteratively resample each variable given its Markov blanket; converges to true posterior under **detailed balance**.
- **Particle filtering** and **variational methods** are also used.

---

## 3. Probabilistic Reasoning over Time (Ch 15)

### Temporal Models

- The world is modeled as a sequence of **time slices**, each with state variables $X_t$ and evidence variables $E_t$.
- **Markov property**: $X_{t+1}$ is independent of $X_{0:t-1}$ given $X_t$.
- **Stationarity**: transition and sensor models are the same for all $t$.

**Components**:
- **Transition model**: $P(X_t \mid X_{t-1})$
- **Sensor model**: $P(E_t \mid X_t)$

### Inference Tasks

| Task | Query | Method |
|---|---|---|
| **Filtering** | $P(X_t \mid e_{1:t})$ | Forward recursion |
| **Prediction** | $P(X_{t+k} \mid e_{1:t})$ | Repeated application of transition model |
| **Smoothing** | $P(X_k \mid e_{1:t})$ for $k < t$ | Forward–backward algorithm |
| **Most likely sequence** | $\arg\max_{x_{1:t}} P(x_{1:t} \mid e_{1:t})$ | Viterbi algorithm |

### Hidden Markov Models (HMMs)

- Single discrete state variable; **transition matrix** $\mathbf{T}$ and **sensor matrix** $\mathbf{O}_t$.
- Forward: $\mathbf{f}_{1:t+1} = \alpha\, \mathbf{O}_{t+1}\, \mathbf{T}\, \mathbf{f}_{1:t}$
- Backward: $\mathbf{b}_{k+1:t} = \mathbf{T}\, \mathbf{O}_{k+1}\, \mathbf{b}_{k+2:t}$
- Time: $O(S^2 t)$; space: $O(St)$.
- **Fixed-lag smoothing** can be done in constant time per update.

### Kalman Filters

- For **continuous** state variables with **linear Gaussian** transition and sensor models.
- The Gaussian family is **closed** under prediction and conditioning — distributions remain Gaussian for all time.
- Update equations involve **mean** $\mu$ and **covariance** $\Sigma$.
- **Extended Kalman Filter (EKF)** handles mild nonlinearities via linearization.
- **Switching Kalman filter** handles multiple motion models.

### Dynamic Bayesian Networks (DBNs)

- Generalize HMMs and Kalman filters: multiple state variables, arbitrary structure within and across time slices.
- Unrolling a DBN over time gives a standard Bayesian network.
- Exact inference is generally intractable; **particle filtering** is widely used in practice.

---

## 4. Making Simple Decisions — Utility Theory (Ch 16)

### Maximum Expected Utility (MEU)

$$\text{MEU} = \arg\max_a \sum_s P(\text{RESULT}(a) = s \mid a, e)\, U(s)$$

An agent is **rational** if and only if it chooses the action that maximizes expected utility.

### Axioms of Utility (von Neumann–Morgenstern)

| Axiom | Meaning |
|---|---|
| **Orderability** | Agent must have a preference (or indifference) between any two lotteries |
| **Transitivity** | $A \succ B \wedge B \succ C \Rightarrow A \succ C$ |
| **Continuity** | Any outcome between best and worst is equivalent to some lottery |
| **Substitutability** | Indifferent outcomes can be substituted in lotteries |
| **Monotonicity** | Higher probability of preferred outcome → preferred lottery |
| **Decomposability** | Compound lotteries can be collapsed |

**Consequences**: A utility function $U$ exists; $U(L) = \sum_i p_i\, U(S_i)$. Utilities are unique up to **affine transformation**: $U'(S) = a\,U(S) + b$, $a > 0$.

### Utility Functions

- **Preference elicitation**: present choices to determine utility scale.
- **Normalized utilities**: worst = 0, best = 1.
- **Money**: typically shows **risk aversion** (concave utility function); **certainty effect**.
- **Multiattribute utility**: when states have multiple attributes, use **preference independence** and **stochastic dominance**.

### Decision Networks (Influence Diagrams)

- Extend Bayesian networks with **decision nodes** and **utility nodes**.
- **Chance nodes** → probabilistic; **decision nodes** → agent's choices; **utility nodes** → reward.
- Evaluate by: set evidence, set decision, compute expected utility for each action.

### Value of Information

$$\text{VPI}(E_j \mid e) = \left(\sum_{e_j} P(e_j \mid e) \max_a \sum_s P(s \mid e, e_j, a)\, U(s)\right) - \max_a \sum_s P(s \mid e, a)\, U(s)$$

- **Nonnegative**, **order-dependent** (submodular in many cases).
- Guides which observations to gather before acting.

---

## 5. Making Complex Decisions — MDPs (Ch 17)

### Markov Decision Processes

An **MDP** consists of:
- A set of **states** $S$ (with initial state $s_0$)
- A set of **actions** $\text{ACTIONS}(s)$
- A **transition model** $P(s' \mid s, a)$
- A **reward function** $R(s)$

A **policy** $\pi(s)$ maps states to actions. An **optimal policy** $\pi^*$ maximizes expected utility.

### Utilities over Time

| Model | Utility Formula |
|---|---|
| **Additive rewards** | $U_h = R(s_0) + R(s_1) + R(s_2) + \cdots$ |
| **Discounted rewards** | $U_h = R(s_0) + \gamma\,R(s_1) + \gamma^2\,R(s_2) + \cdots$, $\gamma \in [0,1)$ |

- With $\gamma < 1$, infinite-horizon utility is bounded: $U \le R_{\max}/(1-\gamma)$.
- **Stationary preferences** imply either additive or discounted rewards.
- **Finite horizon** → nonstationary policy; **infinite horizon** → stationary policy.

### Bellman Equation

The utility of a state under optimal policy:

$$U^*(s) = R(s) + \gamma \max_a \sum_{s'} P(s' \mid s, a)\, U^*(s')$$

### Solving MDPs

| Algorithm | Type | Approach |
|---|---|---|
| **Value iteration** | Dynamic programming | Iteratively update $U(s)$ until convergence; then extract $\pi^*$ |
| **Policy iteration** | Dynamic programming | Alternate **policy evaluation** (solve linear system) and **policy improvement** |
| **Linear programming** | Optimization | Minimize $\sum_s U(s)$ subject to Bellman constraints |

- **Value iteration**: $U_{i+1}(s) \leftarrow R(s) + \gamma \max_a \sum_{s'} P(s' \mid s, a)\, U_i(s')$
- **Policy iteration**: often converges faster; each iteration solves a system of $|S|$ linear equations.
- Both converge to the unique fixed point of the Bellman equation.

### Partially Observable MDPs (POMDPs)

- Agent does not directly observe the state; maintains a **belief state** (probability distribution over states).
- A POMDP can be mapped to a continuous-state MDP on belief states.
- **Value function** is piecewise-linear and convex over belief space.
- Exact solution is PSPACE-hard; approximate methods (point-based, particle) are used in practice.

### Decision-Theoretic Agents

- **Dynamic decision networks (DDNs)**: unroll a DBN over time, add decision and utility nodes.
- Combine **filtering** (belief update) with **MEU** (action selection).
- Can handle partial observability, sensor failure, and information-gathering actions.

---

## 6. Game Theory (Ch 17.5)

### Multi-Agent Decision Making

When uncertainty comes from **other agents**, game theory applies.

| Concept | Definition |
|---|---|
| **Players** | Agents making decisions |
| **Actions** | Choices available to each player |
| **Payoff function** | Utility for each player given a strategy profile |
| **Strategy** | A policy (pure = deterministic; mixed = randomized) |
| **Nash equilibrium** | No player can improve by unilaterally changing strategy |
| **Dominant strategy** | Best regardless of others' choices |

### Key Results

- **Nash's theorem**: every game has at least one Nash equilibrium (possibly in mixed strategies).
- **Prisoner's dilemma**: dominant strategy equilibrium is Pareto-dominated by cooperation.
- **Zero-sum games**: maximin equilibrium = Nash equilibrium; solvable by **linear programming**.
- **Repeated games**: cooperation can emerge with indefinite repetition (e.g., **Tit-for-Tat**).

### Mechanism Design

- Design the **rules of the game** so that self-interested agents achieve a desirable collective outcome.
- Applications: auctions, Internet routing protocols, distributed problem solving.

---

## Summary Table

| Chapter | Core Topic | Key Algorithms/Models |
|---|---|---|
| Ch 13 | Probability foundations | Bayes' rule, conditioning, normalization |
| Ch 14 | Bayesian networks | Variable elimination, Gibbs sampling, likelihood weighting |
| Ch 15 | Temporal reasoning | Forward–backward, Viterbi, Kalman filter, particle filter |
| Ch 16 | Utility & decisions | MEU, decision networks, value of information |
| Ch 17 | Sequential decisions & games | Value iteration, policy iteration, Nash equilibrium |

---

## Related

- [[01_AI_Foundations]] — search, optimization foundations underlying MDP solvers
- [[03_Logic_and_Reasoning]] — logical agents that probability generalizes
- [[06_Reinforcement_Learning]] — learning MDPs from experience (model-free methods)
