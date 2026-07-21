---
title: Variables and Data
tags: [construction, variables, naming, data-types, code-complete]
source: McConnell Code Complete Ch 10-13
created: 2026-07-21
---

# Variables and Data

Source: Steve McConnell, *Code Complete* (2nd ed.), Chapters 10–13

---

## Chapter 10: General Issues in Using Variables

### 10.1 Data Literacy

A good repertoire of data types is a key part of a programmer's toolbox. Understand these terms:

| Category | Terms |
|----------|-------|
| Core types | abstract data type, array, bitmap, boolean, character, container class, double precision, enumerated type, floating point, heap, integer, linked list, literal, local variable, lookup table, named constant, pointer, stack, string, structured variable, tree, typedef, union, variant |
| OOP | member data, private |
| Data structures | B-tree, index |
| Advanced | referential integrity |

**Resources:** Cormen et al. *Introduction to Algorithms*; Sedgewick *Algorithms in C++*

### 10.2 Making Variable Declarations Easy

**Implicit Declarations** — one of the most hazardous features in any language (e.g., `acctNo` vs `acctNum` in VB without `Option Explicit`).

Mitigation strategies:
- **Turn off implicit declarations** — `Option Explicit` in VB, strict mode elsewhere
- **Declare all variables** even when not required
- **Use naming conventions** for common suffixes (Num vs No)
- **Check variable names** via compiler cross-reference lists

### 10.3 Guidelines for Initializing Variables

Improper data initialization is one of the **most fertile sources of error**. Three root causes:
1. Variable never assigned a value (contains garbage bits)
2. Value is outdated (stale from previous assignment)
3. Partially initialized (some members set, some not — especially pointer/memory issues)

**Key guidelines:**

| # | Guideline |
|---|-----------|
| 1 | **Initialize each variable as it's declared** — defensive programming, inexpensive insurance |
| 2 | **Initialize close to first use** — don't batch all declarations at the top; keep declaration+init+use together (Principle of Proximity) |
| 3 | **Declare and define at the same time** — `int accountIndex = 0;` |
| 4 | **Use `final`/`const` when possible** — prevents reassignment after initialization |
| 5 | **Pay special attention to counters and accumulators** (i, j, k, sum, total) — common to forget resetting |
| 6 | **Initialize class member data in constructor** — allocate in constructor, free in destructor |
| 7 | **Check need for reinitialization** — inside loops or between calls |
| 8 | **Initialize named constants once** (Startup()); **initialize variables with executable code** close to use |
| 9 | **Use compiler auto-initialization** if available (document the dependency) |
| 10 | **Check input parameters for validity** before use |
| 11 | **Use memory-access checkers** for bad pointer detection |
| 12 | **Initialize working memory** at program start — fill with known value (0xCC for breakpoint on Intel, 0xDEADBEEF) |

### 10.4 Scope

Scope = a variable's "celebrity status" — how widely known/referenced it is.

**Localize References to Variables**

**Span** — number of lines between consecutive references to a variable.
```
a = 0;
b = 0;        // span of b = 1 (1 line between references)
c = 0;
b = a + 1;    // span of b = 0 (adjacent to previous reference)
```

Average span = sum of individual spans / count of spans. Lower is better.

**Live Time** — total statements from first to last reference (inclusive). Lower is better.

Benefits of short live time:
- Reduces window of vulnerability
- Gives accurate picture of code intent
- Reduces initialization errors during modification
- More readable — less context to hold in mind
- Easier refactoring into smaller routines

**Guidelines for Minimizing Scope:**

| # | Guideline |
|---|-----------|
| 1 | Initialize variables used in a loop **immediately before the loop** |
| 2 | Don't assign a value **until just before it's used** |
| 3 | **Group related statements** — process one set of variables completely before another set |
| 4 | **Break related statements into separate routines** |
| 5 | **Begin with most restricted visibility** — expand only if necessary. Prefer: loop-local → routine-local → private → protected → package → global (last resort) |

> **Convenience vs. Intellectual Manageability:** Maximizing scope makes programs easy to *write* but hard to *read*. Minimizing scope supports intellectual manageability — the less you keep in mind, the fewer errors.

### 10.5 Persistence

Forms of persistence:
- **Block/routine life** — local variables, loop counters
- **Until freed** — heap allocations (`new`/`delete`, garbage collection)
- **Program life** — globals, statics
- **Forever** — database/files between executions

**Avoiding persistence problems:**
- Use debug code/assertions to check critical variables for reasonable values
- Set variables to "unreasonable values" when done (e.g., `null` after `delete`)
- Write code that **doesn't assume data persists** across calls
- Declare and initialize data right before use — be suspicious of data without nearby initialization

### 10.6 Binding Time

Binding time = when a variable and its value are bound together. **Later binding = more flexibility, more complexity.**

Binding time spectrum (earliest → latest):
1. **Coding time** — hard-coded magic numbers (worst)
2. **Compile time** — named constants (good balance)
3. **Load time** — reading from registry/config file
4. **Object instantiation time** — reading each time a window is created
5. **Just in time** — reading each time the window is drawn (most flexible)

> **Principle:** Build in as much flexibility as needed — but don't add flexibility (and complexity) beyond what's required.

### 10.7 Relationship Between Data Types and Control Structures

Michael Jackson's structural correspondence:

| Data Type | Control Structure | Example |
|-----------|-------------------|---------|
| **Sequential** data | Sequential statements | Reading name, SSN, address in order |
| **Selective** data | `if`/`case` statements | Hourly vs. salaried employee processing |
| **Iterative** data | `for`/`while` loops | Processing a list of records |

Real data is combinations of these building blocks.

### 10.8 Using Each Variable for Exactly One Purpose

**Key principles:**
- **Use each variable for one purpose only** — don't reuse `temp` for discriminant AND swap
- **Avoid variables with hidden meanings** — "hybrid coupling" (Page-Jones). e.g., `pageCount = -1` meaning "error occurred" — the integer is moonlighting as a boolean
- **Make sure all declared variables are used** — unreferenced variables correlate with higher fault rates (Card, Church, Agresti 1986)

---

## Chapter 11: The Power of Variable Names

Names matter more to the **reader** than the writer. Code is read far more times than it's written.

### 11.1 Considerations in Choosing Good Names

**The Most Important Consideration:** The name must **fully and accurately describe** the entity. State in words what the variable represents — often that statement itself is the best name.

**Problem Orientation:** Name should speak to the *problem* (what), not the *solution* (how).
- ❌ `inputRec`, `bitFlag`, `calcVal`
- ✅ `employeeData`, `printerReady`, `sum`

**Optimum Name Length:** Research (Gorla et al. 1990) found 10–16 characters minimizes debugging effort. 8–20 characters nearly as good.

**Effect of Scope on Name Length:**
- Long names: rarely used variables, global variables
- Short names (i, j, k): local loop counters with very limited scope
- **Use qualifiers on global-namespace names** — `uiEmployee` vs `dbEmployee`, or `namespace`/`package`

**Computed-Value Qualifiers** — put modifier at the **end**: `revenueTotal`, `expenseAverage` (not `totalRevenue`, `averageExpense`).

Exception: `Num` — use `Count`/`Total` (total count) and `Index` (specific) instead to avoid confusion.

**Common Opposites:** `begin/end`, `first/last`, `locked/unlocked`, `min/max`, `next/previous`, `old/new`, `opened/closed`, `visible/invisible`, `source/target`, `source/destination`, `up/down`

### 11.2 Naming Specific Types of Data

**Loop Indexes:**
- Simple loops: `i`, `j`, `k` are acceptable (well-established convention)
- Outside the loop or nested loops: use descriptive names (`teamIndex`, `eventIndex`, `recordCount`)
- Don't use `i`, `j`, `k` for anything other than loop indexes

**Status Variables:**
- ❌ `flag`, `statusFlag`, `printFlag`
- ✅ `dataReady`, `characterType`, `reportType` with named constants/enums (`ReportType_Annual`)
- Think of a better name than `flag`

**Temporary Variables:**
- `temp`, `x` signal incomplete understanding
- Replace with descriptive names: `discriminant` not `temp`
- Most variables are temporary — singling out a few may indicate uncertain purpose

**Boolean Variables:**
- Useful names: `done`, `error`, `found`, `success`/`ok`
- Names must imply `true`/`false` — ❌ `status`, `sourceFile`; ✅ `statusOK`, `sourceFileAvailable`
- `Is` prefix makes it a question: `isDone?`, `isError?` — but `if (isFound)` slightly less readable than `if (found)`
- **Use positive names** — ❌ `notFound` (double-negative: `if not notFound`)

**Enumerated Types:**
- Prefix members with group name: `Color_Red`, `Planet_Earth`, `Month_January`
- In languages with namespace qualification (e.g., Java enums), simplify to `Color.Red`

**Named Constants:**
- Name the **abstract entity**, not the number: ✅ `CYCLES_NEEDED`, ❌ `FIVE`, ❌ `BAKERS_DOZEN`

### 11.3 The Power of Naming Conventions

**Benefits:**
- Let you take more for granted (one global decision vs. many local ones)
- Transfer knowledge across projects
- Learn code faster on new projects
- Reduce name proliferation (`pointTotal` vs `totalPoints`)
- Compensate for language weaknesses (emulate named constants, enums)
- Emphasize relationships (`employeeAddress`, `employeePhone`, `employeeName`)

> Any convention is often better than no convention. The power comes from the fact that a convention *exists*, not from which specific convention is chosen.

**When to have a naming convention:** multiple programmers, hand-off expected, code reviews, large programs, long-lived projects, many unusual terms.

### 11.4 Informal Naming Conventions

**Language-Independent Guidelines:**
- **Differentiate variable names from routine names** — `variableName` vs. `RoutineName()`
- **Differentiate classes and objects** — 5 options: capital letter (Widget widget), ALL_CAPS (WIDGET widget), `t_` prefix, `a` prefix, or specific variable names (`employeeWidget`)
- **Identify global variables** — `g_` prefix
- **Identify member variables** — `m_` prefix
- **Identify type definitions** — `t_Color`, `t_Menu`
- **Identify named constants** — `c_` prefix or ALL_CAPS
- **Identify enumerated type elements** — `Color_Red` or ALL_CAPS
- **Identify input-only parameters** — `const` prefix in languages without enforcement
- **Format for readability** — camelCase or underscores, don't mix

**Language-Specific Conventions:**

| Entity | C | C++ | Java |
|--------|---|-----|------|
| Constants/Macros | ALL_CAPS | ALL_CAPS | ALL_CAPS |
| Classes/Types | MixedCase | MixedCase | MixedCase (initial cap) |
| Variables/Functions | lowercase_underscore | camelCase (initial lower) | camelCase (initial lower) |
| Pointers | `p` prefix | `p` prefix | N/A |

### 11.5 Standardized Prefixes (Hungarian Notation)

Two-part prefixes:
1. **UDT abbreviation** (user-defined type): `wn` (window), `scr` (screen), `doc` (document), `pa` (paragraph)
2. **Semantic prefix**: `c` (count), `first`, `last`, `lim`, `max`, `min`, `i` (index), `g` (global), `m` (class-level), `p` (pointer)

Examples: `firstPa`, `iPa`, `cPa`, `firstPaActiveDocument`

Pitfall: Don't neglect the descriptive name after the prefix — `ipaActiveDocument` > `ipa`

### 11.6 Creating Short Names That Are Readable

**Abbreviation guidelines:**
- Use standard abbreviations (dictionary-based)
- Remove nonleading vowels
- Remove articles (and, or, the)
- Use first letter(s) of each word
- Truncate consistently
- Keep first and last letters
- Maximum 3 significant words
- Remove useless suffixes (-ing, -ed)
- Keep most noticeable sound per syllable

**Pitfalls:**
- Don't abbreviate by removing one character
- Abbreviate consistently (Num everywhere OR No everywhere, not both)
- Create pronounceable names (telephone test)
- Avoid misreadable combinations (ENDB > BEND)
- Use a thesaurus to resolve naming collisions
- Document extremely short names with translation tables
- Maintain a project-level "Standard Abbreviations" document in version control

### 11.7 Kinds of Names to Avoid

| Avoid | Reason |
|-------|--------|
| Misleading names/abbreviations | `FALSE` = "Fig and Almond Season" |
| Names with similar meanings | `input` vs `inputValue`, `fileNumber` vs `fileIndex` |
| Different meanings, similar names | `clientRecs` vs `clientReps` — need 2+ character difference or difference at beginning/end |
| Sound-alike names | `wrap`/`rap` (telephone test applies) |
| Numerals in names | `file1`/`file2` — use arrays or meaningful names |
| Misspelled words | `hilite` — hard to remember the "correct" misspelling |
| Commonly misspelled English words | `calender`, `concieve`, `independance` |
| Case-only differentiation | `frd` vs `FRD` vs `Frd` |
| Multiple natural languages | Standardize on one English variant (color/colour) |
| Standard type/variable/routine names | Don't shadow built-ins |
| Totally unrelated names | `margaret`, `pookie` — not clever, just confusing |
| Hard-to-read characters | `1`/`l`/`I`, `0`/`O`, `2`/`Z`, `S`/`5`, `G`/`6` |

---

## Chapter 12: Fundamental Data Types

### 12.1 Numbers in General

| Guideline | Detail |
|-----------|--------|
| **Avoid "magic numbers"** | Replace literals with named constants; the only acceptable literals are `0` and `1` |
| **Anticipate divide-by-zero** | Check denominator before every division |
| **Make type conversions obvious** | `y = x + (float)i` or `CSng(i)` |
| **Avoid mixed-type comparisons** | `if (i == x)` with int/float almost guaranteed to fail — convert manually |
| **Heed compiler warnings** | Top programmers fix code to eliminate ALL compiler warnings |

### 12.2 Integers

- **Check for integer division** — `7/10 = 0`, not 0.7. Reorder: `(10*7)/10` not `10*(7/10)`
- **Check for integer overflow** — think through largest possible value for each term. On 32-bit, max signed = 2,147,483,647
- **Check overflow in intermediate results** — `(1000000 * 1000000) / 1000000` can overflow even if final result fits

### 12.3 Floating-Point Numbers

> Many fractional decimals can't be accurately represented in binary. 1/3 ≈ 0.33333330 (7 digits accuracy on 32-bit).

| Guideline | Detail |
|-----------|--------|
| **Avoid add/sub on numbers of greatly different magnitudes** | 1,000,000.00 + 0.1 = 1,000,000.00. Sort and add smallest first. |
| **Avoid equality comparisons** | `0.1 * 10 ≠ 1.0`. Use `abs(a-b) < ACCEPTABLE_DELTA` instead. |
| **Anticipate rounding errors** | Solutions: (1) higher precision, (2) BCD variables, (3) switch to integer (dollars→cents × 100), (4) use language-specific `Currency` type |

### 12.4 Characters and Strings

**Avoid magic characters and strings.** Use named constants or resource files.

Reasons:
- Strings may change (version names, report titles)
- Internationalization (string resource files)
- Memory concerns in embedded systems
- Clarity — `ESCAPE` vs `0x1B`

**Watch for off-by-one errors** — substrings index like arrays.

**Unicode strategy:** Decide early. Use Unicode for multilingual; ISO 8859 for single alphabetic language. Convert at I/O boundaries.

**C-Specific String Guidelines:**
- Difference between string pointers and character arrays — suspect any `=` with strings (use `strcmp`, `strcpy`, `strlen`)
- Naming convention: `ps` (pointer to string), `ach` (array of characters)
- Declare C strings as **length CONSTANT+1** — use CONSTANT for operations, CONSTANT+1 for declaration
- Initialize strings to null: `char name[LEN+1] = {0};`, use `calloc()` not `malloc()`
- Prefer character arrays over pointers when memory permits — better compiler warnings
- Use `strncpy()`/`strncmp()` not `strcpy()`/`strcmp()` — bounded versions prevent runaway strings

### 12.5 Boolean Variables

| Guideline | Example |
|-----------|---------|
| **Document your program** | Assign complex expression to a named boolean: `finished = (idx < 0) \|\| (MAX < idx)` |
| **Simplify complicated tests** | Break monolithic `if` into named boolean components |
| **Create your own boolean type** | If language lacks it: `typedef int BOOLEAN;` or `enum Boolean { True=1, False=(!True) };` |

### 12.6 Enumerated Types

Enumerated types allow each member of a class to be described in English — powerful alternative to "1=red, 2=green, 3=blue."

Benefits:
- **Readability** — `if (chosenColor == Color_Red)` vs `if (chosenColor == 1)`
- **Reliability** — compiler-enforced type checking (no assigning `Country_England` to a `Color` variable)
- **Modifiability** — add elements by editing the type definition only
- **Alternative to booleans** — `Status_Success`, `Status_Warning`, `Status_FatalError` richer than true/false

Guidelines:
- **Check for invalid values** — `Case Else` trap
- **Define `First` and `Last` entries** for loop iteration
- **Reserve first entry as "invalid"** — catches uninitialized variables (many compilers default to 0)
- **Beware explicit value assignment** — gaps create "invalid" loop values
- **Simulate enums** in languages without them using classes with `static final` members

### 12.7 Named Constants

A named constant "parameterizes" your program — single-point control for values that might change.

> Be a fanatic about rooting out literals. Use a text editor to search for 2–9 to make sure you haven't used them accidentally.

Guidelines:
- Use in **data declarations** and **loop limits**
- Avoid literals, **even "safe" ones** — `NUM_MONTHS_IN_YEAR` clarifies intent even if months never change
- **Simulate with scoped variables/classes** if language doesn't support them
- **Use consistently** — don't use a constant in one place and a literal in another for the same entity

### 12.8 Arrays

| Guideline | Detail |
|-----------|--------|
| **Check index bounds** | Most common array problem — accessing out-of-bounds elements |
| **Consider containers** instead of arrays | Sets, stacks, queues with sequential access can be more reliable (Mills & Linger 1986) |
| **Check endpoints** | First element, last element, off-by-one errors |
| **Multidimensional subscripts** | Ensure correct order — `Array[i][j]` not `Array[j][i]` |
| **Watch for index cross-talk** | Nested loops: `Array[j]` when you mean `Array[i]` |
| **C `ARRAY_LENGTH()` macro** | `#define ARRAY_LENGTH(x) (sizeof(x)/sizeof(x[0]))` |

### 12.9 Creating Your Own Types (Type Aliasing)

> Programmer-defined data types are one of the most powerful capabilities for clarifying understanding and protecting against change — without designing new classes.

```cpp
typedef float Coordinate;  // for coordinate variables
// Later, change one line:
typedef double Coordinate; // upgraded precision everywhere
```

Reasons to create your own types:
- **Make modifications easier** — one-line change propagates everywhere
- **Avoid excessive information distribution** — centralize type knowledge (information hiding)
- **Increase reliability** — Ada-style range types: `type Age is range 0..99`
- **Make up for language weaknesses** — `typedef int Boolean;`

Guidelines:
| # | Guideline |
|---|-----------|
| 1 | **Functionally oriented names** — `Coordinate`, `Currency`, `Age` (not `BigInteger`, `LongString`) |
| 2 | **Avoid predefined types** except in typedef/type definitions |
| 3 | **Don't redefine predefined types** — don't create your own `Integer` |
| 4 | **Define substitute types for portability** — `INT32`, `LONG64` (clean distinction from built-in types) |
| 5 | **Consider creating a class** rather than a simple typedef when more flexibility is needed |

---

## Chapter 13: Unusual Data Types

### 13.1 Structures

Structures bundle related data items together.

**When to use structures (vs. naked variables):**

| Benefit | Example |
|---------|---------|
| **Clarify data relationships** | `employee.name` vs `name` — shows grouping clearly |
| **Simplify operations on blocks of data** | `newEmployee = oldEmployee` one assignment vs. N assignments |
| **Simplify parameter lists** | `EasyWayRoutine(employee)` vs `HardWayRoutine(name, address, phone, ssn, gender, salary)` |
| **Reduce maintenance** | Adding a field doesn't require changing every copy/swap block |

> Generally prefer **classes** over structures for privacy and functionality. Use structures when you need to directly manipulate blocks of data.

### 13.2 Pointers

> Pointer usage is one of the most error-prone areas of modern programming. Many modern languages (Java, C#, VB) don't provide a pointer data type.

**Paradigm:** Every pointer = **location in memory** (address) + **knowledge of how to interpret contents** (base type). Same memory bits can be interpreted as string, integer, float, or char — only one interpretation is correct.

**General Tips:**

| # | Strategy |
|---|----------|
| 1 | **Isolate pointer operations in routines or classes** — `NextLink()`, `InsertLink()`, `DeleteLink()` |
| 2 | **Declare and define pointers at the same time** — `Employee *e = new Employee;` |
| 3 | **Delete at the same scoping level as allocation** — constructor/destructor symmetry |
| 4 | **Check pointers before using them** — verify pointer is within expected memory range |
| 5 | **Check the referenced variable before using it** — reasonableness checks on pointed-to values |
| 6 | **Use dog-tag fields** — add a known-value field for corruption detection; check before delete; corrupt upon free |
| 7 | **Add explicit redundancies** — duplicate critical fields, check for match |
| 8 | **Use extra pointer variables for clarity** — `followingNode = startNode->next` rather than `currentNode->next->previous` |
| 9 | **Simplify complicated pointer expressions** — `quantityDiscount = rates->discounts->factors->net;` extracted before the loop |
| 10 | **Draw a picture** — linked-list diagrams clarify pointer reassignment |
| 11 | **Delete in the right order** — save pointer to next before freeing current in linked lists |
| 12 | **Allocate a reserve parachute of memory** — for graceful shutdown on out-of-memory |
| 13 | **Shred your garbage** — overwrite memory with junk data before `delete` |
| 14 | **Set pointers to null after deleting** — dangling pointer writes produce errors you can find |
| 15 | **Check for bad pointers before deleting** — `ASSERT(pointer != NULL)` |
| 16 | **Keep track of pointer allocations** — maintain a list, check membership before free |
| 17 | **Write cover routines** — `SAFE_NEW` and `SAFE_DELETE` centralize all pointer safety |
| 18 | **Use a non-pointer technique** if possible |

**C++-Specific:**
- Understand **pointers vs. references** — reference must always refer to an object, can't be changed after init
- Use **pointers for pass-by-reference**, `const` references for pass-by-value
- Use `auto_ptr` for automatic deletion on scope exit

**C-Specific:**
- Use **explicit pointer types** (not `void*` or `char*` for everything)
- **Avoid type casting** — turns off compiler type checking
- **Asterisk rule:** must have `*` in assignment to pass value back: `*parameter = SOME_VALUE;`
- Use `sizeof()` for memory allocation

### 13.3 Global Data

> Most experienced programmers conclude: using global data is riskier than using local data. Even if global variables don't always produce errors, they're hardly ever the *best* way.

**Common Problems with Global Data:**

| Problem | Description |
|---------|-------------|
| **Inadvertent changes (side effects)** | `GetOtherAnswer()` silently changes global `theAnswer` |
| **Aliasing problems** | Same variable accessed by two names — global + parameter alias |
| **Re-entrant code problems** | Multithreaded code shares global data across copies |
| **Code reuse hindered** | Can't extract a class without dragging along its global data |
| **Uncertain initialization order** | C++ translation unit init order undefined |
| **Modularity damaged** | Can't think about one routine at a time — must consider all routines sharing the global |

**Legitimate Uses (disciplined, rare):**
- Preservation of truly global values (program state, configuration)
- Emulation of named constants in languages that lack them
- Emulation of enumerated types
- Streamlining extremely common data (but usually = missing class)
- Eliminating tramp data (data passed through chains without being used)

**Alternatives to Global Data (in order of preference):**

1. **Make variables local first** — make global only as needed, never the reverse
2. **Distinguish global from class variables** — use access routines for cross-class access
3. **Use access routines** — the workhorse approach

**Access Routines Advantages:**
- Centralized control over data
- Ensure all references are barricaded (e.g., stack overflow check in `PushStack()`)
- Automatic information hiding
- Easy to convert to abstract data type
- Build abstraction at problem-domain level, not implementation level

```cpp
// Direct global data access — implementation-level
node = node.next;

// Access routine — problem-domain level
account = NextAccount(account);
```

**Using Access Routines:**
- Hide data in a class with `static` keyword
- Provide `Get()`/`Set()` routines
- Require all code to go through access routines — `g_` prefix convention
- Don't dump all globals into one barrel — organize by class
- Use locking for concurrent access control
- Keep all accesses at the same level of abstraction
- Build abstraction into the access routines (problem domain, not implementation)

**Reducing Risks (when you must use globals):**
- Naming convention that makes globals obvious (`g_` prefix)
- Well-annotated list of all global variables
- Don't use globals for intermediate results
- Don't disguise globals as monster objects passed everywhere

---

## Key Points Summary

| Chapter | Key Takeaway |
|---------|-------------|
| **Ch 10** | Minimize scope; keep references close; use each variable for one purpose; bind at the latest appropriate time |
| **Ch 11** | Names fully describe the entity; naming conventions exist; favor read-time convenience over write-time convenience |
| **Ch 12** | Avoid magic numbers; check for overflow/divide-by-zero; use enums and named constants; create your own types |
| **Ch 13** | Structures clarify data; pointers are error-prone (isolate them); avoid global data (use access routines) |

---

## Checklists

### General Considerations in Using Data (Ch 10)

- [ ] Each routine checks input parameters for validity
- [ ] Variables declared close to first use
- [ ] Variables initialized as they're declared (or close to first use)
- [ ] Counters and accumulators initialized/reinitialized properly
- [ ] All variables have smallest scope possible
- [ ] References to variables as close together as possible (short span + live time)
- [ ] Control structures correspond to data types
- [ ] All declared variables are used
- [ ] Variables bound at appropriate times (balance flexibility vs complexity)
- [ ] Each variable has one and only one purpose (no hidden meanings)

### Naming Variables (Ch 11)

- [ ] Name fully and accurately describes what the variable represents
- [ ] Name refers to real-world problem, not programming-language solution
- [ ] Computed-value qualifiers at end of name
- [ ] Loop indexes are meaningful (not i/j/k for long/nested loops)
- [ ] No unnamed "temporary" variables
- [ ] Boolean names imply true/false meaning
- [ ] Enumerated types have category prefix/suffix
- [ ] Named constants named for abstract entities, not numbers
- [ ] Convention distinguishes local/class/global, types/constants/enums/variables
- [ ] Names formatted for readability (camelCase or underscores, not mixed)
- [ ] Abbreviations used sparingly and consistently
- [ ] All words abbreviated consistently

### Fundamental Data Types (Ch 12)

- [ ] No magic numbers (only 0 and 1 acceptable as literals)
- [ ] Divide-by-zero anticipated
- [ ] Type conversions obvious
- [ ] No mixed-type comparisons
- [ ] Integer division and overflow handled
- [ ] No addition/subtraction on numbers with greatly different magnitudes
- [ ] No equality comparisons on floating-point numbers
- [ ] No magic characters or strings
- [ ] Boolean variables used to document and simplify conditional tests
- [ ] Enumerated types replace named constants where possible
- [ ] Invalid values checked in enumerated type tests
- [ ] First entry in enum reserved for "invalid"
- [ ] Named constants used for data declarations and loop limits
- [ ] All array indexes within bounds
- [ ] Different type for each kind of data that might change
- [ ] Type names oriented toward real-world entities

### Unusual Data Types (Ch 13)

- [ ] Structures used to organize groups of related data
- [ ] Class considered as alternative to structure
- [ ] All variables local or class-scope unless absolutely must be global
- [ ] Variable naming conventions differentiate local/class/global
- [ ] All global variables documented
- [ ] No pseudoglobal data (mammoth objects passed everywhere)
- [ ] Access routines used instead of global data
- [ ] Pointer operations isolated in routines
- [ ] Pointers checked for validity before use
- [ ] Pointers set to null after freeing
- [ ] Linked list pointers freed in correct order
- [ ] Reserve memory parachute allocated
- [ ] Pointers used only as last resort
