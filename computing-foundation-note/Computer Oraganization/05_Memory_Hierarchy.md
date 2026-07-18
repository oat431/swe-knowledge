---
title: "Large and Fast: Exploiting Memory Hierarchy"
source: "Patterson & Hennessy – Computer Organization and Design, 4th ed. (Revised)"
chapter: 5
tags:
  - cache
  - virtual-memory
  - tlb
  - computer-architecture
  - co-and-d
---

# 5 Large and Fast: Exploiting Memory Hierarchy

## 5.1 Introduction

### The Memory Hierarchy

Programs exhibit **locality**, which makes a memory hierarchy possible:

- **Temporal locality**: if an item is referenced, it will tend to be referenced again soon.
- **Spatial locality**: if an item is referenced, items whose addresses are close by will tend to be referenced soon.

A **memory hierarchy** uses multiple levels of memories — as the distance from the processor increases, both the size and access time increase.

| Technology | Typical Access Time | $ per GB (2008) |
|------------|---------------------|-----------------|
| SRAM | 0.5–2.5 ns | $2000–$5000 |
| DRAM | 50–70 ns | $20–$75 |
| Magnetic disk | 5,000,000–20,000,000 ns | $0.20–$2 |

### Key Terminology

- **Block (line)**: the minimum unit of information that can be present or not in a cache.
- **Hit**: data requested is found in the upper level.
- **Miss**: data requested is not found in the upper level.
- **Hit rate**: fraction of memory accesses found in a level.
- **Miss rate**: 1 − hit rate.
- **Hit time**: time to access the upper level (including hit/miss determination).
- **Miss penalty**: time to fetch a block from the lower level and deliver it to the processor.

---

## 5.2 The Basics of Caches

### Direct-Mapped Cache

Each memory location maps to exactly one cache location:

> **(Block address) mod (Number of blocks in the cache)**

If the number of entries is a power of 2, use the low-order log₂(cache size) bits as the index.

#### Address Decomposition (Direct Mapped)

```
| Tag | Index | Block offset | Byte offset |
```

- **Tag**: upper address bits used to identify the block.
- **Index**: selects the cache block.
- **Block offset**: selects the word within the block.
- **Byte offset**: selects the byte within the word (2 bits for MIPS).

Each cache entry has a **valid bit** indicating whether it contains valid data.

#### Total Bits in a Cache

For a direct-mapped cache with 2ⁿ blocks and 2ᵐ-word blocks (32-bit address):

```
Tag size = 32 − (n + m + 2)
Total bits = 2ⁿ × (2ᵐ × 32 + tag size + 1)
```

> **Example**: 16 KB cache, 4-word blocks → 2¹⁰ blocks, tag = 18 bits, total = 2¹⁰ × 147 = 147 Kbits (≈ 18.4 KB for 16 KB data).

### Handling Cache Misses

On an instruction cache miss:
1. Send original PC value (current PC − 4) to memory.
2. Instruct main memory to perform a read; wait for completion.
3. Write cache entry: data from memory → data portion, upper address bits → tag, set valid bit.
4. Restart instruction execution (re-fetches from cache this time).

On a data miss: stall the processor until memory responds.

### Handling Writes

| Scheme | Description | Pros | Cons |
|--------|-------------|------|------|
| **Write-through** | Write to both cache and memory | Simple; no inconsistency | Slow (every write goes to memory); needs write buffer |
| **Write-back** | Write only to cache; write to memory on replacement | Better performance; fewer memory writes | More complex; needs dirty bit |

- **Write buffer**: holds data waiting to be written to memory; processor continues after writing to cache + buffer.
- **Write allocate**: on a write miss, fetch block from memory, then overwrite (most common).
- **No write allocate**: update memory directly without loading into cache.

### Multiword Blocks and Spatial Locality

Larger blocks exploit spatial locality → lower miss rate. However:

- Too-large blocks → fewer blocks in cache → more conflicts.
- Miss penalty increases with block size (more data to transfer).

**Memory bandwidth optimization**:
- **Wider memory**: read multiple words per access.
- **Interleaved memory**: multiple banks accessed in parallel.
- **DDR DRAM**: transfers on both clock edges for double bandwidth.
- **Burst mode**: sequential accesses to buffered row with low latency.

### Example: Intrinsity FastMATH

- 16 KB instruction cache + 16 KB data cache (split cache).
- 16-word blocks, direct-mapped.
- Instruction miss rate: 0.4%; Data miss rate: 11.4%; Combined: 3.2%.

> **Split cache** enables simultaneous instruction and data access, doubling cache bandwidth — outweighs the slightly higher miss rate vs. a combined cache.

---

## 5.3 Measuring and Improving Cache Performance

### CPU Time with Memory Stalls

```
CPU time = (CPU execution clock cycles + Memory-stall clock cycles) × Clock cycle time
```

Simplified (ignoring write buffer stalls):

```
Memory-stall cycles = (Instructions / Program) × (Misses / Instruction) × Miss penalty
```

### Average Memory Access Time (AMAT)

```
AMAT = Hit time + Miss rate × Miss penalty
```

### Associativity

| Type | Description | Comparators |
|------|-------------|-------------|
| **Direct mapped** | Block goes in exactly one place (1-way) | 1 |
| **Set associative** | Block can go in one of n places (n-way) | n per set |
| **Fully associative** | Block can go anywhere | Size of cache |

- Set index: `(Block number) mod (Number of sets)`
- Increasing associativity generally **decreases miss rate** (especially 1→2 way: ~20–30% reduction).
- Disadvantage: more comparators, potential increase in hit time.

#### Replacement Policies

- **LRU (Least Recently Used)**: replace block unused for the longest time. Costly for high associativity.
- **Random**: simple hardware; miss rate ~1.1× LRU for 2-way.
- For high associativity, LRU is approximated or random is used.

### Multilevel Caches

- **L1**: focuses on minimizing **hit time** (small, fast).
- **L2**: focuses on minimizing **miss rate** (larger, higher associativity).

```
Total CPI = Base CPI + L1 stalls/instruction + L2 stalls/instruction
```

- **Global miss rate**: fraction of references that miss in all levels.
- **Local miss rate**: misses at a level / accesses to that level (much higher than global for L2).

> **Example**: Base CPI=1, L1 miss rate=2%, L2 access=20 cycles, main memory=400 cycles, global miss rate=0.5%.
> CPI = 1 + 2%×20 + 0.5%×400 = 1 + 0.4 + 2.0 = **3.4** (vs. 9.0 without L2).

---

## 5.4 Virtual Memory

### Motivation

- **Sharing**: multiple processes safely share main memory with protection.
- **Illusion of large memory**: programs can exceed physical memory size.

### Key Concepts

| Term | Definition |
|------|------------|
| **Virtual address** | Address generated by the processor |
| **Physical address** | Address used to access main memory |
| **Page** | Virtual memory block (typically 4–16 KB) |
| **Page fault** | Accessed page not in main memory (miss to disk) |
| **Page table** | Maps virtual page numbers to physical page numbers |
| **Swap space** | Disk area reserved for full virtual memory space |

### Address Translation

```
Virtual address → [Virtual page number | Page offset]
                        ↓ (page table lookup)
                  [Physical page number | Page offset] → Physical address
```

### Page Table

- Indexed by virtual page number; each entry contains physical page number + valid bit.
- Each process has its own page table.
- **Page table register** points to the start of the active page table.

### Design Decisions (driven by enormous page fault cost)

1. **Large pages** (4–16 KB) to amortize disk access time.
2. **Fully associative** placement — any virtual page can go in any physical page.
3. **Software handling** of page faults (overhead is small relative to disk access).
4. **Write-back** only (write-through to disk is impractical).
5. **Dirty bit** tracks modified pages to avoid writing back clean pages.
6. **LRU approximation** using **reference bit (use bit)** for replacement.

### Reducing Page Table Size

1. **Limit register**: restricts page table size to actual usage.
2. **Two segments** (stack + heap): two page tables growing in opposite directions.
3. **Inverted page table**: indexed by hash of virtual address; size = number of physical pages.
4. **Multi-level page tables**: first-level maps large segments, second-level maps pages within segments.
5. **Page the page tables** themselves (page tables reside in virtual address space).

### TLB (Translation-Lookaside Buffer)

A cache for page table entries — avoids accessing memory for every translation.

- **Typical values**: 16–512 entries, 0.5–1 cycle hit time, 10–100 cycle miss penalty, 0.01%–1% miss rate.
- Often fully associative (small, so cost is manageable) or set-associative for larger TLBs.
- Each entry: tag (virtual page number) + physical page number + valid + dirty + reference bits.
- On **TLB miss**: check page table → if valid, load into TLB; if invalid, **page fault** → invoke OS.
- Replacement: random (MIPS) or LRU approximation.

### Integrating TLB, Cache, and Virtual Memory

```
Virtual address → TLB lookup → Physical address → Cache lookup → Data
```

Possible combinations:

| TLB | Page Table | Cache | Possible? |
|-----|-----------|-------|-----------|
| Hit | — | Hit | Yes (normal fast path) |
| Miss | Hit | Hit | Yes (TLB miss, then cache hit) |
| Miss | Hit | Miss | Yes (TLB miss, then cache miss) |
| Miss | Miss | Miss | Yes (page fault) |
| Hit | Miss | — | **Impossible** (can't have TLB entry for absent page) |
| Miss | Miss | Hit | **Impossible** (data can't be in cache if page not in memory) |

### Cache Addressing Options

| Type | Description | Pros/Cons |
|------|-------------|-----------|
| **Physically indexed, physically tagged (PIPT)** | Translate first, then access cache | No aliasing; TLB in critical path |
| **Virtually indexed, virtually tagged (VIVT)** | Access cache with virtual address | Fast; but aliasing problem |
| **Virtually indexed, physically tagged (VIPT)** | Index with page offset (physical), tag with physical address | Good compromise; no aliasing |

### Protection

Hardware must provide:
1. **Two modes**: user and supervisor (kernel).
2. **Protected state**: user/supervisor bit, page table pointer, TLB — readable by user, writable only in supervisor mode.
3. **Mode transitions**: `syscall` (user → kernel), `ERET` (kernel → user).

- **Write access bit** in page table/TLB prevents unauthorized writes.
- **ASID (Address Space ID)**: avoids flushing TLB on context switch by tagging entries with process ID.

### TLB Miss Handling (MIPS Example)

```asm
TLBmiss:
    mfc0 $k1, Context    # get page table entry address
    lw   $k1, 0($k1)     # load page table entry
    mtc0 $k1, EntryLo    # put into TLB entry register
    tlbwr                # write to TLB at random location
    eret                 # return from exception
```

- TLB miss: ~13 cycles (if code + PTE in caches).
- Page fault: OS saves full process state, reads page from disk, restores, retries instruction.

---

## 5.5 A Common Framework for Memory Hierarchies

### Four Questions for Any Two Levels

| Question | Cache | Virtual Memory | TLB |
|----------|-------|----------------|-----|
| **1. Where can a block be placed?** | Direct mapped / Set associative / Fully associative | Fully associative | Fully associative or set associative |
| **2. How is a block found?** | Index + tag comparison | Page table (separate lookup) | Tag comparison |
| **3. Which block to replace?** | LRU or random | LRU (approximated via reference bit) | Random or LRU approximation |
| **4. What happens on a write?** | Write-through or write-back | Write-back only | Write-back (copy dirty/ref bits on eviction) |

### The Three Cs Model

All cache misses classified into:

| Type | Description | Reduction Strategy |
|------|-------------|-------------------|
| **Compulsory** (cold-start) | First access to a block never in cache | Increase block size |
| **Capacity** | Cache too small to hold all needed blocks | Increase cache size |
| **Conflict** (collision) | Multiple blocks compete for same set (direct-mapped/set-associative only) | Increase associativity |

- Fully associative caches have **no conflict misses**.
- Every design change that reduces misses may have negative effects (e.g., larger cache → longer hit time).

---

## 5.8 Cache Coherence

### The Coherence Problem

In multiprocessors with shared memory, each processor has its own cache. Without coordination, different processors can see different values for the same memory location.

### Coherence Properties

A memory system is coherent if:
1. A read by processor P to location X after P's write to X (with no intervening writes by others) returns P's written value.
2. A read returns the most recently written value if sufficiently separated in time.
3. **Writes to the same location are serialized** — all processors see writes in the same order.

### Snooping Protocols

- Every cache monitors (snoops) the shared bus/network for accesses to cached blocks.
- **Write invalidate**: before writing, invalidate all other cached copies → exclusive access.
  - Ensures write serialization and coherence.
  - **False sharing**: unrelated variables in the same cache block cause unnecessary invalidations.

### Directory-Based Protocols

- Sharing status kept in a centralized **directory** (one per block).
- Higher overhead but scales to larger processor counts than snooping.

---

## 5.10 Real Stuff: AMD Opteron X4 and Intel Nehalem

| Feature | Intel Nehalem | AMD Opteron X4 |
|---------|---------------|----------------|
| Virtual address | 48 bits | 48 bits |
| Physical address | 44 bits | 48 bits |
| L1 I-cache | 32 KB per core | 64 KB per core |
| L1 D-cache | 32 KB per core | 64 KB per core |
| L2 cache | 512 KB per core | 512 KB per core |
| L3 cache | 8 MB shared | 2 MB shared |
| TLBs | 3 per core (hardware miss handling) | 4 per core (hardware miss handling) |

- Both have on-chip memory controllers → reduced memory latency.
- CPI and cache miss rates are highly correlated (correlation coefficient ≈ 0.97 for SPECint2006).

---

## Key Takeaways

1. **Locality** (temporal + spatial) is the fundamental principle enabling memory hierarchies.
2. **AMAT = Hit time + Miss rate × Miss penalty** — optimize by reducing all three.
3. **Associativity** reduces conflict misses; 2-way is the sweet spot for most designs.
4. **Multilevel caches**: L1 optimizes hit time, L2+ optimizes miss rate.
5. **Virtual memory** provides protection, sharing, and the illusion of unlimited memory.
6. **TLB** caches translations to avoid double memory access on every reference.
7. **Write-back** is preferred for virtual memory (and increasingly for caches) due to bandwidth.
8. **Cache coherence** in multiprocessors requires hardware protocols (snooping or directory-based).


## Related

- [[Computer Organization Overview]] — All computer organization topics
- [[04_Processor_Design]] — Processor pipeline and memory access
- [[06_IO_and_Storage]] — Disk and flash storage
- [[07_Parallel_Computing]] — Cache coherence in multiprocessors
