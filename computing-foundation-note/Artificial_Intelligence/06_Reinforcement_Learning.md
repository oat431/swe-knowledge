---
tags: [reinforcement-learning, q-learning, policy-search, artificial-intelligence]
source: "Russell & Norvig, Artificial Intelligence: A Modern Approach, Chapter 21"
---

# Reinforcement Learning (Ch 21)

> **Core Idea:** Without labeled examples, an agent learns optimal behavior through interaction with the environment, from reward and punishment signals.

## 1. Introduction

Reinforcement Learning (RL) differs from supervised learning — the agent is NOT told the correct action to take in each state, but only receives a **reward signal** to judge what is good and what is bad.

### Reward

- Reward is the environment's feedback signal to the agent's behavior
- Animals seem hardwired to recognize pain and hunger as **negative rewards**, pleasure and food intake as **positive rewards**
- Some tasks provide reward only at the end (e.g., chess), others provide it frequently (e.g., table tennis every point scored)
- The agent must "hardwire" recognition of reward signals rather than treating them as ordinary perceptual inputs

### Relationship to MDP

- RL builds on the **Markov Decision Process (MDP)** framework (see Ch 17)
- Unlike Ch 17: here the agent **does not know** the transition model $P(s'|s,a)$ or reward function $R(s)$
- The agent faces an **unknown MDP**

### Three Agent Designs

| Type | Learning Goal | Model Required? |
|---|---|---|
| **Utility-based agent** | Learn state utility function $U(s)$ | ✅ Needs environment model to infer action outcomes |
| **Q-learning agent** | Learn action-utility function $Q(s,a)$ | ❌ No model needed |
| **Reflex agent** | Learn policy directly $\pi(s)$ | ❌ No model needed |

> **Q-learning limitation:** Because it doesn't know what states actions lead to, Q-learning agents cannot perform lookahead, which severely limits learning ability.

---

## 2. Passive Reinforcement Learning

**Passive learning:** The agent's policy $\pi$ is **fixed**; the goal is to learn the utility function $U^\pi(s)$ for that policy.

### Direct Utility Estimation

**Basic idea** (Widrow & Hoff, 1960): State utility = expected cumulative reward (reward-to-go) from following policy $\pi$ starting from that state.

**Algorithm:**
- Each trial provides one reward-to-go sample for each visited state
- Maintain a **running average** table for each state
- In the limit of infinite trials, sample averages converge to true expectations

**Utility definition:**

$$U^\pi(s) = E\left[\sum_{t=0}^{\infty} \gamma^t R(S_t)\right]$$

Where $S_0 = s$ and $\gamma$ is the discount factor.

**Drawbacks:**
- Reduces RL to a standard supervised learning problem
- **Ignores dependencies between states** — utility values must satisfy the Bellman equation
- Hypothesis space is much larger than needed (includes functions violating Bellman equation)
- Convergence is typically very **slow**

### Adaptive Dynamic Programming (ADP)

**Core idea:** Exploit constraints between state utilities — learn the transition model, then solve the MDP with dynamic programming.

**Algorithm (PASSIVE-ADP-AGENT):**
1. Maintain a frequency table for transition model $P(s'|s, \pi(s))$
2. Update frequency counts after each observed transition $s \xrightarrow{a} s'$
3. Solve the Bellman equation using **policy evaluation**:

$$U^\pi(s) = R(s) + \gamma \sum_{s'} P(s'|s, \pi(s)) U^\pi(s')$$

**Characteristics:**
- Exploits inter-state constraints by learning the transition model
- Provides a **standard benchmark** for evaluating other RL algorithms
- **Infeasible** in large state spaces (e.g., backgammon has ~$10^{50}$ states)

### Temporal-Difference Learning (TD)

**Core idea:** Adjust utility estimates directly from observed transitions to satisfy Bellman equation constraints, **without learning a transition model**.

**TD update rule:**

$$U^\pi(s) \leftarrow U^\pi(s) + \alpha \left( R(s) + \gamma U^\pi(s') - U^\pi(s) \right)$$

Where:
- $\alpha$ is the learning rate
- $R(s) + \gamma U^\pi(s') - U^\pi(s)$ is the **temporal difference error**
- The update adjusts $U^\pi(s)$ toward the utility of its successor state

**TD vs ADP Comparison:**

| Property | TD | ADP |
|---|---|---|
| Adjustment per transition | **One adjustment** to observed transition only | Adjusts all states needing consistency |
| Model required? | ❌ No transition model needed | ✅ Needs to learn transition model |
| Computational complexity | Simple, low computation | Requires solving linear equations or value iteration |
| Convergence speed | Slower, higher variance | Faster (limited by model learning) |
| Large state spaces | ✅ Feasible | ❌ Infeasible |

**Convergence condition:** Learning rate $\alpha(n)$ decreases with state visit count (satisfies Robbins-Monro conditions), guaranteeing convergence to correct values.

**Extension — Prioritized Sweeping:**
- Use heuristics to prioritize adjustments, executing only the most important ones
- Prioritize states whose successors just experienced large utility changes
- Can achieve learning speed close to ADP, with orders of magnitude better computational efficiency

### Bayesian RL and Robust Control

- **Bayesian RL:** Maintain prior $P(h)$ over possible model hypotheses $h$, update posterior $P(h|e)$ using Bayes' rule, select policy maximizing posterior-weighted expected utility
- **Robust Control:** Consider a set of possible models $H$, select the policy that performs best in the worst case: $\pi^* = \arg\max_\pi \min_h u_h^\pi$

---

## 3. Active Reinforcement Learning

**Active learning:** The agent must **decide what actions to take** rather than executing a fixed policy.

### Exploration vs Exploitation

**Core dilemma:**
- **Exploitation:** Choose the best action according to current estimates to maximize immediate reward
- **Exploration:** Try new actions to improve the environmental model for better long-term returns

> Pure exploitation may lead to suboptimal behavior; pure exploration wastes time on actions that yield no practical benefit.

**Greedy Agent Failure:**
- Greedy agents always choose the action recommended by the current optimal policy
- Experiments show greedy agents **rarely** converge to optimal policies
- Reason: learned model ≠ true environment; the optimal policy under the learned model may be suboptimal in the real environment

### GLIE Schemes (Greedy in the Limit of Infinite Exploration)

**GLIE Requirements:**
1. Each action in each state is tried **infinitely many times** (avoid missing optimal actions due to bad luck)
2. Eventually **becomes greedy** (actions become optimal relative to the learned true model)

**Simple GLIE scheme:** Choose a random action with probability $1/t$, otherwise follow the greedy policy.

**Better scheme:** Give higher weight to less-tried actions while tending to avoid actions known to have low utility — achievable by modifying the Bellman equation.

---

## 4. Generalization in RL

- For large state spaces, storing utility for each state in a table is impractical
- Can use **function approximation** (e.g., neural networks, linear functions) to represent the utility function
- Direct utility estimation naturally reduces to a standard inductive learning problem, applicable to Ch 18 learning techniques

---

## 5. Policy Search

- Search directly in policy space rather than learning a utility function
- Applicable to reflex agents: learn direct mapping from states to actions $\pi(s)$
- Parameterized policy representation makes search more efficient

---

## Key Formulas

| Formula | Name | Meaning |
|---|---|---|
| $U^\pi(s) = E\left[\sum_{t=0}^{\infty} \gamma^t R(S_t)\right]$ | Utility definition | Expected cumulative discounted reward from state $s$ under policy $\pi$ |
| $U^\pi(s) = R(s) + \gamma \sum_{s'} P(s'|s,\pi(s)) U^\pi(s')$ | Bellman equation (passive) | Utility constraint under fixed policy |
| $U(s) = R(s) + \gamma \max_a \sum_{s'} P(s'|s,a) U(s')$ | Bellman equation (active) | Utility constraint under optimal policy |
| $U^\pi(s) \leftarrow U^\pi(s) + \alpha(R(s) + \gamma U^\pi(s') - U^\pi(s))$ | TD update rule | Adjust utility estimate using TD error |

## Summary

- **Passive learning:** Fixed policy, learn utility function
  - Direct utility estimation: simple but slow, ignores inter-state constraints
  - ADP: learn model + dynamic planning, efficient but computationally heavy
  - TD: no model, direct updates using TD error, simple and efficient
- **Active learning:** Learn policy and utility simultaneously
  - Exploration vs exploitation tradeoff is the core issue
  - GLIE schemes guarantee eventual convergence to optimal policy
- **Generalization:** Use function approximation for large state spaces
- **Policy search:** Direct optimization in policy space

## Related

- [[AI Overview]] — All AI topics
- [[04_Uncertainty_and_Decisions]] — MDP framework
- [[05_Machine_Learning]] — Supervised learning methods
