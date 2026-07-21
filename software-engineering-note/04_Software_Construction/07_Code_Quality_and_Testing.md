---
tags:
  - construction
  - quality
  - testing
  - debugging
  - refactoring
  - code-complete
source: "McConnell, Code Complete 2nd Edition, Chapters 20–24"
---

# 07 — Code Quality & Testing

> **Source:** Steve McConnell, *Code Complete* 2nd Ed., Ch 20–24
> **Scope:** Software-quality landscape, collaborative construction (pair programming, formal inspections), developer testing, debugging, refactoring (specific refactorings, safe refactoring strategies).

---

## Ch 20 — The Software-Quality Landscape

### 20.1 External vs. Internal Quality Characteristics

**External** (user-visible):
- **Correctness** — free from faults in spec, design, implementation
- **Usability** — ease of learning and use
- **Efficiency** — minimal resource usage (memory, CPU)
- **Reliability** — mean time between failures; performs under stated conditions
- **Integrity** — prevents unauthorised access; data consistency
- **Adaptability** — usable in environments beyond original design
- **Accuracy** — free from error in quantitative outputs (≠ correctness)
- **Robustness** — continues functioning under invalid inputs or stress

**Internal** (developer-facing):
- **Maintainability** — ease of modification for fixes, features, performance
- **Flexibility** — modifiable for uses beyond original design
- **Portability** — ease of moving to a different environment
- **Reusability** — ease of using parts in other systems
- **Readability** — ease of understanding at the statement level
- **Testability** — degree to which unit/system testing can verify requirements
- **Understandability** — coherence at system-organisational and detailed levels

> Trade-offs exist: focusing on correctness can hurt robustness; focusing on adaptability helps robustness. Know which characteristics matter for your project.

### 20.2 Techniques for Improving Software Quality

- **Explicit quality objectives** — Weinberg & Schulman (1974) study: teams told to optimise a specific objective (min memory, readable code, etc.) ranked 1st or 2nd in that objective. Programmers respond to stated goals.
- **Explicit QA activity** — make quality a visible priority so programmers respond.
- **Testing strategy** — plan testing alongside requirements, architecture, and design.
- **Software-engineering guidelines** — standards for all activities.
- **Informal technical reviews** — desk-checking, walking through code with peers.
- **Formal technical reviews / quality gates** — inspections, peer reviews, audits at transitions between phases.
- **Change-control procedures** — uncontrolled change destabilises quality.
- **Measurement** — measure results to know whether your QA plan works.
- **Prototyping** — realistic models of key functions; leads to better designs and improved maintainability (Gordon & Bieman 1991).

### 20.3 Relative Effectiveness of Quality Techniques

**Defect-detection rates (modal):**

| Technique | Modal Rate |
|---|---|
| Informal design reviews | 35% |
| Formal design inspections | 55% |
| Informal code reviews | 25% |
| Formal code inspections | 60% |
| Modeling/prototyping | 65% |
| Personal desk-checking | 40% |
| Unit test | 30% |
| Integration test | 35% |
| System test | 40% |
| High-volume beta test (>1000 sites) | 75% |

> **Key insight:** No single technique exceeds ~75% modal rate. Testing alone (unit + integration + system) often achieves <60% cumulative. **Use a combination of techniques.**

- Myers (1978): any combination of two methods nearly doubled defects found. Different people find different defects — only ~20% of errors found by inspections were found by more than one inspector.
- Human processes (inspections, walk-throughs) find different kinds of errors than computer-based testing.
- **Cost:** Inspections are cheaper than testing. IBM: 3.5 staff-hours per error via inspection vs. 15–25 hours via testing (Kaplan 1995).
- **Cost of fixing:** Earlier detection = cheaper fix. One-step techniques (inspections) detect symptom + cause together; two-step techniques (testing) require additional diagnosis.

**Recommended combination for above-average quality:**
1. Formal inspections of all requirements, all architecture, critical designs
2. Modeling or prototyping
3. Code reading or inspections
4. Execution testing

### 20.4 When to Do Quality Assurance

- Errors inserted earlier (requirements, architecture) are more entangled and expensive to remove later.
- Single architectural error can affect dozens of routines; a single construction error rarely affects more than one.
- QA should be planned from project start, part of the technical fabric throughout, and punctuate the end.

### 20.5 The General Principle of Software Quality

> **Improving quality reduces development costs.**

- Industry average: 10–50 lines of delivered code per person per day.
- The single biggest activity on most projects is **debugging and rework** — ~50% of time on traditional cycles.
- Reducing debugging by preventing errors improves productivity.
- NASA study (Card 1987): increased QA → decreased error rate, no increase in overall development cost.
- IBM (Jones 2000): projects with lowest defects had shortest schedules and highest productivity.
- DeMarco & Lister (1985): median-time programmers produced the *most* defects; both faster and slower groups had fewer defects.
- **Resources are redistributed** from downstream debugging/refactoring to upstream QA — upstream activities have more leverage.

---

## Ch 21 — Collaborative Construction

### 21.1 Overview

"Collaborative construction" encompasses pair programming, formal inspections, informal technical reviews, code reading, and other techniques where developers share responsibility for code and work products.

> Studies: developers insert 1–3 defects/hour into designs and 5–8 defects/hour into code (Humphrey 1997).

**Impressive results from inspections:**
- IBM: each hour of inspection prevented ~100 hours of related work (testing + defect correction)
- Raytheon: reduced rework cost from ~40% to ~20% of total project cost
- HP: inspection program saved ~$21.5M/year
- ICI: maintenance cost for inspected programs was ~10% of uninspected equivalents
- Large programs: each inspection hour avoided 33 hours of maintenance; up to 20× more efficient than testing
- 55% one-line changes in error → 2% after reviews introduced
- 11 programs: without reviews → 4.5 errors/100 LOC; with reviews → 0.82 errors/100 LOC (over 80% reduction)
- Capers Jones: all projects achieving ≥99% defect-removal rates used formal inspections; none below 75% did.

> **Human reviewers catch what testing won't:** unclear error messages, inadequate comments, hard-coded values, repeated code patterns (Wiegers 2002).

**Collective ownership benefits:** better code quality (multiple eyes), lessened impact of departures, shorter defect-correction cycles.

### 21.2 Pair Programming

One programmer types, the other watches for mistakes and thinks strategically.

**Keys to success:**
- Support with coding standards (avoid style arguments)
- Person without keyboard must be an *active* participant
- Don't pair-program trivial code; use for complex work
- Rotate pairs and work assignments regularly (some recommend daily)
- Match pace; don't force incompatible personalities
- Avoid pairing two newbies
- Assign a team leader for coordination

**Benefits:**
- Holds up better under stress
- Improves code quality (rises to best programmer's level)
- Shortens schedules (faster, fewer defects)
- Disseminates corporate culture, mentors juniors, fosters collective ownership

### 21.3 Formal Inspections

Developed by Michael Fagan at IBM. Key distinctions from informal reviews:
- **Checklists** focus attention on past problem areas
- Focus on **defect detection**, not correction
- Reviewers **prepare beforehand** with a list of issues
- **Distinct roles:** moderator, author, reviewer, scribe
- Moderator is not the author and is trained
- Meeting held only if all participants are prepared
- **Data collected** and fed into future inspections
- **Management does not attend** (except for project-plan inspections)

**Roles:**
- **Moderator** — keeps pace productive, manages logistics, follows up action items
- **Author** — minor role; explains unclear parts; code should speak for itself
- **Reviewer** — finds defects during preparation and meeting
- **Scribe** — records defects and action items (not author or moderator)
- **Management** — should not attend; results not used for performance appraisal

**Procedure stages:**
1. **Planning** — moderator distributes materials + checklist
2. **Overview** — optional, if reviewers are unfamiliar with the project (dangerous: can gloss over unclear points)
3. **Preparation** — reviewers work alone; ~500 LOC/hour for application code, ~125 LOC/hour for system code. Assign perspectives (maintainer, customer, designer) or scenarios.
4. **Inspection Meeting** — someone other than author paraphrases/reads code. All logic explained. Scribe records errors; discussion stops once error recognised. Rate: ~150–200 nonblank noncomment lines/hour. **Max 2 hours.** No solution discussion.
5. **Inspection Report** — within a day, lists each defect with type and severity.
6. **Rework** — author fixes assigned defects.
7. **Follow-Up** — moderator ensures rework is done; may reinspect.
8. **Third-Hour Meeting** — informal, optional solution discussion.

**Egos:** Critique the code, not the author. Author acknowledges each alleged defect and moves on; author retains ultimate responsibility for resolution.

**Continuous improvement:** Track error types, update checklists, measure preparation/inspection rates, adjust.

### 21.4 Other Collaborative Practices

**Walk-Throughs:**
- Loosely defined; hosted/moderated by the author
- Focus on technical issues; 30–60 minutes
- All participants prepare; emphasis on error detection, not correction
- Management doesn't attend
- Typical effectiveness: 20–40% defect detection
- **Caveat:** Informal walk-throughs are significantly less effective than inspections. If the work product doesn't justify formal inspection overhead, use code reading instead.

**Code Reading:**
- 2+ people read code independently (~1000 lines/day), then meet with author
- NASA SEL: 3.3 defects/hour vs. 1.8 for testing; found 20–60% more errors over project life
- Less meeting time, more individual review time
- Valuable when reviewers are geographically dispersed
- AT&T study: 90% of defects found during preparation, only 10% during the review meeting itself

**Dog-and-Pony Shows:**
- Customer demonstrations; management reviews, not technical reviews
- Don't rely on them for technical quality improvement

**Comparison Table (summarised):**

| Property | Pair Programming | Formal Inspection | Walk-Through |
|---|---|---|---|
| Defined roles | Yes | Yes | No |
| Formal training | Maybe (coaching) | Yes | No |
| Who drives | Person at keyboard | Moderator | Author |
| Focus | Design, coding, testing, correction | Defect detection only | Varies |
| Checklist-driven | Informal | Yes | No |
| Follow-up | Yes | Yes | No |
| Process improvement | No | Yes | No |
| Typical defect % | 40–60% | 45–70% | 20–40% |

---

## Ch 22 — Developer Testing

### 22.1 Role of Developer Testing

Testing types during construction:
- **Unit testing** — single routine/class in isolation
- **Component testing** — class/package involving multiple programmers, in isolation
- **Integration testing** — combined execution of multiple components
- **Regression testing** — rerunning previous tests after changes
- **System testing** — final configuration; security, performance, resource, timing

> **Black-box** (tester can't see internals) vs. **White-box/glass-box** (tester knows internals — this is developer testing).

Testing ≠ Debugging: testing detects errors; debugging diagnoses and corrects root causes.

**Limitations of testing alone:**
- Individual testing steps find <50% of errors each; combined <60%
- Testing's goal (find errors) runs counter to other development goals (prevent errors)
- Testing can never prove absence of errors
- Testing by itself doesn't improve quality — like weighing yourself more often to lose weight
- Testing requires expecting to find errors — Myers study: experienced programmers found only 5 of 15 known defects; main cause was not examining output carefully enough

**Time allocation:** developer testing should take ~8–25% of total project time.

### 22.2 Recommended Approach

1. Test for each relevant **requirement**
2. Test for each relevant **design concern**
3. Use **basis testing** + **data-flow tests** to exercise every line
4. Use a **checklist** of common errors

**Test-first vs. test-last:**
- Writing tests first takes no more effort — just resequences
- Detects defects earlier (cheaper to fix)
- Forces thinking about requirements/design before coding
- Exposes requirements problems sooner
- Saved test cases can also be used for "test-last" regression
- **Not a panacea** — subject to general limitations of developer testing

**Limitations of developer testing:**
- Developers write "clean tests" (does it work?) not "dirty tests" (how does it break?). Mature orgs: 5 dirty tests per 1 clean test.
- Optimistic coverage perception: developers believe 95% coverage, achieving 50–60% typically.
- Statement coverage alone is insufficient; aim for **100% branch coverage**.

### 22.3 Bag of Testing Tricks

**Structured Basis Testing:**
Count minimum paths: 1 (straight path) + 1 for each `if`, `while`, `repeat`, `for`, `and`, `or` + 1 for each `case` (+1 more if no default). Create test cases for each true/false of each keyword.

**Data-Flow Testing:**
Based on data states: **defined**, **used**, **killed** (and **entered**/**exited** at routine boundaries).

Suspicious patterns:
- Defined-Defined (wasteful, error-prone)
- Defined-Exited (unused local variable)
- Defined-Killed (extraneous or missing code)
- Entered-Killed, Entered-Used (uninitialised local)
- Killed-Killed (double free)
- Killed-Used (use after free)
- Used-Defined (check for previous definition)

Test all **defined-used** combinations — stronger than just testing all definitions.

**Equivalence Partitioning:**
Two inputs that flush out the same errors → only need one. Partition input space into equivalence classes, test one value per class.

**Boundary Analysis:**
Test just-below, on, and just-above boundaries. Applies to simple boundaries (max, min, off-by-one) and **compound boundaries** (combinations of variables that produce extreme computed values).

**Classes of Bad Data:** too little data, too much data, wrong kind, wrong size, uninitialised.
**Classes of Good Data:** nominal, minimum normal config, maximum normal config, compatibility with old data.

**Error Guessing:** based on intuition, past experience, checklists of common errors.

**Hand-check convenience:** Use round numbers ($20,000 not $90,783.82) to make hand-verification easy.

### 22.4 Typical Errors

**Error distribution is not uniform:**
- 80% of errors in 20% of classes/routines (Endres 1975, Boehm 1987b)
- 50% of errors in 5% of classes (Jones 2000)
- IBM OS/360: error-prone routines had up to 50 defects/1000 LOC; fixing cost 10× development cost

**Error classification (Beizer):**
- 25% structural, 22% data, 16% functionality, 10% construction, 9% integration, 8% functional requirements

**Key findings:**
- 85% of errors can be fixed without modifying more than one routine (Endres 1975)
- Top error sources: thin domain knowledge, fluctuating requirements, communication breakdown (Curtis et al. 1988)
- ~95% of errors are programmers' fault (not OS, compiler, or hardware)
- Clerical errors (typos): 18–36% of construction errors. Three of the most expensive software errors ever involved changing a *single character*.
- Misunderstanding design: 16–19% of errors
- 85% of errors fixable in <few hours; ~1% take longer

**Construction defect proportion:**
- Small projects: ~75% from coding
- All projects: ≥35% from construction (up to 75% on large projects)
- Construction errors cost 25–50% as much as design errors to fix individually, but 1–2× as much in total due to greater volume

**Expected error rates:**
- Industry average: 1–25 defects/1000 LOC delivered
- Microsoft Apps Division: 10–20 during in-house testing, 0.5 in release
- Cleanroom: 3 during testing, 0.1 in release
- Space shuttle: 0 defects in 500,000 LOC
- TSP (Humphrey): ~0.06 defects/1000 LOC

> **General Principle restated:** cheaper to build high-quality software than to build and fix low-quality software. Cleanroom productivity: 740 LOC/work-month vs. industry 250–300.

**Errors in testing itself:** Test cases often have higher error density than the code being tested. Treat test code with the same care as production code.

### 22.5 Test-Support Tools

- **Scaffolding:** mock objects, stub routines, test harnesses/drivers, dummy files
- **Diff tools:** compare actual vs. expected output for regression testing
- **Test-data generators:** random data, weighted toward realistic ranges
- **Coverage monitors:** measure what code is exercised (typical without: 50–60%)
- **Data recorder/logging:** "black box" for post-failure diagnosis
- **Symbolic debuggers:** step through code, watch variables, learn language behaviour
- **System perturbers:** memory filling (0xCC for uninitialised detection), memory shaking, selective memory failing, bounds checkers
- **Error databases:** track recurring errors, status, severity; feed into process improvement

### 22.6 Improving Your Testing

- **Plan to test** from the beginning — make testing as important as design/coding
- **Regression testing** — rerun same tests after changes; essential for quality
- **Automate** — manual testing error rate ≈ bug rate in code being tested (Beizer); only ~50% of manual tests executed properly

### 22.7 Keeping Test Records

Track per defect: administrative description, full description, reproduction steps, workaround, severity, origin (requirements/design/coding/testing), subclassification, classes/routines changed, LOC affected, hours to find, hours to fix.

Analyse: defects per class/routine (normalised), test hours per defect, defects per test case, programming hours per defect fixed, % code coverage, outstanding defects by severity.

---

## Ch 23 — Debugging

### 23.1 Overview

Debugging = identifying root cause of an error and correcting it. Can consume up to 50% of development time.

**Performance variation (Gould 1975):**

| | Fastest 3 | Slowest 3 |
|---|---|---|
| Avg debug time (min) | 5.0 | 14.1 |
| Defects not found | 0.7 | 1.7 |
| New defects inserted fixing | 3.0 | 7.7 |

Extrapolated: slowest group takes ~13× as long to fully debug. Confirmed by other studies (Gilb 1977, Curtis 1981).

**Defects as opportunities** to learn about: the program, the kinds of mistakes you make, your code quality from a reader's perspective, how you solve problems, how you fix defects.

**Ineffective approaches (The Devil's Guide):**
- Find defects by guessing (scatter print statements randomly)
- Don't waste time understanding the problem
- Fix with the most obvious (superficial) fix
- Debug by superstition (blame the machine/compiler)

> **Assume the error is your fault.** It's the most productive mindset for debugging.

### 23.2 Finding a Defect — The Scientific Method

1. **Stabilise the error** — make it occur reliably; narrow to simplest reproducing test case
2. **Locate the source (fault):**
   a. Gather data producing the defect
   b. Analyse data; form hypothesis
   c. Design experiment to prove/disprove
   d. Prove or disprove
3. Fix the defect
4. Test the fix
5. Look for similar errors

**Tips for finding defects:**
- Use **all available data** to form hypotheses — account for everything observed
- **Refine test cases** further
- **Exercise code in unit tests** — defects are easier to find in small fragments
- Use available tools (debuggers, memory checkers, picky compilers)
- **Reproduce the error several different ways** (triangulate)
- Generate more data for more hypotheses
- Use results of **negative tests** (disproved hypotheses narrow the search)
- **Brainstorm** multiple hypotheses before testing
- Keep a notepad; list things to try
- **Narrow suspicious region** — binary search: remove/disable half the code at a time
- Be suspicious of **previously defective** classes/routines
- Check **recently changed** code — use diff tools
- **Expand** the suspicious region if you're stuck
- **Integrate incrementally** — add one piece at a time
- Check for **common defects** using checklists
- **Talk to someone else** — "confessional debugging"
- **Take a break** — let subconscious work

**Brute-force techniques (guaranteed to solve the problem, context-dependent):**
- Full design/code review on broken code
- Throw away section and redesign/recode from scratch
- Compile at pickiest warning level and fix all warnings
- Strap on unit test harness; test in isolation
- Create automated test suite; run all night
- Step through loop manually in debugger
- Compile with different compiler or in different environment
- Replicate end-user's full machine configuration
- Integrate in small pieces, fully testing each

> **Set a maximum time for quick-and-dirty debugging.** If you exceed it, switch to a brute-force approach.

**Syntax errors:**
- Don't trust line numbers in compiler messages
- Don't trust compiler messages literally
- Don't trust the compiler's second message — fix the first and recompile
- Divide and conquer: remove code sections and recompile
- Use syntax-directed editors to find misplaced comments/quotes

### 23.3 Fixing a Defect

> Defect corrections have >50% chance of being wrong the first time (Yourdon 1986b).

**Guidelines:**
- **Understand the problem before fixing** — triangulate with cases that should and shouldn't reproduce
- **Understand the program**, not just the problem — at least the vicinity (few hundred lines)
- **Confirm the defect diagnosis** — rule out competing hypotheses first
- **Relax** — rushing leads to incomplete diagnosis and corrections
- **Save original source code** before fixing
- **Fix the problem, not the symptom** — avoid special-case bandages that barnacle the code
- **Change code only for good reason** — no voodoo programming (random changes hoping they work)
- **Make one change at a time**
- **Check your fix** — triangulate again; run regression tests
- **Add a unit test** that exposes the defect
- **Look for similar defects** — they tend to occur in groups

### 23.4 Psychological Considerations

**Psychological set:** seeing what you expect to see (e.g., reading "Paris in the the Spring" and missing the duplicate "the"). Good formatting, naming, and style help defects stand out as variations.

**Psychological distance:** ease of differentiating two items. `stoppt` vs. `stcppt` — almost invisible. Choose names with large psychological distance.

### 23.5 Debugging Tools

- **Source-code comparators (diff)** — pinpoint what changed between versions
- **Compiler warning messages** — set to pickiest level; treat warnings as errors; establish project-wide compile settings
- **Extended syntax/logic checking** (e.g., `lint`) — uninitialised variables, `=` vs. `==`
- **Execution profilers** — uncover hidden defects disguised as performance issues
- **Test frameworks/scaffolding** — isolate troublesome code
- **Debuggers** — breakpoints, stepping, backwards execution, data examination, logging. **Use both brain and debugger** — neither is a substitute for the other.

---

## Ch 24 — Refactoring

### 24.1 Kinds of Software Evolution

> "All successful software gets changed." — Fred Brooks

Reality: code evolves substantially during initial development. Requirements change ~1–4% per month (Jones 2000).

**Key distinction:** Does quality improve or degrade under modification? Treat modifications as opportunities to tighten the original design.

**The Cardinal Rule of Software Evolution:** evolution should improve the internal quality of the program.

### 24.2 Introduction to Refactoring

> **Refactoring** (Fowler): "a change made to the internal structure of the software to make it easier to understand and cheaper to modify without changing its observable behavior."

**Reasons to Refactor ("Code Smells"):**
- **Duplicated code** — "Copy and paste is a design error" (Parnas). Violates DRY principle.
- **Routine too long** — >1 screen in OOP is rarely needed
- **Loop too long or too deeply nested** — extract loop body into routines
- **Class has poor cohesion** — unrelated responsibilities → split into multiple classes
- **Class interface lacks consistent abstraction** — interface degrades under expedient modifications
- **Parameter list too long** — warning of poor abstraction
- **Changes compartmentalised within a class** — class has distinct responsibilities → split
- **Parallel modifications to multiple classes** — rearrange so changes affect one class
- **Inheritance hierarchies modified in parallel**
- **Case statements modified in parallel** — consider polymorphism
- **Related data items not organised into classes** — extract class
- **Routine uses more features of another class than its own** — move the routine
- **Primitive data type overloaded** — e.g., integer for Money → create Money class
- **Class doesn't do very much** — eliminate and redistribute
- **Tramp data** — data passed through chains of routines just to reach its destination
- **Middleman object** — just passes calls; eliminate and call directly
- **One class overly intimate with another** — strengthen encapsulation
- **Poor routine name** — rename everywhere; harder later, do it now
- **Public data members** — hide behind access routines
- **Subclass uses only small % of parents' routines** — convert is-a to has-a
- **Comments explain difficult code** — "Don't document bad code — rewrite it" (Kernighan & Plauger)
- **Global variables used** — isolate in access routines
- **Setup/takedown code around routine calls** — refactor the interface
- **Speculative "might be needed someday" code** — remove it; adds complexity, slows project

**Reasons NOT to Refactor:**
- "Refactoring" is not a synonym for any code change. Change is not a virtue in itself; purposeful, disciplined change supports steady quality improvement.

### 24.3 Specific Refactorings

**Data-Level:**
- Replace magic number with named constant
- Rename variable/constant/class/routine with clearer name
- Move expression inline (replace intermediate variable)
- Replace expression with a routine (eliminate duplication)
- Introduce intermediate variable (name the purpose)
- Convert multi-use variable to multiple single-use variables
- Use local variable instead of input-only parameter
- Convert data primitive to a class (Money, Temperature, Color, etc.)
- Convert set of type codes to class/enum (or class with subclasses for polymorphic behaviour)
- Change array to object (when elements are different types)
- Encapsulate a collection (return read-only, provide add/remove routines)
- Replace traditional record with data class

**Statement-Level:**
- Decompose boolean expression (introduce well-named intermediate variables)
- Move complex boolean expression into well-named boolean function
- Consolidate duplicated fragments in conditional branches
- Use `break` or `return` instead of loop control variable
- Return as soon as you know the answer (vs. assigning return value through nested logic)
- Replace conditionals (especially repeated case statements) with polymorphism
- Create and use null objects instead of testing for null values

**Routine-Level:**
- Extract routine/method
- Move routine's code inline (when body is simple and self-explanatory)
- Convert long routine to a class
- Substitute simple algorithm for complex algorithm
- Add/remove parameter

### 24.4 Refactoring Safely

*(Content truncated in source — the following is based on standard refactoring discipline from the chapter context.)*

Key principles for safe refactoring:
- Refactor in small, incremental steps — one refactoring at a time
- Run the full test suite after each refactoring
- Use version control to revert if needed
- Keep refactoring separate from adding functionality
- Refactor before adding a feature (make the change easy, then make the easy change)
- Don't refactor and fix bugs simultaneously

### 24.5 Refactoring Strategies

- **Target error-prone modules** — identify and redesign/rewrite the most defective code
- **Refactor when you touch code** — leave the campground cleaner than you found it
- **Refactor before adding a feature** — make room for the change
- **Refactor after adding a feature** — clean up the implementation
- **Define an interface between cleanup and new code** — create a clean boundary
- **Plan refactoring time** — allocate explicit time for cleanup in estimates
- **Use refactoring to understand unfamiliar code** — improve naming and structure as comprehension grows

---

## Key Takeaways (Ch 20–24)

1. **Quality is free in the end** — it requires reallocating resources to prevent defects cheaply instead of fixing them expensively.
2. **No single technique is sufficient** — combine inspections, testing, prototyping, and reviews for effective defect removal.
3. **Collaborative construction finds defects testing misses** — and does so more cheaply. Formal inspections are the gold standard.
4. **Developer testing is necessary but insufficient alone** — test-first, basis testing, data-flow testing, boundary analysis, and automation all matter.
5. **Debugging is a science, not superstition** — use the scientific method; 10×+ performance differences between good and poor debuggers.
6. **Refactoring is continuous improvement** — code evolves; steer evolution toward higher internal quality. Refactor in small, safe steps with a test safety net.
7. **The General Principle** ties it all together: improving quality reduces development costs — the projects with the fewest defects have the shortest schedules and highest productivity.
