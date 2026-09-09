# Operating System — Memory Management, Virtual Memory, Paging & Segmentation

## Topics Covered

1. Why Virtual Memory?
2. Address Space
3. Concept of Paging
4. Concept of Segmentation
5. Virtual Memory Overview
6. Linux Practical Demonstrations
7. Quick Revision Questions

---

# 1. Why Virtual Memory?

## The Real-World Problem

Suppose a computer has:

```text
RAM = 8 GB
```

But the applications currently running need:

```text
Chrome       → 3 GB
VS Code      → 2 GB
Other Apps   → 4 GB
Operating System → 2 GB
------------------------
Total        → 11 GB
```

The required memory is greater than the available physical RAM.

### Question

> How can the computer continue running these applications?

This leads to the concept of **Virtual Memory**.

---

## What is Virtual Memory?

**Virtual memory is a memory-management technique that gives processes the illusion of having a large, continuous memory space, even when physical RAM is limited.**

The operating system and hardware manage the mapping between:

```text
Virtual Memory
      ↓
Physical Memory
```

Some required data can be kept in RAM, while less-needed data may be moved to secondary storage when necessary.

```text
             PROCESS
                |
                v
       Virtual Address Space
                |
                v
       +------------------+
       | Memory Management|
       +------------------+
           /          \
          /            \
         v              v
       RAM             SSD
    Physical         Secondary
     Memory           Storage
```

### Important

Virtual memory is **not faster than RAM**.

RAM is much faster than SSD/HDD. Excessive use of disk-backed virtual memory can significantly reduce performance.

---

# 2. Why Do We Need Virtual Memory?

Virtual memory provides several important benefits.

### 1. Run Programs Larger Than Physical RAM

A process may have a large virtual address space even if the available physical RAM is smaller.

### 2. Better Memory Utilization

Only the parts of a program that are currently required need to be kept in RAM.

### 3. Process Isolation

Each process gets its own virtual address space.

For example:

```text
Process A:
Virtual Address 1000 → Physical Address 5000

Process B:
Virtual Address 1000 → Physical Address 9000
```

Both processes can use the same virtual address without accessing each other's physical memory.

### 4. Protection

A process normally cannot directly access another process's memory.

This improves system stability and security.

### 5. Easier Programming Model

Programs can work with a large, private address space without needing to know exactly where their data is physically located in RAM.

---

# 3. Address Space

## What is an Address?

Memory is divided into locations, and each location has an address.

A simple conceptual example:

```text
Address
-------
0
1
2
3
4
5
...
1000
```

A program uses addresses to access its instructions and data.

However, modern operating systems generally do not allow a normal application to directly work with raw physical RAM addresses.

Instead, processes use **virtual addresses**.

---

# 4. Virtual Address

A **virtual address** is an address generated/used by a process.

Conceptually:

```text
Program
   |
   | Virtual Address
   v
Address Translation
   |
   | Physical Address
   v
Physical RAM
```

The translation is handled by the operating system together with hardware, especially the **Memory Management Unit (MMU)**.

---

# 5. Virtual Address vs Physical Address

## Virtual Address

An address from the process's point of view.

Example:

```text
Process A
Virtual Address = 1000
```

## Physical Address

The actual location in physical RAM.

Example:

```text
Physical Address = 5000
```

So:

```text
Virtual Address 1000
        ↓
    Translation
        ↓
Physical Address 5000
```

---

# 6. Important Real Example — Process Isolation

Suppose two programs are running:

```text
Process A
Virtual Address 1000
        ↓
Physical Address 5000
```

```text
Process B
Virtual Address 1000
        ↓
Physical Address 9000
```

Both processes believe they are accessing address `1000`.

But they actually access different physical memory.

```text
             Virtual Address
                   1000
                  /    \
                 /      \
                v        v
          Process A    Process B
                |          |
                v          v
          Physical 5000  Physical 9000
```

This is one of the key ideas behind **process isolation**.

---

# 7. Process Address Space

A process normally has several logical memory regions.

A simplified process address-space diagram:

```text
+----------------------+
| Stack                |
+----------------------+
|                      |
|       Free Space     |
|                      |
+----------------------+
| Heap                 |
+----------------------+
| Data                 |
+----------------------+
| Code / Text          |
+----------------------+
```

## Code / Text

Contains program instructions.

Example:

```c
printf("Hello");
```

The compiled instructions are stored in the code/text region.

## Data

Contains program data such as global/static variables.

## Heap

Used for dynamically allocated memory.

Example in C:

```c
int *arr = malloc(100 * sizeof(int));
```

## Stack

Used for function calls, local variables, function parameters, etc.

---

# 8. Memory Management Unit (MMU)

The **MMU (Memory Management Unit)** is hardware responsible for translating virtual addresses into physical addresses.

Conceptually:

```text
CPU
 |
 | Virtual Address
 v
+----------------+
|      MMU       |
| Address        |
| Translation    |
+----------------+
 |
 | Physical Address
 v
RAM
```

The MMU works with data structures such as **page tables**.

---

# 9. Paging

Paging is one of the most important virtual-memory concepts.

## Basic Idea

Paging divides:

- Virtual memory into **fixed-size pages**
- Physical memory into **fixed-size frames**

```text
Virtual Memory

+--------+
| Page 0 |
+--------+
| Page 1 |
+--------+
| Page 2 |
+--------+
| Page 3 |
+--------+
```

Physical memory:

```text
RAM

+---------+
| Frame 0 |
+---------+
| Frame 1 |
+---------+
| Frame 2 |
+---------+
| Frame 3 |
+---------+
| Frame 4 |
+---------+
```

The size of a page and frame is the same.

For example:

```text
Page Size = 4 KB
Frame Size = 4 KB
```

---

# 10. Pages and Frames

### Page

A fixed-size block of **virtual memory**.

### Frame

A fixed-size block of **physical memory (RAM)**.

Remember:

```text
Virtual Memory → Pages

Physical Memory → Frames
```

Easy memory trick:

```text
PAGE = Virtual

FRAME = Physical
```

---

# 11. Pages Do Not Need to Be Contiguous

Suppose a process has four pages:

```text
Page 0
Page 1
Page 2
Page 3
```

They can be placed in different frames:

```text
Page 0 → Frame 3
Page 1 → Frame 0
Page 2 → Frame 4
Page 3 → Frame 1
```

Physical RAM may look like:

```text
+---------+
| Frame 0 | ← Page 1
+---------+
| Frame 1 | ← Page 3
+---------+
| Frame 2 | ← Other Process
+---------+
| Frame 3 | ← Page 0
+---------+
| Frame 4 | ← Page 2
+---------+
```

Therefore, the complete process does **not** need one large continuous block of physical RAM.

---

# 12. Page Table

How does the system know where each page is stored?

It uses a **Page Table**.

Example:

```text
Page       Frame
----------------
0    →       3
1    →       0
2    →       4
3    →       1
```

The page table stores the mapping:

```text
Virtual Page → Physical Frame
```

Conceptually:

```text
CPU
 |
 | Virtual Address
 v
Page Table
 |
 | Page → Frame
 v
Physical RAM
```

---

# 13. Practical Paging Example

Suppose:

```text
Page Size = 4 KB
```

A program needs:

```text
10 KB
```

The memory requirement can be divided into:

```text
Page 0 → 4 KB
Page 1 → 4 KB
Page 2 → 2 KB
```

So the process needs 3 pages.

These pages can be placed anywhere in available frames.

Example:

```text
RAM

Frame 0 → Page 1
Frame 1 → Other Process
Frame 2 → Page 0
Frame 3 → Other Process
Frame 4 → Page 2
```

The pages don't need to be adjacent.

---

# 14. Why Paging is Useful

Paging provides several advantages.

### 1. No Need for Large Contiguous Physical Memory

A process can use separate frames.

### 2. Supports Virtual Memory

Pages can be moved between RAM and secondary storage as needed.

### 3. Reduces External Fragmentation

Because memory is allocated in fixed-size frames, external fragmentation is greatly reduced.

### 4. Provides Memory Protection

Page-table permissions can control whether memory is readable, writable, or executable.

---

# 15. Paging and Address Translation

A virtual address can conceptually be divided into:

```text
+-------------------+----------------+
| Page Number       | Offset         |
+-------------------+----------------+
```

The:

- **Page number** identifies the page.
- **Offset** identifies a location inside that page.

The page number is used to find the corresponding frame.

```text
Virtual Address
      |
      +---- Page Number
      |
      +---- Offset
              |
              v
        Page Table
              |
              v
         Frame Number
```

The physical address is then formed from:

```text
Frame Number + Offset
```

---

# 16. Simple Address Translation Example

Suppose:

```text
Page Size = 100 bytes
Virtual Address = 250
```

Then:

```text
Page Number = 250 / 100 = 2
Offset = 250 % 100 = 50
```

So:

```text
Virtual Address 250
       |
       +---- Page = 2
       |
       +---- Offset = 50
```

Suppose:

```text
Page 2 → Frame 7
```

Then the physical address is:

```text
Physical Address
= Frame × Page Size + Offset
= 7 × 100 + 50
= 750
```

Therefore:

```text
Virtual Address 250
        ↓
Page 2 + Offset 50
        ↓
Page Table
        ↓
Frame 7 + Offset 50
        ↓
Physical Address 750
```

This is a useful classroom calculation.

---

# 17. Segmentation

Paging divides memory into **fixed-size blocks**.

Segmentation uses a different idea.

> Segmentation divides a program according to its logical parts.

A program can be viewed as:

```text
Program
   |
   +--- Code
   |
   +--- Data
   |
   +--- Stack
   |
   +--- Heap
```

These logical parts can be treated as segments.

---

# 18. Example of Segments

```text
Segment 0 → Code
Segment 1 → Data
Segment 2 → Stack
Segment 3 → Heap
```

Unlike pages, segments are generally **variable-sized**.

For example:

```text
Code → 20 KB
Data → 10 KB
Stack → 8 KB
Heap → 50 KB
```

The sizes don't have to be equal.

---

# 19. Real-Life Segmentation Analogy

Imagine a college bag.

Instead of putting everything randomly into one large compartment, you organize it into logical sections:

```text
Bag
 |
 +--- Books
 |
 +--- Laptop
 |
 +--- Charger
 |
 +--- Documents
```

Each section has a different purpose and potentially a different size.

Similarly, a program can be organized into logical segments:

```text
Program
 |
 +--- Code
 +--- Data
 +--- Stack
 +--- Heap
```

### Easy memory trick

```text
PAGING
P → Pieces
Fixed-size pieces

SEGMENTATION
S → Sections
Logical sections
```

---

# 20. Paging vs Segmentation

| Paging | Segmentation |
|---|---|
| Fixed-size blocks | Variable-size blocks |
| Virtual memory is divided into pages | Program is divided into logical segments |
| Physical memory is divided into frames | Segments represent logical program parts |
| Page size is fixed | Segment size can vary |
| Does not directly represent program structure | Closely represents program structure |
| Reduces external fragmentation | Can suffer from external fragmentation |
| Uses page tables | Uses segment tables |

---

# 21. Virtual Memory — Complete Picture

Suppose you open a large application such as Chrome.

Conceptually:

```text
                  PROCESS
                     |
                     v
             Virtual Address Space
                     |
                     v
                  PAGES
                     |
                     v
                PAGE TABLE
                     |
          +----------+----------+
          |                     |
          v                     v
        RAM                    SSD
     Physical               Secondary
      Frames                 Storage
```

Only the pages currently required by the process need to be present in RAM.

Other pages may remain on secondary storage.

---

# 22. Page Fault

Suppose the CPU needs a page.

First, the system checks whether the page is currently in RAM.

```text
CPU requests page
       |
       v
Is page in RAM?
    /       \
  YES       NO
   |         |
   v         v
Continue   Page Fault
             |
             v
       Locate page on
       secondary storage
             |
             v
       Load page into RAM
             |
             v
          Continue
```

A **page fault** occurs when a required page is not currently available in physical memory.

### Important

A page fault is not automatically an error.

It is a normal mechanism of virtual-memory systems.

However, frequent page faults can make a system slow because accessing storage is much slower than accessing RAM.

---

# 23. Demand Paging

**Demand paging** is a technique where pages are loaded into RAM when they are actually needed.

Instead of loading the entire program into RAM:

```text
Load Everything
```

the system can load pages as required:

```text
Start Program
     |
     v
Load Required Pages
     |
     v
Run
     |
     v
Need another page?
     |
     v
Load it
```

This saves RAM and allows programs to use large virtual address spaces.

---

# 24. Virtual Memory and Swap

Linux and other operating systems can use secondary storage as part of their memory-management strategy.

Linux may have a **swap area**, which can be a partition or a swap file.

Conceptually:

```text
RAM
+----------------+
| Active Pages   |
+----------------+
| Active Pages   |
+----------------+
| Free Frames    |
+----------------+

Swap
+----------------+
| Less-used data |
+----------------+
```

When memory pressure becomes high, the OS may move some memory contents to swap.

### Important

```text
RAM  → Fast
SSD  → Much slower than RAM
HDD  → Even slower
```

Therefore, heavy swapping can cause noticeable performance problems.

---

# 25. Linux Practical — Observe RAM

Run:

```bash
free -h
```

Example:

```text
              total   used   free
Mem:           15Gi    5Gi    7Gi
Swap:           2Gi    0Gi    2Gi
```

### Understand the output

```text
total
```

Total physical memory.

```text
used
```

Memory currently in use according to the tool's accounting.

```text
free
```

Currently unused memory.

```text
Swap
```

Configured swap space.

### Important Linux Note

Do not assume:

```text
free = memory that applications can use
```

Linux aggressively uses available RAM for caches. `available` is often more useful when estimating how much memory can be allocated without significant swapping.

---

# 26. Watch Memory Usage Live

Run:

```bash
watch -n 1 free -h
```

Now open:

```text
Chrome
VS Code
File Manager
Other applications
```

Observe the memory values changing.

Stop the command with:

```text
Ctrl + C
```

### Learning Goal

Students can visually observe that running applications change system memory usage.

---

# 27. Practical — Observe Processes

Run:

```bash
top
```

or:

```bash
htop
```

If `htop` is not installed:

```bash
sudo apt install htop
```

Example:

```text
PID    USER    %CPU    %MEM    COMMAND
1234   user     5.2     3.4    chrome
2345   user     2.1     2.5    code
```

Explain:

```text
PID
```

Process ID.

```text
%MEM
```

Approximate percentage of physical memory associated with the process.

```text
COMMAND
```

The running program/process command.

### Classroom Activity

1. Open Chrome.
2. Observe `top`/`htop`.
3. Open VS Code.
4. Observe memory usage.
5. Close VS Code.
6. Observe the changes.

---

# 28. Practical — Process Memory Information

Run:

```bash
ps aux
```

For a specific process:

```bash
ps -p PID -o pid,cmd,%mem,rss,vsz
```

Example:

```text
PID   CMD       %MEM    RSS    VSZ
1234  chrome     3.2   50000  200000
```

## RSS

**RSS = Resident Set Size**

At a beginner level:

> The amount of physical memory currently resident for the process.

## VSZ

**VSZ = Virtual Memory Size**

At a beginner level:

> The amount of virtual address space associated with the process.

Do not treat VSZ as simply "RAM used".

---

# 29. Practical — Observe System Memory Information

Run:

```bash
cat /proc/meminfo
```

You will see information such as:

```text
MemTotal
MemFree
MemAvailable
SwapTotal
SwapFree
...
```

This demonstrates that Linux exposes system memory information through:

```text
/proc
```

`/proc` is a virtual filesystem that exposes information about processes and the running kernel.

---

# 30. Practical — Observe Process Address Space

Run:

```bash
cat /proc/self/maps
```

You may see entries resembling:

```text
55abc...-55abd... r-xp
7fabc...-7fac... rw-p
...
```

You do not need to explain every hexadecimal address initially.

Tell students:

> A process has its own virtual address space containing multiple memory regions.

You can connect this to:

```text
+------------------+
| Stack            |
+------------------+
|                  |
|    Free Space    |
|                  |
+------------------+
| Heap             |
+------------------+
| Data             |
+------------------+
| Code / Text      |
+------------------+
```

---

# 31. Practical Activity — Compare Two Processes

Open two applications:

```text
Terminal
VS Code
```

Find their PIDs:

```bash
ps aux | grep code
```

Then inspect a particular PID:

```bash
ps -p PID -o pid,cmd,%mem,rss,vsz
```

Students can compare:

```text
Process A
Virtual Memory → ...
Physical Memory → ...

Process B
Virtual Memory → ...
Physical Memory → ...
```

This helps connect the theory of **virtual address spaces** to real Linux processes.

---

# 32. Common Student Confusions

## Confusion 1

> Is virtual memory the same as RAM?

**No.**

Virtual memory is an abstraction/memory-management technique.

RAM is physical memory.

---

## Confusion 2

> Is swap the same as virtual memory?

Not exactly.

Virtual memory is the broader concept.

Swap is one mechanism used by operating systems to move memory contents between RAM and secondary storage.

---

## Confusion 3

> Is every virtual address a physical RAM address?

**No.**

A virtual address must be translated to a physical address before accessing physical memory.

---

## Confusion 4

> Is a page the same as a frame?

They have the same size, but they refer to different things.

```text
Page  → Virtual Memory
Frame → Physical Memory
```

---

## Confusion 5

> Does paging mean the entire process is split into RAM immediately?

No.

With virtual memory and demand paging, only required pages may be loaded into RAM.

---

## Confusion 6

> Is a page fault always an error?

No.

A page fault can be a normal part of demand paging.

---

# 33. One Complete Example

Suppose:

```text
Computer RAM = 8 GB
```

A process has a large virtual address space.

The process is divided into pages:

```text
Page 0
Page 1
Page 2
Page 3
Page 4
...
```

Physical RAM contains frames:

```text
Frame 0
Frame 1
Frame 2
Frame 3
...
```

The page table maps:

```text
Page 0 → Frame 3
Page 1 → Frame 1
Page 2 → Frame 7
Page 3 → Not currently in RAM
```

If the CPU accesses Page 2:

```text
CPU
 ↓
Virtual Address
 ↓
Page Table
 ↓
Page 2 → Frame 7
 ↓
RAM
```

If the CPU accesses Page 3:

```text
CPU
 ↓
Virtual Address
 ↓
Page Table
 ↓
Page 3 not in RAM
 ↓
Page Fault
 ↓
Load Page 3
 ↓
RAM
 ↓
Continue execution
```

This is the core idea behind virtual memory and paging.

---

# 34. Complete Concept Map

```text
                  MEMORY MANAGEMENT
                         |
                         v
                 Virtual Memory
                         |
            +------------+------------+
            |                         |
            v                         v
      Address Space                 Paging
            |                         |
            v                         v
      Virtual Address          Pages + Frames
            |                         |
            v                         v
          MMU                    Page Table
            |                         |
            v                         v
     Physical Address            Address Translation
            |                         |
            +------------+------------+
                         |
                         v
                        RAM
                         |
                 Page Not Present?
                         |
                         v
                    Page Fault
                         |
                         v
                       Swap/
                  Secondary Storage
```

---

# 35. Paging vs Segmentation — Quick Revision

```text
PAGING
P → Pieces

Virtual memory
    ↓
Fixed-size pages

Physical memory
    ↓
Fixed-size frames
```

```text
SEGMENTATION
S → Sections

Program
    ↓
Logical sections

Code
Data
Stack
Heap
```

### One-line Difference

> **Paging divides memory into fixed-size blocks; segmentation divides a program into logical, variable-size sections.**

---

# 36. 50-Minute Lecture Plan

## 0–5 Minutes — Real-World Problem

Ask:

> "My laptop has 8 GB RAM. How can I run many applications simultaneously?"

Introduce:

**Virtual Memory**

---

## 5–12 Minutes — Address Space

Explain:

```text
Process
   ↓
Virtual Address
   ↓
MMU / Translation
   ↓
Physical Address
   ↓
RAM
```

Use the Process A / Process B example.

---

## 12–25 Minutes — Paging

Explain:

```text
Virtual Memory → Pages
Physical RAM   → Frames
```

Then introduce:

```text
Page Table
```

Do the numerical example:

```text
Page Size = 100 bytes
Virtual Address = 250
Page = 2
Offset = 50
Page 2 → Frame 7
Physical Address = 750
```

---

## 25–33 Minutes — Segmentation

Explain:

```text
Code
Data
Stack
Heap
```

Use the college-bag analogy.

---

## 33–40 Minutes — Virtual Memory Complete Picture

Explain:

```text
CPU
 ↓
Virtual Address
 ↓
Page Table
 ↓
Physical Frame
 ↓
RAM
```

Then:

```text
Page Not in RAM
 ↓
Page Fault
 ↓
Load from Storage
 ↓
RAM
```

---

## 40–47 Minutes — Linux Practical

Run:

```bash
free -h
```

Then:

```bash
top
```

Then:

```bash
ps aux
```

Then:

```bash
cat /proc/meminfo
```

Finally:

```bash
cat /proc/self/maps
```

---

## 47–50 Minutes — Quick Questions

Ask students:

1. Why do we need virtual memory?
2. What is a virtual address?
3. What is a physical address?
4. What is an MMU?
5. What is a page?
6. What is a frame?
7. What is a page table?
8. Why don't pages need to be contiguous?
9. What is segmentation?
10. Difference between paging and segmentation?
11. What is a page fault?
12. What is swap?
13. Is virtual memory faster than RAM?

---

# 37. Exam-Oriented Definitions

### Virtual Memory

> Virtual memory is a memory-management technique that provides processes with a large virtual address space independent of the available physical RAM.

### Address Space

> Address space is the range of memory addresses that a process can use.

### Virtual Address

> A virtual address is an address generated or used by a process and translated into a physical address by the memory-management hardware.

### Physical Address

> A physical address refers to an actual location in physical memory (RAM).

### Paging

> Paging is a memory-management technique that divides virtual memory into fixed-size pages and physical memory into fixed-size frames.

### Page

> A page is a fixed-size block of virtual memory.

### Frame

> A frame is a fixed-size block of physical memory.

### Page Table

> A page table stores mappings between virtual pages and physical frames.

### Segmentation

> Segmentation is a memory-management technique that divides a program into logical, variable-size segments such as code, data, stack, and heap.

### Page Fault

> A page fault occurs when a process accesses a page that is not currently present in physical memory.

---

# 38. Final Memory Trick

Remember this complete chain:

```text
PROCESS
   ↓
VIRTUAL ADDRESS SPACE
   ↓
VIRTUAL ADDRESS
   ↓
PAGE NUMBER + OFFSET
   ↓
PAGE TABLE
   ↓
FRAME NUMBER
   ↓
PHYSICAL ADDRESS
   ↓
RAM
```

If the required page is not in RAM:

```text
PAGE NOT IN RAM
      ↓
PAGE FAULT
      ↓
LOAD PAGE
      ↓
RAM
      ↓
CONTINUE EXECUTION
```

And remember:

```text
PAGE  → Virtual Memory
FRAME → Physical Memory

PAGING       → Fixed-size pieces
SEGMENTATION → Logical sections
```

---

# 39. Key Takeaways

By the end of this topic, students should understand:

- Why virtual memory is needed.
- Difference between virtual and physical memory.
- What a process address space is.
- What virtual and physical addresses mean.
- The role of the MMU.
- What pages and frames are.
- How a page table maps pages to frames.
- Why paging allows non-contiguous physical allocation.
- What segmentation means.
- Difference between paging and segmentation.
- What a page fault is.
- Basic idea of demand paging.
- Basic role of swap.
- How to observe memory and processes in Linux.

> **Core idea:** A program works with a virtual address space. The OS and hardware translate those virtual addresses into physical memory locations, using mechanisms such as paging, while virtual memory allows the system to manage memory efficiently even when physical RAM is limited.
