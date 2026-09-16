# OS — Shortest Job First (SJF) — Non-Preemptive
## Student-Oriented Notes + Numerical Assignment

---

## 1. Learning Objectives

After studying this topic, you should be able to:

- Explain Shortest Job First (SJF).
- Understand non-preemptive scheduling.
- Select the correct process at every scheduling decision.
- Draw an SJF Gantt chart.
- Calculate CT, TAT, WT and RT.
- Calculate average waiting time and average turnaround time.
- Solve SJF numericals with different arrival times, idle time and ties.
- Explain starvation and other limitations of SJF.

---

# 2. Quick Revision: FCFS

In FCFS:

> **The process that arrives first gets the CPU first.**

Example:

```text
P1 → P2 → P3 → P4
```

If P1 needs 10 units while P2 and P3 need only 2 and 1 units, the short processes may wait for a long process.

SJF tries to reduce this problem.

---

# 3. What is SJF?

**SJF = Shortest Job First**

SJF selects the process having the **smallest CPU Burst Time (BT)** from the processes that are currently available.

### Simple definition

> SJF is a CPU scheduling algorithm in which the process with the shortest CPU burst time is selected first.

Think:

```text
FCFS → Who came first?

SJF  → Who needs the least CPU time?
```

---

# 4. Why do we use SJF?

Consider:

| Process | Burst Time |
|---|---:|
| P1 | 8 |
| P2 | 2 |
| P3 | 1 |
| P4 | 3 |

FCFS order might be:

```text
P1 → P2 → P3 → P4
```

SJF order:

```text
P3 → P2 → P4 → P1
```

Short processes finish earlier, which generally reduces the average waiting time.

---

# 5. Types of SJF

SJF is commonly discussed in two forms:

### 1. Non-Preemptive SJF

Once a process gets the CPU:

> **It runs until it finishes.**

A newly arrived process cannot interrupt it.

### 2. Preemptive SJF

The preemptive version is called:

> **SRTF — Shortest Remaining Time First**

A running process can be interrupted if another process arrives with a shorter remaining CPU time.

**These notes focus only on Non-Preemptive SJF.**

---

# 6. What does "Non-Preemptive" mean?

Suppose:

```text
P1 starts at time 0
BT = 8
```

At time 2, another process arrives:

```text
P2
BT = 1
```

In non-preemptive SJF, P2 cannot take the CPU immediately.

P1 continues until completion:

```text
0                    8
|-------- P1 --------|
         ↑
       P2 arrives
```

### Remember

> **Non-preemptive = Once started, don't stop until completion.**

---

# 7. Important Terms

| Term | Meaning |
|---|---|
| AT | Arrival Time |
| BT | Burst Time / CPU execution time |
| ST | Start Time |
| CT | Completion Time |
| TAT | Turnaround Time |
| WT | Waiting Time |
| RT | Response Time |

---

# 8. Important Formulas

### Completion Time

CT is the time at which a process finishes.

### Turnaround Time

```text
TAT = CT − AT
```

### Waiting Time

```text
WT = TAT − BT
```

Therefore:

```text
WT = CT − AT − BT
```

### Response Time

```text
RT = ST − AT
```

For **non-preemptive SJF**:

```text
RT = WT
```

because a process starts only once and then runs until completion.

### Average Waiting Time

```text
Average WT = Sum of all WT / Number of processes
```

### Average Turnaround Time

```text
Average TAT = Sum of all TAT / Number of processes
```

---

# 9. The Golden Rule of SJF

> **At every scheduling decision, choose the shortest BT among the processes that have already arrived.**

Do **not** simply sort the entire table by Burst Time.

---

# 10. SJF Solving Algorithm

Use these steps in every numerical:

```text
1. Start from the current time.
        ↓
2. Check which processes have arrived.
        ↓
3. Ignore processes that have not arrived yet.
        ↓
4. Compare BT of available processes.
        ↓
5. Select the process with the smallest BT.
        ↓
6. Run it until completion.
        ↓
7. Update the current time.
        ↓
8. Repeat until all processes finish.
        ↓
9. Calculate CT, TAT, WT and RT.
```

---

# 11. Numerical Example 1 — Basic SJF

### Given

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 6 |
| P2 | 0 | 2 |
| P3 | 0 | 4 |
| P4 | 0 | 3 |

All processes arrive at time 0.

At time 0:

```text
P1 = 6
P2 = 2
P3 = 4
P4 = 3
```

Shortest = P2.

Then P4, P3 and P1.

Execution order:

```text
P2 → P4 → P3 → P1
```

### Gantt Chart

```text
0    2      5       9          15
| P2 |  P4  |  P3   |    P1     |
```

### Calculation

| Process | AT | BT | CT | TAT = CT−AT | WT = TAT−BT |
|---|---:|---:|---:|---:|---:|
| P1 | 0 | 6 | 15 | 15 | 9 |
| P2 | 0 | 2 | 2 | 2 | 0 |
| P3 | 0 | 4 | 9 | 9 | 5 |
| P4 | 0 | 3 | 5 | 5 | 2 |

```text
Average WT = (9 + 0 + 5 + 2) / 4
           = 4
```

```text
Average TAT = (15 + 2 + 9 + 5) / 4
            = 7.75
```

---

# 12. Numerical Example 2 — Different Arrival Times

### Given

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 7 |
| P2 | 1 | 4 |
| P3 | 2 | 2 |
| P4 | 3 | 1 |

### Time = 0

Only P1 has arrived.

Therefore P1 must start.

Because SJF is non-preemptive:

```text
0 → 7 : P1
```

At time 7:

```text
P2 = 4
P3 = 2
P4 = 1
```

Shortest = P4.

Then P3, then P2.

Execution order:

```text
P1 → P4 → P3 → P2
```

### Gantt Chart

```text
0        7  8    10       14
|   P1   |P4| P3 |   P2    |
```

### Final Table

| Process | AT | BT | CT | TAT | WT |
|---|---:|---:|---:|---:|---:|
| P1 | 0 | 7 | 7 | 7 | 0 |
| P2 | 1 | 4 | 14 | 13 | 9 |
| P3 | 2 | 2 | 10 | 8 | 6 |
| P4 | 3 | 1 | 8 | 5 | 4 |

```text
Average WT = 4.75
Average TAT = 8.25
```

---

# 13. Very Important Mistake to Avoid

Suppose:

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 8 |
| P2 | 1 | 2 |
| P3 | 2 | 1 |

Do NOT immediately write:

```text
P3 → P2 → P1
```

At time 0, only P1 is available.

Therefore:

```text
0 → 8 : P1
```

Then P2 and P3 are considered.

This is **non-preemptive SJF**.

---

# 14. CPU Idle Time

Sometimes no process is available.

Example:

| Process | AT | BT |
|---|---:|---:|
| P1 | 3 | 4 |
| P2 | 6 | 2 |
| P3 | 8 | 3 |

At time 0, no process has arrived.

Therefore:

```text
CPU = IDLE
```

Gantt chart begins:

```text
0        3
|  IDLE  |
```

Then P1 starts at time 3.

### Important

> Never execute a process before its Arrival Time.

---

# 15. Tie in SJF

Suppose:

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 3 |
| P2 | 0 | 2 |
| P3 | 0 | 2 |
| P4 | 0 | 5 |

P2 and P3 both have:

```text
BT = 2
```

If no tie-breaking rule is given, normally use FCFS/order among the tied processes.

So:

```text
P2 → P3
```

Then P1 and P4.

### Rule

```text
Same BT
   ↓
Compare AT
   ↓
Earlier arrival gets preference
   ↓
If AT is also same, use given order
   unless another rule is specified
```

---

# 16. Advantages of SJF

### 1. Low average waiting time

SJF minimizes average waiting time under the standard assumptions when burst times are known accurately.

### 2. Short jobs finish quickly

Small CPU bursts are completed earlier.

### 3. Reduces the convoy effect

Short processes do not automatically have to wait behind a long process when they are available for selection.

---

# 17. Disadvantages of SJF

### 1. Burst time may be difficult to know

The OS may not know exactly how long a process will need the CPU before execution.

### 2. Starvation

A long process may keep waiting if short processes continuously arrive.

Example:

```text
Long Process
     ↓
waiting...

Short → Short → Short → Short → ...
```

### 3. More scheduling decisions

The scheduler has to compare the burst times of available processes.

---

# 18. Convoy Effect vs Starvation

### FCFS → Convoy Effect

A long process can make many short processes wait:

```text
LONG → SHORT → SHORT → SHORT
```

### SJF → Starvation

A long process may keep waiting because short jobs continue to arrive:

```text
SHORT → SHORT → SHORT → SHORT → ...
                      ↑
                    LONG
```

---

# 19. SJF vs FCFS

| Feature | FCFS | Non-Preemptive SJF |
|---|---|---|
| Selection | Arrival order | Shortest BT |
| Preemption | No | No |
| Main advantage | Simple | Low average WT |
| Main issue | Convoy effect | Starvation |
| Burst time needed | No | Yes |
| Can process be interrupted? | No | No |
| Short jobs prioritized? | No | Yes |

---

# 20. Exam Checklist for SJF Numericals

Before submitting:

- [ ] Did I consider Arrival Time?
- [ ] Did I select only from available processes?
- [ ] Did I choose the smallest BT?
- [ ] Did I remember SJF is non-preemptive?
- [ ] Did I draw the Gantt chart?
- [ ] Did I calculate CT?
- [ ] Did I calculate TAT?
- [ ] Did I calculate WT?
- [ ] Did I calculate RT?
- [ ] Did I calculate average WT?
- [ ] Did I calculate average TAT?
- [ ] Did I include CPU idle time if required?
- [ ] Did I handle equal BT correctly?

---

# 21. Quick Memory Trick

```text
SJF = Shortest Job First

AVAILABLE PROCESSES
        ↓
COMPARE BURST TIME
        ↓
SMALLEST BT
        ↓
RUN UNTIL COMPLETE
        ↓
UPDATE TIME
        ↓
REPEAT
```

Formulas:

```text
TAT = CT − AT

WT = TAT − BT

RT = ST − AT

Average WT  = ΣWT / n

Average TAT = ΣTAT / n
```

---

# 22. Numerical Assignment — Non-Preemptive SJF

## Instructions

For each problem:

1. Find the execution order.
2. Draw the Gantt chart.
3. Calculate CT.
4. Calculate TAT.
5. Calculate WT.
6. Calculate RT.
7. Calculate Average WT.
8. Calculate Average TAT.
9. Show the scheduling decisions clearly.

**Do not use preemption.**

---

## Problem 1 — Basic SJF

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 7 |
| P2 | 0 | 3 |
| P3 | 0 | 5 |
| P4 | 0 | 2 |

Tasks:

- Find the SJF execution order.
- Draw the Gantt chart.
- Calculate CT, TAT, WT and RT.
- Find Average WT and Average TAT.

---

## Problem 2 — Different Arrival Times

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 8 |
| P2 | 1 | 4 |
| P3 | 2 | 2 |
| P4 | 4 | 3 |
| P5 | 5 | 1 |

Tasks:

- Apply non-preemptive SJF.
- Find the execution order.
- Draw the Gantt chart.
- Calculate CT, TAT, WT and RT.
- Find Average WT and Average TAT.

---

## Problem 3 — CPU Idle Time

| Process | AT | BT |
|---|---:|---:|
| P1 | 2 | 4 |
| P2 | 6 | 3 |
| P3 | 7 | 1 |
| P4 | 10 | 2 |

Tasks:

- Identify CPU idle periods.
- Apply non-preemptive SJF.
- Draw the complete Gantt chart including IDLE.
- Calculate CT, TAT, WT and RT.
- Find Average WT and Average TAT.

---

## Problem 4 — Tie Case

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 4 |
| P2 | 1 | 2 |
| P3 | 1 | 2 |
| P4 | 2 | 5 |
| P5 | 3 | 2 |

Tasks:

- Apply non-preemptive SJF.
- Handle equal burst-time processes using an appropriate tie-breaking rule.
- Draw the Gantt chart.
- Calculate CT, TAT, WT and RT.
- Find Average WT and Average TAT.

---

## Problem 5 — Challenge Numerical

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 9 |
| P2 | 2 | 4 |
| P3 | 1 | 2 |
| P4 | 5 | 1 |
| P5 | 3 | 6 |
| P6 | 7 | 3 |

Tasks:

- Apply non-preemptive SJF carefully.
- At every decision point, list the available processes and their BTs.
- Find the complete execution order.
- Draw the Gantt chart.
- Calculate CT, TAT, WT and RT.
- Calculate Average WT.
- Calculate Average TAT.
- Identify whether any CPU idle time occurs.

---

# 23. Submission Format

For each problem:

### A. Execution Order

```text
____________________________
```

### B. Gantt Chart

```text
____________________________________________
```

### C. Calculation Table

| Process | AT | BT | ST | CT | TAT | WT | RT |
|---|---:|---:|---:|---:|---:|---:|---:|
| P1 | | | | | | | |
| P2 | | | | | | | |
| P3 | | | | | | | |
| P4 | | | | | | | |

### D. Averages

```text
Average WT  = __________

Average TAT = __________
```

---

# 24. Final Exam Revision

### Definition

> SJF selects the process with the shortest CPU burst time among the available processes.

### Non-preemptive

> Once a process starts executing, it continues until completion.

### Main advantage

> Low/minimum average waiting time under the standard assumptions when burst times are known.

### Main disadvantage

> Starvation of long processes can occur.

### Preemptive version

> SRTF — Shortest Remaining Time First.

### Most important numerical rule

> **Never select a process just because it has the smallest BT. First check whether it has arrived.**

---

## One-Minute Revision

```text
                 SJF
                  |
          Check available
             processes
                  |
          Compare their BT
                  |
          Select shortest
                  |
          Run to completion
                  |
             Update time
                  |
               Repeat
```

**Next topic:** Preemptive SJF → **SRTF (Shortest Remaining Time First)**
