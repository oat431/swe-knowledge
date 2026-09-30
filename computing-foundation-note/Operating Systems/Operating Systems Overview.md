---
tags:
- linux
- os
- overview
- programming
---

# Operating Systems: Overview

The OS is the layer between your code and the hardware. Understanding it helps you write faster, more reliable software, and debug problems that "make no sense" until you understand what the OS is doing underneath.

---

## Topics

### 01 Core Concepts

> [[01 Processes & Threads]]: Process lifecycle, thread models, context switching, user vs kernel threads
> [[01 CPU Scheduling]]: FCFS, SJF, Round Robin, priority, multi-level queues, Linux CFS
> [[01 System Calls & Kernel]]: User mode vs kernel mode, syscall flow, interrupts, signals

### 02 Memory & Storage

> [[02 Memory Management]]: Virtual memory, paging, page tables, TLB, segmentation, thrashing
> [[02 File Systems]]: Inodes, journaling, ext4 vs NTFS, permissions, symbolic vs hard links
> [[02 I O & Devices]]: Block vs character devices, DMA, interrupts vs polling, buffers

### 03 Concurrency

> [[03 IPC Inter-Process Communication]]: Pipes, shared memory, message queues, sockets, signals
> [[03 Synchronization & Deadlocks]]: Mutex, semaphore, condition variables, deadlock conditions, avoidance
> [[03 OS in Practice]]: JVM memory, GC, thread pools, Linux tools (top, ps, vmstat, strace)

### 04 Distributed Systems

> [[04_Distributed_Systems]]: CAP theorem, consensus (Paxos/Raft), consistent hashing, CRDTs, fault tolerance, MapReduce

### 05 Virtualization & Containers

> [[05 Containers - Namespaces & Cgroups]]: The 8 namespaces, cgroups v2, OCI runtime stack, overlayfs, what containers *don't* isolate
> [[05 Virtualization - KVM & Hypervisors]]: Type 1/2, VT-x/EPT, virtio, SR-IOV, live migration, VM vs container vs microVM

### 06 Linux Internals

> [[06 I O Models & Async]]: Blocking → epoll → io_uring, reactor pattern, zero-copy, why Nginx scales
> [[06 Boot & systemd]]: UEFI → GRUB → initramfs → PID 1, units, targets, journald, drop-in overrides
> [[06 Memory Pressure - OOM, PSI, Swap & NUMA]]: OOM killer, exit 137, swappiness/zram, PSI, THP, NUMA
> [[06 OS Security Model - Capabilities, SELinux & Seccomp]]: Splitting root, syscall filtering, MAC vs DAC, container defense in depth

---

## Quick Reference: Key Concepts

| Concept | What It Is | Why Backend Devs Care |
|---------|-----------|----------------------|
| **Context Switch** | Saving/restoring CPU state between threads | Too many threads = thrashing. Size your thread pools. |
| **Virtual Memory** | Each process sees its own address space | Explains OOM, swap, mmap. |
| **Page Fault** | Accessing memory not in RAM | Slow. Optimize data locality. |
| **Deadlock** | Two threads waiting on each other forever | Know the 4 conditions. Design to prevent. |
| **TLB** | Cache for virtual→physical address translation | Large pages reduce TLB misses. |
| **DMA** | Hardware transfers data without CPU | Why async I/O is fast. |

---

## Sources

- SWEBOK v4: Section 5: Operating Systems
- Silberschatz, Galvin, Gagne. *Operating System Concepts.*
