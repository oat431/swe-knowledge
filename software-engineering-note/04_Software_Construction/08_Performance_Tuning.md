---
tags:
  - construction
  - performance
  - optimization
  - tuning
  - code-complete
---

# 08 Performance Tuning

> **Source:** McConnell *Code Complete* 2nd ed., Chapters 25–26
> **Purpose:** Strategic and tactical approaches to software performance: when to optimize, how to measure, and specific code-level techniques that work.

Performance tuning addresses two levels: **strategic** (what to tune and when) and **tactical** (how to tune specific code). Most programs spend 80%+ of their time in <5% of the code. The art is finding that 5% and optimizing only what matters.

---

## 25.1 Performance Overview

Performance is only loosely related to code speed. Users care about **throughput**, not raw execution time. A faster algorithm wrapped in a terrible workflow is still slow for the user.

> *"More computing sins are committed in the name of efficiency (without necessarily achieving it) than for any other single reason — including blind stupidity."* — W.A. Wulf

### The Efficiency Hierarchy (most to least impactful)

| Level | Impact | Example |
|-------|--------|---------|
| **Program requirements** | 10–100× | Relaxing sub-second response to 4-second saved $70M |
| **Program design / architecture** | 10–100× | Switching 13th-order polynomials to dozens of 3rd-order |
| **Class and routine design** | 2–10× | Choosing quicksort over bubblesort, binary search over linear |
| **OS interactions** | 2–10× | Avoiding unnecessary system calls, reducing I/O |
| **Code compilation** | 2–5× | Enabling compiler optimizations (40%+ gains for free) |
| **Hardware** | 2–10× | Faster CPU, more RAM, SSD instead of HDD |
| **Code tuning** | ~2× | Hand-tweaking loops, expressions, logic |

**Key insight:** Bentley (1982) argues improvements at each level can multiply — a 10× improvement across 6 levels implies a million-fold potential. Rare in practice, but the principle holds: always start at the highest level.

---

## 25.2 Introduction to Code Tuning

### The Pareto Principle (80/20 Rule)

- Knuth found <4% of a program accounts for >50% of run time
- Boehm: 20% of routines consume 80% of execution time
- Bentley: tripling the speed of a 5-line square-root routine doubled the speed of a 1000-line program
- **Implication:** Measure first, then optimize only the hot spots

### Old Wives' Tales (Debunked)

| Myth | Reality |
|------|---------|
| Fewer lines of code = faster | A 10-line unrolled loop was 60%+ faster than a 3-line `for` loop |
| Certain operations are "probably" faster | You must measure every time. Results change with compiler, language, CPU, OS |
| Optimize as you go | *"Premature optimization is the root of all evil"* — Knuth. Programmers are terrible at guessing bottlenecks |
| A fast program is as important as a correct one | *"If mine doesn't have to work, I can make it run instantly"* — a correct slow program beats a fast broken one |

### When to Tune

> Jackson's Rules of Optimization:
> 1. Don't do it.
> 2. **(For experts only)** Don't do it yet — not until you have a perfectly clear, unoptimized solution.

1. Make the program **correct** and **modular** first
2. When complete, measure performance
3. If it lumbers, find and tune the hot spots
4. A day's work on hot spots often beats months of speculative optimization

### Compiler Optimizations

Modern compilers can improve speed 40%+ across the board. Write clear, straightforward code — optimizers do better with simple code than with "clever" tricks. Example: straightforward code was 11% faster than tricky code after compiler optimization.

---

## 25.3 Kinds of Fat and Molasses

### Common Sources of Inefficiency

#### 1. Input/Output Operations

In-memory access is **~1000× faster** than disk access:

| Operation | External File | In-Memory | Ratio |
|-----------|:---:|:---:|:---:|
| Random access (C++) | 6.04s | ~0s | ∞ |
| Sequential access (C++) | 3.29s | 0.021s | 150:1 |
| Network file vs local | +10% penalty | — | — |

> Avoid I/O in speed-critical code. Cache in memory whenever possible.

#### 2. Paging (Memory Access Patterns)

Row-major vs column-major traversal matters enormously:

```java
// ❌ Page fault on EVERY access (column-major on row-major array)
for (column = 0; column < MAX_COLUMNS; column++)
    for (row = 0; row < MAX_ROWS; row++)
        table[row][column] = BlankTableElement();

// ✅ Page fault only on row switches
for (row = 0; row < MAX_ROWS; row++)
    for (column = 0; column < MAX_COLUMNS; column++)
        table[row][column] = BlankTableElement();
```

Measured difference: up to **1000×** on memory-constrained systems.

#### 3. System Calls

System calls involve context switches (save user state → enter kernel → restore). Common culprits:
- I/O to disk, keyboard, screen, printer
- Memory management routines
- Utility routines (time, date, etc.)

**Real example:** A `BaseTime` class was initializing its constructor with the system time. Tens of thousands of objects were created. Overriding the constructor to initialize to `0` instead gave as much improvement as all other optimizations combined.

#### 4. Interpreted Languages

| Language | Type | Speed Relative to C++ |
|----------|------|:---:|
| C++ / C# / VB | Compiled | 1:1 |
| Java | Byte code | ~1.5:1 |
| PHP / Python | Interpreted | **>100:1** |

#### 5. Errors Masquerading as Performance Problems

- Debug logging left enabled
- Unindexed database tables → **30× improvement** from adding an index
- Memory leaks forcing GC/paging
- Polling nonexistent devices until timeout

### Relative Costs of Common Operations

| Operation | C++ Cost | Java Cost |
|-----------|:---:|:---:|
| Integer assignment | 1 | 1 |
| Routine call (no params) | 1 | — |
| Routine call (1 param) | 1.5 | 0.5 |
| Polymorphic routine call | 2.5 | 2 |
| Integer division | 5 | 1.5 |
| Floating-point sqrt | 15 | 4 |
| Floating-point sin / log | 25 | 20 |
| Floating-point e^y | 50 | 20 |
| Array access (any dimension) | 1 | 1 |

> **Key takeaway:** Most operations are similarly cheap. The expensive ones are transcendental math functions and polymorphic calls. All speed improvements come from replacing an expensive operation with a cheaper one.

---

## 25.4 Measurement

### Why You Must Measure

> *"No programmer has ever been able to predict or analyze where performance bottlenecks are without data. No matter where you think it's going, you will be surprised."* — Joseph M. Newcomer

**Real example:** A programmer converted a double-nested array summation loop to pointer arithmetic, saving "10,000 multiplications." Measured result: **zero improvement.** The compiler's optimizer had already done it.

### Measurement Rules

- Use **CPU clock ticks**, not wall-clock time (avoid penalizing for other processes)
- Factor out measurement overhead and startup costs
- Use profiling tools or instrument your own timers
- **Measure before and after every single tuning** — more than half of attempted tunings degrade performance

---

## 25.5 Iteration

Multiple rounds of optimization compound. Example: DES encryption implementation:

| Optimization | Time | Improvement |
|-------------|:----:|:---:|
| Initial implementation | 21:40 | — |
| Bit fields → arrays | 7:30 | 65% |
| Unroll innermost loop | 6:00 | 20% |
| Remove final permutation | 5:24 | 10% |
| Combine two variables | 5:06 | 5% |
| Logical identity (combine 2 steps) | 4:30 | 12% |
| Share memory (inner loop) | 3:36 | 20% |
| Share memory (outer loop) | 3:09 | 13% |
| Unfold all loops | 1:36 | 49% |
| Inline all routines | 0:45 | 53% |
| Rewrite in assembler | 0:22 | 51% |
| **Final** | **0:22** | **98% total** |

> At least **two-thirds** of attempted optimizations in this case _doubled_ the run time. Iteration and measurement are essential.

---

## 25.6 Summary: The Code-Tuning Approach

```
1. Develop clean, well-designed, modifiable code
2. If performance is poor:
   a. Save a working version
   b. Measure to find hot spots
   c. Determine if code tuning is appropriate (vs. design/algorithm changes)
   d. Tune the bottleneck
   e. Measure each improvement one at a time
   f. Revert if no improvement (most tunings won't help)
3. Repeat from step 2
```

---

## 26. Code-Tuning Techniques

> These are **anti-refactorings** — they degrade internal structure for performance. Use them only after measurement confirms the need. Most are not generally applicable; adapt to your situation.

---

## 26.1 Logic

### Stop Testing When You Know the Answer

Use **short-circuit evaluation** (native in C++, Java, C#). If your language lacks it, restructure:

```java
// Instead of: if (a && b), use nested ifs:
if (a)
    if (b) ...
```

**Search loops:** Use `break`, `goto`, sentinel values, or restructure to `while` — stop as soon as you find the target.

| Language | Straight | Tuned | Savings |
|----------|:---:|:---:|:---:|
| C++ | 4.27 | 3.68 | 14% |
| Java | 4.85 | 3.46 | 29% |

### Order Tests by Frequency

Put the most common / fastest case first. In `if-then-else` chains, the common case should be checked first.

| Language | Straight | Tuned | Savings |
|----------|:---:|:---:|:---:|
| C# | 0.63 | 0.33 | 48% |
| Java | 0.92 | 0.46 | 50% |
| Visual Basic | 1.36 | 1.00 | 26% |

### Compare Similar Logic Structures

`case` vs `if-then-else` — results are completely unpredictable across languages:

| Language | case | if-then-else | Winner |
|----------|:---:|:---:|:---:|
| Java | 2.56 | 0.46 | if-then-else (6:1) |
| Visual Basic | 0.26 | 1.00 | case (4:1) |
| C# | 0.26 | 0.33 | case (close) |

> **There is no substitute for measuring.** Results defy logical explanation.

### Substitute Table Lookups for Complicated Expressions

Replace complex boolean chains with lookup tables:

```cpp
// ❌ Complicated logic chain
if ((a && !c) || (a && b && c)) category = 1;
else if ((b && !a) || (a && c && !b)) category = 2;
else if (c && !a && !b) category = 3;
else category = 0;

// ✅ Table lookup — faster AND more maintainable
static int categoryTable[2][2][2] = {
    // !b!c  !bc  b!c  bc
    {   0,    3,   2,   2 },  // !a
    {   1,    2,   1,   1 }   //  a
};
category = categoryTable[a][b][c];
```

| Language | Straight | Tuned | Savings |
|----------|:---:|:---:|:---:|
| C++ | 5.04 | 3.39 | 33% |
| Visual Basic | 5.21 | 2.60 | 50% |

### Use Lazy Evaluation

Don't compute until needed. Compute-on-demand with caching beats precomputing everything:

- If a 5000-entry table is only 5% used, compute entries as needed + cache
- Just-in-time strategies: do work closest to when it's actually needed

---

## 26.2 Loops

### Unswitching

Move invariant conditionals outside the loop:

```cpp
// ❌ Decision inside loop — checked every iteration
for (i = 0; i < count; i++) {
    if (sumType == SUMTYPE_NET)
        netSum += amount[i];
    else
        grossSum += amount[i];
}

// ✅ Decision outside loop — checked once
if (sumType == SUMTYPE_NET) {
    for (i = 0; i < count; i++)
        netSum += amount[i];
} else {
    for (i = 0; i < count; i++)
        grossSum += amount[i];
}
```

| Language | Straight | Tuned | Savings |
|----------|:---:|:---:|:---:|
| C++ | 2.81 | 2.27 | 19% |
| Java | 3.97 | 3.12 | 21% |
| Python | 8.14 | 5.87 | 28% |

⚠️ **Hazard:** Two loops must be maintained in parallel.

### Jamming (Fusion)

Combine two loops over the same range into one:

| Language | Straight | Tuned | Savings |
|----------|:---:|:---:|:---:|
| C++ | 3.68 | 2.65 | 28% |
| PHP | 3.97 | 2.42 | 32% |

### Unrolling

Reduce loop overhead by handling multiple elements per iteration:

```java
// Standard loop
for (i = 0; i < count; i++)
    a[i] = i;

// Unrolled once
i = 0;
while (i < count - 1) {
    a[i] = i;
    a[i + 1] = i + 1;
    i += 2;
}
if (i == count - 1)
    a[count - 1] = count - 1;
```

| Language | Straight | Unrolled 1× | Unrolled 2× |
|----------|:---:|:---:|:---:|
| C++ | 1.75 | 1.15 (34%) | 1.01 (42%) |
| Java | 1.01 | 0.58 (43%) | 0.58 (43%) |
| Python | 2.51 | 3.21 (-27%) | 2.79 (-12%) |

⚠️ **Hazards:** Off-by-one errors in cleanup code; seriously degrades readability.

### Minimizing Work Inside Loops

Hoist invariant expressions out:

```cpp
// ❌ Dereference chain inside loop
for (i = 0; i < rateCount; i++)
    netRate[i] = baseRate[i] * rates->discounts->factors->net;

// ✅ Hoist + name the expression
quantityDiscount = rates->discounts->factors->net;
for (i = 0; i < rateCount; i++)
    netRate[i] = baseRate[i] * quantityDiscount;
```

| Language | Straight | Tuned | Savings |
|----------|:---:|:---:|:---:|
| C++ | 3.69 | 2.97 | 19% |
| Java | 4.13 | 2.35 | 43% |

> This also improves readability. Win-win.

### Sentinel Values

Replace compound loop tests with a sentinel value past the end of the search range:

```csharp
// ❌ Two tests per iteration
found = false; i = 0;
while (!found && i < count) {
    if (item[i] == testValue) found = true;
    else i++;
}

// ✅ One test per iteration
item[count] = testValue;  // sentinel
i = 0;
while (item[i] != testValue)
    i++;
if (i < count) { /* found */ }
```

| Language | Straight | Tuned | Savings |
|----------|:---:|:---:|:---:|
| C# | 0.771 | 0.590 | 23% |
| Java | 1.63 | 0.912 | 44% |
| Visual Basic | 1.34 | 0.470 | 65% |

⚠️ Requires space for sentinel at end of array; choose sentinel value carefully.

### Putting the Busiest Loop on the Inside

Minimize the number of loop initializations/terminations by nesting shallow:

```java
// ❌ Outer loop runs 100×, inner 5× = 600 total iterations
for (column = 0; column < 100; column++)
    for (row = 0; row < 5; row++)
        sum += table[row][column];

// ✅ Outer loop runs 5×, inner 100× = 505 total iterations (16% fewer)
for (row = 0; row < 5; row++)
    for (column = 0; column < 100; column++)
        sum += table[row][column];
```

### Strength Reduction

Replace expensive operations with cheaper ones:

```vb
' ❌ Multiplication on every iteration
For i = 0 to saleCount - 1
    commission(i) = (i + 1) * revenue * baseCommission * discount
Next

' ✅ Accumulate by addition instead
incrementalCommission = revenue * baseCommission * discount
cumulativeCommission = incrementalCommission
For i = 0 to saleCount - 1
    commission(i) = cumulativeCommission
    cumulativeCommission = cumulativeCommission + incrementalCommission
Next
```

| Language | Straight | Tuned | Savings |
|----------|:---:|:---:|:---:|
| C++ | 4.33 | 3.80 | 12% |
| Visual Basic | 3.54 | 1.80 | 49% |

---

## 26.3 Data Transformations

### Use Integers Rather Than Floating-Point Numbers

Integer arithmetic is significantly faster than floating point:

| Language | Straight (float) | Tuned (int) | Savings |
|----------|:---:|:---:|:---:|
| C++ | 2.80 | 0.801 | 71% |
| Visual Basic | 6.84 | 0.280 | 96% |

### Use the Fewest Array Dimensions Possible

Flatten multi-dimensional arrays to one-dimensional:

| Language | 2D Array | 1D Array | Savings |
|----------|:---:|:---:|:---:|
| Java | 7.78 | 4.14 | 47% |
| Visual Basic | 9.43 | 3.22 | 66% |
| PHP | 6.24 | 4.10 | 34% |

### Minimize Array References

Hoist invariant array accesses out of inner loops:

```cpp
// ✅ Move discount[discountType] outside inner loop
for (discountType = 0; discountType < typeCount; discountType++) {
    thisDiscount = discount[discountType];
    for (discountLevel = 0; discountLevel < levelCount; discountLevel++)
        rate[discountLevel] = rate[discountLevel] * thisDiscount;
}
```

### Use Supplementary Indexes

- **String-length index:** Store length separately (Visual Basic approach) vs. null-terminated scanning (C approach)
- **Parallel index structures:** Sort/search lightweight keys in memory, access full data on disk only when needed
- **Key insight:** If data items are large or on disk, manipulate index references instead

### Use Caching

Save frequently accessed / computed values:

```java
// ✅ Cache hypotenuse calculation
public double Hypotenuse(double sideA, double sideB) {
    if ((sideA == cachedSideA) && (sideB == cachedSideB))
        return cachedHypotenuse;
    
    cachedHypotenuse = Math.sqrt((sideA * sideA) + (sideB * sideB));
    cachedSideA = sideA;
    cachedSideB = sideB;
    return cachedHypotenuse;
}
```

| Language | Straight | Cached | Savings |
|----------|:---:|:---:|:---:|
| C++ | 4.06 | 1.05 | 74% |
| Java | 2.54 | 1.40 | 45% |

> Caching effectiveness depends on hit rate, cost of generation vs. retrieval, and cache size. More than one cached element adds significant overhead.

---

## 26.4 Expressions

### Exploit Algebraic Identities

Replace costly operations with equivalent cheaper ones:

| Expression | Replacement | Savings |
|-----------|-------------|:---:|
| `not a and not b` | `not (a or b)` | 1 `not` operation |
| `sqrt(x) < sqrt(y)` | `x < y` | **750:1** (C++) |

```cpp
// ❌ Two sqrt() calls
if (sqrt(x) < sqrt(y)) ...

// ✅ Direct comparison
if (x < y) ...
```

| Language | Straight | Tuned | Ratio |
|----------|:---:|:---:|:---:|
| C++ | 7.43 | 0.010 | 750:1 |
| Visual Basic | 4.59 | 0.220 | 20:1 |

### Use Strength Reduction (Expressions)

| Replace | With |
|---------|------|
| Multiplication | Addition |
| Exponentiation | Multiplication |
| Trigonometric functions | Trigonometric identities |
| `long long` / `long` | `int` |
| Floating-point | Fixed-point or integer |
| Double-precision | Single-precision |
| `×2`, `÷2` | `<<1`, `>>1` |

**Polynomial evaluation example:**

```vb
' ❌ Exponentiation (expensive)
For power = 1 To order
    value = value + coefficient(power) * x^power
Next

' ✅ Accumulate powers by multiplication
powerOfX = x
For power = 1 To order
    value = value + coefficient(power) * powerOfX
    powerOfX = powerOfX * x
Next
```

Savings: up to **97%** (Visual Basic) for 2nd-order+ polynomials.

### Initialize at Compile Time

Replace runtime computations with constants:

```cpp
// ❌ log(2) computed every call
unsigned int Log2(unsigned int x) {
    return (unsigned int)(log(x) / log(2));
}

// ✅ log(2) precomputed as constant
const double LOG2 = 0.69314718;
unsigned int Log2(unsigned int x) {
    return (unsigned int)(log(x) / LOG2);
}
```

| Language | Straight | Tuned | Savings |
|----------|:---:|:---:|:---:|
| C++ | 9.66 | 5.97 | 38% |
| Java | 17.0 | 12.3 | 28% |

### Be Wary of System Routines

System math routines are designed for lunar-landing precision. If you need less, write your own:

```cpp
// ❌ Floating-point log() for an integer result
return (unsigned int)(log(x) / LOG2);

// ✅ Pure integer implementation
unsigned int Log2(unsigned int x) {
    if (x < 2) return 0;
    if (x < 4) return 1;
    if (x < 8) return 2;
    // ... up to 31
}
```

| Language | Straight (float log) | Tuned (int) | Ratio |
|----------|:---:|:---:|:---:|
| C++ | 9.66 | 0.662 | 15:1 |
| Java | 17.0 | 0.882 | 20:1 |

### Use the Correct Type of Constants

Match constant types to variable types to avoid runtime type conversions:

| Language | Straight | Tuned | Savings |
|----------|:---:|:---:|:---:|
| C++ | 1.11 | ~0 | 100% |
| Java | 1.66 | 1.11 | 33% |

### Precompute Results

Compute once, use many times:
- Hoist partial expressions outside loops
- Build lookup tables at startup
- Store computed results in data files
- Embed precomputed values in the program

---

## Checklist: Code-Tuning Strategies

### Overall Program Performance
- [ ] Consider changing program requirements before tuning code
- [ ] Consider modifying program/class design
- [ ] Consider avoiding OS interactions and I/O
- [ ] Consider using a compiled language instead of interpreted
- [ ] Consider compiler optimizations
- [ ] Consider hardware upgrades
- [ ] Treat code tuning as a **last resort**

### Code-Tuning Approach
- [ ] Program is fully correct before tuning begins
- [ ] Performance bottlenecks have been measured (not guessed)
- [ ] Effect of each tuning change is measured
- [ ] Failed tunings are backed out
- [ ] Multiple changes are tried for each bottleneck (iteration)

---

## Key Points

1. **Performance ≠ code speed.** Users care about throughput, responsiveness, and correctness — not micro-optimized loops.
2. **Architecture beats tuning.** Program design, algorithm selection, and data structure choice dominate code-level tuning by orders of magnitude.
3. **Measure everything.** You cannot predict bottlenecks. Measure before and after every change. Most tunings fail.
4. **The 80/20 rule is real.** <5% of code typically accounts for >50% of run time. Find that 5% first.
5. **Iterate.** Single optimizations rarely suffice. Combine techniques, measure each, keep what works.
6. **Write clean code first.** Modifiable code is easier to optimize later. Premature optimization produces rigid, buggy systems.
7. **Code tunings are anti-refactorings.** They trade clarity for speed. Use only when measurement justifies the cost.

---

## Sources

- McConnell, Steve. *Code Complete*, 2nd ed. Microsoft Press, 2004. Chapters 25–26.
- Knuth, Donald. "An Empirical Study of Fortran Programs," 1971.
- Bentley, Jon. *Writing Efficient Programs*, 1982.
- Boehm, Barry. *Software Cost Estimation with COCOMO II*, 2000.
- Fowler, Martin. *Refactoring: Improving the Design of Existing Code*, 1999.
