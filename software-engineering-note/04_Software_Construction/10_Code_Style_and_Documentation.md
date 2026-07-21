---
tags: [construction, style, documentation, comments, code-complete]
source: "McConnell, Code Complete (2nd ed.), Chapters 31–32"
created: 2026-07-21
---

# 10 — Code Style and Documentation

> *"Any fool can write code that a computer can understand. Good programmers write code that humans can understand."* — Martin Fowler

---

## Chapter 31: Layout and Style

Layout is the **aesthetic dimension** of programming. Well-formatted code doesn't change execution speed or memory use — it changes how easy the code is to **understand, review, and revise**. The payoff comes months later, when you (or someone else) must read it.

---

### 31.1 Layout Fundamentals

#### The Fundamental Theorem of Formatting

> **Good visual layout shows the logical structure of a program.**

Pretty is secondary. If a style shows structure better but another looks nicer, pick the one that shows structure. Techniques that make **good code look good** and **bad code look bad** are more useful than techniques that make all code look good.

#### Human vs. Computer Interpretation

Layout is a clue to program structure for **humans**. The compiler doesn't care about indentation, but human readers draw conclusions from it. Good layout makes the visual structure match the logical structure.

**Evidence:** Chase & Simon (1973) showed chess experts had far better memory when pieces were in game positions vs. random. Shneiderman (1976) replicated this with code: experts remembered sensibly-arranged statements better than novices, but the gap narrowed when statements were shuffled. **Structure aids perception, comprehension, and memory.**

#### Layout as Religion

Debates about formatting often resemble religious wars. At a coarse level, some forms are clearly better than others. But the specific convention matters less than **consistent application**. Expert programmers cling to their styles — the key is that structure is present, not which structure.

#### Objectives of Good Layout

| Criterion | Description |
|---|---|
| **Accurately represent logical structure** | Primary purpose — indentation and white space must show the logical organization |
| **Consistency** | Rules must apply to most cases without many exceptions |
| **Improve readability** | Logical but unreadable is useless |
| **Withstand modifications** | Changing one line shouldn't force changes to unrelated lines |

---

### 31.2 Layout Techniques

#### White Space

White space is the **primary tool** for showing structure. It includes spaces, tabs, line breaks, and blank lines. A program should give *more* organizational clues than a book, not fewer.

#### Grouping

Related statements belong together — like paragraphs in prose. A paragraph of code should contain statements that accomplish a single task.

#### Blank Lines

Use blank lines to:
- Separate unrelated statements
- Divide groups into paragraphs
- Separate routines from each other
- Highlight comments

**Research:** Gorla, Benander & Benander (1990) found optimal blank-line density is **8–16%** of lines. Above 16%, debug time increases dramatically.

#### Indentation

> **Indent statements under the statement to which they are logically subordinate.**

**Research (Miaria et al., 1983):** Subjects scored **20–30% higher** on comprehension tests with 2–4 space indentation vs. no indentation. Notably, 6-space indentation scored *second lowest* (after no indentation), even though subjects *felt* it was easier. **Aesthetic appeal ≠ readability.**

**Recommendation:** Use **2–4 spaces** for indentation.

#### Parentheses

> **Use more parentheses than you think you need.**

They add clarity at zero runtime cost. If you have to think about how an expression evaluates, add parentheses.

---

### 31.3 Layout Styles

#### 1. Pure Blocks (Visual Basic)

Languages with explicit terminators (`If-Then` / `End If`) naturally create pure blocks. The control construct's begin and end provide solid **visual closure**.

#### 2. Emulating Pure Blocks (Java/C++ standard)

Place the opening brace at the end of the control statement, and the closing brace aligned under the control keyword:

```java
if ( pixelColor == Color_Red ) {
    statement1;
    statement2;
}
```

This is standard in Java and common in C++. **Recommended for most cases.**

#### 3. Begin-End as Block Boundaries

Treat braces as block-boundary statements, placing them on their own lines:

```cpp
if ( pixelColor == Color_Red )
{
    statement1;
    statement2;
}
```

This also works well. A study by Hansen & Yim (1987) found **no statistically significant difference** in understandability between styles 2 and 3.

#### 4. Endline Layout (❌ Avoid)

Indenting code to the middle or end of a line to align with keywords. This style:
- Breaks down with complex conditionals
- Is hard to apply consistently
- Is a **maintenance nightmare** (changing one line forces reformatting others)
- Despite persistent textbook recommendations, it is **inaccurate and hard to maintain**

#### Which Style Is Best?

| Language | Recommendation |
|---|---|
| **Visual Basic** | Pure-block indentation |
| **Java** | Pure-block emulation (Style 2) |
| **C++** | Either pure-block emulation or begin-end boundaries — pick one and be consistent |

---

### 31.4 Laying Out Control Structures

#### Avoid Unindented Begin-End Pairs

```java
// ❌ BAD — violates Fundamental Theorem
for ( int i = 0; i < MAX_LINES; i++ )
{
    ReadLine( i );
}
```

The `begin` and `end` are neither part of the control construct nor part of the statements.

#### Avoid Double Indentation

```java
// ❌ BAD — misrepresents logical structure
for ( int i = 0; i < MAX_LINES; i++ )
    {
        ReadLine( i );
    }
```

Statements are shown as if subordinate to the `begin-end` pair, which they aren't. Double indentation exaggerates complexity — nesting two levels deep would produce four levels of indentation.

#### Blank Lines Between Paragraphs

Groups of statements that belong together (logical blocks) should be separated by blank lines, even when no `begin-end` pairs exist. This forces you to think about which statements truly belong together and creates natural spaces for comments.

#### Single-Statement Blocks

Three options for single-statement blocks after control structures:

```java
// Style 1 — no braces (risk of error when adding statements)
if ( expression )
    one-statement;

// Style 2 — always use braces (safe, consistent) ✅ RECOMMENDED
if ( expression ) {
    one-statement;
}

// Style 3 — same line (hard to debug, breaks indentation consistency)
if ( expression ) one-statement;
```

**Recommendation:** Style 2 for **consistency and safe modifiability**. Always use braces, even for single statements.

#### Complex Expressions

Put each condition on its own line:

```java
// ✅ READABLE
if ( ( ( '0' <= inChar ) && ( inChar <= '9' ) ) ||
     ( ( 'a' <= inChar ) && ( inChar <= 'z' ) ) ||
     ( ( 'A' <= inChar ) && ( inChar <= 'Z' ) ) )
```

This makes errors obvious and intent clear.

#### Gotos

If you must use `goto`:
- Left-align the label in **ALL CAPS**
- Put the `goto` statement on its own line
- Surround the label with blank lines and outdent to the left margin

But better: **avoid gotos entirely** and sidestep the formatting problem.

#### Case Statements

Use **standard indentation increment** (same as loop body indentation). Do NOT use endline layout — it's a maintenance headache when case names change length.

---

### 31.5 Laying Out Individual Statements

#### Statement Length

The traditional 80-character limit is increasingly arbitrary with modern screens. A 90-character line is more readable than one broken purely to avoid column 80. **Occasionally exceeding 80 characters is acceptable.**

#### Spaces for Clarity

- **Logical expressions:** `while ( pathName[ startPath + position ] != ';' )` — separate identifiers with spaces
- **Array references:** `grossRate[ census[ groupId ].gender, census[ groupId ].ageGroup ]`
- **Routine arguments:** spaces after commas make scanning easier

#### Continuation Lines

**Make incompleteness obvious** — break so the first line is syntactically wrong if it stands alone:

```java
// ✅ The && signals incompleteness
while ( pathName[ startPath + position ] != ';' ) &&
       ( ( startPath + position ) <= pathName.length() )
```

**Alternative:** put operators at the *beginning* of the continuation line (easier to scan, illuminates operation structure):

```java
totalBill = totalBill
    + customerPurchases[ customerID ]
    + CitySalesTax( customerPurchases[ customerID ] )
    + StateSalesTax( customerPurchases[ customerID ] );
```

**Indent continuation lines the standard amount** (same as loop-body indentation). Keep closely related elements together — don't break inside array references.

**One argument per line** for long parameter lists:

```java
DrawLine(
    window.north,
    window.south,
    window.east,
    window.west,
    currentWidth,
    currentAttribute
);
```

#### One Statement Per Line

Modern languages allow multiple statements per line. Don't do it. Reasons:
1. Shows true complexity — doesn't hide it
2. Modern compilers don't need formatting clues for optimization
3. Code reads top-to-bottom, not left-to-right
4. Line-oriented debuggers work properly
5. Easy to edit/delete/comment individual statements

#### Side Effects (C++)

Avoid multiple operations per line:

```cpp
// ❌ Order of evaluation is undefined
PrintMessage( ++n, n + 2 );

// ✅ Clear intent
++n;
PrintMessage( n, n + 2 );
```

The "clever" `strcpy` using `while ( *++t = *++s );` ran **11% slower** in profiling than the readable version. **Clarity first, performance second** — until measured.

#### Data Declarations

- **One declaration per line** — easier to find, comment, and modify
- **Declare variables close to first use** — reduces span and live time
- **Order sensibly** — group by type (alphabetic is overkill)
- **In C++**, put the asterisk next to the variable name or use pointer types

---

### 31.6 Laying Out Comments

- **Indent comments with their corresponding code** — poorly indented comments obscure logical structure
- **Set off each comment with at least one blank line** — enables scanning comments without reading code

---

### 31.7 Laying Out Routines

- **Use blank lines** between the routine header, data declarations, and body
- **Use standard indentation for routine arguments** — not endline layout:

```java
// ✅ Standard indentation — maintainable
public bool ReadEmployeeData(
    int maxEmployees,
    EmployeeList *employees,
    EmployeeFile *inputFile,
    int *employeeCount,
    bool *isInputError
)
```

This holds up under modification — renaming the routine doesn't affect parameter formatting.

---

### 31.8 Laying Out Classes

#### Class Interfaces — present members in this order:
1. Header comment describing the class
2. Constructors and destructors
3. Public routines
4. Protected routines
5. Private routines and member data

#### Class Implementations:
1. Header comment describing the file
2. Class data
3. Public routines
4. Protected routines
5. Private routines

#### Files and Programs:
- **One class per file** (unless language restricts this)
- **Give the file a name related to the class name** (`CustomerAccount.cpp` / `CustomerAccount.h`)
- **Separate routines with at least two blank lines**
- **Avoid overemphasizing** — use a hierarchy of separators (asterisks for classes, dashes for routines, blank lines for comments). Less is more.
- **In C++**, order source files: description comment → `#include` → constants → enums → macros → type defs → imports → exports → file-private → classes

---

### Layout Checklist (Condensed)

- [ ] Formatting illuminates logical structure
- [ ] Scheme is consistent and maintainable
- [ ] No double-indented `begin-end` / `{}` pairs
- [ ] Sequential blocks separated by blank lines
- [ ] Complex expressions formatted for readability
- [ ] Single-statement blocks use braces consistently
- [ ] Case statements use standard indentation
- [ ] White space makes expressions, arrays, and arguments readable
- [ ] Incomplete lines are obviously incomplete
- [ ] One statement per line, no side effects
- [ ] One data declaration per line
- [ ] Comments indented with their code
- [ ] Routine arguments use standard indentation
- [ ] One class per file; routines separated by blank lines

---

### Key Points — Chapter 31

1. **Illuminating logical organization** is the first priority of layout. Accuracy, consistency, readability, and maintainability are the criteria.
2. **Looking good is secondary** — a distant second.
3. **VB has pure blocks; Java convention is pure-block emulation.** In C++, either pure-block emulation or begin-end boundaries work.
4. **The specific convention matters less than following it consistently.** Inconsistency hurts readability.
5. **Separate objective from subjective.** Use explicit criteria to ground style discussions.

---

## Chapter 32: Self-Documenting Code

> *"Code as if whoever maintains your program is a violent psychopath who knows where you live."* — Anonymous

---

### 32.1 External Documentation

Documentation exists both **inside** and **outside** source code. External construction documentation includes:

- **Unit Development Folders (UDF) / Software Development Folders (SDF):** Informal documents with design-decision trails, requirements copies, design notes, and current code listings. Sometimes delivered to customers; often internal only.
- **Detailed-Design Document:** Low-level design document describing class/routine-level decisions, alternatives considered, and reasons for selections. Often exists only in the code itself.

---

### 32.2 Programming Style as Documentation

> **The main contributor to code-level documentation isn't comments — it's good programming style.**

Self-documenting code relies on:
- Good program structure
- Straightforward, understandable approaches
- Good variable and routine names
- Named constants instead of literals
- Clear layout
- Minimized control-flow and data-structure complexity

**Example:** A routine computing prime numbers with cryptic variable names (`i`, `num`, `meetsCriteria`) vs. the same code with clear names (`primeCandidate`, `isPrime`, `factorableNumber`). Same code, no comments in either — the second is vastly more readable.

> **"In well-written code, comments are the icing on the readability cake."**

#### Self-Documenting Code Checklist

| Area | Key Questions |
|---|---|
| **Classes** | Well-named? Interface presents consistent abstraction? Can be treated as black box? |
| **Routines** | Name describes exactly what it does? Performs one well-defined task? Interface obvious? |
| **Data Names** | Type names descriptive? Variables named well? Loop counters more informative than `i`, `j`, `k`? Named constants instead of magic numbers? |
| **Data Organization** | Extra variables used for clarity? References close together? Complex data behind abstract access routines? |
| **Control** | Nominal path clear? Related statements grouped? Normal case follows `if` rather than `else`? Nesting minimized? Boolean expressions simplified? |
| **Layout** | Does layout show logical structure? |
| **Design** | Straightforward, avoids cleverness? Implementation details hidden? Written in problem-domain terms? |

---

### 32.3 To Comment or Not to Comment

#### The Commento (Socratic Dialogue Summary)

- **Thrasymachus (purist):** Comments are essential — code without them is unreadable.
- **Callicles (veteran):** Comments are useless — they get out of date, English is less precise than code, you should make the code self-documenting.
- **Glaucon (young gun):** Heavily commented code means more to read. Refactoring eliminates most comments.
- **Ismene (pragmatist):** Good comments are worth their weight in gold. Comments clarify intent at a higher abstraction level. They serve as headings — scan comments to find the right section, then read the code.
- **Socrates (wise):** We'll encourage commenting but won't be naive. Code reviews ensure comments actually help.

#### Resolution

1. **Comments that repeat the code are worse than no comments.**
2. **Good comments explain intent** at a higher level of abstraction.
3. **Comments as headings** — scan to find the right section, then read code.
4. **Writing comments makes you think harder** — if it's hard to comment, the code or your understanding is the problem.
5. **Code reviews** expose when comments are needed.

---

### 32.4 Keys to Effective Comments

#### Six Kinds of Comments

| Kind | Value | Verdict |
|---|---|---|
| **Repeat of the code** | Restates what code does in different words — adds nothing | ❌ Useless |
| **Explanation of the code** | Explains complicated/tricky code | ⚠️ Usually a sign the code should be improved |
| **Marker in the code** | `// TODO`, `// FIXME` — notes to developer | ⚠️ Must be removed before release. Standardize marker style |
| **Summary of the code** | Distills several lines into 1–2 sentences | ✅ Valuable — faster to scan than code |
| **Description of intent** | Explains *purpose* at the problem level | ✅✅ Most valuable |
| **Information not in code** | Copyright, version numbers, design notes, references, optimization notes | ✅ Necessary |

**The three acceptable kinds for completed code:** summary, intent, and information that can't be expressed in code.

#### Commenting Efficiently

**Why commenting takes too long:**
1. The commenting style is time-consuming or tedious → find a new style
2. Words don't come easily → you don't understand the code well enough → time spent "commenting" is actually time spent **understanding**

**Guidelines:**
- **Use styles that don't break down under modification** — avoid leader dots (`.......`), fancy asterisk borders that need realignment, plus-sign underlines. If it's hard to maintain, it won't be maintained.
- **Use `//` for single-line, `/* ... */` for multi-line** in Java/C++ — avoids the tedium of long columns of double slashes.
- **Use the Pseudocode Programming Process** — outline in comments first, then fill in code. When code is done, comments are already done.
- **Integrate commenting into development** — don't leave it for the end. Commenting later takes more time and is less accurate.
- **Performance is not a reason to avoid comments** — create a release version with comments stripped during the build.

#### Optimum Number of Comments

**IBM research (Jones, 2000):** Commenting density of **~1 comment per 10 statements** was optimal. Fewer → hard to understand. More → also reduced understandability.

But focus on **comment quality and efficiency**, not quantity. The Pseudocode Programming Process naturally produces about one comment per few lines.

---

### 32.5 Commenting Techniques

#### Commenting Individual Lines

In good code, line-level comments are **rare**. Reasons to comment a single line:
- The line is complicated enough to need explanation
- The line once had an error and you want a record

**Guidelines:**
- **Avoid self-indulgent comments.** (The `R.I.P. L.V.B.` anecdote — a comment referencing Beethoven's death year in hex had nothing to do with the code.)
- Make the code itself clear enough that line comments are unnecessary.

#### Endline Comments (Mostly Avoid)

Endline comments appear at the end of lines of code. **Problems:**
- Hard to format and align
- Hard to maintain — changing one line's length forces reformatting all others
- Tend to be cryptic (limited space)
- Ambiguous about which lines they apply to
- Most endline comments just repeat the code

**Three exceptions:**
1. **Annotating data declarations** — meaningful comment beside each declaration
2. **Marking ends of blocks** — `} // for clientIndex` for long/nested loops
3. **NOT for maintenance notes** — those belong in version control, not production code

#### Commenting Paragraphs of Code

> **Write comments at the level of the code's intent.**

```java
// ❌ Repeats the code — useless
// check each character in "inputString" until a dollar sign is found
// or all characters have been checked

// ⚠️ Better but still shallow
// find '$' in inputString

// ✅ Explains intent — information not in the code
// find the command-word terminator ($)
```

**Heuristic:** What would you name a routine that does the same thing? That description, unshortened, is your comment at the level of intent.

**Focus the code itself first.** Replace literals with named constants, improve variable names, add clarifying variables. When the code approaches the level of intent, comments become genuinely supplementary.

**Focus on *why*, not *how*:**

```java
// ❌ Focuses on how — repeats code
// if account flag is zero
if ( accountFlag == 0 ) ...

// ✅ Focuses on why — explains intent
// if establishing a new account
if ( accountType == AccountType.NewAccount ) ...
```

**Additional paragraph-commenting guidelines:**
- **Comments should precede the code** they describe — prepare the reader
- **Make every comment count** — excessive comments obscure code
- **Document surprises** — if you used a non-obvious technique (e.g., right-shift instead of divide), explain why and quantify the benefit
- **Avoid abbreviations** — comments should be unambiguous
- **Differentiate major/minor comments** with ellipses (`// ...`) or, better, extract subordinate activities into their own routines
- **Document workarounds** for errors or undocumented features in libraries/languages
- **Justify violations of good style** — prevent well-intentioned "cleanup" that breaks your code
- **Don't comment tricky code — rewrite it!** "Don't document bad code — rewrite it" (Kernighan & Plauger). Research shows heavily commented areas tend to have the most defects. If you think "this is tricky," it *is* tricky. Rewrite it.

#### Commenting Data Declarations

- **Comment the units** of numeric data (inches, feet, meters, milliseconds, degrees...). Better yet, embed units in variable names.
- **Comment the range** of allowable numeric values. Use assertions at routine boundaries to enforce ranges.
- **Comment coded meanings** — use enumerated types if your language supports them; otherwise document what each value represents.
- **Comment limitations on input data** — expected and unexpected values.
- **Document flags to the bit level** — meaning of each bit in a bit field.
- **Stamp variable-related comments with the variable's name** — so string searches find both.
- **Document global data** — purpose and why it must be global. Use naming conventions to highlight global status.

#### Commenting Control Structures

- **Put a comment before each `if`, `case`, loop, or block** — clarify the purpose.
- **Comment the end of each control structure** — especially for long or nested blocks:

```java
for ( tableIndex = 0; tableIndex < tableCount; tableIndex++ ) {
    while ( recordIndex < recordCount ) {
        if ( !IllegalRecordNumber( recordIndex ) ) {
            ...
        } // if
    } // while
} // for
```

- **Treat end-of-loop comments as a warning sign** — if code is complicated enough to need them, consider simplifying.

#### Commenting Routines

> **Avoid the kitchen-sink routine prolog.**

Many textbooks recommend piling up information at the top of every routine: name, purpose, algorithm, inputs, outputs, assumptions, modification history, author, date, phone number, SSN, eye color, blood type... This is **wasteful** and **unmaintained**.

Instead, keep routine comments focused:
- **Describe the routine's purpose** at the level of intent
- **Document assumptions** about inputs, outputs, and global effects
- **Keep it brief** — most information should be in the code itself (good names, clear structure)
- Use tools like **Javadoc/Doxygen** for API documentation, but keep the source comments themselves lean

---

### 32.6 IEEE Standards *(referenced)*

The IEEE provides formal standards for software documentation (e.g., IEEE 830 for requirements, IEEE 1016 for design descriptions). For construction-level documentation, key principles are:
- Documentation should be **traceable** to requirements and design
- Every document should have a **defined audience and purpose**
- Documentation is a **living artifact** — plan for maintenance

---

### Self-Documenting Code Checklist (Condensed)

- [ ] Class interfaces present consistent abstraction
- [ ] Routine names describe exactly what each does
- [ ] Each routine performs one well-defined task
- [ ] Variable names are descriptive; loop counters aren't `i`, `j`, `k`
- [ ] Named constants replace magic numbers
- [ ] Extra variables used for clarity when needed
- [ ] Nominal path through code is clear
- [ ] Related statements grouped; nesting minimized
- [ ] Boolean expressions simplified
- [ ] Code is straightforward and avoids "cleverness"
- [ ] Implementation details hidden; written in problem-domain terms

---

### Key Points — Chapter 32

1. **Good style is the primary documentation.** Comments supplement, not replace, readable code.
2. **Comments should explain *why*, not *what*.** The code already says *what*.
3. **Write comments at the level of intent** — describe the purpose, not the mechanics.
4. **Don't document bad code — rewrite it.** If code is tricky enough to need explanation, it's bad code.
5. **Comments that repeat the code are worse than no comments at all.**
6. **Use styles that are easy to maintain.** If a commenting style is tedious, it won't be maintained.
7. **Integrate commenting into development.** Use the Pseudocode Programming Process — design in comments, then fill in code.
8. **~1 comment per 10 statements** is the empirically optimal density.
9. **Focus documentation effort on the code itself first** — make it so good it barely needs comments, then add comments to make it even better.
10. **The specific convention matters less than consistent application.**
