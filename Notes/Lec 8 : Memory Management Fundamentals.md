# Operating System – Memory Management – Lecture 1

## Why Memory Management? | Memory Hierarchy | RAM, Cache & Registers | Allocation & Deallocation | Fragmentation & Compaction

> **Subject:** Operating System
> **Module:** Memory Management
> **Lecture:** 1
> **Duration:** 50 Minutes
> **Level:** Beginner → Intermediate
> **Approach:** Detailed Theory + Diagrams + Real-World Examples + Linux Practical

---

# 1. Learning Objectives

After completing this lecture, students should be able to:

* Explain why memory management is necessary.
* Understand how CPU, cache, RAM, and storage work together.
* Understand the memory hierarchy.
* Differentiate registers, cache, RAM, and secondary storage.
* Explain how programs are loaded into memory.
* Understand memory allocation and deallocation.
* Explain internal and external fragmentation at an introductory level.
* Understand compaction and why it is required.
* Observe memory usage practically in Linux.

---

# 2. The Big Question

Start with this question:

> **"If my computer has 16 GB RAM, why can't every program simply use as much memory as it wants?"**

Suppose we open:

```text
Chrome       → 3 GB
VS Code      → 2 GB
Android VM   → 4 GB
Database     → 3 GB
Other Apps   → 5 GB
```

Total:

```text
17 GB
```

But RAM is:

```text
16 GB
```

Now what?

The Operating System has to manage memory carefully.

This is the job of:

# Memory Management

---

# 3. What is Memory Management?

## Definition

**Memory Management** is the function of the operating system that manages the computer's main memory and controls how memory is allocated, used, protected, and released.

In simple words:

> The OS decides **who gets memory, how much memory they get, where it is placed, and when it can be released.**

---

# 4. Why Do We Need Memory Management?

Imagine there are four processes:

```text
P1 = 2 GB
P2 = 4 GB
P3 = 1 GB
P4 = 3 GB
```

RAM:

```text
8 GB
```

It is impossible to keep all four processes fully resident in RAM simultaneously.

The OS must manage:

```text
Memory Allocation
        ↓
Memory Protection
        ↓
Memory Tracking
        ↓
Memory Deallocation
        ↓
Efficient Utilization
```

---

# 5. Main Responsibilities of Memory Management

The OS performs several important tasks.

## 5.1 Memory Allocation

Give memory to a process when it needs it.

```text
Process → Request 100 MB
             ↓
          OS Memory Manager
             ↓
          Allocate 100 MB
```

---

## 5.2 Memory Deallocation

Release memory when a process no longer needs it.

```text
Process
   ↓
Done
   ↓
OS releases memory
```

---

## 5.3 Memory Protection

One process should not normally modify another process's private memory.

For example:

```text
Process A
+----------------+
| Private Memory |
+----------------+

Process B
+----------------+
| Private Memory |
+----------------+
```

A should not freely access B's memory.

---

## 5.4 Memory Sharing

The OS can also allow controlled sharing.

Example:

Two processes may share:

```text
Shared Library
```

or use shared memory for communication.

---

## 5.5 Efficient Memory Utilization

The OS tries to avoid unnecessary unused memory.

```text
Good utilization
        ↓
More programs can run
        ↓
Better system performance
```

---

# 6. Program vs Memory

Recall:

```text
Program
   ↓
Stored on SSD/HDD
   ↓
When executed
   ↓
Loaded into RAM
   ↓
Becomes a Process
```

Example:

```text
calculator.exe
```

is stored on storage.

When launched:

```text
Storage
   ↓
RAM
   ↓
Process
   ↓
CPU
```

---

# 7. Big Picture – CPU, Memory and Storage

```text
              +---------+
              |   CPU   |
              +---------+
                   |
                   ↓
              +---------+
              | Cache   |
              +---------+
                   |
                   ↓
              +---------+
              |   RAM   |
              +---------+
                   |
                   ↓
          +-------------------+
          | SSD / HDD         |
          | Secondary Storage |
          +-------------------+
```

The closer memory is to the CPU:

* Faster
* More expensive per byte
* Smaller capacity

The farther away:

* Slower
* Larger capacity
* Cheaper per byte

This creates the:

# Memory Hierarchy

---

# 8. Memory Hierarchy

Memory is organized into levels.

A simplified hierarchy:

```text
               FASTEST
                  ↑
                  |
            +-----------+
            | Registers |
            +-----------+
                  |
            +-----------+
            | L1 Cache  |
            +-----------+
                  |
            +-----------+
            | L2 Cache  |
            +-----------+
                  |
            +-----------+
            | L3 Cache  |
            +-----------+
                  |
            +-----------+
            |   RAM     |
            +-----------+
                  |
            +-----------+
            | SSD / HDD |
            +-----------+
                  |
                  ↓
                SLOWEST
```

---

# 9. Memory Hierarchy – Important Rule

Generally:

```text
Higher in hierarchy
↓
Faster
↓
Smaller
↓
More expensive per byte
```

And:

```text
Lower in hierarchy
↓
Slower
↓
Larger
↓
Cheaper per byte
```

---

# 10. Why Not Use Only the Fastest Memory?

A common question:

> "If registers are fastest, why don't we make the entire computer out of registers?"

Because:

* Registers are extremely expensive.
* They have tiny capacity.
* The CPU has only a limited number of them.

Example:

```text
Register
   → Very Fast
   → Tiny

RAM
   → Slower
   → Large

SSD
   → Much Slower
   → Very Large
```

The system combines all levels to get a practical balance of:

```text
Speed + Capacity + Cost
```

---

# 11. Memory Hierarchy Diagram

```text
                 CPU
                  |
                  v

         +----------------+
         |   Registers    |
         | Fastest        |
         | Smallest       |
         +----------------+
                  |
                  v

         +----------------+
         | L1 Cache       |
         +----------------+
                  |
                  v

         +----------------+
         | L2 Cache       |
         +----------------+
                  |
                  v

         +----------------+
         | L3 Cache       |
         +----------------+
                  |
                  v

         +----------------+
         | RAM            |
         | Main Memory    |
         +----------------+
                  |
                  v

         +----------------+
         | SSD / HDD      |
         | Storage        |
         +----------------+
```

---

# 12. Registers

## What is a Register?

A register is a very small, very fast storage location inside the CPU.

Registers hold values that the CPU currently needs.

Examples include:

* Program Counter
* Stack Pointer
* General-purpose registers
* Instruction-related registers

---

# 13. Example of Register Usage

Suppose:

```text
a = 10
b = 20
c = a + b
```

The CPU may load required values into registers, perform the addition, and keep the result in a register before storing it elsewhere.

Conceptually:

```text
RAM
 |
 | Load
 v
Register A = 10
Register B = 20
 |
 | CPU executes
 v
Register C = 30
 |
 | Store
 v
RAM
```

---

# 14. Characteristics of Registers

* Located inside CPU
* Extremely fast
* Very small capacity
* Used directly during instruction execution
* Usually volatile

---

# 15. Cache Memory

Cache is a small, fast memory placed close to or integrated with the CPU.

Its goal is to reduce the time needed to access frequently used data and instructions.

---

# 16. Why Do We Need Cache?

CPU is extremely fast.

RAM is slower than CPU registers and caches.

If the CPU had to wait for RAM for every operation, performance would suffer.

So cache stores data that the CPU is likely to use again.

```text
CPU
 ↓
Check Cache
 ↓
Data available?
 ├── Yes → Use quickly
 └── No  → Fetch from lower level
```

---

# 17. L1, L2 and L3 Cache

Modern CPUs often contain multiple cache levels.

## L1 Cache

* Smallest
* Fastest cache
* Very close to the CPU core

## L2 Cache

* Larger than L1
* Slightly slower

## L3 Cache

* Usually larger
* Often shared among multiple CPU cores
* Slower than L1/L2 but still much faster than RAM

A typical relationship is:

```text
L1 > L2 > L3 > RAM
```

in terms of speed.

---

# 18. RAM

RAM stands for:

# Random Access Memory

RAM is the system's main memory.

It stores:

* Running programs
* Program data
* OS data
* Temporary working data

---

# 19. Why is RAM Important?

Suppose you start VS Code.

```text
VS Code on SSD
       ↓
Loaded into RAM
       ↓
CPU accesses required instructions/data
```

The CPU works primarily with data available through the memory hierarchy, including RAM.

---

# 20. Characteristics of RAM

* Faster than SSD/HDD
* Slower than cache
* Larger than cache
* Volatile
* Used as main memory
* Managed heavily by the OS

---

# 21. What Does Volatile Mean?

Volatile memory loses its contents when power is removed.

For example:

```text
Power ON
↓
RAM contains active data

Power OFF
↓
RAM contents are not preserved
```

Storage devices such as SSDs are non-volatile.

---

# 22. RAM vs Storage

Students often confuse these.

| RAM                                  | SSD/HDD                    |
| ------------------------------------ | -------------------------- |
| Main memory                          | Secondary storage          |
| Fast                                 | Slower                     |
| Volatile                             | Non-volatile               |
| Used for active programs/data        | Used for persistent files  |
| Smaller                              | Much larger                |
| OS manages actively during execution | Used for long-term storage |

---

# 23. Example

You save:

```text
notes.pdf
```

The file is stored on:

```text
SSD
```

When you open it:

```text
SSD
 ↓
RAM
 ↓
CPU / Cache
 ↓
Application
```

So:

```text
Storage = Keep
RAM = Work
```

---

# 24. Registers vs Cache vs RAM vs Storage

| Feature      | Registers        | Cache                | RAM                  | SSD/HDD            |
| ------------ | ---------------- | -------------------- | -------------------- | ------------------ |
| Speed        | Fastest          | Very Fast            | Fast                 | Slowest            |
| Capacity     | Tiny             | Small                | Large                | Very Large         |
| Location     | CPU              | Near/inside CPU      | Main system memory   | Storage device     |
| Volatile     | Yes              | Yes                  | Yes                  | No                 |
| Main Purpose | Current CPU data | Frequently used data | Active programs/data | Persistent storage |

---

# 25. Practical Observation – Linux Memory

Run:

```bash
free -h
```

Example:

```text
               total    used    free
Mem:            16Gi     7Gi     5Gi
Swap:            4Gi     0Gi     4Gi
```

Your values will be different.

---

# 26. Understanding `free -h`

```text
free
```

shows memory information.

```text
-h
```

means:

> Human-readable format

You may see:

```text
total
used
free
shared
buff/cache
available
```

---

# 27. Why is `available` Important?

Modern Linux uses unused RAM for useful purposes such as caching.

Therefore:

```text
free
```

does not always mean:

```text
RAM available for applications
```

`available` is generally a more useful estimate of memory that can be given to applications without heavy swapping.

---

# 28. Practical – View Memory in Real Time

Run:

```bash
top
```

or:

```bash
htop
```

You can observe:

* RAM usage
* CPU usage
* Processes
* System load

---

# 29. Practical – Check RAM Information

Run:

```bash
cat /proc/meminfo
```

This exposes detailed memory statistics from the Linux kernel.

You may see:

```text
MemTotal
MemFree
MemAvailable
Buffers
Cached
SwapTotal
SwapFree
```

---

# 30. Allocation

Now let's understand the central memory management operation.

# Memory Allocation

## Definition

Memory allocation is the process of assigning a portion of memory to a process or data structure when it is needed.

Example:

```text
Process A requests 200 MB
       ↓
OS Memory Manager
       ↓
200 MB assigned
```

---

# 31. Simple Memory Allocation Example

Suppose RAM contains:

```text
1000 MB
```

Process A requests:

```text
200 MB
```

Then:

```text
+----------------------+
| Process A – 200 MB   |
+----------------------+
| Free – 800 MB        |
+----------------------+
```

Next:

Process B requests:

```text
300 MB
```

Now:

```text
+----------------------+
| Process A – 200 MB   |
+----------------------+
| Process B – 300 MB   |
+----------------------+
| Free – 500 MB        |
+----------------------+
```

---

# 32. Deallocation

When a process finishes:

```text
Process B
   ↓
Terminated
   ↓
Memory released
```

Example:

Before:

```text
+------------------+
| A – 200 MB       |
+------------------+
| B – 300 MB       |
+------------------+
| Free – 500 MB    |
+------------------+
```

After B terminates:

```text
+------------------+
| A – 200 MB       |
+------------------+
| Free – 800 MB    |
+------------------+
```

---

# 33. Why Allocation and Deallocation Matter

Without proper management:

```text
Memory
 ↓
Wasted
 ↓
Poor utilization
 ↓
Programs cannot start
```

The OS must continuously track:

```text
Used Memory
Free Memory
Process Ownership
Protection
```

---

# 34. Simple Memory Map

Imagine memory as rooms in a building.

```text
RAM

+----------------------+
| Operating System     |
+----------------------+
| Process A            |
+----------------------+
| Process B            |
+----------------------+
| Free                 |
+----------------------+
| Process C            |
+----------------------+
| Free                 |
+----------------------+
```

The OS keeps track of which blocks are occupied and which are free.

---

# 35. The Problem with Free Memory

Suppose free memory is scattered:

```text
+-----------+
| Process A |
+-----------+
| Free 100  |
+-----------+
| Process B |
+-----------+
| Free 200  |
+-----------+
| Process C |
+-----------+
| Free 150  |
+-----------+
```

Total free memory:

```text
100 + 200 + 150 = 450 MB
```

Suppose a process needs:

```text
300 MB contiguous space
```

There is enough total free memory.

But no single free block is 300 MB.

This introduces:

# Fragmentation

---

# 36. What is Fragmentation?

Fragmentation means memory is divided into pieces in a way that causes inefficient utilization.

Two important introductory categories are:

1. Internal Fragmentation
2. External Fragmentation

---

# 37. Internal Fragmentation

Internal fragmentation occurs when allocated memory contains unused space inside an allocated block.

Example:

Suppose the OS allocates fixed-size blocks of:

```text
100 MB
```

A process needs:

```text
70 MB
```

The OS allocates:

```text
100 MB
```

Unused:

```text
30 MB
```

Diagram:

```text
+------------------------+
| Process = 70 MB        |
|                        |
| Unused = 30 MB         |
+------------------------+
      100 MB block
```

The 30 MB is wasted inside the allocated region.

---

# 38. Another Internal Fragmentation Example

Required:

```text
18 KB
```

Allocated block:

```text
20 KB
```

Waste:

```text
2 KB
```

This unused 2 KB is internal fragmentation.

---

# 39. External Fragmentation

External fragmentation happens when free memory exists, but it is scattered into separate holes between allocated blocks.

Example:

```text
+----------------+
| Process A      |
+----------------+
| Free 50 MB     |
+----------------+
| Process B      |
+----------------+
| Free 80 MB     |
+----------------+
| Process C      |
+----------------+
| Free 70 MB     |
+----------------+
```

Total free:

```text
50 + 80 + 70 = 200 MB
```

A process requires:

```text
150 MB contiguous
```

The total available memory is enough:

```text
200 MB > 150 MB
```

But the largest individual block is:

```text
80 MB
```

So the process cannot fit into one contiguous block under this simple model.

---

# 40. Internal vs External Fragmentation

| Internal Fragmentation                                                          | External Fragmentation                          |
| ------------------------------------------------------------------------------- | ----------------------------------------------- |
| Waste inside allocated blocks                                                   | Free space scattered outside allocated blocks   |
| Usually associated with fixed-size allocation                                   | Common with variable-size contiguous allocation |
| Allocated memory contains unused space                                          | Free memory exists in small holes               |
| Cannot normally be used by another allocation until block is reused/reallocated | Can potentially be combined by compaction       |

---

# 41. Visual Difference

## Internal Fragmentation

```text
+----------------------+
| Process              |
| Process uses 70 MB   |
| Unused 30 MB         |
+----------------------+
```

Waste is:

```text
INSIDE
```

the allocated block.

---

## External Fragmentation

```text
+------------+
| Process A  |
+------------+
| Free 30 MB |
+------------+
| Process B  |
+------------+
| Free 40 MB |
+------------+
| Process C  |
+------------+
| Free 50 MB |
+------------+
```

Waste is:

```text
OUTSIDE
```

the allocated blocks, in scattered holes.

---

# 42. Why Does External Fragmentation Happen?

Suppose initially:

```text
A
B
C
D
```

Now B and D finish.

```text
A
FREE
C
FREE
```

Free memory is separated.

If many processes start and stop:

```text
A
FREE
C
FREE
E
FREE
F
```

The number of holes can increase.

---

# 43. Compaction

One approach to external fragmentation is:

# Compaction

## Definition

Compaction moves allocated blocks so that scattered free memory is combined into one larger free block.

---

# 44. Before Compaction

```text
+-------------+
| Process A   |
+-------------+
| Free 50 MB  |
+-------------+
| Process B   |
+-------------+
| Free 80 MB  |
+-------------+
| Process C   |
+-------------+
| Free 70 MB  |
+-------------+
```

Total free:

```text
200 MB
```

But it is scattered.

---

# 45. After Compaction

The OS moves processes together:

```text
+-------------+
| Process A   |
+-------------+
| Process B   |
+-------------+
| Process C   |
+-------------+
|             |
| Free        |
| 200 MB      |
|             |
+-------------+
```

Now the free space is contiguous.

---

# 46. Compaction Diagram

```text
BEFORE

+------+---------+------+---------+------+
|  A   |  FREE   |  B   |  FREE   |  C   |
+------+---------+------+---------+------+
       50 MB             80 MB

                 ↓

            COMPACTION

                 ↓

AFTER

+------+------+------+--------------------+
|  A   |  B   |  C   |       FREE         |
+------+------+------+--------------------+
                    200 MB
```

The exact sizes in a real system vary; the diagram demonstrates the concept.

---

# 47. Cost of Compaction

Compaction is not free.

The OS may have to:

* Move memory contents
* Update mappings/addresses
* Perform bookkeeping
* Pause or interfere with execution depending on the memory-management design

Therefore:

> Compaction improves contiguous free space but can introduce significant overhead.

---

# 48. Real-Life Analogy for Fragmentation

Imagine a bookshelf.

You have:

```text
Book
Empty
Book
Empty
Book
Empty
```

There is enough total empty space for a large book collection, but not one large continuous section.

You rearrange the books:

```text
Book
Book
Book
Empty Empty Empty
```

This is similar to compaction.

---

# 49. Why Memory Management Matters for Multitasking

Suppose:

```text
Chrome
VS Code
Spotify
Terminal
```

are running simultaneously.

Each needs memory.

The OS ensures that:

```text
Chrome
   ↕
Own memory

VS Code
   ↕
Own memory

Spotify
   ↕
Own memory
```

This is essential for safe multitasking.

---

# 50. Memory Protection Example

Imagine:

```text
Process A
Secret Data:
Password123
```

Process B should not simply read A's private memory.

The OS uses hardware and OS mechanisms to enforce memory isolation.

Conceptually:

```text
+----------------------+
| Process A Memory     |
| Protected            |
+----------------------+

+----------------------+
| Process B Memory     |
| Protected            |
+----------------------+
```

This is one reason memory management is also a security problem.

---

# 51. Allocation Strategies – Introduction

Before advanced topics like paging and segmentation, it is useful to understand that contiguous allocation can use strategies such as:

* First Fit
* Best Fit
* Worst Fit

These will be studied in detail later.

For now, understand the basic idea.

---

# 52. First Fit

Find the first free block large enough for the request.

Example:

```text
Free Blocks:

100 MB
500 MB
200 MB
300 MB
```

Request:

```text
180 MB
```

First block that fits:

```text
500 MB
```

---

# 53. Best Fit

Choose the smallest free block that is large enough.

Free blocks:

```text
100 MB
500 MB
200 MB
300 MB
```

Request:

```text
180 MB
```

Best fit:

```text
200 MB
```

---

# 54. Worst Fit

Choose the largest free block.

Same example:

```text
100 MB
500 MB
200 MB
300 MB
```

Request:

```text
180 MB
```

Worst fit:

```text
500 MB
```

---

# 55. Why Are These Important?

Different allocation strategies affect:

* Fragmentation
* Memory utilization
* Allocation speed
* Overall performance

These algorithms will be studied in greater detail later.

---

# 56. Linux Practical – Check RAM

Run:

```bash
free -h
```

Record:

```text
Total RAM:
Used:
Available:
Swap:
```

---

# 57. Linux Practical – Monitor Memory

Run:

```bash
top
```

Look at:

```text
MiB Mem
```

Observe how memory usage changes when you:

1. Open a browser.
2. Open VS Code.
3. Start another application.
4. Close an application.

---

# 58. Linux Practical – Process Memory

Run:

```bash
ps aux
```

You will see a list of processes.

Important columns include:

```text
%CPU
%MEM
RSS
VSZ
COMMAND
```

---

# 59. Understanding `%MEM`

`%MEM` represents the process's memory usage as a percentage of the system's physical memory according to the process reporting mechanism.

For example:

```text
chrome
```

may use a noticeable percentage of RAM.

The exact behavior depends on how the process is structured and how shared memory is accounted for.

---

# 60. Linux Practical – Detailed Memory Information

Run:

```bash
cat /proc/meminfo
```

Try:

```bash
grep -E 'MemTotal|MemFree|MemAvailable|SwapTotal|SwapFree' /proc/meminfo
```

You can observe:

```text
MemTotal
MemFree
MemAvailable
SwapTotal
SwapFree
```

---

# 61. Practical Experiment – Observe Memory Change

### Step 1

Run:

```bash
free -h
```

Record memory usage.

### Step 2

Open:

```text
Browser
```

Run again:

```bash
free -h
```

### Step 3

Open:

```text
VS Code
```

Run:

```bash
free -h
```

### Step 4

Close VS Code.

Run:

```bash
free -h
```

Observe the change.

---

# 62. Classroom Activity

Ask students:

> "I have 8 GB RAM. Why does `free -h` not always show exactly 8 GB free after I close every application?"

Discuss:

* OS itself uses memory.
* Kernel memory exists.
* Filesystem/page cache exists.
* Background services are running.
* Memory may be reserved by hardware.
* Free memory is not the same thing as useful available memory.

---

# 63. Important Distinction

Do not teach:

> "Unused RAM is wasted RAM."

Modern operating systems intentionally use available RAM for caching and other purposes.

The better idea is:

> **The OS attempts to use RAM productively while keeping enough memory available for active workloads.**

---

# 64. Allocation vs Deallocation

```text
             MEMORY

Request
   ↓
Allocation
   ↓
Process Uses Memory
   ↓
Process Ends
   ↓
Deallocation
   ↓
Memory Available Again
```

---

# 65. Memory Lifecycle

A simplified lifecycle:

```text
Program
   ↓
Process Created
   ↓
Memory Allocated
   ↓
Program Executes
   ↓
More Memory May Be Requested
   ↓
Memory Released
   ↓
Process Terminates
   ↓
Remaining Memory Reclaimed
```

---

# 66. Fragmentation Lifecycle Example

```text
Initial:

[A][B][C][D][FREE]


B terminates:

[A][FREE][C][D][FREE]


D terminates:

[A][FREE][C][FREE][FREE]


Repeated allocation/deallocation:

[A][FREE][C][FREE][E][FREE]
```

Now free space is scattered.

This is the basic intuition behind external fragmentation.

---

# 67. Why Modern Systems Need More Advanced Memory Management

Simple contiguous memory allocation has problems:

```text
Fragmentation
+
Protection
+
Large Programs
+
Multiple Processes
+
Limited RAM
```

Therefore, modern systems use more advanced approaches such as:

* Paging
* Segmentation
* Virtual Memory
* Demand Paging
* Page Replacement

These will be covered in later lectures.

---

# 68. Preview – Virtual Memory

Suppose:

```text
Physical RAM = 8 GB
```

but the system needs to support processes whose combined virtual address spaces are much larger.

The OS can use:

```text
Virtual Memory
```

to provide each process with a large virtual address space and manage how data is backed by physical memory and storage.

We will study this in detail later.

---

# 69. Preview – Paging

Instead of requiring a process to occupy one large contiguous block:

```text
Process
   ↓
Pages
   ↓
Physical Frames
```

Conceptually:

```text
Virtual Memory

P1 | P2 | P3 | P4

       ↓ mapping

RAM

F5 | F2 | F9 | F1
```

Pages can be placed into different physical frames.

This greatly reduces external fragmentation compared with simple contiguous allocation.

---

# 70. Important Concepts to Remember

### Memory

The physical storage directly or indirectly used by the CPU while programs execute.

### Register

Tiny, very fast CPU storage.

### Cache

Small, fast storage that keeps frequently/recently needed data close to the CPU.

### RAM

Main memory used for active programs/data.

### Storage

Persistent non-volatile storage such as SSD/HDD.

### Allocation

Assigning memory.

### Deallocation

Releasing memory.

### Fragmentation

Inefficient memory utilization caused by the way memory is divided/allocated.

### Compaction

Moving allocated blocks to combine scattered free space.

---

# 71. Common Student Confusions

## Confusion 1

**RAM and storage are the same.**

No.

```text
RAM     → Active working memory
SSD     → Persistent storage
```

---

## Confusion 2

**Cache is stored on SSD.**

No.

CPU cache is a very fast memory close to/inside the CPU.

---

## Confusion 3

**Registers are part of RAM.**

No.

Registers are part of the CPU.

---

## Confusion 4

**Fragmentation means memory is completely lost.**

Not necessarily.

The memory may exist but be inefficiently organized or unusable for a particular allocation pattern.

---

## Confusion 5

**Compaction creates more memory.**

No.

It reorganizes existing memory.

```text
Before:
Free = 200 MB

After:
Free = 200 MB
```

The amount may be the same, but it is now contiguous.

---

# 72. Important Comparison Table

| Concept                | Meaning                          |
| ---------------------- | -------------------------------- |
| Register               | Smallest and fastest CPU storage |
| Cache                  | Fast memory near CPU             |
| RAM                    | Main memory                      |
| SSD/HDD                | Persistent storage               |
| Allocation             | Assign memory                    |
| Deallocation           | Release memory                   |
| Internal Fragmentation | Waste inside allocated block     |
| External Fragmentation | Scattered free-space holes       |
| Compaction             | Combine scattered free space     |

---

# 73. Interview / Viva Questions

### Q1. Why is memory management required?

To efficiently allocate and deallocate memory, protect processes, support multitasking, and maximize useful memory utilization.

### Q2. What is RAM?

RAM is the primary volatile memory used to hold actively running programs and data.

### Q3. What is cache?

A small, fast memory level close to the CPU that stores frequently needed data/instructions.

### Q4. What are registers?

Very small and very fast storage locations inside the CPU used during instruction execution.

### Q5. Why is cache faster than RAM?

Cache is designed for very low-latency access and is located close to or within the processor hierarchy.

### Q6. What is memory allocation?

Assigning a portion of memory to a process or data structure.

### Q7. What is deallocation?

Releasing memory that is no longer needed.

### Q8. What is internal fragmentation?

Unused space inside an allocated memory block.

### Q9. What is external fragmentation?

Free memory scattered into separate holes, making it difficult to satisfy large contiguous requests.

### Q10. What is compaction?

Reorganizing memory by moving allocated blocks so that scattered free space becomes a larger contiguous block.

### Q11. Does compaction increase total memory?

No.

It only reorganizes existing free space.

### Q12. Why can't we make all memory cache?

Because fast memory is much more expensive and has limited capacity.

---

# 74. Practice Questions

## Question 1

A system has:

```text
RAM = 1000 MB
```

Processes need:

```text
P1 = 200 MB
P2 = 300 MB
P3 = 250 MB
```

How much remains?

```text
1000 - 200 - 300 - 250 = 250 MB
```

---

## Question 2

A 100 MB block is allocated to a process that uses only 75 MB.

What is the internal fragmentation?

```text
100 - 75 = 25 MB
```

---

## Question 3

Free blocks are:

```text
50 MB
100 MB
200 MB
```

Total:

```text
350 MB
```

A process requests 250 MB.

Can it be allocated using contiguous allocation?

**No**, because the largest individual free block is only 200 MB.

---

## Question 4

Free blocks are:

```text
50 MB
100 MB
200 MB
```

A process needs 150 MB.

Which block is selected by:

### First Fit?

```text
200 MB
```

because 50 and 100 are too small.

### Best Fit?

```text
200 MB
```

because it is the smallest block that fits.

### Worst Fit?

```text
200 MB
```

because it is also the largest.

---

# 75. Mini Assignment

Run:

```bash
free -h
```

and:

```bash
ps aux --sort=-%mem | head
```

Then record:

```text
1. Total RAM
2. Available RAM
3. Total Swap
4. Top 5 memory-consuming processes
5. Their %MEM values
```

Then answer:

> Why can the total `%MEM` values of processes be different from your simple intuition about total RAM usage?

Consider shared memory, caches, kernel usage, accounting methods, and the difference between process memory metrics.

---

# 76. Final Mental Model

Remember memory as a hierarchy:

```text
                    CPU
                     |
                     v
               +-----------+
               | Registers |
               +-----------+
                     |
                     v
               +-----------+
               |   Cache   |
               | L1 L2 L3  |
               +-----------+
                     |
                     v
               +-----------+
               |    RAM    |
               +-----------+
                     |
                     v
               +-----------+
               | SSD / HDD |
               +-----------+
```

And remember the OS memory-management cycle:

```text
                 PROGRAM
                    |
                    v
                PROCESS
                    |
                    v
             MEMORY REQUEST
                    |
                    v
                ALLOCATION
                    |
                    v
              PROCESS RUNS
                    |
                    v
             MEMORY RELEASE
                    |
                    v
              DEALLOCATION
```

And the fragmentation idea:

```text
Internal Fragmentation
        ↓
Wasted space INSIDE
allocated blocks


External Fragmentation
        ↓
Free space OUTSIDE
allocated blocks
        ↓
Scattered holes
        ↓
Compaction can combine them
```

---

# 77. Key Takeaways

* Memory management is one of the core responsibilities of an operating system.
* The OS allocates, protects, tracks, and releases memory.
* Memory exists in a hierarchy:
  **Registers → Cache → RAM → SSD/HDD**
* Faster memory is generally smaller and more expensive.
* RAM stores actively used programs and data.
* Registers are tiny and extremely fast CPU storage.
* Cache reduces the average time needed to access frequently used data.
* Allocation assigns memory to a process.
* Deallocation returns memory to the available pool.
* Internal fragmentation is unused space inside allocated blocks.
* External fragmentation is scattered free space between allocated blocks.
* Compaction combines scattered free areas into larger contiguous regions.
* Modern operating systems use advanced mechanisms such as paging and virtual memory to overcome many limitations of simple contiguous allocation.

> **Core idea:**
> **The OS must make limited memory appear organized, safe, and efficiently usable by many processes at the same time.**
