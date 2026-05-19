# FOS - A Simple Operating System

A 32-bit x86 educational operating system built from scratch in C and Assembly, developed as a course project at **Ain Shams University** (OS'24), administered by **Dr. Ahmed Salah**. Inspired by **MIT JOS/xv6**, runs on the **Bochs** emulator.

---

## What's Inside

FOS was built across three milestones, each adding a major OS layer:

### MS1 - Preprocessing
Command prompt, system calls, dynamic memory allocator, and kernel locks.

- **Dynamic Allocator** - explicit free-list with boundary tags; First Fit & Best Fit, coalescing on free, realloc
- **System Calls** - full path from user stub -> trap dispatch -> kernel handler, with pointer validation
- **Sleep Locks** - `sleep()` / `wakeup_one()` / `wakeup_all()` built on top of spinlocks and channels

### MS2 - Memory
Full virtual memory system: kernel heap, user heap, page fault handling, and shared memory.

- **Kernel Heap** - dual-zone allocator (`kmalloc`/`kfree`/`sbrk`): small blocks via dynamic allocator, large chunks via page-granularity first-fit; O(1) VA<->PA translation
- **Fault Handler I** - validates faulted addresses, loads pages from disk into memory (placement), updates working set
- **User Heap** - lazy allocation (`malloc`/`free`): virtual ranges reserved immediately, physical frames assigned on access via page fault
- **Shared Memory** - `smalloc`/`sget`: allocate shared objects backed by physical frames, re-map them into other processes' address spaces with read/write permissions

### MS3 - CPU
Page replacement, user-level semaphores, and a preemptive priority scheduler.

- **Nth Chance Clock Replacement** - when the working set is full, scans in FIFO order; a page survives N sweeps without use before eviction. Modified variant gives dirty pages one extra chance
- **User-Level Semaphores** - `create`/`get`/`wait`/`signal`: counting semaphores over shared memory + kernel channels; spinlock-protected for atomicity; FIFO wakeup (no starvation)
- **Priority Round-Robin Scheduler** - multiple priority queues, preemptive on clock tick; processes waiting beyond a starvation threshold are automatically promoted

---

## Stack=

| Layer | Details |
|---|---|
| Language | C (97.6%), x86 Assembly (1.1%) |
| Architecture | 32-bit x86 (i386 ELF) |
| Emulator | Bochs - 256 MB RAM, ATA disk |
| Toolchain | `i386-elf-gcc` cross-compiler |
| Build | GNU Make |
| IDE | Eclipse CDT |

---

## Contributors

Built by the **KoOl OS** team - Ain Shams University.
Course administered by **Dr. Ahmed Salah**.