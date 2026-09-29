# Memory Management

## 1. Introduction to Memory Management

Memory management is an important function of an **Operating System (OS)**.

The OS is responsible for:

* Keeping track of which parts of memory are being used.
* Allocating memory to processes.
* Deallocating memory when processes finish.
* Protecting one process from accessing another process's memory.
* Moving processes between RAM and secondary storage when required.
* Providing efficient use of main memory.

### Why is memory management required?

RAM is limited.

Suppose we have:

```text
RAM = 8 GB

Process P1 = 3 GB
Process P2 = 2 GB
Process P3 = 4 GB
```

All three processes require:

```text
3 + 2 + 4 = 9 GB
```

But RAM has only 8 GB.

The OS needs techniques to manage this situation efficiently.

Some important memory-management techniques are:

```text
Swapping
   ↓
Contiguous Allocation
   ↓
Paging
   ↓
Segmentation
   ↓
Virtual Memory
   ↓
Demand Paging
   ↓
Page Replacement
   ↓
Thrashing
```

---

# 2. Swapping

## What is Swapping?

**Swapping** is a memory-management technique in which a process is temporarily moved from **main memory (RAM)** to **secondary storage (disk/SSD)** and later brought back into RAM.

The main purpose of swapping is to free RAM for other processes.

### Basic idea

```text
              RAM
        +---------------+
        | Process P1    |
        | Process P2    |
        | Process P3    |
        +---------------+
                |
                | Swap Out
                ↓
        +---------------+
        | Disk / SSD    |
        | Process P2    |
        +---------------+
                |
                | Swap In
                ↓
              RAM
```

### Swap Out

Moving a process:

```text
RAM → Disk
```

is called **Swap Out**.

### Swap In

Moving a process:

```text
Disk → RAM
```

is called **Swap In**.

---

## Why is swapping needed?

Swapping helps when:

* RAM is insufficient.
* More processes need to be executed.
* The OS wants to increase the degree of multiprogramming.
* A process is temporarily inactive.

### Example

Suppose:

```text
RAM = 4 GB

P1 = 1 GB
P2 = 1 GB
P3 = 1 GB
P4 = 1 GB
```

Now another process requires 2 GB.

The OS may move one or more inactive processes to disk:

```text
Before:

RAM
+----+
| P1 |
| P2 |
| P3 |
| P4 |
+----+

After swapping P4:

RAM
+----+
| P1 |
| P2 |
| P3 |
|Free|
+----+

Disk
+----+
| P4 |
+----+
```

---

## Advantages of Swapping

* Allows more processes to be handled than can simultaneously fit in RAM.
* Improves memory utilization.
* Helps multiprogramming.

## Disadvantages of Swapping

* Disk/SSD is much slower than RAM.
* Moving large processes takes time.
* Excessive swapping can reduce system performance.

---

# 3. Contiguous Memory Allocation

## What is Contiguous Allocation?

In **contiguous memory allocation**, each process is allocated a **single continuous block of physical memory**.

For example:

```text
RAM

+------------------+
| Operating System |
+------------------+
| Process P1       |
+------------------+
| Process P2       |
+------------------+
| Process P3       |
+------------------+
| Free Space       |
+------------------+
```

The memory allocated to each process must be continuous.

---

# 4. Fixed Partitioning

In fixed partitioning, memory is divided into a fixed number of partitions.

Example:

```text
RAM = 8 GB

+----------------+
| OS             |
+----------------+
| Partition 1    |
+----------------+
| Partition 2    |
+----------------+
| Partition 3    |
+----------------+
| Partition 4    |
+----------------+
```

Each partition can contain one process.

### Problem: Internal Fragmentation

Suppose:

```text
Partition = 4 MB
Process = 2.5 MB
```

The remaining:

```text
4 - 2.5 = 1.5 MB
```

cannot normally be given to another process.

This unused space inside an allocated partition is called:

**Internal Fragmentation**

---

# 5. Dynamic Partitioning

In dynamic partitioning, partitions are created according to the size required by a process.

Example:

```text
Process P1 = 200 MB
Process P2 = 100 MB
Process P3 = 300 MB
```

Memory can be allocated dynamically:

```text
+----------------+
| OS             |
+----------------+
| P1 - 200 MB    |
+----------------+
| P2 - 100 MB    |
+----------------+
| P3 - 300 MB    |
+----------------+
| Free            |
+----------------+
```

This reduces internal fragmentation.

However, it can cause **external fragmentation**.

---

# 6. Fragmentation

Fragmentation means memory is wasted because free memory cannot be used efficiently.

There are two major types:

```text
Fragmentation
      |
      +-------- Internal Fragmentation
      |
      +-------- External Fragmentation
```

---

## 6.1 Internal Fragmentation

Unused memory **inside an allocated block**.

Example:

```text
Allocated block = 10 KB
Process requires = 8 KB

Unused = 2 KB
```

```text
+------------------+
| Process = 8 KB   |
|------------------|
| Unused = 2 KB    |
+------------------+
```

The unused space is inside the allocated block.

---

## 6.2 External Fragmentation

Unused memory exists **between allocated blocks**.

Example:

```text
+-------+
| P1    |
+-------+
| Free  | 20 KB
+-------+
| P2    |
+-------+
| Free  | 10 KB
+-------+
| P3    |
+-------+
| Free  | 30 KB
+-------+
```

Total free memory:

```text
20 + 10 + 30 = 60 KB
```

Suppose a process needs:

```text
50 KB
```

There is 60 KB free in total, but there is no **single contiguous block of 50 KB**.

This is external fragmentation.

---

# 7. Placement Strategies

When a process needs memory, the OS needs to decide which free block should be used.

Important strategies are:

1. First Fit
2. Best Fit
3. Worst Fit

---

## 7.1 First Fit

Allocate the **first block large enough** to accommodate the process.

Example:

```text
Free blocks:

100 KB
500 KB
200 KB
300 KB
600 KB

Process = 250 KB
```

First block that can contain 250 KB:

```text
500 KB
```

So:

```text
250 KB → 500 KB block
```

---

## 7.2 Best Fit

Allocate the **smallest block that is large enough**.

Example:

```text
100 KB
500 KB
200 KB
300 KB
600 KB

Process = 250 KB
```

Suitable blocks:

```text
500 KB
300 KB
600 KB
```

Smallest suitable block:

```text
300 KB
```

Therefore:

```text
250 KB → 300 KB
```

---

## 7.3 Worst Fit

Allocate the process to the **largest available block**.

Example:

```text
100 KB
500 KB
200 KB
300 KB
600 KB
```

Process:

```text
250 KB
```

Largest suitable block:

```text
600 KB
```

Therefore:

```text
250 KB → 600 KB
```

---

# 8. Paging

## What is Paging?

**Paging** is a memory-management technique in which:

* Logical memory is divided into fixed-size blocks called **Pages**.
* Physical memory is divided into fixed-size blocks called **Frames**.

```text
Logical Memory              Physical Memory

+---------+                 +---------+
| Page 0  |                 | Frame 0 |
+---------+                 +---------+
| Page 1  |                 | Frame 1 |
+---------+                 +---------+
| Page 2  |                 | Frame 2 |
+---------+                 +---------+
| Page 3  |                 | Frame 3 |
+---------+                 +---------+
```

### Important

```text
Page → Logical/Virtual Memory
Frame → Physical Memory
```

Page size and frame size are the same.

---

# 9. Page Table

Pages of a process do not need to be stored continuously in RAM.

For example:

```text
Process Pages:

P0
P1
P2
P3
```

They may be stored as:

```text
P0 → Frame 5
P1 → Frame 2
P2 → Frame 8
P3 → Frame 1
```

The OS needs a data structure to maintain this mapping.

This is called a **Page Table**.

Example:

| Page Number | Frame Number |
| ----------- | ------------ |
| 0           | 5            |
| 1           | 2            |
| 2           | 8            |
| 3           | 1            |

---

# 10. Address Translation in Paging

A logical address is divided into:

```text
Logical Address
      |
      +------ Page Number
      |
      +------ Offset
```

The **page number** is used to find the corresponding frame.

The **offset** identifies the exact location inside the page/frame.

### Example

Suppose:

```text
Page size = 100 bytes
Logical address = 250
```

Then:

```text
Page number = 250 / 100 = 2
Offset = 250 % 100 = 50
```

So:

```text
Page = 2
Offset = 50
```

If Page 2 is stored in Frame 7:

```text
Physical address
= Frame × Page size + Offset

= 7 × 100 + 50

= 750
```

---

# 11. Advantages of Paging

* Eliminates external fragmentation.
* Processes do not need contiguous physical memory.
* Efficient memory allocation.
* Supports virtual memory.
* Allows pages to be loaded independently.

## Disadvantage

Paging can cause **internal fragmentation** because the last page may not be completely filled.

---

# 12. Segmentation

## What is Segmentation?

**Segmentation** is a memory-management technique where a program is divided into **logical segments** based on its structure.

Examples:

```text
Code
Data
Stack
Heap
Functions
Arrays
```

Unlike paging, segments are **variable-sized**.

Example:

```text
Program

+----------------+
| Code Segment   |
+----------------+
| Data Segment   |
+----------------+
| Stack Segment  |
+----------------+
| Heap Segment   |
+----------------+
```

---

# 13. Segment Table

The OS maintains a **Segment Table**.

A segment table generally contains:

* Segment number
* Base address
* Limit

Example:

| Segment | Base | Limit |
| ------- | ---: | ----: |
| Code    | 1000 |   400 |
| Data    | 5000 |   300 |
| Stack   | 8000 |   200 |

### Base

Starting physical address of the segment.

### Limit

Size of the segment.

---

# 14. Logical Address in Segmentation

A logical address contains:

```text
Segment Number
+
Offset
```

Example:

```text
Logical Address:

Segment = 1
Offset = 200
```

Suppose:

```text
Segment 1
Base = 5000
Limit = 300
```

Since:

```text
Offset = 200 < Limit = 300
```

the address is valid.

Physical address:

```text
Base + Offset

= 5000 + 200

= 5200
```

If:

```text
Offset >= Limit
```

then a segmentation fault/error can occur.

---

# 15. Paging vs Segmentation

| Paging                                     | Segmentation                                   |
| ------------------------------------------ | ---------------------------------------------- |
| Fixed-size pages                           | Variable-size segments                         |
| Physical memory divided into frames        | Memory allocated according to logical segments |
| Page size is fixed                         | Segment size varies                            |
| Mainly manages physical memory efficiently | Represents logical program structure           |
| Can cause internal fragmentation           | Can cause external fragmentation               |
| Address = Page + Offset                    | Address = Segment + Offset                     |

---

# 16. Virtual Memory

## What is Virtual Memory?

**Virtual memory** is a memory-management technique that allows a process to execute even when the entire process is **not loaded into physical RAM**.

The OS uses a combination of:

```text
RAM + Secondary Storage
```

to provide the illusion of a larger memory space.

### Example

Suppose:

```text
RAM = 4 GB
Program size = 8 GB
```

The program can still run using virtual memory because only the required portions need to be present in RAM at a given time.

---

# 17. Why Virtual Memory?

Without virtual memory:

```text
Entire program
      ↓
Must fit in RAM
```

With virtual memory:

```text
Large Program
      ↓
Only required portions
      ↓
RAM
```

This allows:

* Larger programs to run.
* More processes to stay active.
* Better RAM utilization.
* Greater multiprogramming.

---

# 18. Virtual Address vs Physical Address

### Virtual Address

Address generated by the CPU.

It belongs to the process's virtual/logical address space.

### Physical Address

Actual address in RAM.

```text
CPU
 |
 | Virtual Address
 ↓
MMU
 |
 | Address Translation
 ↓
Physical Address
 |
 ↓
RAM
```

### MMU

**MMU = Memory Management Unit**

It is hardware responsible for translating virtual addresses into physical addresses.

---

# 19. Demand Paging

## What is Demand Paging?

**Demand Paging** is a virtual-memory technique in which a page is loaded into RAM **only when it is actually needed**.

Instead of loading the complete process:

```text
Process
  ↓
Load only required pages
```

---

# 20. Page Fault

What happens when the CPU requires a page that is not currently in RAM?

A **Page Fault** occurs.

Example:

```text
CPU needs Page 5
       ↓
Is Page 5 in RAM?
       ↓
      NO
       ↓
Page Fault
       ↓
Find Page 5 on Disk
       ↓
Load Page 5 into RAM
       ↓
Update Page Table
       ↓
Restart instruction
```

---

# 21. Page Fault Handling

The general process is:

### Step 1

CPU generates a virtual address.

### Step 2

MMU checks the page table.

### Step 3

If the page is present:

```text
Page Hit
```

Memory access continues.

### Step 4

If the page is not present:

```text
Page Fault
```

### Step 5

OS finds a free frame.

### Step 6

Page is loaded from secondary storage.

### Step 7

Page table is updated.

### Step 8

The interrupted instruction is restarted.

---

# 22. Page Hit

A **page hit** occurs when the required page is already present in RAM.

```text
Required Page
      ↓
Page Table
      ↓
Present in RAM
      ↓
Page Hit
```

A page hit is much faster than a page fault.

---

# 23. Page Replacement

## Why is Page Replacement Needed?

Suppose:

```text
RAM has no free frames
```

and the CPU requests a page that is not in RAM.

The OS must remove one existing page and load the required page.

This is called **Page Replacement**.

```text
RAM

+--------+
| Page 1 |
+--------+
| Page 2 |
+--------+
| Page 3 |
+--------+
| Page 4 |
+--------+

New Page = Page 5

No free frame
     ↓
Choose a page to remove
     ↓
Remove Page 1
     ↓
Load Page 5
```

---

# 24. Page Replacement Algorithms

Important page replacement algorithms include:

1. FIFO
2. Optimal
3. LRU
4. Second Chance / Clock

---

# 25. FIFO Page Replacement

**FIFO = First In, First Out**

The page that entered memory first is removed first.

Example:

```text
Pages in memory:

[1] [2] [3]

1 entered first.

New page = 4

Remove 1:

[4] [2] [3]
```

### Advantage

* Simple.
* Easy to implement.

### Disadvantage

FIFO may remove an important/frequently used page.

---

# 26. Optimal Page Replacement

The **Optimal Page Replacement** algorithm replaces the page that will not be used for the longest period in the future.

Example:

```text
Current pages:

1  2  3

Future references:

2  1  4  3  2
```

If Page 4 is required now and we need to replace one page:

* Page 1 → used soon
* Page 2 → used soon
* Page 3 → used later

Therefore Page 3 can be selected.

### Important

Optimal replacement gives the **minimum possible number of page faults** for a given reference string.

However, it requires knowledge of future references, so it is mainly used as a theoretical benchmark.

---

# 27. LRU Page Replacement

**LRU = Least Recently Used**

LRU replaces the page that has not been used for the longest time in the past.

Example:

```text
Pages:

1  2  3

Recent usage:

3 → most recent
2
1 → least recent
```

If replacement is required:

```text
Remove Page 1
```

### Advantage

LRU generally follows the idea that pages used recently are more likely to be used again.

### Disadvantage

Maintaining exact usage information can require additional hardware/software support.

---

# 28. Page Fault vs Page Replacement

These two concepts are related but different.

### Page Fault

Occurs when the required page is **not in RAM**.

### Page Replacement

The action of **removing an existing page** to make room for the required page when no free frame is available.

```text
Page not in RAM
      ↓
Page Fault
      ↓
Free frame available?
      |
   YES → Load page
      |
   NO
      ↓
Page Replacement
      ↓
Remove victim page
      ↓
Load required page
```

---

# 29. Thrashing

## What is Thrashing?

**Thrashing** is a condition in which the system spends more time handling **page faults and swapping pages** than actually executing processes.

In simple words:

> The computer is busy moving pages between RAM and disk instead of doing useful work.

---

# 30. Example of Thrashing

Suppose RAM has very limited frames.

A process repeatedly needs:

```text
Page 1
Page 2
Page 3
Page 4
```

But RAM can hold only:

```text
2 pages
```

The system may repeatedly perform:

```text
Load P1
Load P2
Remove P1
Load P3
Remove P2
Load P4
Remove P3
Load P1
...
```

This causes many page faults.

The CPU spends much of its time waiting for pages.

---

# 31. Causes of Thrashing

Common causes include:

### 1. Too many processes

Too many processes compete for limited physical memory.

### 2. Insufficient RAM

There are not enough frames for the active processes.

### 3. High degree of multiprogramming

Too many processes are kept active simultaneously.

### 4. Poor locality

A process may frequently access pages that are not currently in memory.

---

# 32. Symptoms of Thrashing

A system experiencing thrashing may show:

* Very high page-fault rate.
* Low CPU utilization.
* High disk activity.
* Poor overall performance.
* Applications becoming extremely slow.

A common pattern is:

```text
More processes
      ↓
Less memory per process
      ↓
More page faults
      ↓
More disk activity
      ↓
Less CPU utilization
      ↓
System performance decreases
```

---

# 33. How to Control Thrashing

The OS can reduce thrashing using techniques such as:

### 1. Reduce Degree of Multiprogramming

Suspend or remove some processes from memory.

```text
Too many processes
        ↓
Reduce active processes
        ↓
More frames available per process
```

### 2. Allocate More Frames

Give a process more physical frames when possible.

### 3. Working Set Model

The OS keeps track of the pages that a process is actively using.

This collection of actively used pages is called the **Working Set**.

### 4. Page Fault Frequency

The OS can monitor the page-fault rate.

```text
High page fault rate
        ↓
Give process more frames
```

---

# 34. Working Set

The **working set** is the set of pages that a process is actively using during a particular period of time.

Example:

```text
Recent page references:

2 3 4 3 2 5 4
```

The working set may contain:

```text
{2, 3, 4, 5}
```

If the working set is not sufficiently present in RAM, frequent page faults can occur.

---

# 35. Complete Memory Management Flow

The topics can be understood as one continuous story:

```text
                    MEMORY MANAGEMENT
                           |
                           ↓
                       SWAPPING
                           |
                           ↓
                CONTIGUOUS ALLOCATION
                           |
             +-------------+-------------+
             |                           |
       Fixed Partitioning        Dynamic Partitioning
                                         |
                                         ↓
                                  Fragmentation
                              /                    \
                         Internal                External
                           |
                           ↓
                         PAGING
                           |
                    Page + Frame
                           |
                           ↓
                      Page Table
                           |
                           ↓
                    Virtual Memory
                           |
                           ↓
                    Demand Paging
                           |
                    +------+------+
                    |             |
                Page Hit      Page Fault
                                  |
                                  ↓
                         Is Free Frame?
                           /        \
                         Yes         No
                          |           |
                       Load Page   Page Replacement
                                      |
                           +----------+----------+
                           |          |          |
                          FIFO     Optimal      LRU
                                      |
                                      ↓
                              Too Many Faults
                                      |
                                      ↓
                                  THRASHING
                                      |
                                      ↓
                              Reduce Thrashing
```

---

# 36. Important Differences

## Swapping vs Paging

| Swapping                                        | Paging                            |
| ----------------------------------------------- | --------------------------------- |
| Usually moves a process or large memory portion | Moves individual pages            |
| Process can be moved between RAM and disk       | Pages can be loaded independently |
| Relatively coarse-grained                       | Fine-grained                      |
| Can be expensive                                | Supports efficient virtual memory |

---

## Paging vs Segmentation

| Paging                                      | Segmentation                               |
| ------------------------------------------- | ------------------------------------------ |
| Fixed-size blocks                           | Variable-size blocks                       |
| Page is physical-memory management unit     | Segment represents logical program unit    |
| Page number + offset                        | Segment number + offset                    |
| Internal fragmentation possible             | External fragmentation possible            |
| Programmer normally does not think in pages | Segments can correspond to code/data/stack |

---

## Demand Paging vs Page Replacement

| Demand Paging                                  | Page Replacement                                   |
| ---------------------------------------------- | -------------------------------------------------- |
| Loads pages only when needed                   | Selects a page to remove                           |
| Used for virtual memory                        | Used when RAM has no free frame                    |
| Can cause page faults                          | Handles the lack of free frames after a page fault |
| Decides when a page should be brought into RAM | Decides which existing page should leave RAM       |

---

# 37. Important Terms for Exams

| Term                   | Meaning                                                             |
| ---------------------- | ------------------------------------------------------------------- |
| Page                   | Fixed-size block of logical/virtual memory                          |
| Frame                  | Fixed-size block of physical memory                                 |
| Page Table             | Maps pages to frames                                                |
| MMU                    | Translates virtual addresses to physical addresses                  |
| Page Fault             | Required page is not present in RAM                                 |
| Page Hit               | Required page is already in RAM                                     |
| Swapping               | Moving memory/process contents between RAM and secondary storage    |
| Fragmentation          | Wasted memory space                                                 |
| Internal Fragmentation | Wasted space inside an allocated block                              |
| External Fragmentation | Free memory scattered between allocated blocks                      |
| Virtual Memory         | Allows execution without loading the entire process into RAM        |
| Demand Paging          | Loads pages when they are actually needed                           |
| Page Replacement       | Replaces an existing page with a required page                      |
| FIFO                   | Removes the oldest loaded page                                      |
| Optimal                | Removes page whose next use is farthest in the future               |
| LRU                    | Removes least recently used page                                    |
| Thrashing              | Excessive paging/page faults causing severe performance degradation |
| Working Set            | Pages actively used by a process during a period                    |

---

# 38. Exam-Oriented Questions

## Short Answer Questions

1. What is swapping?
2. What is contiguous memory allocation?
3. Define internal fragmentation.
4. Define external fragmentation.
5. What is paging?
6. Differentiate page and frame.
7. What is a page table?
8. What is segmentation?
9. What is virtual memory?
10. What is demand paging?
11. What is a page fault?
12. What is page replacement?
13. What is FIFO page replacement?
14. What is LRU?
15. What is optimal page replacement?
16. What is thrashing?
17. What is a working set?
18. What is the role of MMU?

---

# 39. Long Answer Questions

### Q1. Explain contiguous memory allocation and its types.

Include:

```text
Definition
↓
Fixed Partitioning
↓
Dynamic Partitioning
↓
Internal Fragmentation
↓
External Fragmentation
↓
First Fit
↓
Best Fit
↓
Worst Fit
```

---

### Q2. Explain paging with address translation.

Include:

```text
Page
Frame
Page Table
Logical Address
Page Number
Offset
Physical Address
MMU
```

---

### Q3. Explain segmentation with a suitable example.

Include:

```text
Segment
Segment Table
Base
Limit
Logical Address
Address Translation
```

---

### Q4. Explain virtual memory and demand paging.

Include:

```text
Virtual Memory
↓
Demand Paging
↓
Page Fault
↓
Page Fault Handling
```

---

### Q5. Explain page replacement algorithms.

Discuss:

```text
FIFO
Optimal
LRU
```

with examples and page-fault calculations.

---

### Q6. What is thrashing? Explain its causes and prevention.

Include:

```text
Definition
↓
Causes
↓
Symptoms
↓
Working Set
↓
Page Fault Frequency
↓
Reducing Degree of Multiprogramming
```

---

# 40. Quick Revision

Remember the complete sequence:

```text
SWAPPING
    ↓
Move processes/pages between RAM and disk
    ↓
CONTIGUOUS ALLOCATION
    ↓
Process gets continuous memory
    ↓
Fragmentation problem
    ↓
PAGING
    ↓
Fixed-size pages + frames
    ↓
PAGE TABLE
    ↓
Virtual → Physical address translation
    ↓
VIRTUAL MEMORY
    ↓
Entire process does not need to be in RAM
    ↓
DEMAND PAGING
    ↓
Load page only when required
    ↓
PAGE FAULT
    ↓
Required page is not in RAM
    ↓
PAGE REPLACEMENT
    ↓
Choose page to remove
    ↓
FIFO / OPTIMAL / LRU
    ↓
Too many page faults
    ↓
THRASHING
```

## One-Line Definitions

> **Swapping:** Moving a process between RAM and secondary storage.

> **Contiguous Allocation:** Allocating one continuous block of memory to a process.

> **Paging:** Dividing logical memory into pages and physical memory into frames.

> **Segmentation:** Dividing a program into variable-sized logical segments.

> **Virtual Memory:** Technique that allows programs to execute without the entire program being present in RAM.

> **Demand Paging:** Loading a page into RAM only when it is required.

> **Page Fault:** Occurs when a required page is not present in RAM.

> **Page Replacement:** Selecting an existing page to remove when a required page needs a frame.

> **Thrashing:** Excessive paging that causes the system to spend more time handling page faults than executing processes.
