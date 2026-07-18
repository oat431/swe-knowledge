---
title: "Instructions: Language of the Computer"
source: "Patterson & Hennessy – Computer Organization and Design, 4th ed. (Revised)"
chapter: 2
tags:
  - isa
  - mips
  - arm
  - x86
  - computer-architecture
  - co-and-d
---

# 2 Instructions: Language of the Computer

> **Source:** *Computer Organization and Design: The Hardware/Software Interface* by David A. Patterson & John L. Hennessy (Morgan Kaufmann)

## 2.1 Introduction

The **instruction set** is the vocabulary of commands understood by a given architecture. Computer languages are quite similar across architectures — more like regional dialects than independent languages — because all computers are built from similar underlying hardware technologies and must provide the same basic operations.

### Stored-Program Concept

> **Big Picture:** Instructions and data of many types can be stored in memory as numbers, leading to the stored-program computer. This is the fundamental insight that makes general-purpose computing possible.

- Instructions are represented as numbers.
- Programs are stored in memory to be read or written, just like data.
- This allows a single machine to run accounting, word processing, or any other program simply by loading different programs into memory.

### Four Design Principles of Hardware

| # | Principle | Example |
|---|-----------|---------|
| 1 | **Simplicity favors regularity** | Fixed instruction length, always 3 operands |
| 2 | **Smaller is faster** | 32 registers rather than many more |
| 3 | **Make the common case fast** | Constants inside instructions (immediate) |
| 4 | **Good design demands good compromises** | Different instruction formats for different needs |

## 2.2 Operations of the Computer Hardware

MIPS arithmetic instructions each perform exactly one operation and always have exactly three operands.

### Arithmetic Instructions

| Instruction | Example | Meaning |
|-------------|---------|---------|
| `add` | `add $s1, $s2, $s3` | `$s1 = $s2 + $s3` |
| `sub` | `sub $s1, $s2, $s3` | `$s1 = $s2 - $s3` |
| `addi` | `addi $s1, $s2, 20` | `$s1 = $s2 + 20` |

**Compiling `f = (g + h) - (i + j)`:**

```mips
add $t0, $s1, $s2    # t0 = g + h
add $t1, $s3, $s4    # t1 = i + j
sub $s0, $t0, $t1    # f = t0 - t1
```

## 2.3 Operands of the Computer Hardware

### MIPS Operands

| Name | Example | Description |
|------|---------|-------------|
| **32 registers** | `$s0–$s7`, `$t0–$t9`, `$zero`, `$a0–$a3`, `$v0–$v1`, `$gp`, `$fp`, `$sp`, `$ra`, `$at` | Fast locations for data. `$zero` always = 0; `$at` reserved for assembler |
| **2³⁰ memory words** | `Memory[0]`, `Memory[4]`, … | Accessed only by data transfer instructions; byte addressed (sequential words differ by 4) |

### Register Conventions

- **Word** – the natural unit of access; 32 bits in MIPS.
- Registers are faster than memory, have higher throughput, and use less energy.
- Compilers keep the most frequently used variables in registers and **spill** the rest to memory.

### Data Transfer Instructions

| Instruction | Example | Meaning |
|-------------|---------|---------|
| `lw` | `lw $s1, 20($s2)` | `$s1 = Memory[$s2 + 20]` (load word) |
| `sw` | `sw $s1, 20($s2)` | `Memory[$s2 + 20] = $s1` (store word) |
| `lh` | `lh $s1, 20($s2)` | Load halfword (signed) |
| `lhu` | `lhu $s1, 20($s2)` | Load halfword (unsigned) |
| `sh` | `sh $s1, 20($s2)` | Store halfword |
| `lb` | `lb $s1, 20($s2)` | Load byte (signed) |
| `lbu` | `lbu $s1, 20($s2)` | Load byte (unsigned) |
| `sb` | `sb $s1, 20($s2)` | Store byte |
| `lui` | `lui $s1, 20` | `$s1 = 20 × 2¹⁶` (load upper immediate) |

### Byte Addressing & Alignment

- MIPS uses **byte addressing** — each byte has a unique address.
- Words must start at addresses that are multiples of 4 (**alignment restriction**).
- MIPS is **big-endian**: the leftmost byte is the word address.
- For array element `A[i]`, the byte offset is `4 × i`.

### Immediate (Constant) Operands

- `addi $s3, $s3, 4` adds the constant 4 directly without a memory load.
- Register `$zero` is hardwired to 0, simplifying many operations (e.g., `move` is just `add` with `$zero`).

## 2.4 Signed and Unsigned Numbers

### Number Representation

- **Binary (base 2)**: bits are numbered 0 (rightmost/LSB) to 31 (leftmost/MSB).
- A 32-bit word can represent 2³² different values.

### Two's Complement Representation

| Bit pattern | Unsigned value | Signed value |
|-------------|---------------|-------------|
| `0000...0000₂` | 0 | 0 |
| `0111...1111₂` | 2,147,483,647 | 2,147,483,647 |
| `1000...0000₂` | 2,147,483,648 | −2,147,483,648 |
| `1111...1111₂` | 4,294,967,295 | −1 |

**Signed value formula:**

$$(x_{31} \times -2^{31}) + (x_{30} \times 2^{30}) + \cdots + (x_1 \times 2^1) + (x_0 \times 2^0)$$

### Key Operations

| Operation | Method |
|-----------|--------|
| **Negate a number** | Invert every bit, then add 1 |
| **Sign extension** | Replicate the MSB (sign bit) to fill wider representation |

- **Overflow** occurs when the result cannot be represented in the available bits (the sign bit is incorrect).

### Other Representations (Less Common)

| Representation | Description |
|---------------|-------------|
| **Sign and magnitude** | Separate sign bit; has +0 and −0; abandoned early |
| **One's complement** | Invert all bits for negation; has +0 and −0 |
| **Biased notation** | Value = stored − bias; used in floating-point exponents |

## 2.5 Representing Instructions in the Computer

### Instruction Formats

All MIPS instructions are **32 bits** long.

#### R-type (Register)

```
| op (6) | rs (5) | rt (5) | rd (5) | shamt (5) | funct (6) |
```

- `op` = opcode (0 for R-type)
- `rs`, `rt` = source registers
- `rd` = destination register
- `shamt` = shift amount
- `funct` = function code (distinguishes add/sub/etc.)

#### I-type (Immediate)

```
| op (6) | rs (5) | rt (5) | address/constant (16) |
```

- Used for `addi`, `lw`, `sw`, `beq`, `bne`
- 16-bit field → constants from −32,768 to 32,767

#### J-type (Jump)

```
| op (6) | target address (26) |
```

### Register Numbering

| Name | Number | Name | Number |
|------|--------|------|--------|
| `$zero` | 0 | `$s0–$s7` | 16–23 |
| `$at` | 1 | `$t8–$t9` | 24–25 |
| `$v0–$v1` | 2–3 | `$k0–$k1` | 26–27 |
| `$a0–$a3` | 4–7 | `$gp` | 28 |
| `$t0–$t7` | 8–15 | `$sp` | 29 |
| | | `$fp` | 30 |
| | | `$ra` | 31 |

### Machine Language Encoding

| Instruction | Format | op | rs | rt | rd | shamt | funct |
|-------------|--------|----|----|----|-----|-------|-------|
| `add` | R | 0 | reg | reg | reg | 0 | 32 |
| `sub` | R | 0 | reg | reg | reg | 0 | 34 |
| `addi` | I | 8 | reg | reg | — | — | constant |
| `lw` | I | 35 | reg | reg | — | — | address |
| `sw` | I | 43 | reg | reg | — | — | address |

### Hexadecimal Conversion

- Base 16 is popular because each hex digit maps to exactly 4 binary digits.

## 2.6 Logical Operations

| Operation | C/Java | MIPS | Description |
|-----------|--------|------|-------------|
| Shift left | `<<` | `sll` | Move bits left, fill with 0s; `sll $t2, $s0, 4` |
| Shift right | `>>` / `>>>` | `srl` | Move bits right, fill with 0s |
| AND | `&` | `and`, `andi` | Bit-by-bit AND (mask operation) |
| OR | `\|` | `or`, `ori` | Bit-by-bit OR |
| NOT | `~` | `nor` (with `$zero`) | `A NOR 0 = NOT A` |

- **Shift left by i** is equivalent to multiplying by 2ⁱ.
- AND is commonly used with a **mask** to isolate specific bits.
- MIPS also supports `xor` (not shown in basic set).

## 2.7 Instructions for Making Decisions

### Conditional Branches

| Instruction | Example | Meaning |
|-------------|---------|---------|
| `beq` | `beq $s1, $s2, L1` | If `$s1 == $s2`, go to L1 |
| `bne` | `bne $s1, $s2, L1` | If `$s1 ≠ $s2`, go to L1 |

### Unconditional Jump

| Instruction | Example | Meaning |
|-------------|---------|---------|
| `j` | `j Exit` | Go to Exit (unconditional) |

### Comparison Instructions

| Instruction | Example | Meaning |
|-------------|---------|---------|
| `slt` | `slt $t0, $s3, $s4` | `$t0 = 1` if `$s3 < $s4` (signed) |
| `sltu` | `sltu $t0, $s3, $s4` | `$t0 = 1` if `$s3 < $s4` (unsigned) |
| `slti` | `slti $t0, $s2, 10` | `$t0 = 1` if `$s2 < 10` (signed) |
| `sltiu` | `sltiu $t0, $s2, 10` | `$t0 = 1` if `$s2 < 10` (unsigned) |

### Building All Conditions

MIPS uses `slt`, `slti`, `beq`, `bne`, and `$zero` to create all relational conditions:

| Condition | MIPS Implementation |
|-----------|-------------------|
| `==` | `beq` |
| `≠` | `bne` |
| `<` | `slt` + `bne` |
| `≤` | `slt` + `beq` (reverse operands) |
| `>` | `slt` + `bne` (reverse operands) |
| `≥` | `slt` + `beq` |

### Bounds Check Shortcut

```
sltu $t0, $s1, $t2       # $t0 = 0 if $s1 >= length or $s1 < 0
beq $t0, $zero, Error    # if bad, goto Error
```

- Treating signed as unsigned gives a low-cost way to check `0 ≤ x < y`.

### Basic Block

A **basic block** is a sequence of instructions without branches (except possibly at the end) and without branch targets (except possibly at the beginning).

## 2.8 Supporting Procedures in Computer Hardware

### Six Steps for Procedure Execution

1. Put parameters in a place where the procedure can access them (`$a0–$a3`)
2. Transfer control to the procedure (`jal`)
3. Acquire storage resources (save registers on stack)
4. Perform the desired task
5. Put the result where the caller can access it (`$v0–$v1`)
6. Return control to the caller (`jr $ra`)

### Key Instructions

| Instruction | Example | Meaning |
|-------------|---------|---------|
| `jal` | `jal ProcedureAddress` | `$ra = PC + 4; go to ProcedureAddress` |
| `jr` | `jr $ra` | Go to address in `$ra` |

### Register Usage Convention

| Registers | Preserved on call? | Usage |
|-----------|-------------------|-------|
| `$s0–$s7` | **Yes** (callee saves) | Saved registers |
| `$t0–$t9` | **No** (caller saves if needed) | Temporary registers |
| `$a0–$a3` | **No** | Argument registers |
| `$v0–$v1` | **No** | Return value registers |
| `$ra` | **Yes** | Return address |
| `$sp` | **Yes** | Stack pointer |
| `$fp` | **Yes** | Frame pointer |
| `$gp` | **Yes** | Global pointer |

### Stack

- **Stack** – last-in-first-out data structure for spilling registers.
- Stack pointer (`$sp`) points to the most recently allocated address.
- Stacks **grow from high to low addresses** in MIPS.
- **Push**: subtract from `$sp`, then `sw`.
- **Pop**: `lw`, then add to `$sp`.

### Leaf vs Non-Leaf Procedures

- **Leaf procedure** – does not call other procedures; simpler register management.
- **Non-leaf procedure** – calls other procedures; must save `$ra` and any needed `$s` and `$a` registers.

### Procedure Frame (Activation Record)

The segment of the stack containing a procedure's saved registers and local variables. A **frame pointer** (`$fp`) can provide a stable base for memory references within a procedure.

### Memory Layout Convention

```
High address  0x7ffffffc  Stack (grows ↓)
                           ↓
              0x10008000  $gp (global pointer)
              0x10000000  Static data
                           ↑
              0x00400000  Text (code)
Low address   0x00000000  Reserved
```

## 2.9 Communicating with People

### ASCII

- 8-bit character encoding; nearly universal for English text.
- Load/store byte instructions (`lb`, `lbu`, `sb`) handle character data.
- **Strings in C**: terminated by null byte (0); variable length.
- **Strings in Java**: include length field; use Unicode (16-bit characters).

### Load/Store for Characters

| Instruction | Example | Meaning |
|-------------|---------|---------|
| `lb` | `lb $t0, 0($sp)` | Load byte (sign-extended) |
| `lbu` | `lbu $t0, 0($sp)` | Load byte (unsigned, zero-extended) |
| `sb` | `sb $t0, 0($gp)` | Store byte |
| `lh` | `lh $t0, 0($sp)` | Load halfword (signed) |
| `lhu` | `lhu $t0, 0($sp)` | Load halfword (unsigned) |
| `sh` | `sh $t0, 0($sp)` | Store halfword |

## 2.10 MIPS Addressing for 32-Bit Immediates and Addresses

### Loading 32-Bit Constants

Use `lui` (load upper immediate) followed by `ori`:

```mips
lui $s0, 61         # upper 16 bits
ori $s0, $s0, 2304  # lower 16 bits
```

### Addressing Modes

| Mode | Description | Operand Location |
|------|-------------|-----------------|
| **1. Immediate** | Operand is constant in the instruction | Instruction |
| **2. Register** | Operand is in a register | Register |
| **3. Base (displacement)** | Address = register + constant | Memory |
| **4. PC-relative** | Branch address = PC + constant | Memory (instruction) |
| **5. Pseudodirect** | Jump address = 26 bits concatenated with upper PC bits | Memory (instruction) |

### PC-Relative Addressing

- Conditional branches use PC-relative addressing.
- 16-bit field → range of ±2¹⁵ words from PC + 4.
- Most branches are nearby (half go < 16 instructions).

### Jump Addressing

- `j` and `jal` use 26-bit address (J-format).
- Actual address = upper 4 bits of PC concatenated with 26-bit field shifted left 2 bits → 28-bit byte address.
- Range: within a 256 MB block.

### Branching Far Away

The assembler can convert a short branch into a conditional skip + unconditional jump:

```mips
# Original:  beq $s0, $s1, L1
# Becomes:
bne $s0, $s1, L2
j L1
L2:
```

## 2.11 Parallelism and Instructions: Synchronization

### The Problem

Parallel tasks need to coordinate when reading/writing shared data. Without synchronization, **data races** produce non-deterministic results.

### Hardware Primitives

| Instruction | Meaning |
|-------------|---------|
| `ll` (load linked) | Load word as 1st half of atomic swap |
| `sc` (store conditional) | Store word as 2nd half; sets register to 1 on success, 0 on failure |

### Atomic Exchange Pattern

```mips
try:  add  $t0, $zero, $s4    # copy exchange value
      ll   $t1, 0($s1)        # load linked
      sc   $t0, 0($s1)        # store conditional
      beq  $t0, $zero, try    # branch if sc failed
      add  $s4, $zero, $t1    # put load value in $s4
```

- If another processor modifies the location between `ll` and `sc`, the `sc` fails (returns 0) and the code retries.
- Can build **locks**, **mutexes**, and more complex synchronization primitives.

## 2.12 Translating and Starting a Program

### Translation Hierarchy

```
C program
    ↓  Compiler
Assembly language (.s)
    ↓  Assembler
Object module (.o)
    ↓  Linker (+ libraries)
Executable (a.out)
    ↓  Loader
Running program in memory
```

### Compiler

Transforms high-level language → assembly language. Optimizing compilers today produce code nearly as good as expert assembly programmers.

### Assembler

- Translates assembly → machine code.
- Creates **pseudoinstructions** (e.g., `move $t0, $t1` → `add $t0, $zero, $t1`).
- Maintains a **symbol table** mapping labels to addresses.
- Object file contains: header, text segment, static data, relocation info, symbol table, debug info.

### Linker

Combines independently assembled modules:

1. Place code and data symbolically in memory.
2. Determine addresses of labels.
3. Patch internal and external references.

Produces an **executable file** with no unresolved references.

### Loader

1. Reads executable header to determine segment sizes.
2. Creates address space.
3. Copies instructions and data into memory.
4. Sets up stack pointer, registers.
5. Jumps to start-up routine → calls `main`.

### Dynamically Linked Libraries (DLLs)

- Linked at runtime, not at build time.
- **Lazy procedure linkage**: each library routine is linked only on first call.
- Saves memory (don't load unused routines) and allows library updates without recompilation.

### Java Translation

```
Java source
    ↓  Compiler
Java bytecodes (.class)
    ↓  JVM (interpreter)
Running program
    ↓  JIT compiler (optional)
Native machine code
```

- **JVM** (Java Virtual Machine): interprets bytecodes; portable.
- **JIT** (Just In Time) compiler: compiles hot methods to native code at runtime; improves performance over time.

## 2.13 A C Sort Example to Put It All Together

### Procedure Translation Steps

1. **Allocate registers** to program variables
2. **Produce code** for the procedure body
3. **Preserve registers** across the procedure invocation

### Key Patterns

| Pattern | MIPS Approach |
|---------|--------------|
| Array indexing | Shift left by 2 (`sll`) to convert index to byte offset |
| Array access | Add offset to base address, then `lw`/`sw` |
| Loop test | `slt` + `beq`/`bne` |
| Procedure call | `jal`; save/restore registers on stack |

### Compiler Optimization Impact

| Optimization | Speedup | Instruction Count | CPI |
|-------------|---------|-------------------|-----|
| None | 1.00× | 114.9M | 1.38 |
| O1 (medium) | 2.37× | 37.5M | 1.79 |
| O2 (full) | 2.38× | 40.0M | 1.66 |
| O3 (inlining) | 2.41× | 45.0M | 1.46 |

> **Understanding Program Performance:** Execution time is the only valid measure. O3 is fastest despite higher instruction count because it reduces CPI through inlining.

## 2.14 Arrays versus Pointers

### Array Index Version vs Pointer Version

| Aspect | Array (`clear1`) | Pointer (`clear2`) |
|--------|-----------------|-------------------|
| Address calculation | Inside loop (recompute each iteration) | Outside loop (increment pointer) |
| Instructions per iteration | 6 | 4 (optimized) |
| Key optimization | — | **Strength reduction** + **induction variable elimination** |

- Modern optimizing compilers produce equivalent code for both styles.
- Pointer arithmetic accounts for the size of the pointed-to object.

## 2.16 Real Stuff: ARM Instructions

### ARM vs MIPS Comparison

| Feature | ARM | MIPS |
|---------|-----|------|
| Year announced | 1985 | 1985 |
| Instruction size | 32 bits | 32 bits |
| Address space | 32 bits, flat | 32 bits, flat |
| Data addressing modes | 9 | 3 |
| Integer registers | 15 GPR × 32 bits | 31 GPR × 32 bits |
| `$zero` register | No (separate opcodes needed) | Yes |
| Conditional execution | All instructions (4-bit condition field) | Separate branch instructions |
| Shift as part of data ops | Yes | No (separate shift instructions) |
| Block load/store | Yes (16-bit mask) | No |
| Multiply | `mul` | `mult`/`multu` |
| Divide | — | `div`/`divu` |

### ARM Unique Features

- **Every instruction is conditional** (4-bit condition code prefix).
- **Barrel shifter**: second register operand can be shifted before operation.
- **Block load/store** (`LDM`/`STM`): save/restore multiple registers in one instruction.
- **12-bit rotated immediate**: 8-bit value rotated right by 2×top4 bits; captures all powers of 2.

### ARM Condition Codes

| Code | Bits Set By |
|------|------------|
| N (negative) | Result MSB |
| Z (zero) | Result = 0 |
| C (carry) | Carry out |
| V (overflow) | Signed overflow |

- `CMP` subtracts and sets codes; `CMN` adds; `TST` ANDs; `TEQ` XORs.

## 2.17 Real Stuff: x86 Instructions

### Evolution Timeline

| Year | Milestone | Key Addition |
|------|-----------|-------------|
| 1978 | 8086 | 16-bit architecture; assembly-compatible extension of 8080 |
| 1980 | 8087 | Floating-point coprocessor (~60 FP instructions) |
| 1982 | 80286 | 24-bit address space; memory protection |
| 1985 | 80386 | 32-bit architecture; 32-bit registers and addressing |
| 1989–95 | 80486 / Pentium / Pentium Pro | Performance improvements; 4 new instructions |
| 1997 | MMX | 57 multimedia instructions (SIMD) |
| 1999 | SSE | 8 × 128-bit registers; 70 new instructions |
| 2001 | SSE2 | Double-precision FP; 144 new instructions |
| 2003 | AMD64 | 64-bit addressing; 16 GPRs; 16 SSE registers |
| 2004 | SSE3 | 13 instructions (Intel adopts AMD64) |
| 2006 | SSE4 | 54 instructions |
| 2008 | AVX | 256-bit SSE registers; 128 new instructions |

### x86 Registers (80386)

| Register | Use |
|----------|-----|
| `EAX–EDI` | 8 general-purpose registers (32-bit) |
| `EIP` | Instruction pointer (PC) |
| `EFLAGS` | Condition codes |
| `CS–GS` | Segment registers |

### x86 vs RISC (MIPS/ARM)

| Feature | x86 | MIPS/ARM |
|---------|-----|----------|
| GPR count | 8 | 32 / 16 |
| Instruction format | Variable (1–15 bytes) | Fixed (4 bytes) |
| Memory operands | Yes (in most instructions) | No (load/store only) |
| Addressing modes | Many (register indirect, based, scaled index, autoincrement) | 3 / 9 |
| Condition codes | Yes (EFLAGS) | No / Yes (ARM) |
| Operand structure | Two-operand (dest = source) | Three-operand |

### x86 Instruction Encoding

- Opcode byte may include addressing mode and register.
- **Postbyte** ("mod, reg, r/m"): specifies addressing mode for memory operands.
- **Scaled index mode** second postbyte: "sc, index, base."
- Instructions range from 1 to 15 bytes.

### x86 Strengths and Weaknesses

- **Weakness**: Complex encoding, variable-length instructions, limited registers, two-operand format.
- **Strength**: Backward compatibility; most frequently used components are not hard to implement fast; massive software ecosystem.

## 2.18 Fallacies and Pitfalls

### Fallacies

| Fallacy | Reality |
|---------|---------|
| **More powerful instructions → higher performance** | Complex instructions (e.g., `REP MOVSB`) are often slower than simpler sequences using registers |
| **Write in assembly for highest performance** | Modern compilers produce code competitive with hand-written assembly; portability and maintainability matter |
| **Successful ISAs don't change** | x86 has grown from ~100 to >1000 instructions over 30 years |

### Pitfalls

| Pitfall | Explanation |
|---------|-------------|
| **Word address ≠ byte address + 1** | Sequential word addresses differ by 4 (byte addressing) |
| **Pointer to automatic variable outside its scope** | Local variables are deallocated when procedure returns; dangling pointers cause chaos |

## 2.19 Concluding Remarks

### Instruction Categories and Frequency (SPEC CPU2006)

| Class | Examples | HLL Correspondence | Integer Freq. | FP Freq. |
|-------|----------|-------------------|---------------|----------|
| Arithmetic | `add`, `sub`, `addi` | Assignment statements | 16% | 48% |
| Data transfer | `lw`, `sw`, `lb`, `lbu`, `lui` | Data structure references | 35% | 36% |
| Logical | `and`, `or`, `nor`, `sll`, `srl` | Assignment statements | 12% | 4% |
| Conditional branch | `beq`, `bne`, `slt`, `slti` | If statements and loops | 34% | 8% |
| Jump | `j`, `jr`, `jal` | Procedure calls/returns | 2% | 0% |

### MIPS Instructions Covered in This Chapter

**Real Instructions:**

| Category | Instructions |
|----------|-------------|
| Arithmetic | `add`, `sub`, `addi` |
| Data transfer | `lw`, `sw`, `lh`, `lhu`, `sh`, `lb`, `lbu`, `sb`, `ll`, `sc`, `lui` |
| Logical | `and`, `or`, `nor`, `andi`, `ori`, `sll`, `srl` |
| Branch | `beq`, `bne`, `slt`, `sltu`, `slti`, `sltiu` |
| Jump | `j`, `jr`, `jal` |

**Pseudoinstructions:**

| Instruction | Meaning |
|-------------|---------|
| `move $t0, $t1` | `add $t0, $zero, $t1` |
| `li $t0, imm` | Load immediate |
| `blt`, `ble`, `bgt`, `bge` | Branch on comparison |

### The Big Picture

> **Execution time** is the only valid and unimpeachable measure of performance. The stored-program concept — instructions as numbers, programs in memory — is the foundation of all modern computing.

## Related

- [[01_Computer_Abstractions]] — Previous chapter on abstractions and performance
- [[03_Computer_Arithmetic]] — Next chapter on integer and floating-point arithmetic
- [[Computer Organization Overview]] — Full chapter index
- [[Computing Foundation Overview]] — All computing foundation topics
