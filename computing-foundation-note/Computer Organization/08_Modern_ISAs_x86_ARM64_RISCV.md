---
title: "Modern ISAs: x86-64, ARM64, RISC-V"
source: "Hennessy & Patterson supplement, the MIPS-era vault meets the 2020s silicon"
chapter: 8
tags:
  - computer-architecture
  - computer-organization
  - co-and-d
  - x86-64
  - arm64
  - risc-v
  - isa
  - calling-convention
  - memory-ordering
---

# 08 Modern ISAs: x86-64, ARM64, RISC-V

> **Why this note exists:** the rest of this vault follows Patterson & Hennessy's *Computer Organization and Design* (MIPS edition). MIPS is a superb teaching vehicle but a dead commercial ISA. The machines you actually operate (cloud VMs, laptops, Kubernetes nodes, CI runners, your phone) are overwhelmingly **x86-64** and **ARM64**, with **RISC-V** arriving fast as a third pillar. This note is a **translation layer, not a replacement:** every concept from [[02_Instruction_Set_Architecture]], [[04_Processor_Design]], and [[05_Memory_Hierarchy]] still holds; only the vocabulary changes.

---

## 8.1 The CISC/RISC Line Blurred

H&P Chapter 2 presented the 1980s debate: RISC (simple, fixed-length, load/store (MIPS) vs CISC (rich, variable-length, memory operands) x86, VAX). The resolution was not a winner but a **convergence:**

- **Modern x86 decodes CISC instructions into internal RISC-like micro-ops** (µops). A legacy `add eax, [rbx+8]` is cracked into a load µop and an add µop. The pipeline that executes them looks much like the MIPS datapath from [[04_Processor_Design]]: register-to-register, fixed-size internal operations, out-of-order scheduling.
- The legacy CISC *encoding* survives only as a compatibility skin: a compact variable-length format that a dedicated decoder front-end translates.
- Meanwhile "RISC" ISAs added complex instructions back (fused multiply-add, crypto extensions, bit manipulation), because code density and common-case latency matter.

> **Lesson:** ISA style (CISC vs RISC) turned out to be a *syntax* question; performance is decided by the *implementation* (pipelining, caches, speculation, Chapters 4–5 of this vault).

---

## 8.2 x86-64 (AMD64)

### History: 16 → 32 → 64

| Year | Step | Notes |
|------|------|-------|
| 1978 | 8086: 16-bit | The origin of the backward-compatibility burden |
| 1985 | 80386: 32-bit (IA-32) | Protected mode, flat memory; still the mode most OS code conceptually targets |
| 2000 | **AMD designs the 64-bit extension (AMD64/x86-64)** | Intel's own clean-sheet 64-bit ISA (Itanium/IA-64) failed; Intel licensed AMD64 |
| 2003 | AMD Opteron / Athlon 64 ship | x86-64 becomes the server + desktop standard |

### Register file expansion

| Class | IA-32 | x86-64 | With SIMD |
|-------|-------|--------|-----------|
| General purpose | 8 (eax…edi) | **16** (rax…r15) | N/A |
| Vector | N/A | 16 × XMM (128-bit) | **32 × ZMM with AVX-512** (512-bit) |
| Matrix | N/A | N/A | AMX tile registers (Sapphire Rapids+) for AI inference |

The 8→16 GPR expansion reduced spills to memory; H&P's "compilers keep variables in registers" principle paying off decades later.

### Costs of the design

- **Variable-length encoding (1–15 bytes per instruction).** Saves code size, but the front-end must *decode to find instruction boundaries*; inherently serial, power-hungry. AMD and Intel both added decoded-µop caches to skip re-decoding hot code.
- **Backward compatibility burden.** Decades of legacy modes (real mode, 16-bit operands, prefix soup) live in every chip. This is exactly the "good design demands good compromises" principle from [[02_Instruction_Set_Architecture]] taken to an extreme.

### Who uses it

Servers and desktops: the overwhelming majority of cloud compute (AWS/Azure/GCP x86 instances), virtually all Windows/Linux desktops outside Apple Silicon, game consoles (PS4/PS5/Xbox), and the entire legacy enterprise estate.

---

## 8.3 ARM64 (AArch64)

### A clean 64-bit redesign

ARMv8-A (2011) introduced **AArch64**, a *new* execution state, deliberately **not** backward-compatible with 32-bit ARM, so the ISA could be regularized the way H&P teaches:

- **31 general-purpose 64-bit registers** (x0–x30; x31 is `sp` or the zero register `xzr` depending on encoding) + a dedicated PC that is *not* a GPR.
- **Fixed 32-bit instruction encoding:** every instruction is exactly 4 bytes. Fetch and decode become trivial (compare the x86 pain above).
- Load/store architecture with rich addressing modes; 32 × 128-bit SIMD/FP registers (v0–v31).

### Why it won mobile and is winning servers

ARM's power-efficiency roots are in battery-powered devices: simpler decode, fewer transistors per instruction retired, and a licensing model that encouraged many competing implementations. The result:

| Implementation | Domain |
|----------------|--------|
| **Apple Silicon** (M1–M4, 2020+) | Desktop/laptop; huge wide OoO cores proved ARM64 can beat x86 perf/watt at any power point |
| **AWS Graviton** (Neoverse-based, gen 1–4) | Cloud general-purpose compute, often ~20–40% better price-performance for scale-out workloads |
| **Ampere Altra/AmpereOne** | Up to 128+ simple, deterministic cores per socket for cloud |
| **Qualcomm Snapdragon X Elite** | Windows-on-ARM laptops |

### ARMv8 → ARMv9, one line each

- **SVE2** (Scalable Vector Extension 2): vector-length-agnostic SIMD: write the loop once, hardware runs it at 128/256/…-bit widths.
- **Confidential Compute Architecture (CCA):** hardware-enforced "Realems"; encrypted, attested execution environments the hypervisor itself cannot read (ARM's answer to Intel TDX / AMD SEV-SNP).

### Licensing model

ARM Holdings designs the cores (Cortex-A/X, Neoverse) and **licenses** them: companies either take ready-made cores (most phone vendors) or an **architecture license** to design their own cores implementing A64 (Apple, historically Qualcomm). ARM does not fabricate; TSMC/Samsung do.

---

## 8.4 RISC-V

### Open ISA

RISC-V (UC Berkeley, 2010; foundation 2015) is a royalty-free, openly documented ISA (the specification is a set of Creative Commons / BSD-licensed documents. **No license fees, no permission needed to build a compatible chip.** For H&P readers: Patterson himself co-created it, and the newest edition of *Computer Organization and Design* has a RISC-V version) it *is* the modern MIPS, pedagogically.

### Modularity: base + extensions

| Piece | Meaning |
|-------|---------|
| **RV32I / RV64I** | Minimal base integer ISA (32-bit / 64-bit) ;  ~40 instructions |
| **M** | Multiply/divide |
| **A** | Atomics (load-reserved/store-conditional + AMOs) |
| **F / D** | Single / double precision float |
| **C** | Compressed 16-bit encodings for code density |
| **V** | Vector extension (scalable, like SVE2) |
| **H** | Hypervisor extension for virtualization |

"RV64GC" = RV64I + M + A + F + D + C ("general-purpose"). You implement only what you need, a microcontroller can be RV32EC.

### The tension: fragmentation vs freedom

- **Risk:** unlike x86/ARM there is no single gatekeeper enforcing one profile (vendors could ship incompatible extension soups (the "EISA problem"). Mitigation: the **RVA profiles** (e.g. RVA22, RVA23)) ratified bundles that app-ecosystem OSes target, analogous to ARM's SBSA.
- **Freedom:** domain-specific extensions (crypto, DSP, custom accelerators) can be added without negotiation; precisely the "make the common case fast" design principle applied per-vendor.

### Adoption (2024–2026 momentum)

- **Espressif ESP32-C3/C6:** RISC-V in cheap Wi-Fi/BLE microcontrollers shipping in the tens of millions.
- **SiFive:** commercial core IP (U-series MCUs, P-series application processors), the "ARM-style licensee" model for RISC-V.
- **Western Digital:** billions of RISC-V cores inside disk/SSD controllers (compute-in-storage).
- **China:** heavy state and industry investment (Alibaba T-Head XuanTie, SiEngine, SpacemiT) driven by sanctions-resilience, a geopolitical accelerant.
- **High-performance push:** SiFive P870, Ventana Veyron chiplets, NVIDIA's internal use, Qualcomm/Google/Intel co-founding Quintauris (2023) for automotive-grade RISC-V. RISC-V is now credibly the **third pillar** alongside x86-64 and ARM64.

---

## 8.5 ISA Comparison Table

| | **x86-64** | **ARM64 (AArch64)** | **RISC-V (RV64GC)** |
|---|---|---|---|
| Encoding | Variable, 1–15 bytes | **Fixed 32-bit** | Base 32-bit + optional 16-bit (C ext.) |
| GPRs | 16 × 64-bit | 31 × 64-bit + xzr | 32 × 64-bit + zero (x0) |
| Philosophy | CISC skin over RISC µop core; compatibility-first | Clean RISC, load/store, designed for perf/watt | Minimal open RISC, modular extensions |
| License | Proprietary (Intel + AMD cross-license; effectively closed duopoly) | Licensed (royalty + architecture licenses) | **Open, royalty-free** |
| Ecosystem | Mature-everything (OSes, compilers, binaries) | Mobile-dominant → server rising (Graviton, Apple) | MCU/embedded strong; app-ecosystem growing via RVA profiles |
| Flagship implementations | Intel Core/Xeon, AMD Ryzen/EPYC | Apple M-series, Cortex-X/A/Neoverse, Graviton | SiFive P870, XuanTie C910/C920, ESP32-C3 (RV32) |
| Memory model | **Strong: TSO** | Weak (relaxed), acquire/release | Weak (RVWMO), acquire/release |

---

## 8.6 Same C, Three Assemblies

```c
// sum the first n elements of an array
long sum(long *a, long n) {
    long s = 0;
    for (long i = 0; i < n; i++) s += a[i];
    return s;
}
```

**x86-64 (AT&T syntax, System V AMD64 ABI)**

```asm
sum:
    xorl  %eax, %eax        # s = 0   (return value lives in rax)
    xorl  %ecx, %ecx        # i = 0
.Lloop:
    cmpq  %rsi, %rcx        # compare i with n   (n arrived in rsi)
    jge   .Ldone
    addq  (%rdi,%rcx,8), %rax   # s += a[i] - memory operand + scaled index
    incq  %rcx
    jmp   .Lloop
.Ldone:
    ret
```

**ARM64 (AAPCS64)**

```asm
sum:
    mov   x2, #0            // s = 0 (return value in x0)
    mov   x3, #0            // i = 0
.Lloop:
    cmp   x3, x1            // compare i, n      (n arrived in x1)
    b.ge  .Ldone
    ldr   x4, [x0, x3, lsl #3]  // x4 = a[i]    (a arrived in x0)
    add   x2, x2, x4        // s += x4
    add   x3, x3, #1
    b     .Lloop
.Ldone:
    mov   x0, x2            // return s in x0
    ret
```

**RISC-V (RV64)**

```asm
sum:
    li    t0, 0             # s = 0
    li    t1, 0             # i = 0
.Lloop:
    bge   t1, a1, .Ldone    # if i >= n, exit    (n arrived in a1)
    slli  t2, t1, 3         # byte offset = i*8
    add   t2, a0, t2        # &a[i]              (a arrived in a0)
    ld    t2, 0(t2)         # t2 = a[i]
    add   t0, t0, t2        # s += a[i]
    addi  t1, t1, 1
    j     .Lloop
.Ldone:
    mv    a0, t0            # return s in a0
    ret
```

**What the snippets show**

- **Addressing:** x86 folds load+scale into one memory operand (`(%rdi,%rcx,8)`); ARM64 uses register+shifted-register addressing (`[x0, x3, lsl #3]`); RISC-V base ISA is deliberately spartan (shift, add, then `ld`) with all three being pure load/store architectures at heart.
- **Condition codes:** x86 and ARM64 set flags via `cmp`; RISC-V has **no flags at all**, branches compare two registers directly (regularity over special state).
- **Zero register:** ARM64 `xzr` and RISC-V `x0` are hardwired zero, a direct descendant of MIPS `$zero` from [[02_Instruction_Set_Architecture]]. x86 lacks one (`xor %eax,%eax` is the idiom).

### Calling conventions (brief)

| | System V AMD64 (Linux/macOS x86) | AAPCS64 (ARM64) | RISC-V ABI |
|---|---|---|---|
| Integer args | rdi, rsi, rdx, rcx, r8, r9 (then stack) | x0–x7 | a0–a7 |
| Return | rax (integer) | x0 | a0 |
| Callee-saved | rbx, rbp, r12–r15 | x19–x28 | s0–s11 |
| Stack pointer | rsp | sp (x31 context) | sp (x2) |

Windows x64 uses a different convention (rcx, rdx, r8, r9 + shadow space), a portability gotcha for hand-written asm, irrelevant for compiler output.

---

## 8.7 What Does *Not* Change Across ISAs

The deep content of this vault is ISA-neutral:

- **Memory hierarchy** ([[05_Memory_Hierarchy]]): caches, TLBs, locality, the memory wall; identical physics on EPYC, Neoverse, or a SiFive core. Cache misses dominate performance on all three.
- **Pipelining and hazards** ([[04_Processor_Design]]): every modern implementation of all three ISAs is a deep, out-of-order, superscalar pipeline with forwarding, stalls, and branch prediction.
- **Parallelism** ([[07_Parallel_Computing]]): coherence protocols (MESI), SIMD, and multicore scaling are implementation concerns, not ISA concerns.

You learned MIPS to understand *these*; the ISA was scaffolding. Swapping scaffolding is mostly a vocabulary exercise.

---

## 8.8 Software Portability: and the One Place It Bites

### Why source code rarely cares

Compiled languages are portable because **compilers retarget:** the same C/Rust/Go source compiles to correct x86-64, ARM64, or RISC-V binaries. Docker made this visible; `docker buildx` multi-arch images (`linux/amd64`, `linux/arm64`) are routine. Interpreted languages (Python, JS) port with the interpreter.

What *does* care: hand-written assembly, inline asm, JITs, and anything that assumed byte order (all three mainstream ISAs are little-endian in practice) or pointer-size/alignment quirks.

### Where it genuinely bites: memory ordering models

Multiprocessors define *which order other cores observe your stores in*:

| Model | ISAs | Meaning |
|-------|------|---------|
| **Strong (TSO)** | x86-64 | Loads are never reordered with older loads; stores never with older stores; only StoreLoad reordering is allowed. Hardware enforces the rest. |
| **Weak (relaxed)** | ARM64, RISC-V (RVWMO) | Loads and stores may be observed out of order unless you insert **barriers** / acquire-release annotations (`ldar`/`stlr` on ARM; `lr.aq`/`sc.rl` or `fence` on RISC-V). |

**The classic bug (double-checked locking, lock-free queue, or any handmade "flag then data" publish):**

```c
// thread A: publish data, then set flag
data = 42;
flag = 1;

// thread B: spin on flag, then read data
while (!flag) ;
assert(data == 42);   // CAN FAIL on ARM64/RISC-V
```

On x86, TSO guarantees B sees `data = 42` once it sees `flag = 1` (stores are observed in program order). On ARM64 or RISC-V, **the store to `flag` may become visible before the store to `data`** (B spins past the flag and reads a stale 0. The code is data-race-free *only if* you use atomics with release/acquire semantics (`atomic_store_explicit(&flag, 1, memory_order_release)`), which emit `stlr`/`ldar` on ARM64 and fence-equivalents on RISC-V) and plain x86 stores on TSO, which is why **it worked on the developer's laptop**.

Practical consequences for operators:

- Concurrency bugs "only on ARM" (Graviton, Apple Silicon, Raspberry Pi) are usually **missing barriers**, exposed by the weak memory model, not flaky hardware. Test your concurrent code on `linux/arm64` before betting production on it.
- Compilers emitting C11/C++11 atomics, and runtimes like Go/Java, handle this correctly, the exposure is in hand-rolled lock-free code and unsafe FFI.
- RISC-V's RVWMO is similar in spirit to ARM's, with its own `fence` instruction; the same discipline applies.

---

## Sources

- Patterson & Hennessy, *Computer Organization and Design: The Hardware/Software Interface*, MIPS and **RISC-V editions** (Elsevier).
- Hennessy & Patterson, *Computer Architecture: A Quantitative Approach*, 6th ed. (Morgan Kaufmann, 2017); appendix A (ISA fundamentals), and the quantitative treatment of memory models.
- AMD64 Architecture Programmer's Manual, Volumes 1–5 (AMD, 2023+); Intel 64 and IA-32 Architectures Software Developer's Manual (Intel).
- *ARM Architecture Reference Manual for A-profile architecture* (DDI 0487, Arm Ltd.); armv9-A announcement whitepaper (2021); AWS Graviton product briefs.
- RISC-V Unprivileged ISA Specification, Volumes I–II (Ratified: RV32/64 GC, V 1.0, H); riscv.org; *The RISC-V Reader* (Patterson & Waterman, 2017).
- System V AMD64 ABI (gitlab.com/x86-psABIs); AAPCS64 (IHI 0055, Arm); RISC-V ABIs Specification (riscv-elf-psabi-doc).
- Sewall et al., "The RISC-V Memory Consistency Model and Mapping to Microarchitecture" and related RVWMO literature (ISCA 2020 tutorial materials).
- Sorin, Hill & Wood, *A Primer on Memory Consistency and Cache Coherence* (Morgan & Claypool, 2nd ed. 2018), the definitive treatment of TSO vs weak models.

---

**Related notes:** [[02_Instruction_Set_Architecture]] · [[04_Processor_Design]] · [[05_Memory_Hierarchy]] · [[07_Parallel_Computing]] · [[09_Speculative_Execution_and_Security]]
