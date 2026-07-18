---
tags: [ai-ethics, philosophy, ai-future, artificial-intelligence]
---

# AI Ethics and Future

> **Source:** Russell & Norvig, *Artificial Intelligence: A Modern Approach*, Ch 26–27

## Chapter 26 — Philosophical Foundations

### 26.1 Weak AI: Can Machines Act Intelligently?

**Weak AI** holds that machines can *simulate* intelligent behaviour — the question is whether the output is indistinguishable from human performance, not whether the machine truly "understands."

#### The Turing Test
- Proposed by Alan Turing (1950) as an operational definition of intelligence.
- A human judge converses with a machine and a human (both hidden); if the judge cannot reliably tell which is the machine, the machine passes.
- **Turing's "polite convention":** in everyday life we never have direct access to others' mental states, yet we extend the courtesy of assuming they think — the same courtesy should extend to sufficiently capable machines.
- Weak AI sidesteps metaphysical debates: intelligence is assessed by *behaviour*, not by internal experience.

#### The Mathematical Objection (Gödel's Incompleteness)
- Gödel (1931) showed that for any consistent formal system F powerful enough to do arithmetic, there exists a true sentence G(F) that F cannot prove.
- **Lucas–Penrose claim:** humans can see the truth of G(F) while machines (as formal systems) cannot, so machines are mentally inferior.
- **Three counter-arguments:**
  1. Computers are finite, so they can be described in propositional logic (not subject to Gödel's theorem).
  2. There are sentences that any given human cannot consistently assert — this doesn't make humans mentally defective.
  3. There is no evidence that humans are immune from the incompleteness limitations; the claim that humans transcend formal systems is unprovable.

#### The Argument from Informality
- **Dreyfus critique:** human behaviour is too complex and context-dependent to be captured by any set of rules; computers can only follow rules → computers cannot be truly intelligent.
- The **qualification problem** — it is impossible to specify all preconditions for an action in logical rules.
- Dreyfus targets GOFAI ("Good Old-Fashioned AI"): rule-based, logical agents.
- **Rebuttal:** probabilistic reasoning (Ch 13–17), learning algorithms (Ch 18–21), and embodied cognition address many of Dreyfus's objections. His critique is valid against simple rule-based systems, not against modern AI.
- **Embodied cognition:** cognition takes place within a body embedded in an environment; robotics and perception are central, not peripheral.

### 26.2 Strong AI: Can Machines Really Think?

**Strong AI** claims that a suitably programmed computer does not merely *simulate* a mind — it literally *has* a mind, with understanding, consciousness, and mental states.

#### The Argument from Consciousness
- Jefferson (1949): a machine must *feel* thoughts and emotions, not merely manipulate symbols.
- Key concepts: **consciousness**, **phenomenology** (direct experience), **intentionality** (beliefs that are "about" something in the real world).
- Turing's response: the question is ill-defined; the "polite convention" will naturally extend to machines as they become more sophisticated.

#### The Mind–Body Problem
- **Dualism** (Descartes): mind and body are separate substances — raises the question of how the immaterial mind controls the material body.
- **Physicalism / Monism**: mental states *are* physical states (brain states). Most modern philosophers hold this view. It allows, in principle, for strong AI.
- **Brain-in-a-vat thought experiment:** a brain fed simulated sensory input would have the same brain state as a real person, yet wouldn't truly "know" it is eating a hamburger.
  - **Wide content:** mental state depends on brain state *and* environment history.
  - **Narrow content:** mental state depends only on brain state (functional role).

#### Functionalism
- A mental state is any intermediate *causal condition* between input and output.
- Any two systems with **isomorphic causal processes** have the same mental states — regardless of substrate.
- **Brain replacement experiment** (Moravec): gradually replace neurons with electronic equivalents that preserve input–output behaviour. If behaviour is unchanged, is consciousness preserved?
  - Functionalists say yes (consciousness depends on functional organisation, not substrate).
  - Biological naturalists (Searle) say no (consciousness requires specific biological causal powers).

#### Biological Naturalism and the Chinese Room
- **Searle (1980):** mental states are high-level *emergent features* caused by low-level neuron processes. Running the right program is *not sufficient* for having a mind.
- **Chinese Room argument:** a human follows rules to manipulate Chinese symbols, producing intelligent Chinese output without understanding Chinese → syntax does not constitute semantics.
- Searle's axioms:
  1. Computer programs are formal (syntactic).
  2. Human minds have mental contents (semantic).
  3. Syntax alone is neither constitutive of nor sufficient for semantics.
  4. Brains cause minds.
- **Systems reply:** the *room as a whole* understands Chinese, even if the human inside does not — just as the CPU doesn't take cube roots but the computer does.
- **Searle's rebuttal:** understanding is not in the human and cannot be in the paper, so there is no understanding.
- **Critique:** Searle's argument is essentially an **intuition pump** — it reinforces prior intuitions rather than providing proof. Whether axiom 3 is accepted determines the conclusion.

#### Consciousness, Qualia, and the Explanatory Gap
- **Qualia:** the intrinsic, subjective quality of experience (what it *feels like* to see red, taste chocolate, etc.).
- **Inverted spectrum thought experiment:** two people could have identical functional organisation but different subjective colour experiences.
- **Explanatory gap:** no currently accepted scientific reasoning bridges the gap between physical brain processes and subjective experience.
- Dennett (1991): denies qualia exist as a distinct philosophical category — they are a confusion.
- Turing's position: consciousness is a mystery, but it need not be solved before building intelligent programs.

---

## Chapter 27 — AI: The Present and the Future

### 27.1 AI Components and Achievements

Modern AI systems combine multiple techniques. Key components that have achieved human-level or superhuman performance:

| Component | Example | Achievement |
|---|---|---|
| Speech recognition | Deep speech models | Near-human transcription accuracy |
| Image classification | Deep CNNs (ResNet, etc.) | Superhuman on ImageNet |
| Game playing | AlphaGo, AlphaZero | Defeated world champions in Go, chess, shogi |
| Machine translation | Transformer-based models | Near-human quality for major language pairs |
| Robotics | Self-driving cars (Waymo, etc.) | Millions of autonomous miles driven |
| Medical diagnosis | AI radiologists | Match or exceed specialist accuracy in some tasks |

### 27.2 The State of the Art

- **Narrow AI dominance:** virtually all deployed AI systems are narrow — they excel at specific tasks but lack general intelligence.
- **Deep learning revolution** (2012–present): large neural networks trained on massive datasets have driven breakthroughs in vision, language, and decision-making.
- **Foundation models / Large Language Models:** GPT, LLaMA, and similar models demonstrate emergent capabilities from scale — few-shot learning, reasoning, code generation.
- **Reinforcement learning successes:** game playing (Atari, Go, StarCraft), robotic control, chip design, protein folding (AlphaFold).

### 27.3 Ethical and Societal Concerns

#### Fairness and Bias
- AI systems trained on historical data can **perpetuate and amplify** existing biases (racial, gender, socioeconomic).
- **Algorithmic fairness** metrics: demographic parity, equalised odds, calibration — often mathematically incompatible with each other (impossibility theorems).
- Mitigation: bias audits, diverse training data, fairness-aware learning, post-hoc corrections.

#### Transparency and Explainability
- Many high-performing models (deep neural networks) are **black boxes** — their decisions are difficult to interpret.
- **Explainable AI (XAI):** techniques to make model decisions understandable to humans (LIME, SHAP, attention visualisation).
- Regulatory pressure: GDPR "right to explanation"; EU AI Act requirements for high-risk systems.

#### Privacy
- AI systems consume vast amounts of personal data.
- **Differential privacy**, **federated learning**, and **data minimisation** are technical responses.
- Tension between model performance (more data → better models) and individual privacy rights.

#### Safety and Robustness
- **Adversarial examples:** small, carefully crafted perturbations can cause misclassification.
- **Distribution shift:** models trained on one data distribution may fail silently on another.
- **Specification gaming:** optimising a reward function may produce unintended behaviours that satisfy the letter but not the spirit of the objective.

#### Autonomy and Control
- **Alignment problem:** ensuring AI systems pursue goals aligned with human values and intentions.
- **Value alignment:** how to specify human values formally — the reward modelling approach (inverse reinforcement learning, RLHF).
- **Corrigibility:** designing systems that allow humans to correct or shut them down.
- **Superintelligence risk** (Bostrom, 2014): a sufficiently advanced AI with misaligned goals could be catastrophic; the control problem must be solved *before* such systems are built.

#### Economic Impact and Employment
- AI automation may displace large categories of jobs (transportation, manufacturing, clerical, some professional services).
- Historical precedent: technological revolutions create new jobs, but transition periods can be painful.
- **Complementarity:** AI augments human capabilities in many domains (decision support, creative tools, medical diagnosis).
- Policy responses: education reform, universal basic income proposals, retraining programmes.

#### Autonomous Weapons
- Lethal autonomous weapons systems (LAWS) can select and engage targets without human intervention.
- Debate over meaningful human control, accountability, and the threshold for use of force.
- Campaigns to ban or regulate autonomous weapons (e.g., Campaign to Stop Killer Robots).

### 27.4 The Future of AI

#### Near-Term Trends
- Continued scaling of foundation models; multimodal models (text + image + audio + video).
- AI integration into scientific discovery (materials science, drug design, climate modelling).
- Autonomous systems: vehicles, drones, warehouse robots becoming mainstream.
- AI-assisted software engineering (code generation, testing, debugging).

#### Long-Term Questions
- **Artificial General Intelligence (AGI):** systems that can learn and perform any intellectual task a human can. Timeline and feasibility remain highly uncertain.
- **Whole brain emulation:** scanning and simulating a complete brain at neuron-level detail. Technically distant but conceptually well-defined.
- **Superintelligence:** if achieved, the most transformative (and potentially dangerous) technology in human history.
- **Existential risk:** ensuring that advanced AI systems are beneficial and under human control is arguably the most important problem facing humanity.

### 27.5 Principles for Beneficial AI

- **Asilomar AI Principles** (2017): 23 principles endorsed by researchers covering research goals, ethics, and long-term issues.
- Key principles:
  1. **Safety:** AI systems should be safe and secure throughout their operational lifetime.
  2. **Transparency:** the purpose and behaviour of AI systems should be explainable.
  3. **Fairness:** AI should not unduly discriminate or create不公平 outcomes.
  4. **Human control:** humans should choose how and whether to delegate decisions to AI.
  5. **Non-subversion:** the power of AI should respect and improve social and civic processes.
  6. **Shared benefit:** AI should benefit and empower as many people as possible.
  7. **Shared prosperity:** economic prosperity from AI should be broadly shared.

---

## Summary

| Topic | Key Takeaway |
|---|---|
| Weak AI vs Strong AI | Weak AI simulates intelligence; strong AI claims machines can truly have minds |
| Turing Test | Behavioural test — if a machine fools a judge, it exhibits intelligence |
| Gödel's objection | Applies to infinite formal systems; does not prove human superiority |
| Chinese Room | Syntax ≠ semantics (Searle); controversial intuition pump |
| Functionalism | Mental states are defined by causal roles, not substrate |
| Qualia | Subjective experience; the "hard problem" of consciousness |
| AI fairness | Historical bias in data → biased models; fairness metrics are mutually incompatible |
| Alignment | Ensuring AI goals match human values; the central safety challenge |
| Superintelligence | If achieved, must be aligned *before* it is too powerful to control |
| Beneficial AI | Safety, transparency, fairness, human control, shared benefit |


## Related

- [[AI Overview]] — All AI topics
- [[01_AI_Foundations]] — What is AI
- [[05_Machine_Learning]] — Current AI capabilities
- [[07_NLP_and_Perception]] — AI applications
