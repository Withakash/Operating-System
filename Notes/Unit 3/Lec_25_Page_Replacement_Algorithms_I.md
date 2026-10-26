# Lecture 25: Page Replacement Algorithms — I
## Operating Systems | Student Notes

**Lecture duration:** 50 minutes  
**Topics:** Need for Page Replacement, FIFO, Optimal, and LRU

## Learning Objectives

- Explain why page replacement is needed in virtual memory.
- Define page faults, reference strings, and victim pages.
- Apply FIFO, Optimal, and LRU algorithms to numerical problems.
- Count page faults and hits for a given frame count.
- Compare the algorithms and explain Belady's anomaly.

---

## 1. Need for Page Replacement

### Quick Revision: Pages and Frames

Virtual memory divides a process's address space into fixed-size **pages**. Physical memory (RAM) is divided into same-size **frames**. A page can be loaded into any suitable free frame.

When a process accesses a page that is not currently resident in RAM, a **page fault** occurs. If a free frame is available, the operating system can load the page into it. If no free frame is available, the OS may need to remove a resident page to make room.

Choosing which resident page to remove is called **page replacement**.

### Why Is It Needed?

- Physical RAM is limited.
- A process may access more pages than can fit in its available frames.
- When a page fault occurs and no suitable free frame is available, a resident page must be selected for removal.
- A good choice can reduce future page faults and unnecessary storage I/O.

### Basic Flow

```text
       Process accesses a page
                 |
                 v
          Is page in RAM?
            /         \
          Yes          No
           |            |
           v            v
     Continue       Page fault
                    |
                    v
             Is a free frame
                available?
               /          \
             Yes            No
              |              |
              v              v
        Load page       Select a victim
        into frame      page to replace
              |              |
              |       Write back if needed
              |              |
              +-------+------+
                      |
                      v
              Load requested page
                      |
                      v
               Update page table
                      |
                      v
               Resume execution
```

### Important Terms

| Term | Meaning |
|---|---|
| Page | Fixed-size block of virtual memory |
| Frame | Fixed-size block of physical memory |
| Page fault | Exception when a page cannot be accessed through its current mapping; often because it is not resident |
| Page replacement | Removing a resident page to make room for another page |
| Victim page | Page selected for removal |
| Reference string | Sequence of page numbers accessed by a process |
| Page fault count | Number of page faults during the reference sequence |
| Frame count | Number of physical frames available in the example |

A page fault and a page replacement are not the same thing: if a free frame exists, the fault can be handled without evicting a page.

---

## 2. How to Solve Page Replacement Numericals

1. Write the reference string.
2. Note the number of available frames.
3. Process page references from left to right.
4. If the page is already in a frame, record a **Hit (H)**.
5. If the page is absent, record a **Page Fault (F)**.
6. If a free frame exists, place the page there. Otherwise, choose a victim using the specified algorithm.
7. Count the page faults carefully.

**Example reference string used for comparison:**

`7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2`

Number of frames: **3**

The same reference string and frame count are used for FIFO, Optimal, and LRU so the results can be compared fairly.

---

## 3. FIFO (First-In, First-Out) Algorithm

### Definition

**FIFO** replaces the page that has been in memory the longest—the page that entered the frames earliest.

FIFO treats resident pages like a queue:

- The oldest loaded page is at the front.
- A newly loaded page is added at the back.
- When replacement is required, the oldest page is removed.

A hit does **not** change a page's arrival order.

### FIFO Worked Example

Reference string: `7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2`  
Frames available: **3**

| Step | Page | Frame 1 | Frame 2 | Frame 3 | Result |
|---:|---:|---:|---:|---:|---|
| 1 | 7 | 7 | – | – | F |
| 2 | 0 | 7 | 0 | – | F |
| 3 | 1 | 7 | 0 | 1 | F |
| 4 | 2 | 2 | 0 | 1 | F (7 out) |
| 5 | 0 | 2 | 0 | 1 | H |
| 6 | 3 | 2 | 3 | 1 | F (0 out) |
| 7 | 0 | 2 | 3 | 0 | F (1 out) |
| 8 | 4 | 4 | 3 | 0 | F (2 out) |
| 9 | 2 | 4 | 2 | 0 | F (3 out) |
| 10 | 3 | 4 | 2 | 3 | F (0 out) |
| 11 | 0 | 0 | 2 | 3 | F (4 out) |
| 12 | 3 | 0 | 2 | 3 | H |
| 13 | 2 | 0 | 2 | 3 | H |

**Total FIFO page faults = 10**  
**Total hits = 3**

### Advantages

- Simple to understand and implement.
- Can be implemented with a queue.
- Requires little usage-history information.

### Disadvantages

- Ignores how recently or frequently a page is used.
- May remove a frequently used page simply because it is old.
- Can exhibit **Belady's anomaly**: increasing the number of frames can sometimes increase page faults.

**Memory aid:** FIFO = remove the page that entered RAM earliest.

---

## 4. Optimal Page Replacement Algorithm

### Definition

The **Optimal** algorithm removes the resident page whose **next use is farthest in the future**. If a page will never be used again in the given reference string, it is an ideal candidate for replacement.

For a known reference string and a fixed frame count, Optimal gives the minimum possible number of page faults. Exact future references are not normally available to a real operating system, so Optimal is mainly a theoretical benchmark.

### How to Apply Optimal

When a page fault occurs and all frames are full:

1. Look ahead at the remaining reference string.
2. Find when each resident page will next be referenced.
3. Replace the page whose next use is farthest away.
4. If a page will never be used again, it can be selected.
5. If multiple choices are equally suitable, more than one replacement may be optimal.

### Optimal Worked Example

Reference string: `7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2`  
Frames available: **3**

| Step | Page | Frame 1 | Frame 2 | Frame 3 | Result |
|---:|---:|---:|---:|---:|---|
| 1 | 7 | 7 | – | – | F |
| 2 | 0 | 7 | 0 | – | F |
| 3 | 1 | 7 | 0 | 1 | F |
| 4 | 2 | 2 | 0 | 1 | F (7 out; not used again) |
| 5 | 0 | 2 | 0 | 1 | H |
| 6 | 3 | 2 | 0 | 3 | F (1 out; not used again) |
| 7 | 0 | 2 | 0 | 3 | H |
| 8 | 4 | 2 | 4 | 3 | F (0's next use is farther than 2 or 3's) |
| 9 | 2 | 2 | 4 | 3 | H |
| 10 | 3 | 2 | 4 | 3 | H |
| 11 | 0 | 2 | 0 | 3 | F (4 out; not used again) |
| 12 | 3 | 2 | 0 | 3 | H |
| 13 | 2 | 2 | 0 | 3 | H |

**Total Optimal page faults = 7**  
**Total hits = 6**

### Advantages

- Gives the minimum possible page-fault count for the known sequence and frame count.
- Provides a benchmark for evaluating practical algorithms.
- Illustrates the ideal replacement decision.

### Disadvantages

- Requires knowledge of future page references.
- Exact future knowledge is not normally available to a real OS.
- Not generally implementable as a practical online algorithm.

**Memory aid:** Optimal = replace the page needed farthest in the future.

---

## 5. LRU (Least Recently Used) Algorithm

### Definition

**LRU** replaces the page that has not been used for the longest time in the past.

LRU uses recent history as an estimate of near-future use. It often works well, but a recently used page is not guaranteed to be used again soon.

### How to Apply LRU

When a page fault occurs and all frames are full:

1. Check the last time each resident page was used.
2. Find the page whose most recent use is farthest in the past.
3. Replace that page with the requested page.
4. Update recency information after every reference, including hits.

**Important:** A hit updates recency in LRU. It does not change arrival order in FIFO.

### LRU Worked Example

Reference string: `7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2`  
Frames available: **3**

| Step | Page | Frame 1 | Frame 2 | Frame 3 | Result |
|---:|---:|---:|---:|---:|---|
| 1 | 7 | 7 | – | – | F |
| 2 | 0 | 7 | 0 | – | F |
| 3 | 1 | 7 | 0 | 1 | F |
| 4 | 2 | 2 | 0 | 1 | F (7 least recently used) |
| 5 | 0 | 2 | 0 | 1 | H |
| 6 | 3 | 2 | 0 | 3 | F (1 least recently used) |
| 7 | 0 | 2 | 0 | 3 | H |
| 8 | 4 | 4 | 0 | 3 | F (2 least recently used) |
| 9 | 2 | 4 | 0 | 2 | F (3 least recently used) |
| 10 | 3 | 3 | 0 | 2 | F (4 least recently used) |
| 11 | 0 | 3 | 0 | 2 | H |
| 12 | 3 | 3 | 0 | 2 | H |
| 13 | 2 | 3 | 0 | 2 | H |

**Total LRU page faults = 9**  
**Total hits = 4**

### Advantages

- Uses recent usage history, which can be a useful predictor of future use.
- Often performs better than FIFO on many workloads, though not all.
- Does not exhibit Belady's anomaly because it is a **stack algorithm**.

### Disadvantages

- Exact implementation can require tracking accesses, adding overhead.
- Real systems often use approximations rather than exact LRU.
- Past use does not guarantee future use.

**Memory aid:** LRU = remove the page whose last use was longest ago.

---

## 6. FIFO vs Optimal vs LRU

| Basis | FIFO | Optimal | LRU |
|---|---|---|---|
| Full form | First-In, First-Out | Optimal Page Replacement | Least Recently Used |
| Replaces | Page loaded earliest | Page used farthest in the future | Page used longest ago |
| Information used | Arrival order | Future references | Past references |
| Implementation | Simple queue | Requires future knowledge | Tracks or approximates recent use |
| Main purpose | Simple baseline | Theoretical benchmark | Practical policy / approximations |
| Belady's anomaly | Can occur | Does not occur | Does not occur |
| Fault count | Depends on sequence | Minimum possible for that sequence and frame count | Depends on sequence; not always better than every alternative |

### Results for the Worked Example

Reference string: `7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2`  
Number of frames: **3**

| Algorithm | Page Faults | Hits |
|---|---:|---:|
| FIFO | 10 | 3 |
| Optimal | 7 | 6 |
| LRU | 9 | 4 |

Optimal has the fewest faults for this particular string. The same ranking is not guaranteed for every possible reference string.

---

## 7. Belady's Anomaly

**Belady's anomaly** occurs when increasing the number of available page frames causes the number of page faults to increase.

FIFO can exhibit this anomaly.

### Classic Example

Reference string:

`1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5`

| Number of Frames | FIFO Page Faults |
|---:|---:|
| 3 | 9 |
| 4 | 10 |

Although the system has one extra frame, FIFO produces more page faults for this reference string.

### Which Algorithms Can Exhibit It?

- **FIFO:** Yes.
- **LRU:** No; it is a stack algorithm.
- **Optimal:** No; it is also a stack algorithm.

---

## 8. Common Mistakes in Numericals

1. **Counting every reference as a fault.** Check whether the page is already resident.
2. **Replacing a page when a free frame exists.** Fill free frames first.
3. **Mixing FIFO and LRU.** FIFO tracks load order; LRU tracks most recent use.
4. **Using future references for LRU.** LRU uses past usage; Optimal looks forward.
5. **Updating FIFO order on a hit.** A FIFO hit does not change arrival order.
6. **Failing to update LRU on a hit.** A hit makes that page most recently used.
7. **Assuming Optimal always has one unique victim.** Ties may allow multiple optimal choices.
8. **Confusing a page fault with a replacement.** A fault can be handled using a free frame.

---

## 9. Practice Questions

### A. Theory

1. Why is page replacement required?
2. Define page fault, reference string, and victim page.
3. Explain FIFO with a numerical example.
4. Explain Optimal and why it is difficult to implement exactly.
5. Explain LRU and how it differs from FIFO.
6. What is Belady's anomaly? Which of these algorithms can exhibit it?
7. Why is Optimal useful as a benchmark?
8. Compare FIFO, Optimal, and LRU.

### B. Numericals

Assume all frames are initially empty.

**Question 1 — FIFO**

Reference string: `1, 2, 3, 1, 4, 5, 2, 1`  
Frames: `3`

**Question 2 — Optimal**

Reference string: `1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5`  
Frames: `3`

**Question 3 — LRU**

Reference string: `2, 3, 2, 1, 5, 2, 4, 5, 3, 2, 5, 2`  
Frames: `3`

### Practice Answer Key

- Question 1 (FIFO): **6 page faults**
- Question 2 (Optimal): **6 page faults**
- Question 3 (LRU): **7 page faults**

For full exam working, draw a row for each reference and show frame contents after every step. Apply the algorithm's rule rather than memorizing a result.

---

## 10. Important 10-Mark University Questions

1. **What is page replacement?** Explain the need for page replacement and describe FIFO with a numerical example.
2. **Explain Optimal page replacement.** Solve a reference-string problem and explain why Optimal is mainly a theoretical benchmark.
3. **Explain LRU with a numerical example.** Discuss its advantages and limitations.
4. **Compare FIFO, Optimal, and LRU** in terms of replacement policy, implementation, page faults, and Belady's anomaly.
5. **What is Belady's anomaly?** Explain it with a FIFO example and state why LRU and Optimal do not exhibit it.

---

## 11. One-Minute Revision

- **Page replacement** is needed when a page fault occurs and no suitable free frame is available.
- **FIFO:** Remove the page loaded earliest.
- **Optimal:** Remove the page whose next use is farthest in the future.
- **LRU:** Remove the page whose last use was farthest in the past.
- A **hit** means the page is already resident; a **fault** means it is not accessible through the current mapping.
- **Optimal** gives the minimum possible page-fault count for a known reference string and fixed frame count.
- **Belady's anomaly** can occur in FIFO.
- LRU and Optimal do not exhibit Belady's anomaly.
- Show frame contents step by step in numerical answers.

---

**End of Lecture 25: Page Replacement Algorithms — I**
