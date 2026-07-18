---
title: "Computer Abstractions and Technology"
source: "Patterson & Hennessy – Computer Organization and Design, 4th ed. (Revised)"
chapter: 1
tags:
  - abstractions
  - performance
  - computer-architecture
  - co-and-d
---

# 1 Computer Abstractions and Technology

## 1.1 Introduction

### Classes of Computing Applications

| Class | Characteristics |
|-------|----------------|
| **Desktop** | Good performance for single users at low cost; typically runs third-party software |
| **Server** | Large workloads, high expandability, emphasis on **dependability**; spans low-end (~$1K) to supercomputers (millions of dollars) |
| **Embedded** | Largest class by volume; runs one predetermined application; stringent cost/power constraints; low tolerance for failure |

- Embedded processors vastly outnumber desktop processors (cell phones alone exceeded PCs 4.5:1 by 2007).

### What You Can Learn in This Book

1. How high-level programs are translated into hardware instructions
2. The **hardware/software interface** (instruction set architecture)
3. What determines program performance and how to improve it
4. Hardware techniques for improving performance
5. Why the industry switched from sequential to parallel processing

> **Understanding Program Performance** – depends on: *algorithm*, *programming language & compiler*, *processor & memory system*, *I/O system*.

## 1.2 Below Your Program

### Software Layers (outermost → innermost)

1. **Application software**
2. **Systems software** – operating system, compiler, loader, assembler
3. **Hardware**

### Translation Chain

```
High-level language (C, Java)
        ↓  Compiler
Assembly language (MIPS)
        ↓  Assembler
Binary machine language
```

- **Operating system** – handles I/O, allocates memory, provides protected sharing among programs.
- **Compiler** – translates high-level language → assembly language.
- **Assembler** – translates symbolic assembly → binary machine instructions.
- **High-level languages** provide: natural notation, higher productivity (conciseness), and **machine independence**.

## 1.3 Under the Covers

### Five Classic Components of a Computer

> **Big Picture:** The five components are **input**, **output**, **memory**, **datapath**, and **control** (the last two combined = **processor**).

### Key Hardware Concepts

| Component | Description |
|-----------|-------------|
| **Input device** | Feeds data to computer (keyboard, mouse) |
| **Output device** | Conveys results (LCD display) |
| **Processor (CPU)** | Active part – datapath + control; adds, tests, signals I/O |
| **Memory (DRAM)** | Stores running programs and their data; volatile |
| **Motherboard** | PCB containing processor, memory, I/O connectors |

### Memory Hierarchy

```
Registers (inside processor)
    ↓
Cache (SRAM) – small, fast buffer
    ↓
Main memory (DRAM) – volatile
    ↓
Secondary memory (magnetic disk / flash) – nonvolatile
```

- **DRAM** – access ~50–70 ns; dominant since 1975.
- **Magnetic disk** – access ~5–20 ms; ~100,000× slower than DRAM but much cheaper per GB.
- **Flash memory** – nonvolatile semiconductor; 100–1000× faster latency than disk; bits wear out after 100K–1M writes.
- **Cache (SRAM)** – faster but less dense and more expensive than DRAM.

### Abstraction and the ISA

> **Big Picture:** Both hardware and software consist of hierarchical layers. The **instruction set architecture (ISA)** is the critical interface between hardware and low-level software. Multiple implementations can obey the same architecture abstraction.

- **ISA** – everything a programmer needs to know to make a binary program work (instructions, registers, memory access, I/O).
- **ABI** – ISA + operating system interface for application programmers.
- **Implementation** – hardware that obeys the architecture abstraction.

### Networking

- **LAN** (Ethernet) – up to 10 Gbps, within a building.
- **WAN** – continental, optical fiber backbone.
- **Wireless (802.11)** – 1 to ~100 Mbps; shared medium.

### Moore's Law

- Transistor capacity doubles every **18–24 months**.
- DRAM capacity quadrupled every 3 years (60%/year) for 20 years.
- Enables the entire technology progression: vacuum tubes → transistors → ICs → VLSI.

## 1.4 Performance

### Response Time vs. Throughput

| Metric | Definition |
|--------|------------|
| **Response time** (execution time) | Total time to complete a single task |
| **Throughput** (bandwidth) | Number of tasks completed per unit time |

$$\text{Performance}_X = \frac{1}{\text{Execution Time}_X}$$

"X is *n* times faster than Y" means: $\frac{\text{Execution Time}_Y}{\text{Execution Time}_X} = n$

### Measuring Time

- **Wall clock (elapsed) time** – everything included.
- **CPU execution time** – time CPU spends on the task (excludes I/O wait, other programs).
  - *User CPU time* – in the program itself.
  - *System CPU time* – in the OS on behalf of the program.

### Clock Cycles

- **Clock cycle** (tick/period) – discrete time interval of the processor clock.
- **Clock rate** = 1 / clock cycle time (e.g., 4 GHz → 250 ps period).

### The Classic CPU Performance Equation

$$\boxed{\text{CPU Time} = \text{Instruction Count} \times \text{CPI} \times \text{Clock Cycle Time}}$$

$$\text{CPU Time} = \frac{\text{Instruction Count} \times \text{CPI}}{\text{Clock Rate}}$$

| Factor | Description | Determined by |
|--------|-------------|---------------|
| **Instruction Count** | Number of instructions executed | Algorithm, ISA, compiler |
| **CPI** (Clock cycles per instruction) | Average cycles per instruction | ISA, microarchitecture, compiler, program |
| **Clock cycle time** | Duration of one cycle | Technology, microarchitecture |

$$\text{CPU Clock Cycles} = \sum_{i=1}^{n} (\text{CPI}_i \times C_i)$$

> **Big Picture:** Time = (Instructions/Program) × (Cycles/Instruction) × (Seconds/Cycle). Always bear in mind that **time** is the only complete and reliable measure of performance.

### Factors Affecting Performance

| Component | Affects | How |
|-----------|---------|-----|
| Algorithm | Instruction count, possibly CPI | Determines number of source-level operations |
| Programming language | Instruction count, CPI | Statements → instructions; features affect CPI |
| Compiler | Instruction count, CPI | Translation efficiency |
| ISA | Instruction count, clock rate, CPI | All three aspects |

## 1.5 The Power Wall

### CMOS Dynamic Power

$$\text{Power} = \text{Capacitive Load} \times \text{Voltage}^2 \times \text{Frequency Switched}$$

- Clock rate and power grew together for decades, then **flattened** around 2004–2005.
- Voltage reductions (~15%/generation) kept power manageable until ~1V floor.
- At ~1V, further voltage lowering causes excessive **leakage current** (~40% of total power in 2008).
- Practical cooling limit: ~100–120W for commodity desktop chips.

### Implications

- Can no longer simply increase clock rate for performance gains.
- Designers turn to **parallelism** (multiple cores) as the way forward.

## 1.6 The Sea Change: Uniprocessors → Multiprocessors

### The Paradigm Shift

- Since ~2002, uniprocessor performance growth slowed from **52%/year** to **~20%/year**.
- Since 2006, all desktop/server companies ship **multicore microprocessors** (multiple processors/"cores" per chip).
- Plan: **double cores per chip** every semiconductor generation (~2 years).

### Why Parallel Programming Is Hard

1. **Performance programming** adds complexity beyond correctness.
2. Must **divide work evenly** across processors (load balancing).
3. Must minimize **communication and synchronization** overhead.
4. **Dependencies** between sub-tasks limit parallelism.

> *Analogy:* Eight reporters on one story — scheduling, load balancing, and coordination overhead all reduce the theoretical 8× speedup.

### Parallelism Themes Across the Book

| Chapter | Section | Topic |
|---------|---------|-------|
| 1 | 1.5–1.6 | Power wall motivates parallelism |
| 2 | 2.11 | Synchronization (Load Linked / Store Conditional) |
| 3 | 3.6 | Floating-point associativity |
| 4 | 4.10 | Advanced ILP (superscalar, speculation, VLIW, OOO) |
| 5 | 5.8 | Cache coherence |
| 6 | 6.9 | RAID (parallel I/O) |
| 7 | Full chapter | Multicores, multiprocessors, clusters, GPUs, Roofline model |

## 1.7 Real Stuff: Manufacturing & Benchmarking the AMD Opteron X4

### IC Manufacturing Process

1. **Silicon crystal ingot** (8–12" diameter) → sliced into **wafers** (<0.1" thick)
2. Wafers go through **20–40 processing steps** (patterning transistors, conductors, insulators)
3. **Wafer test** → map good/bad dies
4. **Dice** the wafer into individual **dies** (chips)
5. **Bond** good dies into packages → final test → ship

- **Yield** = percentage of good dies on a wafer.
- Cost rises quickly with die size (lower yield, fewer dies/wafer).
- "Shrinking" a die to the next process generation improves yield and die count.

### Power as a Design Constraint

- AMD Opteron X4 2356: 4 cores, 2.5 GHz, **120W** TDP on ~1 cm² die surface.
- Power must be distributed via hundreds of pins + multiple interconnect levels.
- Heat must be removed from a tiny surface area.

---

## Key Takeaways

1. **Abstraction** is the fundamental technique for managing complexity — layers of hardware and software hide lower-level details.
2. The **ISA** is the critical hardware/software interface; multiple implementations can exist for one ISA.
3. **Performance** = f(Instruction Count, CPI, Clock Rate) — measured as execution time.
4. The **power wall** ended the era of ever-increasing clock rates and forced the shift to **multicore**.
5. **Parallelism** is now the primary path to performance improvement — but parallel programming remains a fundamental challenge.
6. **Moore's Law** (transistor doubling every 18–24 months) has driven the technology revolution for nearly 40 years.


## Related

- [[Computer Organization Overview]] — All computer organization topics
- [[02_Instruction_Set_Architecture]] — How instructions encode operations
- [[04_Processor_Design]] — How the processor executes instructions
- [[05_Memory_Hierarchy]] — How memory levels affect performance
