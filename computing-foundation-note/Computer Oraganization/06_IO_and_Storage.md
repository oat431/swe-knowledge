---
tags:
  - io
  - storage
  - raid
  - computer-architecture
  - co-and-d
---

# 06 I/O & Storage

> **Source:** Patterson & Hennessy, *Computer Organization and Design* — Chapter 6

I/O systems prioritize **dependability and cost** over raw performance (unlike processors/memory). They must also handle **expandability** and **device diversity**.

---

## 6.1 Introduction — I/O Device Characteristics

Three axes classify I/O devices:

| Axis | Categories |
|------|-----------|
| **Behavior** | Input (read once), Output (write-only), Storage (reread/rewrite) |
| **Partner** | Human or Machine |
| **Data rate** | Peak transfer rate between device and memory/processor |

Data rates span **eight orders of magnitude** — from keyboard (~0.0001 Mbit/s) to magnetic disk (800–3000 Mbit/s).

### Performance Metrics for I/O

- **Throughput** — data moved per unit time (GB/sec) or I/O operations per second
- **Response time** — elapsed time for a single operation (key for desktop/laptop)
- **I/O rate** — number of I/O operations per second (key for servers/TP)

---

## 6.2 Dependability, Reliability, and Availability

> *Computer system dependability is the quality of delivered service such that reliance can justifiably be placed on this service.*

### Key Terms

| Term | Definition |
|------|-----------|
| **Failure** | Actual behavior deviates from specified behavior |
| **MTTF** | Mean Time To Failure — reliability measure |
| **MTTR** | Mean Time To Repair |
| **MTBF** | MTTF + MTTR |
| **AFR** | Annual Failure Rate — percentage of devices expected to fail per year |
| **Availability** | MTTF / (MTTF + MTTR) |

### Failure Causes (from studies)

Human operators are the **#1 source of failures** across all studies:

| Source | Typical Range |
|--------|--------------|
| **Operator** | 15–60% |
| **Software** | 7–55% |
| **Hardware** | 14–39% |

### Three Ways to Improve MTTF

1. **Fault avoidance** — prevent faults by construction
2. **Fault tolerance** — use redundancy (e.g., RAID) to continue despite faults
3. **Fault forecasting** — predict and replace before failure

---

## 6.3 Disk Storage

Magnetic disks: rotating platters + movable read/write heads. **Nonvolatile**.

### Disk Anatomy

- **Platters:** 1–4, each with two recordable surfaces
- **Rotation:** 5400–15,000 RPM
- **Tracks:** 10,000–50,000 per surface (concentric circles)
- **Sectors:** 100–500 per track, typically 512 bytes each (moving to 4096)
- **Cylinder:** all tracks under the heads at a given position across all surfaces

**Zone Bit Recording (ZBR):** outer tracks have more sectors (constant bit spacing), increasing capacity.

### Disk Access Time Components

```
Access Time = Seek + Rotational Latency + Transfer Time + Controller Overhead
```

| Component | Typical Range | Notes |
|-----------|--------------|-------|
| **Seek time** | 3–13 ms (advertised avg) | Actual avg often 25–33% of advertised due to locality |
| **Rotational latency** | 2–5.6 ms (avg = 0.5 rotation) | 15,000 RPM → 2.0 ms; 5,400 RPM → 5.6 ms |
| **Transfer time** | 70–125 MB/sec (surface) | Cache transfers: up to 375 MB/sec |
| **Controller overhead** | ~0.2 ms | |

### Disk Evolution

Costs: **$0.20–$5.00/GB** (2008). Densities improved >50 years. High-level interfaces (ATA, SCSI) organize disks more like **tapes** — sequential blocks may span different tracks.

---

## 6.4 Flash Storage

First credible disk challenger. Semiconductor, nonvolatile, **100–1000× faster** than disk latency, smaller, lower power, more shock resistant.

### NOR vs NAND Flash

| Characteristic | NOR Flash | NAND Flash |
|---------------|-----------|------------|
| **Access** | Random (byte-addressable) | Block-oriented (2048-byte min) |
| **Read time** | 0.08 µs | 25 µs |
| **Write time** | 10 µs | 1500 µs erase + 250 µs write |
| **Read BW** | 10 MB/s | 40 MB/s |
| **Write BW** | 0.4 MB/s | 8 MB/s |
| **Wearout** | 100,000 writes/cell | 10,000–100,000 writes/cell |
| **Cost/GB (2008)** | $65 | $4 |
| **Typical use** | BIOS | USB keys, SSDs |

### Key Flash Concepts

- **Wear leveling** — controller remaps heavily-written blocks to less-used ones
- Flash bits **wear out** (unlike disks/DRAM) — limits write cycles
- **Hybrid hard disks** — combine ~1 GB flash + disk for faster boot and energy savings

---

## 6.5 Connecting Processors, Memory, and I/O Devices

### Bus vs Network Interconnects

- **Bus** — shared communication link; versatile, low cost, but creates bottleneck
- **Modern trend** — transitioned from parallel shared buses to **high-speed serial point-to-point interconnections**

### Bus Types

| Type | Characteristics |
|------|----------------|
| **Processor-memory bus** | Short, high speed, matched to memory system |
| **I/O bus** | Long, many device types, wide bandwidth range |
| **Backplane bus** | Allows processors, memory, and I/O to coexist on single bus |

### Synchronous vs Asynchronous

- **Synchronous bus** — clocked, fixed protocol, fast but limited length (clock skew)
- **Asynchronous interconnect** — no clock, uses **handshaking protocol**, accommodates diverse devices and longer distances

### I/O Standards Comparison

| Standard | Use | Devices | Peak BW | Max Length |
|----------|-----|---------|---------|------------|
| **Firewire (1394)** | External | 63 | 50–100 MB/s | 4.5 m |
| **USB 2.0** | External | 127 | 1.5–60 MB/s | 5 m |
| **PCI Express** | Internal | 1/lane | 250 MB/s per lane (scalable ×32) | 0.5 m |
| **Serial ATA** | Internal | 1 | 300 MB/s | 1 m |
| **Serial Attached SCSI** | External | 4 | 300 MB/s | 8 m |

### x86 I/O Architecture

```
Processor ←→ North Bridge (memory controller hub) ←→ South Bridge (I/O controller hub)
                  ↕                                        ↕
            Memory / GPU                           USB, SATA, PCIe, PCI, etc.
```

- **North bridge** = DMA controller + memory + graphics
- **South bridge** = I/O hub connecting diverse peripherals
- Modern trend: north bridge absorbed into the processor die (e.g., AMD Opteron, Intel Nehalem)

---

## 6.6 Interfacing I/O Devices to the Processor

### OS Responsibilities for I/O

1. **Protection** — user programs can't access I/O devices directly
2. **Abstraction** — provide high-level device access routines
3. **Interrupt handling** — handle I/O interrupts (like exceptions)
4. **Scheduling** — equitable access, throughput optimization

### Giving Commands to I/O Devices

**Memory-mapped I/O** — portions of address space assigned to I/O devices; reads/writes to those addresses are interpreted as device commands. User programs are blocked via address translation protection.

### Three Communication Methods

#### 1. Polling

Processor periodically checks device **Status register**. Simple, predictable, good for real-time embedded.

**Problem:** wastes processor time — processor is orders of magnitude faster than I/O devices.

#### 2. Interrupt-Driven I/O

Device signals processor via **interrupt** when it needs attention. Two key distinctions from other exceptions:

- **Asynchronous** — not tied to any instruction
- **Conveys information** — device identity, priority level

**Interrupt priority levels:**
- MIPS provides Status register (interrupt mask, enable) and Cause register (pending interrupts, exception code)
- OS implements Interrupt Priority Levels (IPLs) by manipulating the mask field

```
Steps for handling interrupt:
1. AND pending interrupts with mask → find enabled candidates
2. Select highest priority
3. Save mask, then disable equal/lower priority interrupts
4. Save processor state
5. Enable higher-priority interrupts
6. Call interrupt handler
7. On return: disable interrupts, restore mask and state
```

#### 3. Direct Memory Access (DMA)

Device controller transfers data **directly to/from memory** without processor involvement.

```
DMA Transfer Steps:
1. Processor sets up DMA: device ID, operation, memory address, byte count
2. DMA performs transfer autonomously (becomes bus master)
3. DMA interrupts processor on completion (or error)
```

**Advantage:** frees processor during large block transfers
**Complication:** DMA bypasses cache → **stale data problem**

Solutions to DMA coherence:
- Route I/O through cache (expensive, may displace useful data)
- OS **cache flushing** — selective invalidation/writeback before/after DMA
- Hardware cache invalidation (used in multiprocessors, applicable to I/O)

### Data Transfer Spectrum

```
Polling ←→ Interrupt-Driven ←→ DMA
Lowest cost                      Lowest processor utilization
Lowest latency (single device)   Best for high-bandwidth devices
```

---

## 6.7 I/O Performance Measures

### Terminology Trap

- I/O systems use **base 10**: 1 GB = 10⁹ bytes
- Memory uses **base 2**: 1 GB = 2³⁰ = 1,073,741,824 bytes

### Transaction Processing Benchmarks (TPC)

| Benchmark | Focus |
|-----------|-------|
| **TPC-C** | Complex query environment (since 1992) |
| **TPC-H** | Ad hoc decision support |
| **TPC-W** | Web-based transactional |
| **TPC-E** | Brokerage firm workload |

All measure **transactions per second** with response time constraints and scaling requirements.

### File/Web Benchmarks

- **SPECSFS** — NFS file server throughput + response time
- **SPECWeb** — Web server (static + dynamic pages)
- **SPECPower** — Power and performance of small servers
- **filebench** — Configurable file system benchmark framework

---

## 6.8 Designing an I/O System

Methodology:

1. **Find the weakest link** — component constraining the I/O path
2. **Configure that component** to sustain required bandwidth
3. **Configure the rest** to match

---

## 6.9 RAID — Redundant Arrays of Inexpensive Disks

### Why RAID?

Amdahl's Law applies to I/O: if CPU improves but I/O doesn't, I/O dominates:

```
Year 0: CPU 90s + I/O 10s = 100s (I/O = 10%)
Year 6: CPU 11s + I/O 10s = 21s  (I/O = 47%)
```

### RAID Levels

| Level | Name | Redundancy | Disks Needed | Key Property |
|-------|------|-----------|--------------|-------------|
| **RAID 0** | Striping | None | n | Data spread across disks; highest performance, no fault tolerance |
| **RAID 1** | Mirroring | Full copy | 2n | Most expensive; simplest recovery |
| **RAID 2** | ECC | Error correcting code | n + check disks | Fallen into disuse |
| **RAID 3** | Bit-interleaved parity | 1 parity disk | n + 1 | Every access goes to all disks; good for large transfers |
| **RAID 4** | Block-interleaved parity | 1 parity disk | n + 1 | Small reads can be independent; parity disk is write bottleneck |
| **RAID 5** | Distributed block-interleaved parity | Distributed parity | n + 1 | Parity spread across all disks; eliminates RAID 4 bottleneck |
| **RAID 6** | P + Q redundancy | Two check disks | n + 2 | Tolerates two simultaneous failures |

### Key RAID Concepts

- **Striping** — logically sequential blocks allocated to separate disks
- **Mirroring** — writing identical data to multiple disks
- **Protection group** — set of data disks sharing a common check disk/block
- **Parity** — sum modulo 2; reconstruct failed disk by subtracting good data from parity

### RAID Tradeoffs

| Aspect | Mirroring (RAID 1) | Parity (RAID 3–6) |
|--------|-------------------|-------------------|
| **Cost** | Highest (2× disks) | Lower (1–2 extra disks) |
| **Recovery** | Fastest (just read mirror) | Slower (must read all other disks) |
| **Write performance** | Write to both copies | Must update parity |

---

## 6.10 Fallacies and Pitfalls

1. **Don't use AMAT for out-of-order processors** — if the processor continues executing during cache misses, only full simulation gives accurate results

2. **Disk access time ≠ advertised seek time** — measured average seek is often 25–33% of advertised due to locality

3. **Sector-track-cylinder model is outdated** — high-level interfaces (ATA, SCSI) organize disks like tapes; sequential blocks may be on different tracks

---

## Key Takeaways

- I/O design trades off **dependability, cost, performance, and expandability**
- **Availability** = MTTF / (MTTF + MTTR) — reducing repair time helps as much as reducing failures
- **Disk access** = seek + rotational latency + transfer + controller overhead
- **Flash** is 100–1000× faster than disk but has write endurance limits
- **DMA** offloads large transfers from processor but creates cache coherence issues
- **RAID** provides fault tolerance for storage; RAID 5/6 are the most common production choices

---

## Related

- [[05_Memory_Hierarchy]] — Cache and virtual memory concepts that underpin I/O coherence
- [[07_Parallel_Computing]] — Parallelism extends to I/O (RAID, multiprocessor coherence)
- [[Computer Organization Overview]] — Chapter map and reading paths
