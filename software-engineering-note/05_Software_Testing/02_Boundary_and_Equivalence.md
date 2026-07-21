---
tags:
  - testing
  - boundary-value
  - equivalence-class
  - unit-testing
  - software-testing
---

# 02 Boundary Value & Equivalence Class Testing

> **Source:** Jorgensen, *Software Testing: A Craftsman's Approach*, 4th ed., Chapters 5–6
> **Purpose:** Specification-based (black-box) test case design using input/output domain analysis — the two most fundamental functional testing techniques.

Boundary value testing and equivalence class testing are the best-known specification-based testing techniques. Both treat the program as a function mapping its input domain to its output range, and both derive test cases from that domain knowledge without looking at the source code.

---

## 1. Boundary Value Testing (Ch. 5)

### Core Idea

Errors tend to cluster near the **extreme values** of input variables. Loop counters are off-by-one, `<` is used where `≤` was intended, and boundary conditions are where programs break. Boundary value analysis (BVA) tests at and just around those edges.

The standard five test values for each bounded variable:

```
min, min+, nom, max−, max
```

Robust forms add: `min−` and `max+`.

### Two Deciding Factors

| Factor | Option A | Option B |
|--------|----------|----------|
| **Invalid values?** | No — Normal | Yes — Robust |
| **Fault assumption?** | Single fault | Multiple fault (interactions) |

These produce four variants:

| | Single Fault | Multiple Faults |
|---|---|---|
| **Valid only** | Normal BVA (4n+1 cases) | Worst-Case BVA (5ⁿ cases) |
| **Valid + Invalid** | Robust BVA (6n+1 cases) | Robust Worst-Case (7ⁿ cases) |

### 1.1 Normal Boundary Value Testing

- **Single fault assumption:** only one variable at an extreme; all others held at nominal.
- Holds all but one variable at `nom` and lets the remaining variable run through `{min, min+, nom, max−, max}`.
- **Formula:** `4n + 1` unique test cases for `n` variables.

**Example (2 variables, x₁ ∈ [a, b], x₂ ∈ [c, d]):**

```
{〈x₁_nom, x₂_min〉, 〈x₁_nom, x₂_min+〉, 〈x₁_nom, x₂_nom〉, 〈x₁_nom, x₂_max−〉, 〈x₁_nom, x₂_max〉,
 〈x₁_min, x₂_nom〉, 〈x₁_min+, x₂_nom〉, 〈x₁_max−, x₂_nom〉, 〈x₁_max, x₂_nom〉}
```

### 1.2 Robust Boundary Value Testing

- Adds `min−` and `max+` — values just outside the valid range.
- Same single-fault assumption; still holds all but one at nominal.
- **Formula:** `6n + 1` test cases.
- **Key value:** forces attention on **exception handling**. What happens when a physical quantity exceeds its maximum?

### 1.3 Worst-Case Boundary Value Testing

- **Rejects** the single-fault assumption. Interested in what happens when **multiple variables** simultaneously take extreme values.
- Takes the Cartesian product of the five-value set `{min, min+, nom, max−, max}` for each variable.
- **Formula:** `5ⁿ` test cases.
- Subsumes normal BVA (normal BVA cases are a proper subset).
- Best applied where physical variables interact and failure is costly.

### 1.4 Robust Worst-Case Boundary Value Testing

- Cartesian product of the seven-value set `{min−, min, min+, nom, max−, max, max+}`.
- **Formula:** `7ⁿ` test cases — for truly paranoid testing.

### 1.5 Comparison Table

| Variant | Invalid Values | Fault Model | Test Cases (n vars) | n=3 Example |
|---------|:---:|:---:|:---:|---:|
| Normal BVA | No | Single | 4n + 1 | 13 |
| Robust BVA | Yes | Single | 6n + 1 | 19 |
| Worst-Case | No | Multiple | 5ⁿ | 125 |
| Robust Worst-Case | Yes | Multiple | 7ⁿ | 343 |

### 1.6 Limitations

1. **Variables must be independent.** BVA falls apart when there are semantic dependencies among variables (e.g., month, day, year in NextDate — February 31 is invalid regardless of boundary position).
2. **Variables must support an ordering relation.** Works for numeric ranges, temperatures, dates. Does NOT work for car colors, football teams, PINs, or Boolean values.
3. **Variables should refer to physical quantities.** Physical boundaries (temperature limits, load capacity) are meaningful; logical boundaries (PIN 0000 vs 9999) rarely reveal faults.

### 1.7 Special Value Testing

- Uses domain knowledge, experience, and intuition — also called *ad hoc* testing.
- No formal guidelines; depends entirely on tester skill.
- Often **more effective** than mechanical BVA for problems with rich domain logic (e.g., leap years, end-of-February cases in NextDate).
- "Testimony to the craft of software testing."

### 1.8 Random Testing

- Uses a random number generator instead of fixed boundary values.
- Avoids tester bias. Raises the question: **how many test cases are enough?**
- Statistical answer emerges from coverage metrics (Chapters 8–9).

### 1.9 Output-Based Boundary Analysis

BVA can also be applied to the **output range**, not just the input domain:

- **Commission problem:** boundary thresholds at $1000 and $1800 where commission rate changes (10% → 15% → 20%).
- Choose input combinations that produce outputs at and near these thresholds.
- Output-based BVA often reveals faults that input-based BVA misses because input-based cases may all fall within the same output partition.

---

## 2. Equivalence Class Testing (Ch. 6)

### Core Idea

An **equivalence class** is a set of inputs that the program "treats the same way." Testing one representative from each class achieves a form of completeness (all classes covered) while avoiding redundancy (no two tests from the same class).

Mathematically, equivalence classes form a **partition** of the input domain:
- Mutually disjoint subsets
- Union covers the entire domain

### Two Deciding Factors (Same as BVA)

| Factor | Weak (Single Fault) | Strong (Multiple Fault) |
|--------|:---:|:---:|
| **Normal** (valid only) | Weak Normal | Strong Normal |
| **Robust** (valid + invalid) | Weak Robust | Strong Robust |

### 2.1 Weak Normal Equivalence Class Testing

- Pick **one value** from each equivalence class.
- Number of test cases = number of classes in the partition with the **most** subsets.
- **Single fault assumption:** failure of a test case could be due to any variable — ambiguity remains.
- Good for **regression testing** where failure probability is low.

### 2.2 Strong Normal Equivalence Class Testing

- **Multiple fault assumption:** test all combinations.
- Take the **Cartesian product** of equivalence classes across variables.
- Number of test cases = product of class counts per variable.
- Provides completeness and better fault isolation.

### 2.3 Weak Robust Equivalence Class Testing

- Same as weak normal, plus **invalid value classes**.
- For each invalid class, one test case: one variable takes the invalid value, all others stay valid.
- Single fault assumption on invalid values.

### 2.4 Strong Robust Equivalence Class Testing

- Cartesian product of **all** classes — valid and invalid.
- Most thorough, but test case count explodes.

### 2.5 Traditional Equivalence Class Testing

The historical precursor to weak robust testing, from the FORTRAN/COBOL era:

1. Test with all valid values.
2. Test with one invalid value of x₁, others valid.
3. Repeat for each remaining variable.

Rooted in the GIGO (Garbage In, Garbage Out) era when ~80% of source code was input validation logic.

### 2.6 The Craft: Choosing Equivalence Relations

The effectiveness of equivalence class testing hinges entirely on **how you define the classes**. This is where tester craftsmanship matters.

**Triangle problem — output-based classes:**

```
R1 = {equilateral}    R2 = {isosceles}
R3 = {scalene}        R4 = {not a triangle}
```

Four weak normal cases: `(5,5,5)`, `(2,2,3)`, `(3,4,5)`, `(4,1,2)`.

Can also partition by **side equality**:
```
D1 = {a = b = c}        D2 = {a = b, a ≠ c}
D3 = {a = c, a ≠ b}     D4 = {b = c, a ≠ b}
D5 = {all different}    D6–D8 = {triangle inequality violations}
```

**NextDate function — refined classes:**

Instead of just `{valid month, invalid month}`, use:

| Variable | Equivalence Classes |
|----------|-------------------|
| Month | M1 = {30-day months}, M2 = {31-day months}, M3 = {February} |
| Day | D1 = {1–28}, D2 = {29}, D3 = {30}, D4 = {31} |
| Year | Y1 = {2000}, Y2 = {non-century leap year}, Y3 = {common year} |

This yields `3 × 4 × 3 = 36` strong normal test cases — and catches leap-year logic, end-of-February edge cases, and invalid dates (Feb 30, June 31) that mechanical BVA misses.

**Commission problem — output-based classes:**

```
S1 = {sales ≤ 1000}     → 10% commission
S2 = {1000 < sales ≤ 1800} → 15% commission
S3 = {sales > 1800}     → 20% commission
```

Output-based partitioning is far more useful than input-based for the commission problem because it stresses the rate-change thresholds.

---

## 3. BVA vs Equivalence Class Testing

| Aspect | Boundary Value | Equivalence Class |
|--------|:---:|:---:|
| Focus | Edges of ranges | Partitions of the domain |
| Redundancy | High (same nominal repeated) | Low (one per class) |
| Gaps | Misses interior logic | Misses boundary interactions |
| Best for | Physical quantities, numeric ranges | Logic-driven partitions, output classes |
| Dependency handling | Poor (assumes independence) | Better (can encode dependencies in class definitions) |

### Hybrid: "Edge Testing" (ISTQB)

When equivalence classes are defined by bounded intervals, combine both techniques:
- Use BVA at the **edges** of each equivalence class partition.
- Tests values at the boundaries between classes, not just at the overall min/max.

---

## 4. Key Examples Summary

### 4.1 Triangle Problem

| Method | Notable Results |
|--------|----------------|
| Normal BVA (13 cases) | No scalene triangle case; three identical equilateral cases |
| Worst-Case (125 cases) | Thorough but many redundant "not a triangle" cases |
| Weak Normal EC | 4 cases — one per output class, clean and efficient |
| Weak Robust EC | Adds 6 invalid-input cases (negative, >200 per side) |

### 4.2 NextDate Function

| Method | Notable Results |
|--------|----------------|
| Normal BVA | Tests Jan 1 in 5 different years — wasteful. Ignores February entirely. |
| Worst-Case (125 cases) | June 31, 1912 — mechanically generated garbage |
| Refined EC (36 cases) | February 29 across leap/common years, end-of-month transitions — much better |
| Random Testing | ~56% of random dates land in 31-day months (matches probability) |

### 4.3 Commission Problem

| Method | Notable Results |
|--------|----------------|
| Input BVA | Almost all cases land in the 20% commission zone — uninteresting |
| Output BVA (25 cases) | Exercises $1000 and $1800 thresholds — much higher yield |
| Weak Robust EC | 8 cases — only 1 legitimate input; rest test error handling |

---

## 5. Guidelines

1. **Start with equivalence classes** — they force you to think about what the program actually distinguishes.
2. **Apply BVA at class boundaries** — the "edge testing" hybrid is usually best.
3. **Prefer output-based classes** when the output space has clear partitions (commission rates, triangle types).
4. **Use robust forms sparingly** — in strongly typed languages, invalid-value testing is often a waste. Focus on semantic invalid cases (Feb 30) rather than syntactic ones (month = −1).
5. **Special value testing fills the gaps** — after mechanical methods, apply domain knowledge. For NextDate: test Feb 28 in leap and non-leap years, Dec 31, etc.
6. **Watch for variable dependencies** — both BVA and EC assume independence. When variables interact (date logic, tax brackets), encode the interaction in your equivalence class definitions.

---

## Sources

- Jorgensen, Paul C. *Software Testing: A Craftsman's Approach*, 4th ed., CRC Press, 2014. Chapters 5–6.
- Myers, Glenford J. *The Art of Software Testing*, Wiley, 1979.
- ISTQB Foundation Level Syllabus — "edge testing" (BVA + equivalence partitioning hybrid).


## Related

- [[Software Testing Overview]] — All testing topics
- [[01_Testing_Fundamentals]] — Testing fundamentals and math
- [[03_Decision_Table_and_Path]] — Decision table and path testing
