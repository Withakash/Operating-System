# Lec 28: Disk Scheduling Algorithms
## Operating Systems — Student Notes

## 1. Need for Disk Scheduling

**Disk scheduling** is the method used by the operating system to decide the order in which disk I/O requests are served.

A hard disk has a moving read/write head. Moving it between tracks takes time.

### Why is disk scheduling needed?
- Reduce **seek time** (time taken to move the head).
- Improve disk performance.
- Reduce average waiting time.
- Serve disk requests efficiently.

### Important Terms
- **Seek Time:** Time taken to move the disk head to the required track.
- **Head Movement:** Total distance travelled by the disk head.
- **Seek Sequence:** Order in which requests are served.

For numerical questions, we commonly calculate:

**Total Head Movement** = Sum of the absolute differences between consecutive head positions.

---

## 2. Example Used for All Algorithms

Assume:
- Request queue: `98, 183, 37, 122, 14, 124, 65, 67`
- Initial head position: `53`
- Disk cylinders: `0–199`
- Initial direction for SCAN-family algorithms: **towards higher-numbered cylinders**

The results below use these assumptions. For SCAN and C-SCAN, the head travels to the disk boundary before reversing or wrapping around.

## 3. FCFS — First Come, First Served

**FCFS** serves requests in the order they arrive.

### Seek sequence

```text
53 → 98 → 183 → 37 → 122 → 14 → 124 → 65 → 67
```

### Calculation

```text
|98 - 53|  = 45
|183 - 98| = 85
|37 - 183| = 146
|122 - 37| = 85
|14 - 122| = 108
|124 - 14| = 110
|65 - 124| = 59
|67 - 65|  = 2
```

**Total Head Movement = 640 cylinders**

**Advantage:** Simple and fair; requests are served in arrival order.

**Disadvantage:** May cause large head movement and poor performance.

---

## 4. SSTF — Shortest Seek Time First

**SSTF** serves the request closest to the current head position.

At each step, choose the pending request with the smallest distance from the head.

### Seek sequence

```text
53 → 65 → 67 → 37 → 14 → 98 → 122 → 124 → 183
```

### Calculation

```text
53 → 65  = 12
65 → 67  = 2
67 → 37  = 30
37 → 14  = 23
14 → 98  = 84
98 → 122 = 24
122 → 124 = 2
124 → 183 = 59
```

**Total Head Movement = 236 cylinders**

**Advantage:** Usually reduces head movement compared with FCFS.

**Disadvantage:** Requests far away may wait for a long time (**starvation**).

---

## 5. SCAN — Elevator Algorithm

**SCAN** moves the head in one direction, serving requests along the way. At the disk boundary, it reverses direction.

It works like an elevator: move in one direction, then return.

### Seek sequence

```text
53 → 65 → 67 → 98 → 122 → 124 → 183 → 199
    → 37 → 14 → 0
```

**Total Head Movement:**

```text
(199 - 53) + (199 - 0)
= 146 + 199
= 345 cylinders
```

**Advantage:** Provides more balanced waiting times than SSTF in many workloads.

**Disadvantage:** It may travel to the disk boundary even when no request is there.

---

## 6. C-SCAN — Circular SCAN

**C-SCAN** serves requests in one direction only. After reaching one boundary, it jumps to the other boundary and continues in the same direction.

### Seek sequence

```text
53 → 65 → 67 → 98 → 122 → 124 → 183 → 199
    → 0 → 14 → 37
```

**Total Head Movement:**

```text
(199 - 53) + (199 - 0) + (37 - 0)
= 146 + 199 + 37
= 382 cylinders
```

The movement from `199` to `0` is counted in this total.

**Advantage:** Gives more uniform waiting times across cylinder positions.

**Disadvantage:** The jump back to the beginning adds movement without serving requests.

---

## 7. LOOK — Improved SCAN

**LOOK** is similar to SCAN, but the head reverses at the last request in that direction instead of travelling all the way to the disk boundary.

### Seek sequence

```text
53 → 65 → 67 → 98 → 122 → 124 → 183
    → 37 → 14
```

**Total Head Movement:**

```text
(183 - 53) + (183 - 14)
= 130 + 169
= 299 cylinders
```

**Advantage:** Avoids unnecessary travel to the disk boundary.

**Disadvantage:** Waiting time still depends on request positions and direction.

---

## 8. C-LOOK — Improved C-SCAN

**C-LOOK** is similar to C-SCAN, but it jumps from the highest pending request to the lowest pending request instead of going to the disk boundaries.

### Seek sequence

```text
53 → 65 → 67 → 98 → 122 → 124 → 183
    → 14 → 37
```

**Total Head Movement:**

```text
(183 - 53) + (183 - 14) + (37 - 14)
= 130 + 169 + 23
= 322 cylinders
```

**Advantage:** Avoids unnecessary boundary travel while maintaining one-direction service.

**Disadvantage:** The jump between requests adds head movement without serving intermediate requests.

---

## 9. Comparison Table

| Algorithm | Working | Main Limitation |
|---|---|---|
| FCFS | Arrival order | Large head movement |
| SSTF | Nearest request first | Starvation possible |
| SCAN | Reverses at disk boundaries | May travel to unused boundary |
| C-SCAN | Serves in one direction; returns to opposite boundary | Extra return movement |
| LOOK | Reverses at last request | Waiting time varies |
| C-LOOK | Jumps between last and first pending requests | Extra jump movement |

## 10. Quick Revision

- **FCFS:** First request first.
- **SSTF:** Closest request first.
- **SCAN:** Moves to the boundary and reverses.
- **C-SCAN:** Moves in one direction and returns to the opposite boundary.
- **LOOK:** Reverses at the last request, not the boundary.
- **C-LOOK:** Jumps from the highest pending request to the lowest pending request.

## 11. Practice Questions

1. Define disk scheduling and explain why it is needed.
2. What is seek time?
3. Explain FCFS with an example.
4. Explain SSTF. What is starvation?
5. Differentiate SCAN and C-SCAN.
6. Differentiate SCAN and LOOK.
7. Differentiate C-SCAN and C-LOOK.
8. Calculate total head movement for FCFS using a given request queue.

## 12. University Exam Questions — 5 Marks

1. Explain the need for disk scheduling and define seek time.
2. Explain FCFS and SSTF with suitable examples.
3. Explain SCAN and C-SCAN with diagrams.
4. Compare SCAN, LOOK, C-SCAN, and C-LOOK.
5. Given a request queue and initial head position, calculate total head movement using any two disk scheduling algorithms.

---
**End of Lec 28 Notes**
