---
title: "AI Foundations — What is AI & Intelligent Agents"
tags:
  - ai
  - agents
  - rationality
  - artificial-intelligence
source: "Russell & Norvig, Artificial Intelligence: A Modern Approach, 3rd Ed., Ch 1–2"
---

# AI Foundations

## 1 What Is AI? (Ch 1)

### 1.1 Four Definitions of AI

Russell & Norvig organize the definitions of AI along two dimensions: **thought vs. behavior** and **human vs. rational**.

| | **Human-based** | **Ideal/rational** |
|---|---|---|
| **Thinking** | *Thinking humanly* — cognitive modeling (Newell & Simon's GPS) | *Thinking rationally* — laws of thought / logic (Aristotle, syllogisms) |
| **Behavior** | *Acting humanly* — Turing Test | *Acting rationally* — rational agent approach ✓ (this book) |

> **Rationality** = doing the "right thing," given what the agent knows.

**The Turing Test (Turing, 1950):** A computer passes if a human interrogator cannot distinguish its written responses from a human's. Requires:
- Natural language processing
- Knowledge representation
- Automated reasoning
- Machine learning

The **Total Turing Test** adds video signal (computer vision) and physical object manipulation (robotics).

**Rational Agent approach (adopted in the book):** An agent acts so as to achieve the best outcome, or when there is uncertainty, the best expected outcome. More general than "laws of thought" (correct inference is only one mechanism for rationality), and more amenable to scientific development than human-imitation approaches.

### 1.2 Foundations of AI

AI draws from multiple disciplines:

| Discipline | Key Question | Key Contributors |
|---|---|---|
| **Philosophy** | Can formal rules draw valid conclusions? How does mind arise from brain? | Aristotle (syllogisms), Descartes (dualism vs. materialism), Bacon/Locke/Hume (empiricism, induction), Hobbes ("reasoning is computation") |
| **Mathematics** | What can be formally computed? How to reason under uncertainty? | Boole (propositional logic), Frege (first-order logic), Gödel (incompleteness), Turing (computability), Cook/Karp (NP-completeness), Bayes (probabilistic reasoning) |
| **Economics** | How to maximize payoff? | Adam Smith (agent-based economics), von Neumann & Morgenstern (game theory), Bellman (MDPs), Simon (satisficing) |
| **Neuroscience** | How do brains process information? | Broca (localized brain functions), Golgi/Cajal (neurons), McCulloch & Pitts (artificial neuron model) |
| **Psychology** | How do humans think and act? | Helmholtz, Wundt (experimental psych), Craik (knowledge-based agent), cognitive psychology (brain as information processor) |
| **Computer Engineering** | How to build efficient computers? | Turing, Zuse, Atanasoff, Babbage/Lovelace, von Neumann |
| **Control Theory** | How can artifacts operate under their own control? | Wiener (cybernetics, feedback), Ashby (homeostatic devices) |
| **Linguistics** | How does language relate to thought? | Chomsky (syntactic structures, creativity in language) |

### 1.3 History of AI

| Period | Key Events |
|---|---|
| **1943–1955 (Gestation)** | McCulloch & Pitts (artificial neurons, 1943), Hebb (Hebbian learning, 1949), Minsky & Edmonds (SNARC, 1950), Turing (1950 paper) |
| **1956 (Birth)** | Dartmouth workshop — McCarthy coined "artificial intelligence." Newell & Simon demonstrated Logic Theorist. |
| **1952–1969 (Early enthusiasm)** | GPS (General Problem Solver), Samuel's checkers learning, McCarthy's Lisp (1958), Advice Taker, blocks world microworlds |
| **1966–1973 (Reality check)** | Machine translation failures, combinatorial explosion, Minsky & Papert's *Perceptrons* limitations |
| **1969–1979 (Knowledge-based systems)** | DENDRAL, MYCIN (expert systems), shift from weak methods to domain-specific knowledge |
| **1980–present (AI industry)** | R1 commercial expert system, Japanese Fifth Generation, AI boom then "AI Winter" |
| **1986–present (Neural net revival)** | Back-propagation reinvented (Rumelhart & McClelland), connectionist models |
| **1987–present (Scientific method)** | Rigorous empirical methodology, HMMs for speech, Bayesian networks (Pearl), data mining |
| **1995–present (Intelligent agents)** | SOAR architecture, Internet agents, AGI movement, "Friendly AI" concerns |
| **2001–present (Big data)** | Large datasets sometimes matter more than algorithms (Yarowsky, Banko & Brill) |

### 1.4 State of the Art (circa 2010)

- **Robotic vehicles:** STANLEY (2005 DARPA Grand Challenge), CMU's BOSS
- **Speech recognition:** automated airline booking systems
- **Autonomous planning:** NASA's Remote Agent, MAPGEN, MEXAR2
- **Game playing:** Deep Blue defeats Kasparov (1997)
- **Spam filtering:** learning classifiers over 1B+ messages/day
- **Logistics planning:** DART (1991 Gulf War)
- **Robotics:** iRobot Roomba, PackBot
- **Machine translation:** statistical Arabic→English translation

---

## 2 Intelligent Agents (Ch 2)

### 2.1 Agents and Environments

> **Agent** = anything that can be viewed as perceiving its environment through **sensors** and acting upon that environment through **actuators**.

- **Percept** — agent's perceptual inputs at a given instant
- **Percept sequence** — complete history of everything the agent has ever perceived
- **Agent function** — abstract mathematical mapping from percept sequences → actions
- **Agent program** — concrete implementation of the agent function in software/hardware

**Vacuum-cleaner world** (toy example): two locations A and B, agent perceives location and dirt status, can move left/right or suck.

### 2.2 Rationality

> **Rational agent** — for each possible percept sequence, selects an action expected to **maximize its performance measure**, given the evidence from percepts and built-in knowledge.

**What determines rationality (four factors):**
1. Performance measure (criterion of success)
2. Prior knowledge of the environment
3. Available actions
4. Percept sequence to date

**Key distinctions:**
- **Rationality ≠ Omniscience** — rational agents maximize *expected* performance, not *actual* (impossible to know all outcomes)
- **Rationality ≠ Perfection** — rationality is about the best action given available information
- **Information gathering** — rational agents should act to modify future percepts (e.g., looking before crossing the street)
- **Learning** — agents should adapt from experience, not rely solely on designer's prior knowledge
- **Autonomy** — an agent that relies on prior knowledge rather than its own percepts lacks autonomy; rational agents should be autonomous

### 2.3 PEAS: Specifying Task Environments

**PEAS = Performance, Environment, Actuators, Sensors**

| Agent Type | Performance Measure | Environment | Actuators | Sensors |
|---|---|---|---|---|
| **Taxi driver** | Safe, fast, legal, comfortable trip, profits | Roads, traffic, pedestrians, customers | Steering, accelerator, brake, signal, horn, display | Cameras, sonar, speedometer, GPS, accelerometer, engine sensors |
| **Medical diagnosis** | Healthy patient, reduced costs | Patient, hospital, staff | Display of questions, diagnoses, treatments, referrals | Keyboard entry of symptoms, findings, patient answers |
| **Part-picking robot** | % of parts in correct bins | Conveyor belt with parts, bins | Jointed arm and hand | Camera, joint angle sensors |
| **Interactive English tutor** | Student's score on test | Students, testing agency | Display of exercises, suggestions, corrections | Keyboard entry |

### 2.4 Properties of Task Environments

Seven key dimensions that determine agent design:

| Property | Description | Examples |
|---|---|---|
| **Fully vs. Partially Observable** | Can the agent see the complete state? | Chess = fully; taxi driving = partially (can't see other drivers' intentions) |
| **Single vs. Multi-agent** | One agent or multiple interacting agents? | Crossword = single; chess = multi (competitive); taxi = multi (cooperative + competitive) |
| **Deterministic vs. Stochastic** | Is the next state fully determined by current state + action? | Chess = deterministic; taxi = stochastic (tire blowouts, unpredictable traffic) |
| **Episodic vs. Sequential** | Does each decision affect future episodes? | Part inspection = episodic; chess/taxi = sequential |
| **Static vs. Dynamic** | Does the environment change while agent deliberates? | Crossword = static; taxi = dynamic; chess with clock = semidynamic |
| **Discrete vs. Continuous** | State, time, percepts, actions — discrete or continuous? | Chess = discrete; taxi = continuous |
| **Known vs. Unknown** | Are the rules/outcomes of the environment known to the agent? | Solitaire = known (rules known, cards hidden); new video game = unknown |

> **Hardest case:** partially observable, multiagent, stochastic, sequential, dynamic, continuous, unknown → taxi driving in a foreign country.

### 2.5 Agent Structures (Types of Agent Programs)

Four basic agent architectures (in increasing sophistication):

1. **Simple reflex agents** — act only on the current percept (condition-action rules)
2. **Model-based reflex agents** — maintain internal state to track unobserved aspects of the world
3. **Goal-based agents** — have goals that influence decision-making
4. **Utility-based agents** — use a utility function to evaluate tradeoffs between states
5. **Learning agents** — can improve performance over time by modifying components

---

## Key Concepts Summary

| Concept | Definition |
|---|---|
| **Artificial Intelligence** | The study of rational agents — systems that perceive and act to achieve the best outcomes |
| **Rational Agent** | Acts to maximize expected performance measure given percepts and knowledge |
| **Agent Function** | Abstract mapping from percept sequences to actions |
| **PEAS** | Framework for specifying task environments (Performance, Environment, Actuators, Sensors) |
| **Turing Test** | A machine passes if a human interrogator can't distinguish it from a human |
| **Limited Rationality** | Acting appropriately when there isn't enough time for all computations |
| **Autonomy** | Agent's behavior determined by its own percepts, not solely by designer's prior knowledge |
| **Bounded Optimality** | The best program possible given computational constraints |


## Related

- [[AI Overview]] — All AI topics
- [[02_Search_and_CSP]] — Problem-solving agents
- [[05_Machine_Learning]] — Learning agents
