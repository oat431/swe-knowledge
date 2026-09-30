---
tags:
  - parallel-computing
  - multicores
  - multiprocessors
  - simd
  - gpu
  - roofline-model
  - computer-architecture
  - co-and-d
source: "Patterson & Hennessy — Computer Organization and Design, Ch 7"
---

# 07 · Parallel Computing

> Moore's Law gave us more transistors; power limits stopped us from using them for higher clock speeds. The answer: **do more work at the same time.** This chapter covers shared-memory multiprocessors, message-passing clusters, GPUs, and the roofline model that tells you which architecture wins for a given workload.

---

## Why Parallelism?

| Problem | Era | Consequence |
|---------|-----|-------------|
| **Power wall** | ~2004 | Clock speed plateaued at ~4 GHz; Dennard scaling ended |
| **ILP wall** | ~2000s | Superscalar/wide-issue hit diminishing returns |
| **Memory wall** | ongoing | DRAM latency ~100+ cycles; bandwidth is the bottleneck |

**Solution:** Replicate processor cores → **multicore** and **multiprocessor** designs.

---

## 1. Parallelism Taxonomy (Flynn's)

| Category | Description | Example |
|----------|-------------|---------|
| **SISD** | Single Instruction, Single Data | Classic uniprocessor |
| **SIMD** | Single Instruction, Multiple Data | GPU shaders, AVX-512 |
| **MISD** | Multiple Instruction, Single Data | Rare (fault-tolerant systems) |
| **MIMD** | Multiple Instruction, Multiple Data | Multicore CPUs, clusters |

Most modern systems are **MIMD** with **SIMD** extensions.

---

## 2. Shared-Memory Multiprocessors

All cores share a single **physical address space**. Communication happens by reading/writing shared memory.

### 2.1 UMA vs NUMA

| Type | Full Name | Memory Access | Latency |
|------|-----------|---------------|---------|
| **UMA** | Uniform Memory Access | All cores see same latency | Equal for all |
| **NUMA** | Non-Uniform Memory Access | Local memory is faster | Varies by socket |

```
UMA (Symmetric):             NUMA:
┌────────────────┐          ┌────────┐  ┌────────┐
│ Core0 │ Core1  │          │Core0   │  │Core1   │
│ Core2 │ Core3  │          │local   │  │local   │
├────────────────┤          │memory  │  │memory  │
│   Shared Cache  │          └───┬────┘  └───┬────┘
├────────────────┤              │   interconnect  │
│   Main Memory   │              └────────────────┘
└────────────────┘
```

### 2.2 Cache Coherence

Multiple caches holding the same address → must stay **coherent**.

**Coherence invariant:** Every read returns the most recently written value.

**Snooping protocols** (bus-based, scales to ~16 cores):
- All caches snoop (listen to) the shared bus
- **MESI protocol** — four states per cache line:

| State | Meaning | Dirty? | Shared? |
|-------|---------|--------|---------|
| **M**odified | Only copy, modified | ✅ | ❌ |
| **E**xclusive | Only copy, clean | ❌ | ❌ |
| **S**hared | Multiple copies, clean | ❌ | ✅ |
| **I**nvalid | Not valid | — | — |

**Directory protocols** (for larger systems):
- A central **directory** tracks which caches hold each line
- Point-to-point messages instead of bus snooping
- Scales to hundreds of cores

### 2.3 Memory Consistency

**Coherence** = single location's ordering. **Consistency** = ordering across all locations.

**Sequential Consistency:** All operations appear in *some* total order consistent with each processor's program order. Strong but slow.

**Relaxed models** (x86-TSO, ARM): allow reordering for performance. Require explicit **fences/barriers** when ordering matters.

---

## 3. Synchronization

### 3.1 Atomic Operations

Hardware primitives for lock-free programming:

```
// Atomic exchange (test-and-set)
atomic_exchange(lock, new_val) → old_val

// Load-linked / Store-conditional (LL/SC)
LL:  old = load_linked(addr)
SC:  store_conditional(addr, new) → success/fail

// Compare-and-swap (CAS) — used by x86 LOCK CMPXCHG
CAS(addr, expected, new) → old_value
```

### 3.2 Spin Lock

```c
void lock(int *lock_var) {
    while (atomic_exchange(lock_var, 1) == 1)
        ;  // spin — wastes cycles
}

void unlock(int *lock_var) {
    *lock_var = 0;
}
```

**Problem:** Spinning wastes memory bandwidth on shared bus.

**Improvement:** Exponential backoff — wait longer between retries.

### 3.3 Implementing Locks

| Approach | Pros | Cons |
|----------|------|------|
| Test-and-set spin lock | Simple, fast when uncontended | Bus traffic under contention |
| Test-and-test-and-set | Spins on local cached copy | Still wastes some cycles |
| Ticket lock | Fair (FIFO ordering) | Still spins |
| Queued lock (MCS, CLH) | O(1) handoff, no contention | More complex |

---

## 4. Message-Passing Multiprocessors

Each node has **private memory**; communication via **send/receive** messages.

```
┌──────────┐    Network     ┌──────────┐
│  Node 0  │◄──────────────►│  Node 1  │
│  CPU+Mem  │    (Ethernet,  │  CPU+Mem  │
└──────────┘  InfiniBand)   └──────────┘
```

| Shared Memory | Message Passing |
|---------------|-----------------|
| Implicit communication (loads/stores) | Explicit send/receive |
| Hardware manages coherence | Software manages data movement |
| Easier programming model | More scalable |
| ~16–64 cores typical | Thousands of nodes (supercomputers) |

**MPI (Message Passing Interface):** Standard library for cluster programming.

---

## 5. SIMD — Data-Level Parallelism

One instruction operates on **multiple data elements** simultaneously.

### 5.1 Vector Architecture

```
// C[0..63] = A[0..63] + B[0..63]
// One instruction, 64 additions:

LD   V1, R1       // Load 64 elements into vector register V1
LD   V2, R2       // Load 64 elements into vector register V2
ADDV V3, V1, V2   // 64 parallel additions
ST   V3, R3       // Store 64 results
```

**Key features:**
- **Vector registers** — hold 64+ elements (e.g., 512-bit = 16×32-bit floats)
- **Vector functional units** — pipelined, one result per clock
- **Vector length register (VLR)** — handles arrays not divisible by vector width
- **Strip mining** — loop that processes remainder elements

### 5.2 x86 SIMD Extensions

| Extension | Width | Introduced |
|-----------|-------|------------|
| MMX | 64-bit | 1997 |
| SSE | 128-bit | 1999 |
| AVX | 256-bit | 2011 |
| AVX-512 | 512-bit | 2016 |

### 5.3 Multithreading

Execute multiple threads on same core to hide latency:

| Type | Description | Example |
|------|-------------|---------|
| **Coarse-grained** | Switch on long latency events (cache miss) | Barrel processor |
| **Fine-grained (SMT)** | Switch every cycle | Intel Hyper-Threading |
| **Simultaneous (SMT)** | Multiple threads issue in same cycle | 2-way HT on Intel |

**SMT benefit:** Uses idle functional units from stalled threads. ~10–30% throughput gain.

---

## 6. GPU Architecture

GPUs are massively parallel **SIMD** processors optimized for throughput.

### 6.1 GPU vs CPU

| Aspect | CPU | GPU |
|--------|-----|-----|
| **Cores** | 4–64, complex | 1000s+, simple |
| **Latency** | Low (deep pipelines, branch prediction) | High (hide with massive parallelism) |
| **Throughput** | Moderate | Very high |
| **Control flow** | Excellent (branch prediction) | Poor (divergence hurts) |
| **Memory** | Low latency, moderate bandwidth | High latency, very high bandwidth |

### 6.2 NVIDIA GPU Programming Model (CUDA)

```
Host (CPU)                Device (GPU)
┌──────────┐             ┌──────────────────────┐
│ Allocate │───copy───►  │ Global Memory (VRAM)  │
│ Launch   │             │                       │
│ kernel   │             │ ┌─────┐ ┌─────┐       │
│          │             │ │ SM 0│ │ SM 1│ ...   │
│          │             │ │cores│ │cores│       │
│          │             │ │share│ │share│       │
│          │             │ │mem  │ │mem  │       │
│          │             │ └─────┘ └─────┘       │
└──────────┘             └──────────────────────┘
```

**Hierarchy:**
- **Thread** — single execution unit
- **Warp** — 32 threads execute in lockstep (SIMD)
- **Thread block** — group of threads sharing shared memory
- **Grid** — collection of thread blocks for one kernel launch

**Key concept:** Threads in a warp must execute the same instruction. **Branch divergence** (threads take different paths) serializes execution.

### 6.3 GPU Memory Hierarchy

| Level | Latency | Scope | Size |
|-------|---------|-------|------|
| Registers | 1 cycle | Per thread | 64 KB/SM |
| Shared memory | ~5 cycles | Per block | 48–164 KB/SM |
| L1 cache | ~30 cycles | Per SM | Configurable |
| L2 cache | ~200 cycles | Global | 6–40 MB |
| Global (VRAM) | ~400 cycles | All threads | 8–80 GB |

---

## 7. Roofline Model

A visual performance model that combines **compute** (FLOPS) and **memory bandwidth** into one chart.

### 7.1 The Model

```
GFLOPS/sec
    ▲
    │         ╱ ceiling (peak FLOPS)
    │        ╱
    │       ╱
    │      ╱
    │     ╱  ← compute-bound kernels
    │    ╱
    │   ╱
    │  ╱ ← memory-bound kernels
    │ ╱
    │╱
    └──────────────────────► Arithmetic Intensity (FLOPs/Byte)
```

**Attainable performance:**
$$P = \min\left(\text{Peak FLOPS},\ \text{Peak Bandwidth} \times \text{Arithmetic Intensity}\right)$$

- **Ridge point** = where the two limits meet = minimum intensity needed to be compute-bound
- Kernels **left** of ridge point → **memory-bound** (bottleneck: bandwidth)
- Kernels **right** of ridge point → **compute-bound** (bottleneck: FLOPS)

### 7.2 Using the Roofline

1. **Profile** the kernel: count FLOPs and bytes transferred
2. **Calculate** arithmetic intensity = FLOPs ÷ Bytes
3. **Plot** on roofline — see which limit applies
4. **Optimize** the bottleneck:
   - Memory-bound → improve data reuse, tiling, compression
   - Compute-bound → use SIMD, better algorithm

### 7.3 Example Comparison

| Platform | Peak FLOPS | Bandwidth | Ridge Point |
|----------|------------|-----------|-------------|
| Intel CPU (AVX-512) | 2 TFLOPS | 100 GB/s | 20 FLOPs/Byte |
| NVIDIA GPU (A100) | 19.5 TFLOPS | 2 TB/s | 9.75 FLOPs/Byte |

GPU has lower ridge point → more kernels are compute-bound on GPU.

---

## 8. Amdahl's Law & Parallel Efficiency

### Amdahl's Law

$$\text{Speedup}(N) = \frac{1}{(1 - P) + \frac{P}{N}}$$

Where $P$ = fraction parallelizable, $N$ = number of processors.

**Example:** If 95% parallelizable ($P = 0.95$), max speedup with infinite processors = $\frac{1}{0.05} = 20\times$.

**Takeaway:** Even small serial portions severely limit speedup. Optimize the serial part first.

### Gustafson's Law

Argues that as $N$ grows, the problem size grows too — so parallel fraction stays high:

$$\text{Scaled Speedup} = (1 - P) + N \times P$$

### Parallel Metrics

| Metric | Formula | Meaning |
|--------|---------|---------|
| **Speedup** | $S = T_1 / T_N$ | How much faster with N processors |
| **Efficiency** | $E = S / N$ | Fraction of peak utilization |
| **Scalability** | How $E$ changes with $N$ | Strong vs weak scaling |

---

## 9. Parallel Programming Patterns

### Map-Reduce

```
Map:  Apply function to each element independently → embarrassingly parallel
Reduce: Combine results (sum, max, etc.) → sequential bottleneck
```

### Parallel Patterns

| Pattern | Description | Example |
|---------|-------------|---------|
| **Embarrassingly parallel** | No dependencies | Image processing, Monte Carlo |
| **Task parallelism** | Different tasks on different cores | Web server, pipeline |
| **Data parallelism** | Same operation on different data | Matrix multiply, SIMD |
| **Pipeline** | Stages execute on different cores | CPU pipeline, streaming |

---

## Summary — Architecture Choices

| Target | Best Architecture | Why |
|--------|-------------------|-----|
| Latency-sensitive (OS, DB) | Multicore CPU | Deep OoO, fast branch prediction |
| Throughput (rendering, ML) | GPU | Massive SIMD, high bandwidth |
| Large-scale (weather, genomics) | Cluster (MIMD) | Distributed memory scales limitlessly |
| Embedded | SIMD + simple cores | Power-efficient |

---

## Related

- [[Computer Organization Overview|← Back to Overview]]
- [[04_Processor_Design]] — Pipeline hazards (SIMD divergence is a hazard at scale)
- [[05_Memory_Hierarchy]] — Cache coherence builds on cache design
- [[06_IO_and_Storage]] — DMA and bus bandwidth relate to parallel data movement
