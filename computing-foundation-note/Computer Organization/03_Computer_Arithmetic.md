---
tags: [arithmetic, floating-point, ieee-754, computer-architecture, co-and-d]
---

# 03 — Computer Arithmetic

> **Source:** *Computer Organization and Design* — Patterson & Hennessy, Chapter 3

## Overview

Chapter 3 covers how computers perform arithmetic: integer addition/subtraction with overflow detection, multiplication and division algorithms, and the IEEE 754 floating-point standard. The finite word size of computers means arithmetic can produce results that don't fit — understanding how hardware handles this is essential.

---

## 1 Addition and Subtraction

Binary addition works digit-by-digit right to left with carries, just like decimal. Subtraction is performed by negating the second operand (two's complement) and adding.

### Overflow Detection

Overflow occurs when the result cannot fit in the word size.

| Operation | Operand A | Operand B | Overflow when result |
|---|---|---|---|
| A + B | ≥ 0 | ≥ 0 | < 0 |
| A + B | < 0 | < 0 | ≥ 0 |
| A – B | ≥ 0 | < 0 | < 0 |
| A – B | < 0 | ≥ 0 | ≥ 0 |

**Key insight:** Adding operands of different signs → overflow impossible. Subtracting operands of same sign → overflow impossible.

### MIPS Overflow Handling

- `add`, `addi`, `sub` — trap on overflow
- `addu`, `addiu`, `subu` — ignore overflow (used by C compilers)

Overflow is detected via an **exception** (also called **interrupt**) — an unscheduled procedure call. The faulting instruction address is saved in the **EPC** (Exception Program Counter), and the CPU jumps to a predefined handler address.

### SIMD / Multimedia Arithmetic

Partitioning carry chains in a 64-bit adder allows parallel operations on vectors of eight 8-bit, four 16-bit, or two 32-bit operands. **Saturating operations** clamp results to max/min instead of wrapping around — useful for audio/video.

---

## 2 Multiplication

### Basic Algorithm (Paper-and-Pencil)

For each bit of the multiplier (right to left):
1. If multiplier bit = 1 → add multiplicand to product
2. Shift multiplicand left 1 bit
3. Shift multiplier right 1 bit

Repeat for all 32 bits. An n-bit × m-bit product needs **n + m bits**.

### Hardware Evolution

| Version | Key Idea |
|---|---|
| First (Fig 3.4) | 64-bit Multiplicand register shifts left; 64-bit Product register accumulates |
| Refined (Fig 3.6) | 32-bit ALU; Product shifts right; multiplier stored in right half of Product register |
| Fast (Fig 3.8) | 31 adders in parallel tree → log₂(32) = 5 add delays instead of 32 |

### Signed Multiplication

Works with the same algorithm — just extend the sign bit during shifts.

### MIPS Multiply

- `mult $s2, $s3` → 64-bit signed product in **Hi, Lo**
- `multu $s2, $s3` → 64-bit unsigned product
- `mflo $s1` → copy Lo (low 32 bits of product)
- `mfhi $s1` → copy Hi (high 32 bits; 0 if no overflow for unsigned)

**Compiler trick:** Multiplication by powers of 2 is replaced with left shifts (strength reduction).

---

## 3 Division

### Basic Algorithm (Restoring Division)

1. Subtract divisor from remainder
2. If remainder ≥ 0 → shift quotient left, set LSB = 1 (step 2a)
3. If remainder < 0 → restore by adding divisor back, shift quotient left, set LSB = 0 (step 2b)
4. Shift divisor right 1 bit
5. Repeat n+1 times

### Hardware

Similar to multiply — a 64-bit Remainder register, 32-bit Divisor register, and 32-bit Quotient register. The refined version halves register/ALU widths by shifting the remainder left.

### Signed Division

- Negate quotient if operand signs differ
- Sign of remainder matches the sign of the dividend
- Ensures: `Dividend = Quotient × Divisor + Remainder`

### MIPS Divide

- `div $s2, $s3` → Lo = quotient, Hi = remainder (signed)
- `divu $s2, $s3` → unsigned version

Hi and Lo registers are shared between multiply and divide.

### Faster Division

- **SRT division:** Guesses several quotient bits per step using a lookup table; corrects wrong guesses in subsequent steps (typically 4 bits/step)
- **Nonrestoring division:** Doesn't add divisor back immediately; saves cycles
- **Nonperforming division:** Skips subtraction when result would be negative; averages ⅓ fewer operations

---

## 4 Floating Point

### Why Floating Point?

Integers can't represent fractions or very large/small numbers. Scientific notation solves this:

$$(-1)^S \times F \times 2^E$$

### IEEE 754 Formats

#### Single Precision (32-bit)

```
| 1 bit sign | 8 bits exponent | 23 bits fraction |
```

- **Bias:** 127
- **Value:** $(-1)^S \times (1 + \text{Fraction}) \times 2^{(\text{Exponent} - 127)}$
- **Hidden 1:** Normalized numbers have an implicit leading 1
- **Range:** ±1.0 × 2⁻¹²⁶ to ±1.111... × 2¹²⁷

#### Double Precision (64-bit)

```
| 1 bit sign | 11 bits exponent | 52 bits fraction |
```

- **Bias:** 1023
- **Range:** ±1.0 × 2⁻¹⁰²² to ±1.111... × 2¹⁰²³

#### Special Values (IEEE 754)

| Exponent | Fraction | Represents |
|---|---|---|
| 0 | 0 | ±0 |
| 0 | Nonzero | ±denormalized number |
| 1–254 (single) / 1–2046 (double) | Anything | ±floating-point number |
| 255 (single) / 2047 (double) | 0 | ±infinity |
| 255 (single) / 2047 (double) | Nonzero | NaN (Not a Number) |

### Biased Notation

Exponents use biased representation so that the most negative exponent is 00...00 and the most positive is 11...11, enabling integer comparison for sorting floating-point numbers.

---

## 5 Floating-Point Addition Algorithm

1. **Align** exponents — shift the smaller significand right until exponents match
2. **Add** significands
3. **Normalize** the sum — shift left/right, adjust exponent, check overflow/underflow
4. **Round** to fit significand width — if result denormalizes, repeat step 3

## 6 Floating-Point Multiplication Algorithm

1. **Add** biased exponents, subtract one bias
2. **Multiply** significands
3. **Normalize** if necessary, check overflow/underflow
4. **Round** — if result denormalizes, re-normalize
5. **Set sign:** XOR of operand signs

---

## 7 Rounding and Accuracy

### Guard, Round, and Sticky Bits

- **Guard bit:** First extra bit kept during intermediate calculations
- **Round bit:** Second extra bit
- **Sticky bit:** Set if any nonzero bits were shifted off (OR of all bits shifted out)

These bits enable IEEE 754 to round as if intermediate results had infinite precision.

### Rounding Modes (IEEE 754)

| Mode | Description |
|---|---|
| Round toward +∞ | Always round up |
| Round toward –∞ | Always round down |
| Truncate | Round toward zero |
| **Round to nearest even** | Default; ties round to make LSB = 0 |

**Accuracy guarantee:** Result is within 0.5 ULP (units in last place) of the true value.

### Non-Associativity

Floating-point addition is **not associative**: $(x + y) + z \neq x + (y + z)$ in general. This matters for parallel programs — sequential and parallel versions may produce different results.

---

## 8 MIPS Floating-Point Instructions

| Category | Instruction | Example | Meaning |
|---|---|---|---|
| FP add (single) | `add.s` | `add.s $f2,$f4,$f6` | $f2 = $f4 + $f6 |
| FP sub (single) | `sub.s` | `sub.s $f2,$f4,$f6` | $f2 = $f4 – $f6 |
| FP mul (single) | `mul.s` | `mul.s $f2,$f4,$f6` | $f2 = $f4 × $f6 |
| FP div (single) | `div.s` | `div.s $f2,$f4,$f6` | $f2 = $f4 / $f6 |
| FP add (double) | `add.d` | `add.d $f2,$f4,$f6` | $f2 = $f4 + $f6 |
| FP sub (double) | `sub.d` | `sub.d $f2,$f4,$f6` | $f2 = $f4 – $f6 |
| FP mul (double) | `mul.d` | `mul.d $f2,$f4,$f6` | $f2 = $f4 × $f6 |
| FP div (double) | `div.d` | `div.d $f2,$f4,$f6` | $f2 = $f4 / $f6 |
| Load FP | `lwc1` | `lwc1 $f1,100($s2)` | Load 32-bit to FP register |
| Store FP | `swc1` | `swc1 $f1,100($s2)` | Store 32-bit from FP register |
| FP compare | `c.lt.s` | `c.lt.s $f2,$f4` | Set condition if $f2 < $f4 |
| Branch on true | `bc1t` | `bc1t 25` | Branch if FP condition true |
| Branch on false | `bc1f` | `bc1f 25` | Branch if FP condition false |

- **32 FP registers:** $f0–$f31; double precision uses even-odd pairs (e.g., $f2/$f3 = double $f2)
- Separate register set → twice the register bandwidth, customized for FP

---

## 9 Key Concepts Summary

| Concept | Detail |
|---|---|
| **Overflow** | Result too large for word size; detected by sign bit contradiction |
| **Two's complement overflow** | Same signs add → different sign result = overflow |
| **Multiplication** | Shift-and-add; product needs n+m bits |
| **Division** | Restoring algorithm; SRT for speed |
| **IEEE 754** | Standard floating-point format; hidden 1, biased exponent |
| **Guard/Round/Sticky** | Extra bits for accurate rounding |
| **Denormalized numbers** | Fill gap between 0 and smallest normalized number (gradual underflow) |
| **NaN / ∞** | Special encodings for invalid ops and divide-by-zero |

---

## Related

- [[02_Instruction_Set_Architecture]] — MIPS integer instructions that arithmetic builds on
- [[04_Processor_Design]] — How the ALU datapath implements these operations
- [[Computer Organization Overview]]
