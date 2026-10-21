# Lecture 20: Swapping & Contiguous Memory Allocation

**Subject:** Operating Systems\
**Total lecture time:** 50 minutes\
**Purpose:** Exam-oriented notes with definitions, diagrams, examples,
comparison tables, and practice questions.

------------------------------------------------------------------------

## 1. Swapping Concept (10 minutes)

### Definition

**Swapping** is a memory-management technique in which the operating
system temporarily transfers a process from main memory (RAM) to
secondary storage and later brings it back to RAM when it is ready to
continue.

-   **Swap out:** Move a process from RAM to secondary storage.
-   **Swap in:** Bring a process back into RAM.
-   The area on secondary storage used for this purpose is often called
    **swap space**.

### Diagram

``` text
             Main Memory (RAM)                 Secondary Storage
          +----------------------+             +------------------+
          | Operating System     |             |                  |
          +----------------------+   Swap out  |   Process B      |
          | Process A            | ----------> |   (Swap space)   |
          +----------------------+             |                  |
          | Process B            |             +------------------+
          +----------------------+   Swap in
                    <--------------------------
```

### Why is swapping used?

1.  To free RAM when memory is needed by other processes.
2.  To support a larger number of processes than can fit in RAM at one
    time.
3.  To improve memory availability under pressure.

### Advantages

-   Frees RAM for other processes.
-   Helps the operating system manage limited physical memory.
-   Can increase the degree of multiprogramming.

### Disadvantages

-   Disk or SSD access is much slower than RAM.
-   Moving processes takes time and can reduce performance.
-   Excessive swapping can lead to **thrashing**, where the system
    spends too much time moving data rather than executing processes.

### Important exam point

Swapping is different from **context switching**. A context switch
changes which process the CPU executes; it does not necessarily move a
process out of RAM.

------------------------------------------------------------------------

## 2. Contiguous Memory Allocation (15 minutes)

### Definition

**Contiguous memory allocation** is a memory-allocation method in which
each process is placed in one continuous block of physical memory.

### Basic diagram

``` text
Higher memory addresses
+---------------------------+
| Free space                |
+---------------------------+
| Process C                 |
+---------------------------+
| Process B                 |
+---------------------------+
| Process A                 |
+---------------------------+
| Operating System          |
+---------------------------+
Lower memory addresses
```

*This is a simplified example; the actual layout depends on the
operating system.*

### Types of contiguous allocation

#### A. Single-partition allocation

Memory is divided between the operating system and one user process. It
is simple but limits how many user processes can reside in memory.

#### B. Multiple-partition allocation

Memory contains several regions so multiple processes can be loaded at
once. Common approaches are fixed partitioning and variable
partitioning.

### Placement strategies

In variable-partition systems, the OS can use strategies to choose a
free hole:

-   **First fit:** Choose the first hole large enough for the process.
-   **Best fit:** Choose the smallest hole that is large enough.
-   **Worst fit:** Choose the largest available hole.

### Example

Suppose free holes are 100 KB, 300 KB, and 500 KB, and a process needs
250 KB: - First fit chooses the first hole that fits, which is 300 KB if
holes are checked in the order shown. - Best fit chooses 300 KB because
it is the smallest hole that fits. - Worst fit chooses 500 KB.

### Advantages

-   Simple to implement and understand.
-   Address translation can be straightforward.
-   Low overhead compared with some more complex schemes.

### Limitations

-   Can cause internal or external fragmentation, depending on the
    scheme.
-   A process may not fit even when the total free memory is sufficient,
    if no single hole is large enough.
-   Expanding a process can be difficult if adjacent space is
    unavailable.

### Base and limit registers

A simple relocation and protection scheme may use: - **Base register:**
Starting physical address of the process. - **Limit register:** Size or
permitted address range of the process.

For a logical address `L`, the hardware checks that `L` is within the
allowed limit. If valid, the physical address is calculated as:

`Physical address = Base + L`

``` text
CPU generates logical address L
              |
              v
       Is L within limit?
          /       \
        No         Yes
        |           |
  Trap / error      v
                Base + L
                    |
                    v
             Physical address
```

------------------------------------------------------------------------

## 3. Fixed vs Variable Partitioning (10 minutes)

### Fixed partitioning

Main memory is divided into a set of partitions of predetermined sizes.
Each partition can hold at most one process at a time.

``` text
+----------------------+
| OS                   |
+----------------------+
| Partition 1: Process A|
+----------------------+
| Partition 2: Free     |
+----------------------+
| Partition 3: Process B|
+----------------------+
| Partition 4: Free     |
+----------------------+
```

**Advantages** - Simple to manage. - Partition boundaries are
predetermined. - Easy to track free partitions.

**Disadvantages** - A process larger than the largest partition cannot
be loaded into one partition. - Can waste space inside an allocated
partition (internal fragmentation). - The number of resident processes
is limited by the number of partitions.

### Variable partitioning

Partitions are created dynamically according to process size and
available memory.

``` text
+----------------------+
| OS                   |
+----------------------+
| Process A            |
+----------------------+
| Free hole            |
+----------------------+
| Process B            |
+----------------------+
| Process C            |
+----------------------+
| Free hole            |
+----------------------+
```

**Advantages** - Partitions can better match process sizes. - Usually
reduces internal fragmentation compared with fixed partitions.

**Disadvantages** - Free memory can become scattered into small holes
(external fragmentation). - More complex to manage. - Compaction may be
needed.

### Comparison table

  -----------------------------------------------------------------------
  Feature                 Fixed Partitioning      Variable Partitioning
  ----------------------- ----------------------- -----------------------
  Partition size          Predetermined           Created according to
                                                  need

  Number of partitions    Usually fixed           Changes as processes
                                                  enter and leave

  Internal fragmentation  Common                  Usually lower, but can
                                                  still occur

  External fragmentation  Not the main classic    Common
                          problem                 

  Management              Simpler                 More complex

  Compaction              Usually not used to     May be used to combine
                          solve the main issue    scattered free space
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 4. Internal vs External Fragmentation (10 minutes)

### Internal fragmentation

**Internal fragmentation** is wasted space *inside* an allocated memory
block.

Example: A fixed partition is 100 KB, but the process uses only 70 KB.
The remaining 30 KB is internal fragmentation.

``` text
Allocated partition: 100 KB
+---------------------------+
| Process uses 70 KB        |
+---------------------------+
| Waste inside: 30 KB       |
+---------------------------+
```

### External fragmentation

**External fragmentation** occurs when free memory is split into small,
separated holes. Total free memory may be enough for a process, but no
individual hole is large enough.

``` text
+-----------+----------+-----------+----------+-----------+
| Process A | Free 20K | Process B | Free 30K | Process C |
+-----------+----------+-----------+----------+-----------+

Total free = 50 KB, but the largest single hole is only 30 KB.
A 40 KB process cannot fit into either hole without a suitable
rearrangement or another allocation method.
```

### Comparison table

  -----------------------------------------------------------------------
  Internal Fragmentation              External Fragmentation
  ----------------------------------- -----------------------------------
  Waste is inside an allocated block. Free space is scattered between
                                      allocated blocks.

  Common with fixed-size partitions   Common with variable-sized
  and pages.                          contiguous allocation.

  The allocated block is larger than  Total free memory may be sufficient
  the memory actually used.           but not contiguous.

  Reducing allocation-unit size may   Compaction may help when relocation
  reduce it.                          is supported.
  -----------------------------------------------------------------------

### Quick memory aid

-   **Internal = inside** the allocated block.
-   **External = outside** allocated blocks, in separated free holes.

------------------------------------------------------------------------

## 5. Compaction (5 minutes)

### Definition

**Compaction** is a technique used to reduce external fragmentation by
relocating processes so that scattered free holes are combined into one
larger contiguous block.

### Before compaction

``` text
+-----------+----------+-----------+----------+-----------+
| Process A | Free 20K | Process B | Free 30K | Process C |
+-----------+----------+-----------+----------+-----------+
```

### After compaction

``` text
+-----------+-----------+-----------+--------------------+
| Process A | Process B| Process C | Free space: 50 KB  |
+-----------+-----------+-----------+--------------------+
```

### Advantages

-   Combines scattered free holes into a larger contiguous block.
-   May allow a process to be allocated when no individual hole was
    large enough before compaction.

### Limitations

-   Moving processes consumes time and system resources.
-   Requires relocation support so that processes can safely run from
    new physical locations.
-   Does not solve internal fragmentation.

**Important:** Compaction is most relevant to variable-sized contiguous
allocation. It is not generally needed to address external fragmentation
in paging because pages can occupy non-contiguous physical frames.

------------------------------------------------------------------------

## 6. Examination Questions and Quick Revision

### Short-answer questions

1.  **Define swapping.**\
    Swapping temporarily moves a process from RAM to secondary storage
    and brings it back when needed.

2.  **What are swap in and swap out?**\
    Swap out moves a process from RAM to storage; swap in brings it back
    into RAM.

3.  **What is contiguous memory allocation?**\
    It is a technique in which each process occupies one continuous
    block of physical memory.

4.  **Differentiate fixed and variable partitioning.**\
    Fixed partitioning uses predetermined memory regions; variable
    partitioning creates regions according to process sizes and
    available memory.

5.  **What is internal fragmentation?**\
    It is unused space inside an allocated memory block.

6.  **What is external fragmentation?**\
    It is free memory scattered among allocated blocks so a large
    contiguous request may not fit.

7.  **What is compaction?**\
    Compaction relocates processes to combine scattered free holes into
    one larger free block.

8.  **Name three placement strategies.**\
    First fit, best fit, and worst fit.

### Multiple-choice practice

**1. Moving a process from RAM to secondary storage is called:**\
A. Paging\
B. Swap out\
C. Compaction\
D. Compilation\
**Answer: B. Swap out**

**2. Waste inside an allocated partition is called:**\
A. External fragmentation\
B. Thrashing\
C. Internal fragmentation\
D. Swapping\
**Answer: C. Internal fragmentation**

**3. Which technique can combine scattered free holes?**\
A. Compaction\
B. Context switching\
C. Compilation\
D. Spooling\
**Answer: A. Compaction**

**4. In fixed partitioning, partition sizes are:**\
A. Always changed for every process\
B. Predetermined\
C. Never tracked\
D. Stored only on disk\
**Answer: B. Predetermined**

**5. Which placement strategy selects the smallest hole that is large
enough?**\
A. First fit\
B. Best fit\
C. Worst fit\
D. Round robin\
**Answer: B. Best fit**

### Last-minute revision points

-   Swapping moves processes between RAM and secondary storage.
-   Contiguous allocation places each process in one continuous
    physical-memory block.
-   Fixed partitions are predetermined; variable partitions are created
    dynamically.
-   Internal fragmentation is waste inside an allocated block.
-   External fragmentation is scattered free memory.
-   Compaction combines scattered free holes but requires relocation
    support.
-   First fit selects the first suitable hole; best fit selects the
    smallest suitable hole; worst fit selects the largest hole.

------------------------------------------------------------------------

## Suggested 50-minute lecture plan

  Topic                                    Duration
  ------------------------------------ ------------
  Swapping Concept                           10 min
  Contiguous Memory Allocation               15 min
  Fixed vs Variable Partitioning             10 min
  Internal vs External Fragmentation         10 min
  Compaction                                  5 min
  **Total**                              **50 min**
