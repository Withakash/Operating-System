# Lecture 22: Paging Fundamentals

**Subject:** Operating Systems\
**Total lecture time:** 50 minutes\
**Purpose:** Simple, exam-oriented notes with diagrams, examples, and
practice questions.

------------------------------------------------------------------------

## 1. Why Paging? (10 minutes)

### Definition

**Paging** is a memory-management technique that divides a process's
logical memory into fixed-size blocks called **pages** and physical
memory (RAM) into blocks of the same size called **frames**. Any page
can be stored in any available frame.

Paging allows a process to occupy non-contiguous physical memory, so the
whole process does not need to fit into one continuous block.

### Why is paging needed?

-   Reduces **external fragmentation** in physical-memory allocation.
-   Allows a process to be stored across different physical locations.
-   Makes it easier to allocate free frames to processes.
-   Supports virtual memory when combined with mechanisms such as demand
    paging.

### Basic diagram

``` text
Process logical memory                 Physical memory (RAM)
+---------+                            +-----------+
| Page 0  | ------------------------->  | Frame 3   |
+---------+                            +-----------+
| Page 1  | --------------------+       | Frame 1   |
+---------+                    |       +-----------+
| Page 2  | ---------------+   +-----> | Frame 4   |
+---------+                |           +-----------+
                           +----------> | Frame 0   |
                                       +-----------+
```

*The page-to-frame mapping can be non-contiguous. The diagram is
illustrative, not a required placement order.*

### Advantages

-   Eliminates external fragmentation in the allocation of fixed-size
    frames.
-   Makes memory allocation more flexible.
-   Supports virtual-memory techniques such as demand paging.

### Disadvantages

-   Can cause **internal fragmentation**, often in the final page of a
    process.
-   Requires page tables, which consume memory.
-   Address translation adds overhead, although hardware caches such as
    the TLB help reduce it.

------------------------------------------------------------------------

## 2. Pages and Frames (10 minutes)

### Page

A **page** is a fixed-size block of a process's logical (virtual)
address space.

### Frame

A **frame** is a fixed-size block of physical memory (RAM).

**Important:** Page size and frame size are equal in a paging system.

### Diagram

``` text
Logical memory of a process              Physical memory
+-----------+                            +-----------+
| Page 0    |                            | Frame 0   |
+-----------+                            +-----------+
| Page 1    |                            | Frame 1   |
+-----------+                            +-----------+
| Page 2    |                            | Frame 2   |
+-----------+                            +-----------+
| Page 3    |                            | Frame 3   |
+-----------+                            +-----------+

Pages can be placed in any available frames; they do not need to be adjacent.
```

### Example

Suppose: - Page size = 1 KB - Process size = 4 KB

Then the process requires **4 pages**. If free frames are available,
those four pages may be stored in any four frames; the frames need not
be next to each other.

### Logical address format

A logical address is divided into two parts:

``` text
+----------------------+------------------+
| Page number (p)      | Offset (d)       |
+----------------------+------------------+
```

-   **Page number (p):** Identifies the page.
-   **Offset (d):** Identifies a byte or location within that page.

The page number is used to look up the corresponding frame in the page
table. The offset stays the same during translation.

### Example calculation

Assume a page size of 1,024 bytes and a logical address of 2,500 bytes
(counting from address 0).

-   Page number = `2500 // 1024 = 2`
-   Offset = `2500 % 1024 = 452`

So the address refers to **page 2, offset 452**.

------------------------------------------------------------------------

## 3. Page Table (10 minutes)

### Definition

A **page table** is a data structure maintained by the operating system
for a process. It maps each virtual page number to a physical frame
number.

The hardware Memory Management Unit (**MMU**) uses the page table during
address translation.

### Address translation diagram

``` text
CPU generates logical address
       +-------------------+
       | Page no. | Offset |
       +-------------------+
                |
                v
       +------------------+
       |    Page Table    |
       | Page -> Frame    |
       +------------------+
                |
          Frame number
                |
                v
       +-------------------+
       | Frame no.| Offset |
       +-------------------+
                |
                v
          Physical RAM
```

### Example page table

    Page number   Frame number
  ------------- --------------
              0              3
              1              1
              2              4
              3              0

If the CPU generates a logical address with **page number 2 and offset
100**, the page table shows that page 2 is stored in frame 4. The
physical address is therefore **frame 4, offset 100**.

### Key points

-   Each process generally has its own page table or address-space
    mapping.
-   The page table translates pages into frames.
-   The offset is unchanged by translation.
-   A page-table entry may include extra information such as a
    present/valid bit, access permissions, and a modified (dirty) bit.
-   A **Translation Lookaside Buffer (TLB)** is a small, fast cache that
    stores recent page-table translations to speed up address
    translation.

### Physical address calculation

For a page size of (S) bytes:

**Physical address = (Frame number × S) + Offset**

For example, if the frame number is 4, page size is 1,024 bytes, and
offset is 100:

`Physical address = (4 × 1024) + 100 = 4196`

------------------------------------------------------------------------

## 4. Page Fault (10 minutes)

### Definition

A **page fault** occurs when a process accesses a virtual-memory page
that is not currently present in physical RAM, or when an access
violates the permitted memory access rules. In demand paging, the
operating system may load a missing page from storage.

A page fault is an event/trap handled by the operating system; it does
**not** always mean that the program has an error.

### Page fault handling steps

1.  The CPU accesses a virtual address.
2.  The MMU checks the translation and page-table status.
3.  If the page is not present in RAM, a page-fault trap occurs.
4.  The operating system checks whether the access is valid.
5.  If valid, the OS finds a free frame or selects a page to replace.
6.  If necessary, it writes a modified page to storage and loads the
    required page into RAM.
7.  The OS updates the page table and resumes the instruction.
8.  If the access is invalid or prohibited, the OS reports an error or
    terminates the process according to the system's rules.

### Flow diagram

``` text
CPU accesses a virtual address
              |
              v
      Is page in RAM?
         /         \
       Yes          No
        |            |
        v            v
 Continue       Page-fault trap
 execution           |
                      v
             Is access valid?
                /         \
              No           Yes
              |             |
              v             v
        Report error    Find free frame
        / stop process  or choose victim
                            |
                            v
                    Load required page
                            |
                            v
                    Update page table
                            |
                            v
                    Restart instruction
```

### Causes of page faults

-   A page needed by a process has not yet been loaded into RAM.
-   A page was moved out of RAM and must be loaded again.
-   A program accesses an invalid virtual address or violates access
    permissions; this may lead to a protection fault rather than a
    recoverable missing-page fault.

### Page fault vs page replacement

-   **Page fault:** The event that occurs when a needed page is not
    present or an access is invalid.
-   **Page replacement:** A policy/action used when a page must be
    removed from RAM to make room for another page and no free frame is
    available.

### Performance note

A page fault that requires disk or SSD access is much slower than a
normal memory access. Too many page faults can severely reduce
performance.

------------------------------------------------------------------------

## 5. Extra Revision: Common Exam Questions

### Short-answer questions

**1. What is paging?**\
Paging divides logical memory into fixed-size pages and physical memory
into equal-size frames. Pages can be placed in any available frames.

**2. Differentiate between a page and a frame.**\
A page is a fixed-size block of virtual memory; a frame is a same-size
block of physical RAM.

**3. What is a page table?**\
A page table maps virtual page numbers to physical frame numbers.

**4. What is the role of the MMU?**\
The Memory Management Unit translates virtual addresses into physical
addresses and helps enforce memory-access permissions.

**5. What is a page fault?**\
It is a trap caused when a referenced page is not present in RAM or an
access violates memory protection. A missing page may be loaded if the
access is valid.

**6. What is a TLB?**\
A Translation Lookaside Buffer is a fast cache of recent address
translations.

**7. Does paging eliminate all fragmentation?**\
No. Paging eliminates external fragmentation in frame allocation, but it
can cause internal fragmentation.

### Numerical practice

A system has a page size of 1,024 bytes. A process generates logical
address 3,000.

-   Page number = `3000 // 1024 = 2`
-   Offset = `3000 % 1024 = 952`

If page 2 maps to frame 5:

-   Physical address = `(5 × 1024) + 952`
-   Physical address = `6072`

**Answer: 6072** (assuming addresses start at zero and the page table
mapping is given).

### Multiple-choice questions

**1. Paging divides physical memory into:**\
A. Pages\
B. Frames\
C. Segments\
D. Files\
**Answer: B. Frames**

**2. Which structure maps pages to frames?**\
A. Stack\
B. Queue\
C. Page table\
D. File table\
**Answer: C. Page table**

**3. The offset in a logical address identifies:**\
A. The process ID\
B. The page-table size\
C. A location within a page\
D. A physical frame number\
**Answer: C. A location within a page**

**4. A page fault can occur when:**\
A. The requested page is not present in RAM\
B. The CPU is switched off\
C. A file is renamed\
D. The page size changes automatically\
**Answer: A. The requested page is not present in RAM**

**5. Paging can eliminate which type of fragmentation in physical frame
allocation?**\
A. Internal\
B. External\
C. Both types in every case\
D. Neither type\
**Answer: B. External**

------------------------------------------------------------------------

## 6. Last-Minute Revision

-   **Paging:** Fixed-size pages in logical memory and equal-size frames
    in RAM.
-   **Page table:** Maps each page to a frame.
-   **Logical address:** Page number + offset.
-   **Physical address:** Frame number + same offset.
-   **MMU:** Performs address translation.
-   **TLB:** Caches recent translations.
-   **Page fault:** Occurs when a required page is absent or an access
    is invalid.
-   **Page replacement:** Frees a frame when necessary.
-   Paging prevents external fragmentation in frame allocation but may
    cause internal fragmentation.

------------------------------------------------------------------------

## Suggested 50-minute lecture plan

  Topic                                 Duration
  --------------------------------- ------------
  Why Paging?                             10 min
  Pages & Frames                          10 min
  Page Table                              10 min
  Page Fault                              10 min
  Numerical practice and revision         10 min
  **Total**                           **50 min**
