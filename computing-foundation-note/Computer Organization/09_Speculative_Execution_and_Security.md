---
title: "Speculative Execution and Security"
source: "Spectre/Meltdown papers (Kocher et al., Lipp et al., 2018) + subsequent transient-execution research"
chapter: 9
tags:
  - computer-architecture
  - computer-organization
  - co-and-d
  - spectre
  - meltdown
  - side-channel
  - speculative-execution
  - security
  - kubernetes
---

# 09 Speculative Execution and Security

> **Why this note exists:** [[04_Processor_Design]] teaches speculation as a *performance* technique, the machine guesses, and a wrong guess is rolled back. In 2018 researchers proved the rollback is incomplete: guessed execution leaves fingerprints in the **cache**, and those fingerprints can be measured across privilege boundaries. This note covers the mechanism, the named attacks, the mitigations you actually toggle as an operator, and what it costs you. Everything here assumes you understand branch prediction and out-of-order execution from Chapter 4.

---

## 9.1 Recap: Why Processors Speculate

From [[04_Processor_Design]] and [[05_Memory_Hierarchy]]:

- **The memory wall.** A main-memory load takes ~100 ns / hundreds of cycles. If the pipeline stalled on every branch until it resolved (and every load until it returned), IPC would collapse.
- **Branch prediction.** The processor *guesses* taken/not-taken and a target address, then keeps fetching and executing down the guessed path. Correct guess: full speed. Wrong guess: **architectural state is rolled back:** no register writes commit, no stores become visible.
- **Out-of-order execution.** Independent instructions proceed past unresolved ones; results retire in program order.

The design contract for decades: *speculative state is invisible; rollback makes it as if the guess never happened.* The 2018 attacks broke that contract.

---

## 9.2 The Core Leak Mechanism

### Rollback forgets the cache

When a speculative load executes, it **brings data into the cache**, and cache state is *microarchitectural*, not architectural. On rollback the CPU undoes registers and flags but **does not evict the cache lines the speculation touched**. The secret data itself is discarded; the fact that *some address is now cached* survives.

### Measuring it: cache timing side channels

An attacker times accesses to infer what is cached (fast, ~4 cycles L1) vs not (~100+ cycles DRAM):

**Flush+Reload (step by step)**

1. Attacker and victim share a read-only page (e.g. a library). Attacker `clflush`es a chosen cache line, forces it out of all cache levels.
2. Attacker waits a short window while the victim runs. If the victim (or the victim's speculation) touches that line, it is back in cache.
3. Attacker reloads the line and times it: **fast hit ⇒ victim touched it**; slow miss ⇒ it didn't.
4. Repeat over an array, one line per candidate value → reconstruct a secret byte-by-byte.

**Prime+Probe** (no shared page needed): attacker *fills* a cache set with its own data, lets the victim run, then re-reads its lines, lines the victim's speculation evicted show as misses. Coarser (set granularity, not line) but works across privilege domains.

### The transient-execution gadget

The generic attack shape used by nearly every variant below:

```
if (condition_attacker_cannot_see_resolve_fast) {   // mispredicted
    secret = protected_memory[x];                    // executes speculatively
    probe_array[secret * 4096];                      // encodes secret into cache
}
// misprediction detected → architectural rollback
// but probe_array[secret*4096] is CACHED → attacker times the array
```

The whole field is now named: **"transient execution attacks."**

---

## 9.3 Meltdown (CVE-2017-5754)

- **What:** *Rogue data cache loads.* Unprivileged user code reads **kernel memory** (physical RAM of any process and the kernel itself) at up to hundreds of KB/s.
- **Who:** Intel only (empirically; certain ARM Cortex-A75 also affected). **Not AMD**; its speculation aborts the load on a permission fault before data arrives.
- **Why it works:** On affected Intel cores, a load from a kernel address from user mode *does* retrieve data speculatively; the permission check (page fault) is raised **after** the data is already forwarded to dependent speculative instructions. The fault eventually kills the architectural result, but the dependent instruction already encoded the secret byte into the cache. The window between "data available" and "fault takes effect" is the bug.

```asm
; user mode - kernel_addr not mapped for user
mov  al, byte [kernel_addr]      ; faults eventually...
shl  eax, 12                     ; ...but speculation continues:
mov  ebx, byte [probe + rax]     ; secret byte encoded into cache line
```

- **Fix:** **KPTI / KAISER** (Kernel Page Table Isolation). Two page tables per process: a user one that maps *nothing* of the kernel (except minimal trampolines), and a full one used in kernel mode. Every syscall/interrupt now swaps CR3 and flushes TLB state → the "5–30% syscall cost" (PCID support softens it). Shipped in Linux 4.15 / Windows Jan-2018 patches.

---

## 9.4 Spectre v1: Bounds-Check Bypass (CVE-2017-5753)

- **What:** Trick the branch predictor into speculating past a bounds check the CPU's own logic would fail:

```c
if (x < array1_size) {          // train: always true for a while...
    y = array2[array1[x] * 64]; // ...then call with huge x:
}                               // speculation reads OOB array1[x] (a secret)
                                // and encodes it into array2's cache footprint
```

- **Why it works:** the predictor learns "this branch is taken"; the attacker then supplies an out-of-bounds `x`. The check hasn't resolved (size may itself be a slow load), so the body executes speculatively with the secret.
- **Who:** *Everyone.* Intel, AMD, ARM, and in principle any high-performance OoO core; it is inherent to speculation itself, not a single vendor's shortcut. The original paper: Kocher et al., *"Spectre Attacks: Exploiting Speculative Execution"* (IEEE S&P 2019).
- **Fixes: all partial:**
  - **Speculation barriers:** `lfence` (x86) after sensitive checks stops speculation until the check retires; ARMv8 added CSDB/SSBS controls. Expensive if applied broadly.
  - **Retpoline:** mainly for v2 (below) but part of the same toolkit.
  - **Compiler mitigations:** bounds-check with masking (`index &= size-1` so an OOB guess reads a benign in-bounds slot) instead of a branch; speculative-load-hardening passes in LLVM/GCC.
- **Status:** **no complete fix.** This is the "live with it" class: speculation is the performance, and you cannot fully remove the guess without removing the speedup. Vendors ship targeted barriers where the cost is acceptable.

---

## 9.5 Spectre v2: Branch Target Injection (CVE-2017-5715)

- **What:** Instead of mispredicting a *conditional* branch, poison the **indirect branch target predictor** (BTB). The attacker, from one context, trains the predictor so that when the victim (another context, e.g. the kernel via a syscall) reaches an indirect jump/call, the CPU speculatively runs an **attacker-chosen gadget** at a victim-context address, leaking data via the cache channel.
- **Fixes:**
  - **Retpoline:** compile indirect branches into a `call` to a thunk that loops on itself speculatively (`jmp *%reg` replaced by a self-trapping construct); speculation cannot escape to an attacker-controlled target. Cheap, widely deployed; requires compiler support.
  - **IBRS** (Indirect Branch Restricted Speculation): microcode flag: kernel sets it so user-mode training can't influence kernel indirect branches. Early IBRS was slow; **STIBP** extends it per-SMT-thread.
  - **IBPB** (Indirect Branch Prediction Barrier): flush the BTB on context switch; used at privilege transitions.
  - **eIBRS** (enhanced IBRS): hardware default on Intel Ice Lake+ / newer: isolation without the software cost, though later research (BHI, 2022) showed even eIBRS could be influenced within the same privilege level.

---

## 9.6 MDS Family: Microarchitectural Data Sampling (2019)

**ZombieLoad (CVE-2018-12130), RIDL (CVE-2018-12127), Fallout (CVE-2018-12126), RIDL/MSBDS variants.**

- **What:** Instead of leaking data *your own speculation fetched*, these sample stale data sitting in **CPU-internal buffers** (line-fill buffers (LFB), store buffer, load ports) left behind by *other* logical cores / SMT siblings / the kernel / enclaves. A faulting or specially-crafted load speculatively forwards whatever garbage is in the buffer into the cache channel.
- **Impact:** cross-SMT-thread leakage is the scary one: two VMs or containers sharing physical cores via hyperthreading can read each other's secrets.
- **Fixes:** microcode updates (**VERW** instruction repurposed to flush the buffers at privilege transitions) + **disabling SMT** in hostile multi-tenant environments (the only fully safe option at the time). Intel Cascade Lake+ added hardware partitioning of these buffers.

### L1TF / Foreshadow (CVE-2018-3615 et al.)

One-line row: speculative loads that fault on unmapped/L1-present-invalid PTEs return **stale L1 data**, breaking Intel SGX enclaves and enabling VMM→guest leakage; fixed by microcode + L1D flush on VM entry + optional SMT-off, and the root of the "L1TF" entry you'll see in `/sys`.

---

## 9.7 Timeline: 2018 → 2026

| Year | Attack | Vendor/scope | Essence |
|------|--------|--------------|---------|
| Jan 2018 | **Meltdown** | Intel | User reads kernel memory via faulting-speculation window |
| Jan 2018 | **Spectre v1/v2** | All | Bounds-check bypass; indirect-branch injection |
| Jul 2018 | **L1TF/Foreshadow** | Intel | L1 stale data on faulting loads; SGX broken |
| May 2019 | **MDS: ZombieLoad/RIDL/Fallout** | Intel | Sample line-fill/store buffers across SMT |
| Nov 2019 | TAA, iTLB multihit | Intel | More MDS-family buffer sampling |
| 2020 | L1D eviction (CacheOut sibling) | Intel | Cache-line eviction sampling |
| 2022 | **Spectre-BHB** | Intel+ARM | Branch *history* buffer injects in-kernel speculation targets even with eIBRS/CSV2 |
| 2022 | **ÆPIC Leak** | Intel (Ice Lake+) | Uninitialized APIC MMIO reads leak data ;  *no speculation needed*; CDSA |
| 2022 | **Retbleed** | Intel+AMD | Return-instruction training → kernel leak via existing retpoline paths |
| 2023 | **Downfall / GDS** (CVE-2022-40982) | Intel (AVX/AVX-512 gather) | Gather instructions transiently expose SIMD-register data from other SMT threads |
| 2023 | **Inception / SRSO** (CVE-2023-20569) | AMD (Zen 1–4) | Phantom-call training → arbitrary kernel speculation; AMD's Spectre-v2-class problem |
| 2023 | **SLAM** | Intel/AMD/ARM | Linear-address masking weakens kernel ASLR → amplifies any L1 leak ~1000× |
| 2024 | **Native BHI** (CVE-2024-2201) | Intel (even eIBRS-era parts) | Branch-history injection fully in-kernel, no user gadget needed |
| 2024–2026 | RFDS-class, further RIDL/MDS refinements, RISC-V transient-execution papers | Intel Atom; RISC-V cores | The cadence is **annual:** each new microarchitecture ships new transient channels |

> **Takeaway from the table:** 2018 was not a one-off scandal. Transient-execution leakage is a *structural property* of speculative OoO designs, and researchers keep finding new buffers, predictors, and windows. Intel, AMD, and Arm now all publish **transient-execution guidance documents** as a permanent architecture-class disclosure.

---

## 9.8 Why Cloud Cares More Than Your Homelab

- **The threat model is co-residency.** These attacks leak across a *shared physical core* (SMT siblings) or across privilege boundaries on the *same machine*. Multi-tenant clouds run untrusted strangers on adjacent hyperthreads; real, demonstrated cross-VM theft of keys.
- **Single-user desktop:** the attacker would need to already run code on your machine (browser sandbox escape is the realistic vector, which is why browsers got their own mitigations: site isolation). Practical risk for a home server or dev laptop: **low**.
- **Kubernetes / node guidance:**
  - Containers share the host kernel → Meltdown-class is a container-escape-to-host-kernel-memory vector *if* KPTI is off. Keep host kernels patched; that's table stakes.
  - On multi-tenant clusters where tenants are untrusted: disable SMT on worker nodes (`nosmt=1` / `mitigations=auto,nosmt`), or use node isolation (dedicated node pools per tenant, taints/tolerations), or bare-metal-per-tenant.
  - Cloud providers handle the hardware side (their hypervisors insert VERW/L1D flushes at VM boundaries); your job is kernel currency + workload placement policy.
  - Don't blanket-disable mitigations (`mitigations=off`) on anything multi-tenant for a few percent of benchmark; that flag exists for dedicated compute rigs.

---

## 9.9 Mitigations Cheat Table

| Mitigation | Attacks addressed | Where it lives | Cost |
|------------|-------------------|----------------|------|
| **KPTI** (KAISER) | Meltdown, L1TF | Kernel page tables (CR3 switch per transition) | Syscall/IRQ-heavy: ~1–30% (worse pre-PCID); compute: ~0–3% |
| **Retpoline** | Spectre v2, Retbleed (partially) | Compiler (`-mindirect-branch=thunk`) + kernel | Indirect-call overhead; ~0–5% typical, more on syscall-heavy code |
| **IBRS / eIBRS** | Spectre v2 | Microcode / hardware (Ice Lake+) | Legacy IBRS: up to ~10%; eIBRS: near-free but needed BHI patches |
| **IBPB / STIBP** | Spectre v2 cross-context | Microcode, at context switches | Context-switch-heavy workloads a few % |
| **SSBD** (Speculative Store Bypass Disable) | Spectre v4 (store-to-load forwarding) | Microcode | ~2–4% on some workloads |
| **VERW buffer clear + L1D flush** | MDS family, L1TF, TAA | Microcode at privilege/VM transitions | Small steady overhead; SMT-off is the nuclear option |
| **SMT disabled** | All cross-thread (MDS, GDS, ÆPIC) | Boot param | Loses ~20–30% throughput on SMT-friendly workloads ;  the isolation you buy |
| **Microcode updates generally** | Everything vendor-patched | BIOS/UEFI + OS microcode packages (`intel-microcode`, `amd64-microcode`) | Mandatory; keep current ;  this is the operator's #1 action |
| **Compiler speculation barriers/masking** | Spectre v1 | LLVM/GCC flags, kernel source | Localized; lfence-per-check would be ruinous if blanket |

---

## 9.10 Check Your Own Machine (Linux)

```bash
# one file per known vulnerability class
ls /sys/devices/system/cpu/vulnerabilities/
# bug_status mitigation_status
cat /sys/devices/system/cpu/vulnerabilities/meltdown
# → Mitigation: PTI
cat /sys/devices/system/cpu/vulnerabilities/spectre_v1
# → Mitigation: usercopy/swapgs barriers and __user pointer sanitization
cat /sys/devices/system/cpu/vulnerabilities/spectre_v2
# → Mitigation: Enhanced IBRS, IBPB: conditional, RSB filling, PBRSB-eIBRS: SW sequence
cat /sys/devices/system/cpu/vulnerabilities/mds
# → Mitigation: Clear CPU buffers; SMT vulnerable
cat /sys/devices/system/cpu/vulnerabilities/l1tf
cat /sys/devices/system/cpu/vulnerabilities/spec_store_bypass
cat /sys/devices/system/cpu/vulnerabilities/gather_data_sampling   # Downfall
cat /sys/devices/system/cpu/vulnerabilities/retbleed
cat /sys/devices/system/cpu/vulnerabilities/spec_rstack_overflow   # Inception/SRSO

# the bugs= field lists everything the kernel knows this CPU model has
lscpu | grep -i bugs
grep . /sys/devices/system/cpu/vulnerabilities/* | sed 's|/sys/devices/system/cpu/vulnerabilities/||'

# boot-time mitigation selection
cat /proc/cmdline    # look for mitigations=..., nosmt, pti=off etc.
dmesg | grep -iE 'spectre|meltdown|mds|l1tf|mitigat'
```

`Vulnerable` with no mitigation text = update microcode/kernel now. macOS/Windows expose equivalents via their update channels; windows: `Get-SpeculationControlSettings` (PowerShell).

---

## 9.11 Performance Cost: Reality Table

| Workload profile | Typical cost with full mitigations | Notes |
|------------------|-----------------------------------|-------|
| Compute-bound (HPC, tight loops, little syscall/IO) | **~0–3%** | Speculation still works; only barriers at transitions |
| Syscall/IO-heavy (network servers, databases, NGINX-style) | **~3–15%** | KPTI CR3 swaps + TLB effects dominate; PCID/INVPCID recover much of it |
| Pathological (micro-benchmark syscall storms, tiny packets) | **up to ~30%** | The worst-case headline numbers come from here ;  don't plan capacity on it |
| Virtualization hosts (VM entry/exit heavy) | ~2–10% | L1D flush + buffer clears per transition |
| SMT disabled (paranoid multi-tenant) | −20–30% throughput | Not a "cost %" ;  a capacity decision |

The honest summary: **post-2018 hardware+kernel stacks lost low-single-digit performance on average**, concentrated in transition-heavy workloads. The 2018 headlines of "30% slowdown" measured syscall microbenchmarks, not real fleets.

---

## 9.12 The Philosophical Takeaway

- For 25 years the industry **traded security for performance without pricing it:** speculation assumed the microarchitectural side effects of wrong guesses were unobservable. They were observable, via the cache, from day one of out-of-order designs.
- The ISA vendors now formally document **"transient execution attacks"** as an architecture class with standing guidance (Intel's *Transient Execution Attack Mitigations*, Arm's *Cache Speculation Side-channels*, AMD's equivalent whitepapers). Security review of a new core now includes "what does speculation leak?"
- **Relative exposure:** AMD historically escaped Meltdown and MDS (fault-before-forward, partitioned buffers) but got Retbleed and Inception. ARM documented the theoretical issue early (Cortex-A75 was Meltdown-class) and shipped CSV2/CSV3 hardware guarantees against v2-style injection faster; apple Silicon and Graviton benefited from later, security-reviewed designs. RISC-V cores (being younger, often in-order, and less speculatively aggressive) have a smaller transient surface *so far*, and the community is writing the RVWMO-era guidance before, rather than after, the first big CVE. There is no speculation-free lunch: any RISC-V core chasing ARM-class IPC will face the same physics.
- **For the operator:** keep microcode and kernels current, know your tenancy model, and treat `mitigations=` and SMT settings as capacity/security tradeoffs, not as trivia.

---

## Sources

- Kocher, Horn, Fogh, Genkin, Gruss, Haas, Hamburg, Lipp, Mangard, Prescher, Schwarz, Yarom, *"Spectre Attacks: Exploiting Speculative Execution"*, IEEE S&P 2019 (arXiv:1801.01203).
- Lipp, Schwarz, Gruss, Prescher, Haas, Fogh, Horn, Mangard, Kocher, Genkin, Yarom, Hamburg, *"Meltdown: Reading Kernel Memory from User Space"*, USENIX Security 2018 (arXiv:1801.01207).
- Van Bulck et al., *"Foreshadow: Extracting the Keys to the Intel SGX Kingdom"* (L1TF), USENIX Security 2018.
- Schwarz, Lipp, Gruss et al., *"ZombieLoad: Cross-Privilege-Boundary Data Sampling"*, CCS 2019 (MDS family).
- Moghadam et al., *"ÆPIC Leak"*, USENIX Security 2022; wikner & Razavi, *"Retbleed"*, USENIX Security 2022; feyten et al., *"Spectre-BHB"* (Arm/Intel BHI disclosure), 2022.
- Moghadam et al., *"Downfall: Revealing Intel's Transient Data Sampling"* (GDS, CVE-2022-40982), 2023 (gathering.net/downfall); wikner & Razavi, *"Inception"* (SRSO, CVE-2023-20569), ETH Zürich 2023; *SLAM* (VUSec, 2023); koruyeh et al./Intel, *Native BHI* (CVE-2024-2201), 2024.
- Hennessy & Patterson, *Computer Architecture: A Quantitative Approach*, 6th ed.; branch prediction & speculation (Ch. 3); Patterson & Hennessy, *Computer Organization and Design* Ch. 4 (this vault's [[04_Processor_Design]]).
- Intel Software Developer Manual Vol. 3C + *Intel Analysis of Speculative Execution Side Channels* / transient-execution mitigation guidance; AMD64 Programmer's Manual Vol. 2 + AMD *Speculative Return Stack Overflow* and Retbleed whitepapers; arm *Cache Speculation Side-channels* (developer.arm.com), CSV2/CSV3 feature documentation.
- Linux kernel documentation: `Documentation/admin-guide/hw-vuln/*.rst`; KAISER paper (Gruss et al., 2016) for KPTI design.
- Canella et al. (*"A Systematic Evaluation of Transient Execution Attacks and Defenses"*, ACM Computing Surveys 2019) the best single taxonomy.

---

**Related notes:** [[04_Processor_Design]] · [[05_Memory_Hierarchy]] · [[02_Instruction_Set_Architecture]] · [[08_Modern_ISAs_x86_ARM64_RISCV]]
