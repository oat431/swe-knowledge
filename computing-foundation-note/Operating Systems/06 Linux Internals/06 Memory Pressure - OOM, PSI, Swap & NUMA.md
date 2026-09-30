---
tags:
- linux
- os
- programming
- memory
---

# 06 Memory Pressure — OOM, PSI, Swap & NUMA

What the kernel does when RAM runs out, in order: reclaim the page cache, push anonymous pages to swap, and if that still fails — kill something. Every Linux incident postmortem eventually lands here.

---

## The Reclaim Pipeline

```
Allocation request, no free pages
  ▼
1. Reclaim clean page cache        ← free, instant (file pages, re-readable)
  ▼
2. Write back dirty page cache     ← costs I/O
  ▼
3. Swap out anonymous pages        ← heap/stack; only if swap exists
  ▼   (kswapd does this in background when watermarks are low;
  ▼    direct reclaim does it synchronously in the allocating task = latency spike)
4. Compaction (defragment for huge pages)
  ▼
5. OOM killer                      ← nothing reclaimable: pick a victim, SIGKILL
```

Basics of paging/TLB/replacement in [[02 Memory Management]]; block device & page cache mechanics in [[02 I O & Devices]].

---

## The OOM Killer

> `oom_score` ≈ **fraction of RAM used** (per `/proc/<pid>/oom_score`), adjusted by `oom_score_adj` (-1000..+1000, added as a permille of total RAM).

```bash
cat /proc/<pid>/oom_score          # current score
cat /proc/<pid>/oom_score_adj      # the knob
echo -1000 > /proc/<pid>/oom_score_adj   # effectively unkillable (like PID 1)
echo -999  > /proc/$(pidof mydb)/oom_score_adj  # "please not me" — common for critical daemons
```

- **Biggest memory user usually dies** — that's the design: kill one hog, save the system
- **PID 1 is protected** (oom_score_adj forced low) — killing init takes the whole box down; processes running as root get a small score discount
- Post-mortem evidence lives in `dmesg` / `journalctl -k`:

```
Out of memory: Killed process 4242 (java) total-vm:9830412kB, anon-rss:7998120kB...
oom-kill:constraint=CONSTRAINT_NONE,nodemask=NULL,...   ← CONSTRAINT_MEMCG = cgroup OOM
```

Per-service, declaratively: `OOMScoreAdjust=-999` in a systemd unit. Better still: prevent the OOM proactively with **systemd-oomd** (PSI-based, see below).

---

## Container OOMKill vs Host OOM

Same kernel code, different scope:

| | Host OOM | Cgroup/container OOM |
|---|----------|----------------------|
| Trigger | Global memory exhaustion | Task exceeds `memory.max` of **its cgroup** |
| Victim | Highest oom_score system-wide | A process **inside that cgroup only** |
| Evidence | `dmesg`: `CONSTRAINT_NONE` | `dmesg`: `CONSTRAINT_MEMCG`; K8s: `OOMKilled` in `kubectl describe pod`; Docker: `docker inspect` → `OOMKilled: true`, **exit code 137** (128 + SIGKILL(9)) |

- Exit 137 is *always* "something SIGKILLed it" — OOM is the common case, `docker stop` timeout is the other
- K8s twist: container hit its **limits** → `OOMKilled` + restart per restartPolicy; node-level pressure → **kubelet eviction** instead (pod shows `Evicted`, not OOM)
- cgroups v1 vs v2 mechanics: [[05 Containers - Namespaces & Cgroups]]

> The kernel doesn't know "containers." A cgroup memory limit breach runs the same OOM path with `CONSTRAINT_MEMCG`.

---

## Swap

`/proc/sys/vm/swappiness` (0–100): **a balance knob, not a percentage.** It biases reclaim between page cache and anonymous pages — `swappiness=60` (default) does *not* mean "use 60% of swap."

- `0` = reclaim anon pages only as a last resort (still not *never* — pre-5.8 kernels could still swap at 0)
- `100` = treat page cache and anon pages as equals
- Common prod tuning: `10–30` for latency-sensitive DBs

### Compressed swap

| | What | Trade-off |
|---|------|-----------|
| **zram** | Compressed block device **in RAM** used as swap | CPU for capacity: ~2–3x effective RAM, no disk. Default on Fedora, Android, ChromeOS |
| **zswap** | Compressed **write-back cache** in front of a real disk swap | Cheap pages never hit disk; falls back to swap when full |

### "Disable swap for Kubernetes"?

The old rule (`kubelet` refused to start with swap on) existed because swap broke cgroup memory accounting and QoS guarantees — a container could silently page out instead of being OOMKilled at its limit. **Changing since K8s 1.28 (alpha) → 1.30+ (beta, `NodeSwap`)**: swap is supported with `LimitedSwap` (Burstable pods only, swap capped by memory request ratio), because swap cuts cost and softens memory spikes when configured deliberately. Still off in most managed clusters in 2025.

---

## PSI — Pressure Stall Information (kernel 4.20+)

> Instead of "are we out of memory?" (a cliff), PSI answers "**what % of time were tasks stalled waiting for a resource?**" — a continuous pressure signal.

```bash
cat /proc/pressure/memory
# some avg10=12.50 avg60=4.10 avg300=1.02 total=8841234
# full avg10=3.01  avg60=0.80 avg300=0.10 total=1023456
```

| Line | Meaning |
|------|---------|
| `some` | **At least one** task stalled on the resource |
| `full` | **All non-idle** tasks stalled simultaneously = real throughput loss |

Also `/proc/pressure/cpu` and `/proc/pressure/io`, plus per-cgroup versions (`memory.pressure` inside a container's cgroup).

**Who uses it:**
- **systemd-oomd** — kills/restarts units *before* the kernel OOM does, on pressure thresholds (`ManagedOOMMemoryPressure=`); Facebook runs this fleet-wide
- Meta's original use case: detect degraded hosts from memory pressure rather than waiting for OOM bodies
- Monitoring: alert on `full avg60 > N` — **early warning**, vs OOM which is strictly after-the-fact

```bash
systemctl enable --now systemd-oomd
oomctl                                 # current pressure + kill policies per unit
```

---

## Huge Pages

A 4 KiB page → TLB entry covers 4 KiB. A **2 MiB** huge page → one TLB entry covers 512x more; **1 GiB** pages cover 262144x more. Fewer TLB misses for big-heap workloads (see TLB section in [[02 Memory Management]]).

### Transparent Huge Pages (THP)

Kernel auto-promotes to 2 MiB pages. Mode in `/sys/kernel/mm/transparent_hugepage/enabled`:

| Mode | Behavior |
|------|----------|
| `always` | Aggressively promote everything |
| `madvise` | Only where the app opts in (`madvise(MADV_HUGEPAGE)`) |
| `never` | Off |

**Why Redis/MongoDB/Oracle say `never` (or `madvise`):**
- `khugepaged` promotion + compaction causes **random multi-hundred-ms latency spikes**
- Copy-on-write after `fork()` (RDB/AOF snapshots!) amplifies memory: touching one byte copies a **2 MiB** page instead of 4 KiB

### hugetlbfs — Explicit

Pre-reserved, guaranteed (no runtime compaction fail), faulted via `mmap(MAP_HUGETLB)` or mounting `hugetlbfs`:

```bash
echo 1024 > /proc/sys/vm/nr_hugepages      # reserve 1024 × 2MiB = 2 GiB
grep Huge /proc/meminfo
# JVM: -XX:+UseLargePages
```

DPDK, KVM guest memory, and serious DB deployments use hugetlbfs; app-controlled THP uses madvise.

---

## NUMA — Non-Uniform Memory Access

> Multi-socket (and modern chiplet/EPYC) machines: each CPU has **local** memory; reaching another node's memory crosses an interconnect (UPI/Infinity Fabric) at **~1.5–2x latency** and less bandwidth.

```
┌─────────────┐  UPI/xGMI (~1.5–2x slower)  ┌─────────────┐
│ Node 0      │◄───────────────────────────►│ Node 1      │
│ CPU 0-31    │                             │ CPU 32-63   │
│ RAM 128GB   │                             │ RAM 128GB   │
└─────────────┘                             └─────────────┘
  local access: ~80ns                         remote: ~140ns
```

```bash
numactl --hardware            # nodes, sizes, distances
numastat -p <pid>             # per-node allocation of a process
numactl --cpunodebind=0 --membind=0 mysqld    # pin DB to node 0
```

- **DB pinning:** MySQL/Postgres/Redis latency tails shrink when CPU+memory stay on one node; alternatively `numactl --interleave=all` spreads evenly (Oracle's classic recommendation)
- **vm.zone_reclaim_mode**: 0 = prefer remote memory over local reclaim (right for most; reclaim-stall latency > remote-access latency)
- **Cloud VMs mostly hide it:** a vCPU is typically carved from one socket with local memory — `numactl --hardware` shows a single node. Bare-metal DB hosts and EPYC machines are where it bites.

---

## Fragmentation & Compaction

Long uptime + heavy alloc/free → free pages exist but are scattered as single 4 KiB pages; a 2 MiB THP or hugetlb allocation can't find contiguous space. Kernel **compaction** migrates pages to coalesce free blocks — it works, but it runs at allocation time and can stall (`/proc/vmstat` → `compact_stall`). Check fragmentation: `/proc/buddyinfo` (free blocks by order — empty high orders = fragmented), `/sys/kernel/debug/extfrag/extfrag_index`.

---

## Monitoring Cheat Sheet

| Command | What to read |
|---------|--------------|
| `free -h` | **"available" is the number that matters.** `buff/cache` is reclaimable — a "full" machine with big cache is *healthy*, not leaking. `free` ≈ useless |
| `vmstat 1` | **`si`/`so`** sustained > 0 = actively swapping → pressure now. Also `wa` (iowait), `cs` |
| `/proc/meminfo` | **`MemAvailable`** (reclaim estimate), **`Committed_AS`** (total promised memory — vs `CommitLimit`), `SwapFree`, `AnonPages`, `Slab` |
| `sar -B` | pgscan/pgsteal rates — **direct** reclaim scans = latency spikes; pgmajfault |
| `cat /proc/pressure/*` | Live PSI stall percentages |
| `dmesg \| grep -i oom` | Kill history |
| `smem -k`, `/proc/<pid>/smaps_rollup` | Per-process PSS/RSS truth |

```bash
grep -E 'MemAvailable|Committed_AS|SwapFree' /proc/meminfo
```

---

## Sources

- Kernel docs: `Documentation/admin-guide/mm/` (THP, hugetlbpage, numa_memory_policy, concept.rst) — https://docs.kernel.org/admin-guide/mm/index.html
- `Documentation/accounting/psi.rst`; PSI announcement — https://facebookmicrosites.github.io/psi/
- LWN: "Pressure stall information" — https://lwn.net/Articles/759781/
- LWN: "Transparent hugepages" — https://lwn.net/Articles/423584/
- Kubernetes NodeSwap (KEP-2400) — https://github.com/kubernetes/enhancements/tree/master/keps/sig-node/2400-node-system-support-for-swap
- `man 8 oomd`, `oomctl(1)`; systemd-oomd docs — https://systemd.io/OMSD/
- Brendan Gregg. *Systems Performance*, 2nd ed., Ch. 7 (Memory).
