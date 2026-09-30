---
title: "Processor Design"
tags:
  - pipelining
  - datapath
  - hazards
  - computer-architecture
  - co-and-d
source: "Patterson & Hennessy, Computer Organization and Design, Ch 4"
---

# 4. Processor Design

## 4.1 Introduction

Processor design determines **clock cycle time** and **CPI** (clock cycles per instruction). This chapter constructs the **datapath** and **control unit** for MIPS instruction set implementations.

### Basic MIPS Subset

The implementation covers:
- **Memory-reference**: `lw`, `sw`
- **Arithmetic-logical**: `add`, `sub`, `AND`, `OR`, `slt`
- **Branch/Jump**: `beq`, `j`

### Universal Instruction Steps

Every instruction follows these first two steps:
1. **Fetch** — send PC to instruction memory, read the instruction
2. **Decode** — read one or two register operands from register file

After that, behavior diverges by instruction class:
- **Memory** → ALU computes address → access data memory → write back
- **ALU** → ALU performs operation → write result to register
- **Branch** → ALU compares registers → update PC if taken

---

## 4.2 Logic Design Conventions

### Two Types of Logic Elements

| Type | Description | Examples |
|------|-------------|----------|
| **Combinational** | Output depends only on current inputs | ALU, adders, multiplexors |
| **State (sequential)** | Contains internal storage; output depends on inputs + stored state | Registers, memory, flip-flops |

### Clocking Methodology

- **Edge-triggered**: state elements update only on a clock edge
- A combinational block reads from state elements (written in previous cycle) and writes to state elements (readable in next cycle)
- **Clock cycle time** = propagation delay through combinational logic + setup/hold times
- Register file can be **read and written in the same clock cycle** (write in first half, read in second half)

> **Key principle**: no feedback within a single clock cycle in edge-triggered design.

---

## 4.3 Building a Datapath

### Instruction Fetch Unit

```
PC → Instruction Memory → Instruction
  ↓
  +4 → Adder → next PC
```

- **Program Counter (PC)**: 32-bit register, updated every clock cycle
- **Instruction memory**: read-only, treated as combinational logic
- **Adder**: computes PC + 4

### R-Type Instruction Datapath

```
Register File (2 read ports, 1 write port)
  ↓ rs, rt → ALU → result → Write back to rd
```

- **Register file**: 32 registers, 2 read ports + 1 write port
- Read outputs are always valid; write controlled by `RegWrite` signal
- Register numbers are 5 bits (2⁵ = 32)

### Load/Store Datapath

```
Register[base] + Sign-Extend(offset) → ALU → Address → Data Memory
                                                          ↓ (load)
                                                    Write to register
```

- **Sign-extend**: 16-bit immediate → 32-bit signed value
- **Data memory**: separate from instruction memory, has read/write controls

### Branch Datapath

```
PC+4 + (Sign-Extend(offset) << 2) → Branch target address
Register[rs] - Register[rt] → ALU Zero flag → Branch decision
```

- Branch target = PC+4 + (sign-extended offset × 4)
- ALU performs subtraction; Zero output indicates equality

### Complete Single-Cycle Datapath

Combines:
- Instruction fetch (Figure 4.6)
- R-type + memory operations (Figure 4.10)
- Branch operations (Figure 4.9)

Three **multiplexors** needed:
1. ALU input: register value vs. sign-extended immediate (`ALUSrc`)
2. Register write data: ALU result vs. memory data (`MemtoReg`)
3. Next PC: PC+4 vs. branch target (`PCSrc`)

---

## 4.4 A Simple Implementation Scheme

### ALU Control

Two-level control scheme:
- **Main control** → generates 2-bit `ALUOp`
- **ALU control** → uses `ALUOp` + `funct` field → 4-bit ALU operation

| ALUOp | Meaning |
|-------|---------|
| 00 | Add (for lw, sw) |
| 01 | Subtract (for beq) |
| 10 | Determined by funct field (R-type) |

| ALU Control | Operation |
|-------------|-----------|
| 0000 | AND |
| 0001 | OR |
| 0010 | Add |
| 0110 | Subtract |
| 0111 | Set on less than |
| 1100 | NOR |

### Control Signals

| Signal | Purpose |
|--------|---------|
| `RegDst` | Write register = rd (1) or rt (0) |
| `ALUSrc` | ALU input B = sign-extended imm (1) or register (0) |
| `MemtoReg` | Write data = memory (1) or ALU (0) |
| `RegWrite` | Enable register file write |
| `MemRead` | Enable data memory read |
| `MemWrite` | Enable data memory write |
| `Branch` | Instruction is a branch |
| `ALUOp[1:0]` | ALU operation code |

### Control Signal Settings

| Instruction | RegDst | ALUSrc | MemtoReg | RegWrite | MemRead | MemWrite | Branch | ALUOp |
|------------|--------|--------|----------|----------|---------|----------|--------|-------|
| R-format   | 1      | 0      | 0        | 1        | 0       | 0        | 0      | 10    |
| lw         | 0      | 1      | 1        | 1        | 1       | 0        | 0      | 00    |
| sw         | X      | 1      | X        | 0        | 0       | 1        | 0      | 00    |
| beq        | X      | 0      | X        | 0        | 0       | 0        | 1      | 01    |

### Adding Jump Instruction

- Target = `{PC+4[31:28], instruction[25:0], 00}`
- Additional multiplexor controlled by `Jump` signal

### Why Not Single-Cycle?

- Clock cycle = **worst-case instruction time** (load: 800ps)
- Many instructions finish faster (R-type: 600ps, branch: 500ps)
- Violates "make the common case fast"
- Solution: **pipelining**

---

## 4.5 An Overview of Pipelining

### Core Concept

Pipelining **increases throughput** by overlapping instruction execution, not by reducing individual instruction latency.

### Five MIPS Pipeline Stages

| Stage | Name | Action |
|-------|------|--------|
| IF | Instruction Fetch | Fetch instruction from memory, PC + 4 |
| ID | Instruction Decode | Read registers, decode instruction |
| EX | Execute | ALU operation or address calculation |
| MEM | Memory Access | Read/write data memory |
| WB | Write Back | Write result to register file |

### Speedup Analysis

- Ideal speedup = number of pipeline stages (5× for 5-stage)
- Limited by **slowest stage** (memory access or ALU)
- Pipelining improves **throughput**, not **latency**

### Pipeline Hazards

Three types of hazards prevent the next instruction from executing on the next clock cycle:

#### 1. Structural Hazards

Hardware cannot support the combination of instructions in the same cycle.

- **Solution**: duplicate resources (separate instruction and data memories)

#### 2. Data Hazards

Instruction depends on result of a previous instruction still in the pipeline.

```
add $s0, $t0, $t1    # produces $s0
sub $t2, $s0, $t3    # needs $s0 → HAZARD
```

- **Forwarding (bypassing)**: route result from pipeline register directly to ALU input
- **Load-use hazard**: forwarding alone insufficient; need 1-cycle stall

#### 3. Control Hazards

Branch decision not yet made when next instruction must be fetched.

- **Stall**: wait until branch resolved (too slow)
- **Predict not taken**: continue fetching sequentially; flush if wrong
- **Dynamic prediction**: use history to predict branch outcomes

### MIPS Design for Pipelining

Four features that simplify pipelining:
1. All instructions **same length** → easy fetch/decode
2. Few instruction formats, **source fields in same position** → read registers during decode
3. **Memory operands only in lw/sw** → address calc in EX, memory in MEM
4. **Operands aligned in memory** → single memory access per instruction

---

## 4.6 Pipelined Datapath and Control

### Pipeline Registers

Inserted between stages to hold intermediate values:

| Register | Width | Contents |
|----------|-------|----------|
| IF/ID    | 64 bits | Instruction + PC+4 |
| ID/EX    | ~128 bits | Register values, sign-extended imm, control signals |
| EX/MEM   | ~97 bits | ALU result, store data, control signals |
| MEM/WB   | ~64 bits | Memory/ALU result, write register number, control |

### Key Design Points

- Each logical resource used in **only one pipeline stage** (avoids structural hazards)
- **Write register number** must be passed through all pipeline registers to WB stage
- Control signals generated in ID stage, passed through pipeline registers

### Control Signal Flow

```
ID stage: generate all 9 control signals
  → ID/EX: use EX signals, pass remaining
  → EX/MEM: use MEM signals, pass remaining
  → MEM/WB: use WB signals
```

### Register File Convention

- **Write** in first half of clock cycle
- **Read** in second half of clock cycle
- Allows same-register read/write in one cycle without hazard

---

## 4.7 Data Hazards: Forwarding vs. Stalling

### Forwarding (Bypassing)

Route results from internal pipeline registers to ALU inputs without waiting for WB stage.

#### Forwarding Paths

| Source | Destination | ForwardA/ForwardB |
|--------|-------------|-------------------|
| ID/EX (register file) | ALU input | 00 (default) |
| EX/MEM (prior ALU result) | ALU input | 10 |
| MEM/WB (earlier result) | ALU input | 01 |

#### Hazard Detection Conditions

**EX hazard:**
```
if (EX/MEM.RegWrite and EX/MEM.Rd ≠ 0
    and EX/MEM.Rd = ID/EX.Rs) → ForwardA = 10
if (EX/MEM.RegWrite and EX/MEM.Rd ≠ 0
    and EX/MEM.Rd = ID/EX.Rt) → ForwardB = 10
```

**MEM hazard** (with priority handling):
```
if (MEM/WB.RegWrite and MEM/WB.Rd ≠ 0
    and NOT(EX/MEM.RegWrite and EX/MEM.Rd ≠ 0
           and EX/MEM.Rd = ID/EX.Rs)
    and MEM/WB.Rd = ID/EX.Rs) → ForwardA = 01
```

### Load-Use Data Hazard

When a **load** is followed immediately by an instruction that uses the loaded value:

```
lw  $s0, 20($t1)    # data available after MEM stage
sub $t2, $s0, $t3   # needs $s0 in EX stage → TOO EARLY
```

- **Cannot forward** from MEM output to EX input (backward in time)
- **Must stall** for 1 cycle

#### Hazard Detection Unit

```
if (ID/EX.MemRead and
    (ID/EX.Rt = IF/ID.Rs or ID/EX.Rt = IF/ID.Rt))
  → stall the pipeline
```

Stall mechanism:
- Freeze PC and IF/ID register
- Insert **bubble** (nop) by zeroing control signals in ID/EX

### Compiler Optimization

Reorder instructions to avoid stalls:
```
# Original:                    # Reordered:
lw $t1, 0($t0)                lw $t1, 0($t0)
lw $t2, 4($t0)                lw $t2, 4($t0)
add $t3, $t1, $t2     →       lw $t4, 8($t0)    ← moved up
sw $t3, 12($t0)               add $t3, $t1, $t2
lw $t4, 8($t0)                sw $t3, 12($t0)
add $t5, $t1, $t4             add $t5, $t1, $t4
sw $t5, 16($t0)               sw $t5, 16($t0)
```

---

## 4.8 Control Hazards

### Solutions to Control Hazards

| Approach | Penalty | Description |
|----------|---------|-------------|
| Stall | 1+ cycles | Wait for branch resolution |
| Predict not taken | 0 (correct) / 1-2 (wrong) | Continue sequential; flush on taken |
| Delayed branch | 0 | Execute delay slot instruction always |
| Dynamic prediction | 0 (correct) / 2+ (wrong) | Use runtime history |

### Reducing Branch Penalty

Move branch decision to **ID stage** (from MEM):
- Add branch target adder to ID stage
- Add equality comparator to ID stage
- Result: only 1-cycle penalty on taken branches
- Requires additional forwarding to branch comparison logic

### Dynamic Branch Prediction

#### 1-bit Predictor
- Simple: predict based on last outcome
- Problem: loop branch mispredicts twice per loop (enter + exit)

#### 2-bit Predictor
- Must be wrong **twice** before changing prediction
- States: Strongly Taken, Weakly Taken, Weakly Not Taken, Strongly Not Taken

```
Taken → Strongly Taken ← Taken
                ↓ Not taken
         Weakly Taken
                ↓ Not taken
         Weakly Not Taken
                ↑ Taken
        Strongly Not Taken → Taken
```

#### Advanced Predictors
- **Correlating predictors**: use global branch history + local history
- **Tournament predictors**: multiple predictors with selector tracking which is more accurate
- **Branch Target Buffer (BTB)**: caches destination PC for branches

### Delayed Branch

- Always executes the instruction **after** the branch (delay slot)
- Compiler fills delay slot with useful instruction
- Three scheduling strategies:
  1. From before the branch (best)
  2. From branch target
  3. From fall-through

---

## 4.9 Exceptions

### Types of Exceptions

| Event | Source | MIPS Term |
|-------|--------|-----------|
| I/O device request | External | Interrupt |
| OS system call | Internal | Exception |
| Arithmetic overflow | Internal | Exception |
| Undefined instruction | Internal | Exception |
| Hardware malfunction | Either | Exception/Interrupt |

### Exception Handling Mechanism

1. Save faulting instruction address in **EPC** (Exception PC)
2. Record cause in **Cause register**
3. Transfer control to OS at fixed address (e.g., `0x80000180`)

### Exceptions in Pipelined Pipeline

- Treated as a **control hazard**
- Flush instructions following the excepting instruction
- Detected in **EX stage** (overflow) or **ID stage** (undefined instruction)
- Use flush signals: `IF.Flush`, `ID.Flush`, `EX.Flush`
- Save PC+4 of faulting instruction in EPC

### Precise vs. Imprecise Exceptions

| Type | Description |
|------|-------------|
| **Precise** | Exception always associated with correct instruction; all prior complete, none after begin. MIPS supports this. |
| **Imprecise** | Exception may be associated with wrong instruction. Simpler but harder for OS. |

---

## 4.10 Parallelism and Advanced ILP

### Instruction-Level Parallelism (ILP)

Parallelism among instructions that can be exploited.

### Two Approaches to More ILP

1. **Deeper pipelines** → more stages, shorter clock cycle
2. **Multiple issue** → launch multiple instructions per clock cycle

### Static Multiple Issue (VLIW)

- Compiler determines which instructions issue together
- **Issue packet**: set of instructions issued in one cycle
- Compiler handles hazard avoidance via scheduling
- Requires extra hardware: more register ports, multiple ALUs

### Dynamic Multiple Issue (Superscalar)

- Hardware decides at runtime which instructions to issue
- In-order issue, can be out-of-order execute
- More flexible than VLIW; code portable across implementations

### Speculation

- **Guess** about instruction properties to enable earlier execution
- Must verify guess and **roll back** if wrong
- Hardware speculation: buffer results until confirmed
- Compiler speculation: insert check + fix-up code

### Dynamic Pipeline Scheduling

Three major units:
1. **Instruction fetch/issue unit** (in-order)
2. **Functional units** with **reservation stations** (out-of-order execute)
3. **Commit unit** with **reorder buffer** (in-order commit)

Key concepts:
- **Reservation stations**: buffer operands until ready
- **Reorder buffer**: holds results until safe to commit
- **Register renaming**: eliminate anti-dependences
- **Out-of-order execution**: execute when data ready, not in program order
- **In-order commit**: retire results in program order for precise exceptions

### Loop Unrolling

- Duplicate loop body to expose more ILP
- Combined with **register renaming** to eliminate name dependences
- Increases code size but reduces loop overhead

---

## 4.11 Real Stuff: AMD Opteron X4 (Barcelona)

### Microarchitecture Highlights

- Translates x86 instructions to **RISC operations (Rops)** internally
- 12-stage integer pipeline, 17-stage FP pipeline
- Sustains **3 RISC operations per clock cycle**
- Out-of-order execution with speculative pipeline
- **Register renaming**: 16 architectural → 72 physical registers
- Extensive bypass network among functional units

### Pipeline Stages

| Stage | Cycles | Description |
|-------|--------|-------------|
| Fetch + translate | 2 | Fetch x86, decode to Rops |
| Reorder buffer + register renaming | 3 | Allocate, rename |
| Scheduling + dispatch | 2 | Issue to functional units |
| Execution | 1-2 | Execute in functional units |
| Data cache | 2 | Memory access |
| Commit | 2 | In-order retirement |

---

## 4.13 Fallacies and Pitfalls

### Fallacies

- **"Pipelining is easy"** — subtle bugs common; thousands of lines of Verilog
- **"Pipelining ideas are technology-independent"** — delayed branch obsolete with deep pipelines; dynamic scheduling became practical with more transistors

### Pitfalls

- **ISA design impacts pipelining** — variable instruction lengths, complex addressing modes, register-updating modes all complicate pipeline design
- **Failure to consider instruction set design** — DEC VAX required 2.7× more clock cycles than MIPS for same benchmarks

---

## Summary

| Concept | Key Point |
|---------|-----------|
| Pipelining | Overlaps instructions for throughput, not latency reduction |
| Pipeline stages | IF → ID → EX → MEM → WB |
| Structural hazard | Resource conflict; solve with duplication |
| Data hazard | Data dependency; solve with forwarding + stalling |
| Control hazard | Branch uncertainty; solve with prediction |
| Forwarding | Route results from pipeline registers to ALU inputs |
| Load-use stall | 1-cycle stall when load immediately used |
| Branch prediction | 2-bit dynamic predictors achieve >90% accuracy |
| Multiple issue | Launch >1 instruction/clock (VLIW or superscalar) |
| Dynamic scheduling | Out-of-order execute, in-order commit via reorder buffer |
| Speculation | Execute before knowing if correct; verify and rollback |


## Related

- [[Computer Organization Overview]] — All computer organization topics
- [[02_Instruction_Set_Architecture]] — Instructions the processor executes
- [[03_Computer_Arithmetic]] — Arithmetic operations in the ALU
- [[05_Memory_Hierarchy]] — Memory access and cache interactions
- [[07_Parallel_Computing]] — Pipelining and ILP parallelism
