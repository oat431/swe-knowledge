---
tags: [nlp, computer-vision, robotics, artificial-intelligence]
source: "Russell & Norvig — AIMA Ch 22-25"
---

# NLP and Perception (Ch 22-25)

> Based on Russell & Norvig *Artificial Intelligence: A Modern Approach* Chapters 22–25.

---

## Ch 22: Natural Language Processing

### 22.1 Language Models

N-gram models are the foundation of statistical language models. They learn probability distributions from sequences of characters or words.

- **N-gram model:** $P(c_i | c_{i-2:i-1})$ — probability of the next symbol given the previous n-1 symbols
- **Applications:** Language identification, genre classification, named entity recognition

**Smoothing** — solving the zero-frequency problem for sequences not seen in the training corpus:
- **Laplace smoothing (add-one):** The simplest method — add 1 to each count
- **Backoff model:** Fall back to lower-order n-gram when higher-order counts are low
- **Linear interpolation:** Linear combination of trigram, bigram, unigram
$$P^*(c_i|c_{i-2:i-1}) = \lambda_3 P(c_i|c_{i-2:i-1}) + \lambda_2 P(c_i|c_{i-1}) + \lambda_1 P(c_i)$$

**Model Evaluation:**
- **Perplexity:** $\text{Perplexity}(c_{1:N}) = P(c_{1:N})^{-1/N}$ — normalized inverse probability. Lower is better
- **OOV (Out-of-Vocabulary):** Handle unknown words with `<UNK>` token

### 22.2 Text Classification

Classifying text into predefined categories. Spam detection, sentiment analysis, etc.

- **Language model approach:** Learn a separate n-gram model for each class → classify using Bayes' rule
$$\arg\max_c P(c|\text{message}) = \arg\max_c P(\text{message}|c)P(c)$$
- **ML approach:** Convert messages to feature vectors, then apply a classifier
- **Bag of Words:** Use word frequency as features (ignores word order)

### 22.3 Information Retrieval

- **Query processing:** Search user queries against a document collection
- **TF-IDF:** Product of term frequency and inverse document frequency to measure word importance within a document
- **Query expansion:** Add synonyms and related terms to broaden search scope
- **Question Answering:** Extract short answers from search results (e.g., ASKMSR system)

### 22.4 Information Extraction

Extracting structured information from text.

- **Attribute-based extraction:** Pattern matching using regular expressions
- **Relation extraction:** Extract relationships between multiple entities (e.g., FASTUS system — 5-stage cascade FSA)
  1. Tokenization
  2. Complex word handling
  3. Basic phrase processing (noun group, verb group)
  4. Complex phrase processing
  5. Structure merging
- **HMM-based extraction:** Sequence labeling with Hidden Markov Models
- **CRF (Conditional Random Field):** Discriminative model for direct conditional probability modeling

---

## Ch 23: Natural Language for Communication

### 23.1 Phrase Structure Grammars

Syntactic analysis through grammar.

- **PCFG (Probabilistic Context-Free Grammar):** Assign probabilities to each rewrite rule
  ```
  VP → Verb [0.70]
      | VP NP [0.30]
  ```
- **Non-terminal symbols:** NP, VP and other syntactic categories
- **Terminal symbols:** Actual words
- **Lexicon:** Open-class parts of speech (nouns, verbs) and closed-class (articles, prepositions)
- **Parse Tree:** Tree representation of a sentence's syntactic structure

### 23.2 Syntactic Analysis (Parsing)

The process of analyzing a sentence's syntactic structure.

- **Top-down / Bottom-up parsing:** Starting from S / starting from words
- **Chart Parser:** Dynamic programming to avoid redundant computation
- **CYK Algorithm:** Parses in O(n³m) time using Chomsky Normal Form
- **A* Parsing:** Explores only the most probable parses

**PCFG Learning:**
- With treebank: Estimate probabilities by counting rules
- From unlabeled corpus: Inside-Outside algorithm (EM approach)

### 23.3 Augmented Grammars and Semantic Interpretation

- **Definite Clause Grammar (DCG):** Extended grammar based on Prolog
- **Semantic interpretation:** Syntax → semantics transformation (compositional semantics)
- **Metonymy:** Substituting one object for another (figure of speech)
- **Metaphor:** Using words in non-literal sense

### 23.4 Machine Translation

Core idea of statistical machine translation:

- **Translation model $P(F|E)$:** Probability of French sentence F given English sentence E
- **Language model $P(F)$:** Fluency of the target language
- **Decoding:** $\arg\max_F P(F|E)P(F)$
- **IBM Models 1–5:** Alignment and reordering models
- Trained from parallel corpora using the EM algorithm

### 23.5 Speech Recognition

Converting speech to text.

- **Acoustic model:** Modeling acoustic characteristics of phonemes
- **HMM-based:** Phonemes as HMM states, acoustic features as observations
- Requires vocabulary, language model, pronunciation dictionary
- Key challenges: segmentation, coarticulation, noise

---

## Ch 24: Perception (Computer Vision)

### 24.1 Image Formation

Physical and geometric foundations of image formation.

- **Pinhole camera:** Simplest image formation model
  - Perspective Projection: $x = -fX/Z, \quad y = -fY/Z$
  - Vanishing Point: Parallel lines converge at a distant point
- **Lens systems:** Collect more photons while maintaining focus
  - Depth of Field: Range where focus is maintained
- **Scaled Orthographic Projection:** Simplified projection for distant objects
- **Illumination and shading:**
  - Lambert's Cosine Law: $I = \rho I_0 \cos\theta$
  - Diffuse reflection vs Specular reflection
  - Shadows, indirect lighting (Interreflection)
- **Color:** Trichromacy — all colors can be matched with R/G/B

### 24.2 Early Image-Processing Operations

- **Edge detection:** Detect boundaries where brightness changes rapidly (Canny edge detector)
- **Texture analysis:** Frequency/orientation-based pattern analysis
- **Segmentation:** Divide the image into uniform regions
  - Normalized Cuts (Shi & Malik): Graph partitioning based
  - Superpixels: Over-segmentation into hundreds of uniform regions

### 24.3 Object Recognition by Appearance

Appearance-based object recognition.

- **Sliding Window:** Sweep a fixed-size window across the entire image
- **Face detection:** Brightness/gradient features → classifier (SVM, etc.)
  - Estimate rotation → correct orientation → classify
  - Non-maximum suppression to remove duplicates
- **HOG features (Histogram of Oriented Gradients):**
  - Compute gradient direction histograms per cell
  - Effective for pedestrian detection
- **SIFT (Scale-Invariant Feature Transform):** Scale-invariant feature point descriptors

### 24.4 Reconstructing the 3D World

Recovering 3D information from 2D images.

- **Motion Parallax:** Estimate depth from optical flow during camera movement
  - Focus of Expansion
  - Depth ratio extractable
- **Binocular Stereopsis:** Estimate depth from disparity between two cameras
  - Correspondence problem must be solved
- **Multiple Views:** Structure from Motion
- **Texture:** Surface orientation estimation
- **Shading:** Surface normal vector recovery
- **Contour:** T-junction, figure-ground separation
- **Familiar objects:** Distance estimation from known size (Alignment Method)

### 24.5 Object Recognition from Structural Information

- **Deformable Template:** Model spatial relationships between pattern elements
- **Pictorial Structure Model:** Connect body parts in tree structure, optimal matching via dynamic programming
  - Appearance Model + Relationship Model
- **People tracking in video:** Frame-to-frame appearance consistency

### 24.6 Using Vision

- **Manipulation:** Pose estimation for object grasping
- **Navigation:** Lane detection, obstacle avoidance
- **Autonomous driving:** Lateral control (lane keeping), longitudinal control (safe distance), obstacle avoidance

---

## Ch 25: Robotics

### 25.1 Introduction

Robots are agents that manipulate the physical world through physical effectors.

- **Manipulator (Robot Arm):** Fixed in factory assembly lines, moves within workspace via joint chains
- **Mobile Robot:** Moves with wheels/legs — UGV, UAV, AUV, planetary exploration
- **Humanoid Robot:** Human form, integrated walking/manipulation

### 25.2 Robot Hardware

**Sensors:**
- **Remote sensors:** Cameras, laser scanners, GPS
- **Contact sensors:** Bumpers, force/torque sensors
- **Proprioceptive sensors:** Shaft encoders, gyroscopes, accelerometers

**Effectors:**
- **Degrees of Freedom (DOF):** Number of independent movement directions
  - 6 DOF: 3D position (x,y,z) + orientation (yaw, roll, pitch)
- **Joint types:** Revolute, Prismatic
- **Nonholonomic:** Actual DOF > Control DOF (e.g., car)
- **Movement:** Differential drive, Synchro drive, legs, wheels

### 25.3 Robotic Perception

**State estimation:** Converting sensor measurements into internal environment representation.

- **Recursive Filtering:**
$$P(X_{t+1}|z_{1:t+1}, a_{1:t}) = \alpha P(z_{t+1}|X_{t+1}) \int P(X_{t+1}|x_t, a_t) P(x_t|z_{1:t}, a_{1:t-1}) dx_t$$
- **Motion model $P(X_{t+1}|X_t, v_t, \omega_t)$:** Includes Gaussian noise
- **Sensor model $P(z_t|X_t)$:** Landmark or range scan

**Localization:**
- **Monte Carlo Localization (MCL):** Particle filter based
  - Represent belief with particle set
  - Sampling → weight update → resampling
- **Extended Kalman Filter (EKF):** Represent belief with Gaussian
  - Linearize nonlinear models via Taylor expansion
  - Requires landmark recognition

**SLAM (Simultaneous Localization and Mapping):**
- Perform localization and map building simultaneously
- EKF-based: Add landmark positions to state vector
- Uses graph relaxation, EM algorithm

**Machine Learning in Robotics:**
- Low-dimensional embedding
- Self-supervised learning: Robot collects its own training data

### 25.4 Planning to Move

**Configuration Space:**
- Represent robot state with joint angles
- Workspace ↔ Configuration Space: Kinematics / Inverse Kinematics
- Free space vs Occupied space

**Path Planning:**
- **Cell Decomposition:** Decompose free space into grid/cells → graph search
  - Recursive subdivision, Hybrid A*
- **Skeletonization:** Reduce free space to 1D representation
  - Voronoi Graph: Set of equidistant points from obstacles
  - Probabilistic Roadmap (PRM): Graph construction via random sampling
  - RRT (Rapidly-exploring Random Tree)

**Potential Field:** Add cost functions that repel from obstacles → safe paths

### 25.5 Planning Uncertain Movements

Planning under uncertainty.

- **Uncertain environments:** Sensor noise, motion errors → probabilistic models needed
- **Limitations of deterministic planning:** Non-determinism of the real world
- **POMDP approach:** Optimal policy in partially observable environments
- **Reinforcement learning:** Learn in simulation, then transfer to real robot

### 25.6 Moving

**Robotic Motion:**
- **Reactive Architecture:** Sensor → behavior direct mapping (no deliberation)
- **Subsumption Architecture:** Hierarchical behavior layers
- **Feedback Control:** Minimize difference between goal state and current state

**Legged Robot Walking:**
- Statically stable: Center of mass within polygon formed by legs
- Dynamically stable: Maintain stability through motion state

### 25.7 Robotics Applications

- **Industrial Manipulation:** Assembly, welding, painting
- **Autonomous Vehicles:** Self-driving cars
- **Space Robotics:** Planetary exploration
- **Medical Robotics:** Surgical assistance

---

## Summary

| Topic | Core Concepts |
|---|---|
| NLP Basics | N-gram, Smoothing, Perplexity, Bag of Words |
| Text Classification | Bayes classification, TF-IDF, feature vectors |
| Parsing | PCFG, CYK algorithm, parse trees |
| Machine Translation | Translation model P(F\|E), language model, IBM models |
| Speech Recognition | HMM-based acoustic model, vocabulary/language model |
| Image Formation | Perspective projection, lens, Lambert's Law |
| Object Recognition | HOG, SIFT, sliding window, Pictorial Structure |
| 3D Reconstruction | Stereo, optical flow, shading, contour |
| Robot Perception | MCL, EKF, SLAM |
| Path Planning | Configuration space, Cell decomposition, RRT |

## Related

- [[AI Overview]] — All AI topics
- [[05_Machine_Learning]] — ML techniques used in NLP/vision
- [[08_AI_Ethics_and_Future]] — Societal impact
