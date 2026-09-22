# Priority Scheduling — Non-Preemptive

## 1. What is Priority Scheduling?

**Priority Scheduling** is a CPU scheduling algorithm in which the CPU is assigned to the process having the **highest priority** among the processes currently available in the Ready Queue.

For this note, we use:

> **Smaller priority number = Higher priority**

Example:

| Priority | Meaning |
|---:|---|
| 1 | Highest |
| 2 | High |
| 3 | Medium |
| 4 | Low |

In **non-preemptive priority scheduling**, once a process gets the CPU, it continues running until its Burst Time is completed.

---

## 2. Non-Preemptive Priority Scheduling

The scheduler checks the Ready Queue whenever the CPU becomes free.

It selects:

> **The arrived process with the highest priority.**

Once selected, the process **cannot be interrupted** by another process that arrives later.

### Example

Suppose:

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 0 | 7 | 3 |
| P2 | 1 | 4 | 1 |
| P3 | 2 | 3 | 2 |

At time `0`, only P1 has arrived.

So P1 starts:

```text
0 ---------------- 7
        P1
```

Even though P2 and P3 arrive while P1 is executing, P1 continues until time 7.

At time 7:

- P2 → Priority 1
- P3 → Priority 2

Therefore P2 executes first.

Then P3.

### Gantt Chart

```text
0       7       11      14
|-------|-------|-------|
   P1      P2      P3
```

---

# 3. Why is it Non-Preemptive?

Consider:

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 0 | 8 | 3 |
| P2 | 2 | 3 | 1 |

At time 0:

```text
P1 starts
```

At time 2:

```text
P2 arrives
Priority of P2 = 1
```

But P1 is already executing.

Because this is **non-preemptive**, P2 cannot interrupt P1.

```text
0                         8
|-------------------------|
            P1
        ↑
   P2 arrives
   but waits
```

P2 gets the CPU only after P1 finishes.

---

# 4. General Algorithm

Follow these steps to solve a numerical problem.

### Step 1 — Start from time 0

Check which processes have arrived.

### Step 2 — Check the Ready Queue

Consider only processes whose:

```text
AT <= Current Time
```

### Step 3 — Select Highest Priority

Choose the process with the highest priority.

For this note:

```text
Smaller priority number = Higher priority
```

### Step 4 — Execute Completely

Because the algorithm is non-preemptive, run the selected process until it finishes.

### Step 5 — Update Current Time

The new current time becomes the process's Completion Time.

### Step 6 — Repeat

Again check all processes that have arrived and select the highest-priority process.

---

# 5. Important Rule

> **Priority is checked when the CPU becomes free, not continuously during execution.**

This is the most important concept in non-preemptive priority scheduling.

---

# 6. Gantt Chart

A Gantt chart represents the order and execution time of processes.

Example:

```text
0       7       11      14
|-------|-------|-------|
   P1      P2      P3
```

The numbers represent the start/end times of each process.

---

# 7. Scheduling Calculations

After creating the Gantt chart, calculate:

```text
CT → TAT → WT → Average TAT → Average WT
```

---

## Completion Time (CT)

**Completion Time** is the time at which a process finishes execution.

Example:

```text
P1 finishes at time 7

CT(P1) = 7
```

---

## Turnaround Time (TAT)

Turnaround Time is the total time taken by a process from arrival until completion.

\[
TAT = CT - AT
\]

Example:

```text
AT = 2
CT = 10

TAT = 10 - 2
    = 8
```

---

## Waiting Time (WT)

Waiting Time is the total time a process spends waiting in the Ready Queue.

\[
WT = TAT - BT
\]

or directly:

\[
WT = CT - AT - BT
\]

Example:

```text
TAT = 8
BT = 3

WT = 8 - 3
   = 5
```

---

## Average Waiting Time

\[
Average\ WT =
\frac{\sum WT}{Number\ of\ Processes}
\]

---

## Average Turnaround Time

\[
Average\ TAT =
\frac{\sum TAT}{Number\ of\ Processes}
\]

---

# 8. Solved Example

Consider:

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 0 | 7 | 3 |
| P2 | 1 | 4 | 1 |
| P3 | 2 | 3 | 2 |
| P4 | 3 | 2 | 4 |

Smaller number = higher priority.

### Scheduling

At time 0:

```text
Only P1 has arrived
→ P1 executes
```

P1 finishes at time 7.

At time 7:

```text
P2 → Priority 1
P3 → Priority 2
P4 → Priority 4

→ P2 executes
```

Then P3 and P4.

### Gantt Chart

```text
0       7       11      14      16
|-------|-------|-------|-------|
   P1      P2      P3      P4
```

### Final Table

| Process | AT | BT | Priority | CT | TAT | WT |
|---|---:|---:|---:|---:|---:|---:|
| P1 | 0 | 7 | 3 | 7 | 7 | 0 |
| P2 | 1 | 4 | 1 | 11 | 10 | 6 |
| P3 | 2 | 3 | 2 | 14 | 12 | 9 |
| P4 | 3 | 2 | 4 | 16 | 13 | 11 |

### Average WT

\[
Average\ WT = \frac{0+6+9+11}{4}
\]

\[
\boxed{Average\ WT = 6.5}
\]

### Average TAT

\[
Average\ TAT = \frac{7+10+12+13}{4}
\]

\[
\boxed{Average\ TAT = 10.5}
\]

---

# 9. CPU Idle Time

Sometimes no process is available.

Example:

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 2 | 5 | 2 |
| P2 | 4 | 3 | 1 |
| P3 | 5 | 2 | 3 |

At time 0:

```text
No process has arrived.
```

Therefore CPU is idle.

At time 2, P1 arrives and starts.

### Gantt Chart

```text
0       2       7       10      12
| IDLE  |-------|-------|-------|
          P1      P2      P3
```

Always show the **IDLE** period in the Gantt chart.

---

# 10. Same Priority

Two or more processes can have the same priority.

Example:

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 0 | 5 | 2 |
| P2 | 1 | 3 | 1 |
| P3 | 2 | 4 | 1 |

P2 and P3 have the same priority.

In such a case, use **FCFS** as the tie-breaking rule.

Since P2 arrived before P3:

```text
P2 → P3
```

So the rule becomes:

> **Highest priority first; if priority is equal, use FCFS.**

---

# 11. Priority Scheduling Example with Same Priority

Suppose at time 5:

```text
P2 → Priority 1 → AT = 2
P3 → Priority 1 → AT = 4
P4 → Priority 2 → AT = 3
```

Selection:

```text
P2 → P3 → P4
```

because P2 and P3 have the highest priority, and P2 arrived first.

---

# 12. Priority Scheduling vs FCFS

| Feature | FCFS | Priority Scheduling |
|---|---|---|
| Selection | Arrival order | Priority |
| Preemption | No | No in this topic |
| Main factor | Arrival Time | Priority |
| Tie breaking | Arrival order | Usually FCFS |
| Starvation | Generally no | Possible |
| Convoy Effect | Possible | Possible |

---

# 13. Priority Scheduling vs SJF

| Feature | SJF | Priority |
|---|---|---|
| Selection | Smallest Burst Time | Highest Priority |
| Preemptive version | SRTF | Preemptive Priority |
| Uses BT | Yes | No |
| Uses Priority | No | Yes |
| Non-preemptive version | Yes | Yes |

A useful way to remember:

```text
FCFS     → Who came first?
SJF      → Who has the shortest job?
Priority → Who has the highest priority?
```

---

# 14. Starvation

**Starvation** means a process keeps waiting for a very long time because other processes are repeatedly selected before it.

Priority scheduling can cause starvation.

Example:

```text
Low-priority process
        ↓
       waits
        ↓
High-priority processes keep arriving
        ↓
       waits
        ↓
       waits
```

### Solution: Aging

**Aging** gradually increases the priority of a waiting process.

For example:

```text
Waiting longer
      ↓
Priority improves
      ↓
Eventually gets CPU
```

Aging is used to reduce starvation.

---

# 15. Advantages

- Simple concept.
- Important processes can be executed first.
- Useful when processes have different levels of importance.
- Easy to implement.

# 16. Disadvantages

- Low-priority processes may suffer starvation.
- Priority assignment can be difficult.
- Poor priority selection can lead to long waiting times.
- Convoy-like effects can occur in non-preemptive scheduling.

---

# 17. Exam / Numerical Solving Flow

Use this flow for every numerical:

```text
Process Table
      ↓
Sort/consider by Arrival Time
      ↓
Check processes that have arrived
      ↓
Select highest priority
      ↓
Create Gantt Chart
      ↓
Find CT
      ↓
Find TAT
      ↓
Find WT
      ↓
Calculate Average WT
      ↓
Calculate Average TAT
```

Remember:

```text
TAT = CT - AT

WT = TAT - BT

Average WT = ΣWT / n

Average TAT = ΣTAT / n
```

---

# Assignment — Priority Scheduling

### Instructions

For every problem calculate:

1. Gantt Chart
2. Completion Time (CT)
3. Turnaround Time (TAT)
4. Waiting Time (WT)
5. Average TAT
6. Average WT

**Assumption for all questions:**

> Smaller priority number = Higher priority.

**Scheduling type:** Non-Preemptive Priority Scheduling.

---

## Problem 1 — Basic

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 0 | 5 | 3 |
| P2 | 1 | 3 | 1 |
| P3 | 2 | 4 | 2 |
| P4 | 3 | 2 | 4 |

---

## Problem 2 — Different Arrival Times

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 0 | 8 | 2 |
| P2 | 2 | 3 | 1 |
| P3 | 4 | 5 | 4 |
| P4 | 5 | 2 | 3 |
| P5 | 6 | 4 | 5 |

---

## Problem 3 — CPU Idle Time

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 3 | 4 | 2 |
| P2 | 5 | 3 | 1 |
| P3 | 6 | 2 | 3 |
| P4 | 8 | 5 | 4 |

**Hint:** Show the CPU's idle period in the Gantt chart.

---

## Problem 4 — Same Priority

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 0 | 6 | 2 |
| P2 | 1 | 4 | 1 |
| P3 | 2 | 3 | 1 |
| P4 | 4 | 5 | 3 |
| P5 | 5 | 2 | 2 |

**Rule:** If two processes have the same priority, use FCFS.

---

## Problem 5 — Little Difficult ⭐

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 0 | 7 | 3 |
| P2 | 1 | 4 | 1 |
| P3 | 2 | 6 | 4 |
| P4 | 5 | 2 | 2 |
| P5 | 6 | 3 | 1 |
| P6 | 8 | 4 | 5 |

Pay special attention to which processes have **already arrived** when the CPU becomes free.

---

# Assignment Answer Key

> **Only final answers are given below for cross-checking. No full solution.**

## Problem 1

**Gantt Chart:**

```text
0 ── P1 ── 5 ── P2 ── 8 ── P3 ── 12 ── P4 ── 14
```

| Process | CT | TAT | WT |
|---|---:|---:|---:|
| P1 | 5 | 5 | 0 |
| P2 | 8 | 7 | 4 |
| P3 | 12 | 10 | 6 |
| P4 | 14 | 11 | 9 |

**Average WT = 4.75**

**Average TAT = 8.25**

---

## Problem 2

**Gantt Chart:**

```text
0 ── P1 ── 8 ── P2 ── 11 ── P4 ── 13 ── P3 ── 18 ── P5 ── 22
```

| Process | CT | TAT | WT |
|---|---:|---:|---:|
| P1 | 8 | 8 | 0 |
| P2 | 11 | 9 | 6 |
| P3 | 18 | 14 | 9 |
| P4 | 13 | 8 | 6 |
| P5 | 22 | 16 | 12 |

**Average WT = 6.6**

**Average TAT = 11.0**

---

## Problem 3

**Gantt Chart:**

```text
0 ── IDLE ── 3 ── P1 ── 7 ── P2 ── 10 ── P3 ── 12 ── P4 ── 17
```

| Process | CT | TAT | WT |
|---|---:|---:|---:|
| P1 | 7 | 4 | 0 |
| P2 | 10 | 5 | 2 |
| P3 | 12 | 6 | 4 |
| P4 | 17 | 9 | 4 |

**Average WT = 2.5**

**Average TAT = 6.0**

---

## Problem 4

**Gantt Chart:**

```text
0 ── P1 ── 6 ── P2 ── 10 ── P3 ── 13 ── P5 ── 15 ── P4 ── 20
```

| Process | CT | TAT | WT |
|---|---:|---:|---:|
| P1 | 6 | 6 | 0 |
| P2 | 10 | 9 | 5 |
| P3 | 13 | 11 | 8 |
| P4 | 20 | 16 | 11 |
| P5 | 15 | 10 | 8 |

**Average WT = 6.4**

**Average TAT = 10.4**

---

## Problem 5

**Gantt Chart:**

```text
0 ── P1 ── 7 ── P2 ── 11 ── P5 ── 14 ── P4 ── 16 ── P3 ── 22 ── P6 ── 26
```

| Process | CT | TAT | WT |
|---|---:|---:|---:|
| P1 | 7 | 7 | 0 |
| P2 | 11 | 10 | 6 |
| P3 | 22 | 20 | 14 |
| P4 | 16 | 11 | 9 |
| P5 | 14 | 8 | 5 |
| P6 | 26 | 18 | 14 |

**Average WT = 8.0**

**Average TAT = 12.33**

---

# Quick Revision

```text
Priority Scheduling
        ↓
Select highest-priority arrived process
        ↓
Non-Preemptive
        ↓
Process runs until completion
        ↓
Create Gantt Chart
        ↓
CT
        ↓
TAT = CT - AT
        ↓
WT = TAT - BT
        ↓
Average WT / Average TAT
```

### Remember

> **Non-Preemptive Priority = Highest-priority process among the currently arrived processes runs completely.**

> **If priorities are equal → use FCFS.**

> **If no process has arrived → CPU is IDLE.**
