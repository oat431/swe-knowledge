---
tags: [computer-architecture, computer-organization, overview, co-and-d]
---

# Computer Organization: Overview

> **Source:** *Computer Organization and Design: The Hardware/Software Interface* by David A. Patterson & John L. Hennessy (Morgan Kaufmann)

## What Is This?

This vault covers **computer architecture and organization**; how computers work from the hardware/software interface up. It fills SWEBOK's Computing Foundations KA for architecture, ISA, arithmetic, processor design, memory hierarchy, I/O, and parallel computing.

## Files

| File | Topics | Source |
|---|---|---|
| [[01_Computer_Abstractions]] | Abstractions, levels of interpretation, performance metrics, power wall, benchmarks | Ch 1 |
| [[02_Instruction_Set_Architecture]] | MIPS ISA, operands, addressing modes, ARM/x86 comparison | Ch 2 |
| [[03_Computer_Arithmetic]] | Integer arithmetic, floating point, IEEE 754, overflow, rounding | Ch 3 |
| [[04_Processor_Design]] | Datapath, pipelining, data hazards, forwarding, control hazards, exceptions | Ch 4 |
| [[05_Memory_Hierarchy]] | Caches (direct-mapped, set-associative), virtual memory, TLB, cache coherence | Ch 5 |
| [[06_IO_and_Storage]] | Dependability, disk storage, flash, I/O buses, DMA, RAID | Ch 6 |
| [[07_Parallel_Computing]] | Multicores, multiprocessors, SIMD, GPU architecture, roofline model | Ch 7 |
| [[08_Modern_ISAs_x86_ARM64_RISCV]] | x86-64, ARM64/AArch64, RISC-V, calling conventions, memory ordering (TSO vs weak) | Beyond H&P |
| [[09_Speculative_Execution_and_Security]] | Spectre/Meltdown, transient execution attacks, KPTI, retpoline, mitigations | Beyond H&P |

## How These Topics Relate

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#19362D','primaryTextColor':'#CDD3D1','primaryBorderColor':'#1FB854','lineColor':'#1FB854','secondaryColor':'#161212','tertiaryColor':'#1B1717','background':'#1B1717','mainBkg':'#19362D','nodeBorder':'#1FB854','clusterBkg':'#161212','clusterBorder':'#19362D','titleColor':'#1FB854','edgeLabelBackground':'#161212','fontSize':'14px'}}}%%
flowchart TD
    ABS["Abstractions & Technology"] --> ISA["Instruction Set Architecture"]
    ISA --> ARITH["Computer Arithmetic"]
    ISA --> PROC["Processor Design"]
    ARITH --> PROC
    PROC --> MEM["Memory Hierarchy"]
    MEM --> IO["I/O & Storage"]
    PROC --> PAR["Parallel Computing"]
    MEM --> PAR
```


## Reading Paths

| Your Goal | Start Here |
|---|---|
| **How computers work** | [[01_Computer_Abstractions]] → [[02_Instruction_Set_Architecture]] → [[04_Processor_Design]] |
| **Performance optimization** | [[05_Memory_Hierarchy]] → [[07_Parallel_Computing]] |
| **Assembly programming** | [[02_Instruction_Set_Architecture]] → [[08_Modern_ISAs_x86_ARM64_RISCV]] |
| **Real-world hardware (servers, Apple Silicon, cloud)** | [[08_Modern_ISAs_x86_ARM64_RISCV]] |
| **Hardware security** | [[04_Processor_Design]] → [[09_Speculative_Execution_and_Security]] |
| **Hardware design** | [[03_Computer_Arithmetic]] → [[04_Processor_Design]] → [[05_Memory_Hierarchy]] |
| **Systems programming** | [[05_Memory_Hierarchy]] → [[06_IO_and_Storage]] |

## Related

- [[Computing Foundation Overview]]: All computing foundation topics
- [[Operating Systems/Operating Systems Overview|Operating Systems]]: OS concepts that build on architecture
- [[Algorithm/Algorithm Overview|Algorithms]]: Algorithmic complexity and data structures
