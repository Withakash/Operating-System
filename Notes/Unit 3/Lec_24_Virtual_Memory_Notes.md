# Lecture 24: Virtual Memory
## Operating Systems — Student Notes

> **Lecture duration:** 50 minutes  
> **Topics:** Need for Virtual Memory, Virtual Address Space, and Demand Paging / Page Faults

## Learning Objectives

By the end of this lecture, students should be able to:

- Explain why virtual memory is needed.
- Define virtual address space and physical address space.
- Describe how virtual addresses are translated into physical addresses.
- Explain demand paging and page faults.
- Trace the basic steps followed when a page fault occurs.
- Identify the benefits and limitations of virtual memory.

---

## 1. What Is Virtual Memory?

**Virtual memory** is a memory-management technique that allows a process to operate as if it has a large, continuous address space, even when its entire contents are not present in physical RAM at the same time.

The operating system and hardware translate virtual addresses used by a program into physical addresses in RAM. Some parts of a process may remain on secondary storage until they are needed.

Virtual memory is commonly implemented using **paging** and **demand paging**.

### Simple Analogy

Imagine a student studying at a desk:

- **Desk space** represents RAM.
- **Bookshelf** represents secondary storage.
- The student keeps currently needed books on the desk and returns to the shelf when another book is needed.

The desk is limited, so the student does not keep every book open at once. Similarly, a system can keep actively needed pages in RAM and fetch other pages when necessary.

> The analogy is only an aid to understanding. Secondary storage is much slower than RAM, so frequent page fetching can noticeably reduce performance.

### Basic Memory View

```text
            PROCESS
      Uses virtual addresses
                |
                v
       +------------------+
       | MMU + Page Table |
       | Address mapping  |
       +------------------+
          |            |
   Page is in RAM   Page not in RAM
          |            |
          v            v
   Physical RAM    Page-fault handling
                       |
                       v
                 Storage / backing
                 store, if available
```

The **MMU (Memory Management Unit)** is hardware that helps translate addresses and enforce memory access permissions. The operating system manages page tables and handles page faults.

---

## 2. Need for Virtual Memory

Virtual memory addresses several limitations of using physical RAM alone.

### 2.1 A program can have a large address space

A program may need more memory than the amount of RAM currently available for it. Virtual memory allows the operating system to keep only some pages in RAM while other pages remain outside RAM.

**Important:** Virtual memory does not create extra physical RAM. It provides an abstraction and can use secondary storage as backing for pages, but storage is much slower than RAM.

### 2.2 Better RAM utilization

A program may contain code and data that are not needed at the same time. Keeping only currently required pages in RAM can leave more physical memory available for other processes.

### 2.3 Supports multiprogramming

Several processes can run while only parts of their address spaces are resident in RAM. This can improve overall memory utilization, although performance still depends on the working sets of processes and available RAM.

### 2.4 Process isolation and protection

Each process normally sees its own virtual address space. Page-table mappings and permissions help prevent one process from reading or modifying another process's memory without authorization.

### 2.5 Simplifies program development

A program can use a logical address space without needing to know the exact physical RAM locations where its pages are stored.

### 2.6 Controlled sharing

Operating systems can map selected physical pages into multiple processes, for example for shared libraries or shared-memory communication, while applying appropriate access permissions.

### Benefits at a Glance

- Programs can use virtual address spaces larger than their resident physical-memory portion.
- Physical RAM can be shared among processes more flexibly.
- Supports isolation and access protection.
- Can reduce the need to load an entire program before it starts.

### Limitations

- A page fault may require storage I/O and can be slow.
- Too many page faults can cause **thrashing**.
- Page tables and translation hardware require resources.
- Virtual memory cannot compensate indefinitely for insufficient RAM or excessive memory demand.

---

## 3. Virtual Address Space

A **virtual address space** is the range of virtual addresses that a process can use. The operating system and hardware map valid virtual pages to physical frames or arrange for them to be brought into memory when required.

A process may see its address space as a continuous range, even though the corresponding physical pages may be stored in non-contiguous RAM frames.

### Virtual Address vs Physical Address

| Virtual Address | Physical Address |
|---|---|
| Generated by the CPU while executing a process | Identifies a location in physical memory |
| Belongs to a process's virtual address space | Refers to actual RAM |
| Translated using address-mapping structures | Used to access physical RAM |
| The same virtual address can map differently in different processes | Refers to a physical memory location |

### Conceptual Diagram

```text
        Process A's Virtual Space
        +----------------------+
        | Virtual Page 0       | ----+
        +----------------------+     |
        | Virtual Page 1       | --+ |
        +----------------------+   | |
        | Virtual Page 2       | --|-|----+
        +----------------------+   |      |
                                   v      v
                              Physical RAM
                           +------------------+
                           | Frame 4: Page 1  |
                           +------------------+
                           | Frame 1: Page 0  |
                           +------------------+
                           | Frame 7: Page 2  |
                           +------------------+

       Virtual pages can map to non-adjacent physical frames.
       This diagram is illustrative; actual mappings vary.
```

### Virtual Address Space Is Not the Same as RAM

For example, a process may have a large virtual address space, but only a subset of its pages may be resident in RAM at a particular time.

Do not conclude that a 64-bit operating system automatically gives every process all theoretically possible virtual memory. The actual usable address space depends on the processor, operating system, process configuration, and system limits.

### Typical Conceptual Regions

A process's virtual address space may contain regions for:

- Program code
- Global and static data
- Heap (dynamic allocation)
- Memory-mapped files and shared libraries
- Stack

The exact arrangement varies by operating system, architecture, executable format, and security features.

```text
      Conceptual Process Virtual Address Space

      +---------------------------+
      | Stack                     |
      +---------------------------+
      | Memory-mapped regions     |
      +---------------------------+
      | Heap                      |
      +---------------------------+
      | Data                      |
      +---------------------------+
      | Program code              |
      +---------------------------+

      Not to scale; actual layouts vary.
```

---

## 4. How Virtual Addresses Are Translated

In a paging-based virtual-memory system, the virtual address is divided into two main fields:

```text
       Virtual Address
      +----------------+----------------+
      | Virtual Page   | Page Offset    |
      | Number (VPN)   |                |
      +----------------+----------------+
```

- **Virtual Page Number (VPN):** Identifies the virtual page.
- **Page offset:** Identifies the byte or location within that page.

The page table maps a virtual page to a physical frame and stores or references status and protection information.

The physical address is formed from the physical frame number and the unchanged offset:

```text
       Virtual Address
              |
              v
        +-----------+
        | Page Table|
        +-----------+
              |
        Frame number
              |
              v
       +----------------+----------------+
       | Physical Frame | Page Offset    |
       +----------------+----------------+
              |
              v
         Physical RAM
```

### Worked Example: Address Translation

Assume:

- Page size = 1,024 bytes
- Virtual address = 2,500
- The page table maps virtual page 2 to physical frame 7

**Step 1: Find the virtual page number.**

`VPN = floor(Virtual Address / Page Size)`

`VPN = floor(2500 / 1024) = 2`

**Step 2: Find the offset.**

`Offset = Virtual Address mod Page Size`

`Offset = 2500 mod 1024 = 452`

**Step 3: Look up virtual page 2.**

The page table says that virtual page 2 maps to physical frame 7.

**Step 4: Calculate the physical address.**

`Physical Address = (Frame Number × Page Size) + Offset`

`= (7 × 1024) + 452`

`= 7168 + 452`

`= 7620`

**Answer: Physical address = 7620**, assuming the mapping is valid and the page is resident.

If the page-table entry indicates that the page is not resident, the CPU cannot complete this translation directly and page-fault handling may be required.

---

## 5. Demand Paging

**Demand paging** is a virtual-memory technique in which a page is loaded into RAM only when it is needed.

Instead of loading every page of a process into RAM at startup, the operating system can load pages on demand.

### How Demand Paging Works

1. A process starts executing.
2. The CPU accesses a virtual address.
3. The MMU checks the page-table entry.
4. If the page is present and access is permitted, execution continues.
5. If the page is not present, a page fault occurs.
6. The operating system checks whether the access is valid.
7. If valid, the OS locates the required page on storage or obtains it from another appropriate source.
8. If RAM needs space, the OS may select a page to evict. A modified page may need to be written back.
9. The OS loads or otherwise makes the page available, updates the page table, and restarts the interrupted instruction.

### Demand Paging Diagram

```text
             CPU accesses page
                    |
                    v
            Check page-table entry
                    |
              +-----+-----+
              |           |
          Page present  Page absent
              |           |
              v           v
       Check permission  Page fault
              |           |
              v           v
       Access permitted?  OS validates access
          /       \          |
        Yes        No        v
         |          |   Find required page
         v          v        |
     Continue    Exception   v
     execution              Is a frame free?
                          /          \
                        Yes           No
                         |             |
                         |        Select victim page
                         |             |
                         |        Write back if needed
                         |             |
                         +------<------+
                                |
                                v
                       Load/make page resident
                                |
                                v
                       Update page-table entry
                                |
                                v
                       Restart interrupted access
```

This is a simplified flow. Actual operating systems may use demand-zero pages, memory-mapped files, copy-on-write, shared pages, or other mechanisms instead of reading every missing page from a swap area.

### Advantages of Demand Paging

- Avoids loading pages that are never used.
- Can reduce initial loading time.
- Uses RAM more flexibly.
- Allows more processes to coexist when their active memory requirements fit available resources.

### Disadvantages of Demand Paging

- A page fault has overhead.
- Storage access is much slower than RAM access.
- Frequent faults can cause severe performance degradation.
- The operating system must manage page replacement and backing storage where applicable.

---

## 6. Page Fault

A **page fault** is an exception raised when a process accesses a virtual page that cannot currently be accessed through the existing page-table mapping. A common case is that the page is valid but not resident in RAM.

A page fault does **not always mean a program has an error**. A valid page that has not yet been loaded can produce a normal, recoverable page fault.

However, an invalid address or a disallowed access may result in the operating system terminating the process or delivering an error, depending on the system.

### Two Important Cases

| Case | What happens |
|---|---|
| Valid page, currently not resident | OS may load or make the page resident and resume the process |
| Invalid address or prohibited access | OS rejects the access; the process may receive an exception or be terminated |

### What Happens During a Page Fault?

1. The CPU traps into the operating system.
2. The OS determines why the fault occurred.
3. It checks whether the virtual address and operation are valid.
4. If invalid, the OS reports or terminates the offending access.
5. If valid but not resident, the OS obtains the page and finds or frees a frame.
6. If necessary, the OS evicts another page and writes it back if modified.
7. The OS updates the relevant mapping and status information.
8. The process resumes at the interrupted instruction.

### Page Fault Example

Suppose a process accesses virtual page 5, but the page-table entry marks it as not present.

- The MMU triggers a page fault.
- The OS checks that the access is valid.
- The OS locates the page's contents or creates the required page.
- The page is made resident in a physical frame.
- The page table is updated.
- The interrupted instruction is retried.

The instruction can then continue if the mapping and permissions are valid.

---

## 7. Page Replacement: Why It May Be Needed

A page fault does not always require eviction. If a free physical frame is available, the OS can use it. If no suitable frame is free, it may need to choose a resident page to remove. This is called **page replacement**.

A page-replacement algorithm chooses a page to evict. Common algorithms include:

- **FIFO (First-In, First-Out):** Replaces the page that has been in memory the longest.
- **LRU (Least Recently Used):** Replaces the page that has not been used for the longest time, or an approximation of it.
- **Optimal:** Replaces the page whose next use is farthest in the future. This is mainly a theoretical benchmark because the future is not generally known in advance.

Detailed algorithms and numerical problems can be covered in a separate lecture.

### Dirty and Clean Pages

- **Clean page:** Has not been modified since it was loaded or mapped. It may be discarded and reloaded later from its backing source.
- **Dirty page:** Has been modified. Its contents may need to be written back before the frame is reused, unless the page can be discarded safely under the relevant memory-mapping rules.

Writing a dirty page back can add to page-fault handling time.

---

## 8. Thrashing

**Thrashing** occurs when a system spends too much time servicing page faults and moving pages instead of doing useful processing.

It can happen when the combined active memory needs of processes exceed the RAM available to hold their working sets.

### Causes

- Too many memory-intensive processes running together.
- Insufficient physical RAM for the active workloads.
- Poorly sized or changing working sets.
- Excessive page eviction and reloading.

### Effects

- High storage I/O.
- Low CPU utilization despite heavy system activity.
- Slow application response.
- Reduced overall throughput.

### Ways to Reduce Thrashing

- Reduce the number of simultaneously active memory-intensive processes.
- Add RAM when practical.
- Improve workload memory use.
- Use appropriate working-set or page-fault-frequency controls where supported.

> Virtual memory is not a guarantee of high performance. If the active working sets do not fit in RAM, the system may become extremely slow.

---

## 9. Virtual Memory vs Physical Memory

| Basis | Virtual Memory | Physical Memory |
|---|---|---|
| Meaning | Address-space abstraction presented to processes | Actual RAM installed in the system |
| Address type | Virtual address | Physical address |
| Capacity | Depends on architecture and OS limits; can exceed available RAM | Limited by installed and supported RAM |
| Mapping | Translated through page tables and hardware | Directly identifies RAM locations |
| Storage access | A missing page may need to be fetched or recreated | RAM access is much faster than secondary storage |
| Purpose | Isolation, flexible mapping, and management of address spaces | Holds currently resident instructions and data |

---

## 10. Common Misconceptions

**Misconception 1:** Virtual memory is extra RAM.

**Correction:** Virtual memory is an abstraction. Secondary storage may back some pages, but it is much slower than RAM.

**Misconception 2:** Every page fault is an error.

**Correction:** A page fault can be a normal part of demand paging. The OS may resolve it and resume execution.

**Misconception 3:** A virtual address is the same as a physical address.

**Correction:** The virtual address is translated into a physical address; the two values may differ.

**Misconception 4:** A process's entire virtual address space must be in RAM.

**Correction:** Only some pages need to be resident at a particular time.

**Misconception 5:** Virtual memory always makes a system faster.

**Correction:** It improves flexibility and memory utilization, but frequent page faults and thrashing can make performance much worse.

---

## 11. Practice Questions

### A. Short-Answer Questions

1. Define virtual memory.
2. Why is virtual memory needed?
3. What is a virtual address space?
4. Differentiate virtual and physical addresses.
5. What is demand paging?
6. Define page fault.
7. What is the role of the MMU?
8. What is page replacement?
9. Define thrashing and state two causes.
10. State two advantages and two disadvantages of virtual memory.

### B. Numerical Questions

**Question 1**

A system has a page size of 1,024 bytes. A virtual address is 3,000. Find the virtual page number and offset.

**Solution**

- Virtual page number = `floor(3000 / 1024) = 2`
- Offset = `3000 mod 1024 = 952`

**Answer:** VPN = 2, offset = 952.

**Question 2**

A system uses a page size of 1,024 bytes. Virtual page 2 maps to physical frame 5. Translate virtual address 2,500 into a physical address.

**Solution**

- VPN = `floor(2500 / 1024) = 2`
- Offset = `2500 mod 1024 = 452`
- Frame number = 5
- Physical address = `(5 × 1024) + 452 = 5572`

**Answer:** Physical address = 5572, assuming the page is resident and access is allowed.

**Question 3**

A process accesses a valid virtual page that is not currently in RAM. What happens?

**Answer:** A page fault occurs. The OS validates the access, obtains the page, makes it resident in a frame, updates the mapping, and resumes the interrupted instruction.

---

## 12. Important 10-Mark University Questions

1. **Define virtual memory and explain why it is needed.** Discuss its advantages and limitations with a suitable diagram.
2. **Explain virtual address space.** Differentiate virtual and physical addresses and describe the role of the MMU and page table.
3. **Explain demand paging with a flowchart.** Describe the steps performed when a page fault occurs.
4. **What is a page fault?** Explain valid page faults and invalid memory accesses, and describe the operating system's handling process.
5. **What is thrashing?** Explain its causes, effects, and methods of reduction.
6. **Explain virtual-to-physical address translation** with a numerical example.

---

## 13. One-Minute Revision

- **Virtual memory** provides a process with a virtual address space that is mapped to physical memory.
- The **MMU** and page tables support address translation and protection.
- A virtual address in paging contains a **virtual page number + offset**.
- **Demand paging** brings pages into RAM when they are needed.
- A **page fault** occurs when a virtual page cannot be accessed through the current mapping; it may be recoverable or invalid.
- **Page replacement** may be needed when no suitable free frame is available.
- **Thrashing** means excessive paging activity that leaves little time for useful processing.
- Virtual memory improves flexibility but does not replace the speed or capacity of physical RAM.

---

**End of Lecture 24: Virtual Memory**
