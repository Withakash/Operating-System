# Lec 15.0 : Scheduling Algorithms - II — SRTF

## Shortest Remaining Time First (SRTF)

> **Topic:** CPU Scheduling  
> **Lecture:** 15.0  
> **Algorithm:** Shortest Remaining Time First  
> **Type:** Preemptive CPU Scheduling  
> **Related Algorithm:** SJF (Shortest Job First)

---

## 1. Learning Objectives

After this lecture, students should be able to:

- Explain the concept of **SRTF**.
- Differentiate between **SJF and SRTF**.
- Understand why SRTF is a **preemptive** scheduling algorithm.
- Calculate the **remaining burst time** of a process.
- Decide when a running process should be **preempted**.
- Draw an SRTF **Gantt Chart**.
- Calculate:
  - Completion Time (CT)
  - Turnaround Time (TAT)
  - Waiting Time (WT)
  - Average Turnaround Time
  - Average Waiting Time
- Solve SRTF numerical problems systematically.

---

# 2. What is SRTF?

**SRTF = Shortest Remaining Time First**

SRTF is the **preemptive version of SJF (Shortest Job First)**.

The key idea is:

> At every scheduling decision, the CPU runs the process having the **smallest remaining burst time**.

Unlike non-preemptive SJF, a running process can be interrupted if a newly arrived process has a shorter remaining time.

### Simple idea

```text
SJF
 |
 |-- Select process with smallest Burst Time
 |
 |-- Once selected, process continues until completion
```

Whereas:

```text
SRTF
 |
 |-- Select process with smallest Remaining Time
 |
 |-- New process arrives?
 |       |
 |       +-- Yes --> Compare remaining times again
 |
 |-- Preempt if another process has a shorter remaining time
```

---

# 3. Why Do We Need SRTF?

Consider a process currently running:

```text
P1
BT = 10
```

Suppose P1 has already executed for 7 units.

Therefore:

```text
Remaining Time = 10 - 7
               = 3
```

Now suppose a new process arrives:

```text
P2
BT = 2
```

Compare:

```text
P1 remaining = 3
P2 remaining = 2
```

Since:

```text
2 < 3
```

P1 is **preempted** and P2 gets the CPU.

This is the main idea of SRTF.

---

# 4. SJF vs SRTF

| Feature | SJF | SRTF |
|---|---|---|
| Full Name | Shortest Job First | Shortest Remaining Time First |
| Type | Non-preemptive | Preemptive |
| Comparison | Burst Time | Remaining Burst Time |
| Can running process be interrupted? | No | Yes |
| New process can cause preemption? | No | Yes |
| Decision basis | Shortest job | Shortest remaining job |
| Gantt chart may contain repeated process | Usually no | Yes |
| Complexity of numerical | Relatively simpler | More complex |

### Remember

```text
SJF
↓
Compare Burst Time

SRTF
↓
Compare Remaining Burst Time
```

---

# 5. Preemptive Scheduling

## What does "preemptive" mean?

In a preemptive scheduling algorithm, the operating system can **take the CPU away from the currently running process** and assign it to another process.

Example:

```text
P1 is running
     ↓
P2 arrives
     ↓
P2 has shorter remaining time
     ↓
P1 is interrupted
     ↓
P2 gets CPU
```

The interrupted process is not terminated.

It simply goes back to the **Ready Queue** with its updated remaining time.

---

# 6. Non-Preemptive vs Preemptive

### Non-Preemptive

```text
Process selected
      ↓
Process runs
      ↓
Process completes
      ↓
Next process selected
```

### Preemptive

```text
Process selected
      ↓
Process starts running
      ↓
New process arrives
      ↓
Compare remaining times
      ↓
Shorter process?
   /          \
 Yes           No
  ↓             ↓
Preempt        Continue
```

---

# 7. Important Terms

## 7.1 Arrival Time (AT)

The time at which a process enters the Ready Queue.

Example:

```text
P2 AT = 3
```

means P2 becomes available at time `3`.

---

## 7.2 Burst Time (BT)

The total CPU time required by a process.

Example:

```text
P1 BT = 8
```

means P1 needs 8 CPU time units in total.

---

## 7.3 Remaining Time

The amount of CPU time still required by the process.

Formula:

```text
Remaining Time = Original BT - CPU Time Already Executed
```

Example:

```text
BT = 8
Executed = 3

Remaining = 8 - 3
          = 5
```

### Important

When solving SRTF, **do not compare original BT after a process has already executed**.

Compare its **current remaining time**.

---

# 8. Completion Time (CT)

Completion Time is the time at which a process **finally finishes execution**.

Example:

```text
P2:
1 → 2
5 → 8
```

P2 was interrupted.

Its final completion time is:

```text
CT = 8
```

Not `2`.

---

# 9. Turnaround Time (TAT)

Formula:

\[
TAT = CT - AT
\]

Where:

- `CT` = Completion Time
- `AT` = Arrival Time

Example:

```text
AT = 2
CT = 10

TAT = 10 - 2
    = 8
```

---

# 10. Waiting Time (WT)

Formula:

\[
WT = TAT - BT
\]

Example:

```text
TAT = 8
BT = 5

WT = 8 - 5
   = 3
```

---

# 11. Response Time (RT)

Response Time is the time from process arrival until the process **gets CPU for the first time**.

\[
RT = First\ Start\ Time - AT
\]

Example:

```text
P2 arrives at 3
P2 first gets CPU at 5

RT = 5 - 3
   = 2
```

### Important

For the basic SRTF numericals in this lecture, the main calculations are:

```text
CT
TAT
WT
Average TAT
Average WT
```

Response Time may be asked separately.

---

# 12. Core Rule of SRTF

At every scheduling decision:

```text
Choose the process with the smallest REMAINING TIME.
```

A scheduling decision may happen when:

1. CPU becomes free.
2. A new process arrives.
3. The currently running process finishes.
4. A new process arrival causes a shorter remaining time.

---

# 13. Step-by-Step Method to Solve SRTF Numericals

Use the following procedure.

```text
1. Observe all Arrival Times
          ↓
2. Start from the earliest relevant time
          ↓
3. Find all processes that have arrived
          ↓
4. Calculate their current Remaining Time
          ↓
5. Select the process with minimum Remaining Time
          ↓
6. Run the selected process
          ↓
7. Stop when:
      ├── process finishes
      OR
      └── a new process arrives
          ↓
8. At a new arrival, compare remaining times again
          ↓
9. Preempt if required
          ↓
10. Continue until every process finishes
          ↓
11. Draw Gantt Chart
          ↓
12. Calculate CT
          ↓
13. Calculate TAT
          ↓
14. Calculate WT
          ↓
15. Calculate averages
```

---

# 14. How to Track Remaining Time

Suppose:

| Process | BT |
|---|---:|
| P1 | 8 |

P1 runs from:

```text
0 → 3
```

CPU time used:

```text
3
```

Therefore:

```text
Remaining = 8 - 3
          = 5
```

Now:

| Process | Original BT | Executed | Remaining |
|---|---:|---:|---:|
| P1 | 8 | 3 | 5 |

When comparing SRTF processes, use:

```text
Remaining = 5
```

not:

```text
Original BT = 8
```

---

# 15. Worked Example 1 — Basic Preemption

Consider:

| Process | Arrival Time (AT) | Burst Time (BT) |
|---|---:|---:|
| P1 | 0 | 8 |
| P2 | 2 | 3 |

## Step 1: Time = 0

Only P1 has arrived.

```text
P1 remaining = 8
```

So P1 starts.

```text
0 → 2
P1
```

P1 has executed for 2 units.

Therefore:

```text
P1 remaining = 8 - 2
             = 6
```

---

## Step 2: Time = 2

P2 arrives.

Current state:

| Process | Remaining Time |
|---|---:|
| P1 | 6 |
| P2 | 3 |

Compare:

```text
P1 = 6
P2 = 3
```

Since:

```text
3 < 6
```

P1 is preempted.

P2 starts.

```text
2 → 5
P2
```

P2 finishes at time 5.

---

## Step 3: Time = 5

Only P1 remains.

```text
P1 remaining = 6
```

So:

```text
5 → 11
P1
```

P1 finishes at time 11.

---

## Gantt Chart

```text
0        2        5        11
|--------|--------|---------|
    P1       P2       P1
```

Notice that **P1 appears twice**.

That is normal in preemptive scheduling.

---

## Final Table

| Process | AT | BT | CT | TAT | WT |
|---|---:|---:|---:|---:|---:|
| P1 | 0 | 8 | 11 | 11 | 3 |
| P2 | 2 | 3 | 5 | 3 | 0 |

Calculations:

\[
TAT(P1)=11-0=11
\]

\[
WT(P1)=11-8=3
\]

\[
TAT(P2)=5-2=3
\]

\[
WT(P2)=3-3=0
\]

Average Waiting Time:

\[
Average\ WT=\frac{3+0}{2}=\boxed{1.5}
\]

Average Turnaround Time:

\[
Average\ TAT=\frac{11+3}{2}=\boxed{7}
\]

---

# 16. Worked Example 2 — Multiple Preemptions

Consider:

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 7 |
| P2 | 2 | 4 |
| P3 | 4 | 1 |

## Time = 0

Only P1 is available.

```text
P1 remaining = 7
```

P1 starts.

```text
0 → 2
P1
```

At time 2:

```text
P1 remaining = 5
P2 remaining = 4
```

Since:

```text
4 < 5
```

P1 is preempted.

---

## Time = 2

P2 runs.

```text
2 → 4
P2
```

At time 4:

```text
P1 remaining = 5
P2 remaining = 2
P3 remaining = 1
```

Shortest = P3.

P2 is preempted.

---

## Time = 4

P3 runs.

```text
4 → 5
P3
```

P3 finishes.

---

## Time = 5

Remaining:

```text
P1 = 5
P2 = 2
```

P2 is shorter.

```text
5 → 7
P2
```

P2 finishes.

---

## Time = 7

Only P1 remains.

```text
7 → 12
P1
```

P1 finishes.

---

## Gantt Chart

```text
0       2       4    5       7        12
|-------|-------|----|-------|---------|
   P1      P2    P3    P2       P1
```

---

## Final Table

| Process | AT | BT | CT | TAT | WT |
|---|---:|---:|---:|---:|---:|
| P1 | 0 | 7 | 12 | 12 | 5 |
| P2 | 2 | 4 | 7 | 5 | 1 |
| P3 | 4 | 1 | 5 | 1 | 0 |

### Average Waiting Time

\[
Average\ WT=\frac{5+1+0}{3}
\]

\[
\boxed{Average\ WT=2}
\]

### Average Turnaround Time

\[
Average\ TAT=\frac{12+5+1}{3}
\]

\[
\boxed{Average\ TAT=6}
\]

---

# 17. Worked Example 3 — Full SRTF Numerical

Consider:

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 8 |
| P2 | 1 | 4 |
| P3 | 2 | 2 |
| P4 | 3 | 1 |

---

## Time = 0

Only P1 has arrived.

```text
P1 remaining = 8
```

So P1 starts.

```text
0 → 1
P1
```

At time 1:

```text
P1 remaining = 7
P2 remaining = 4
```

Shortest is P2.

Therefore P1 is preempted.

---

## Time = 1

P2 starts.

```text
1 → 2
P2
```

At time 2:

```text
P1 remaining = 7
P2 remaining = 3
P3 remaining = 2
```

Shortest = P3.

P2 is preempted.

---

## Time = 2

P3 starts.

```text
2 → 3
P3
```

At time 3:

```text
P1 remaining = 7
P2 remaining = 3
P3 remaining = 1
P4 remaining = 1
```

P3 and P4 have equal remaining time.

Using the common **FCFS tie-breaking rule**, P3 continues because P3 arrived earlier.

```text
3 → 4
P3
```

P3 finishes.

---

## Time = 4

Remaining:

```text
P1 = 7
P2 = 3
P4 = 1
```

Shortest = P4.

```text
4 → 5
P4
```

P4 finishes.

---

## Time = 5

Remaining:

```text
P1 = 7
P2 = 3
```

Shortest = P2.

```text
5 → 8
P2
```

P2 finishes.

---

## Time = 8

Only P1 remains.

```text
P1 remaining = 7
```

Therefore:

```text
8 → 15
P1
```

P1 finishes.

---

## Gantt Chart

```text
0    1    2    4    5    8         15
|----|----|----|----|----|-----------|
 P1   P2   P3   P4   P2      P1
```

---

## Completion Time

Completion Time is the **final finishing time**.

| Process | CT |
|---|---:|
| P1 | 15 |
| P2 | 8 |
| P3 | 4 |
| P4 | 5 |

For P2:

```text
P2 ran from 1 → 2
P2 ran again from 5 → 8
```

Therefore:

```text
CT(P2) = 8
```

---

## Turnaround Time

Formula:

\[
TAT=CT-AT
\]

| Process | AT | CT | TAT |
|---|---:|---:|---:|
| P1 | 0 | 15 | 15 |
| P2 | 1 | 8 | 7 |
| P3 | 2 | 4 | 2 |
| P4 | 3 | 5 | 2 |

Calculations:

\[
TAT(P1)=15-0=15
\]

\[
TAT(P2)=8-1=7
\]

\[
TAT(P3)=4-2=2
\]

\[
TAT(P4)=5-3=2
\]

---

## Waiting Time

Formula:

\[
WT=TAT-BT
\]

| Process | TAT | BT | WT |
|---|---:|---:|---:|
| P1 | 15 | 8 | 7 |
| P2 | 7 | 4 | 3 |
| P3 | 2 | 2 | 0 |
| P4 | 2 | 1 | 1 |

---

## Average Waiting Time

\[
Average\ WT=\frac{7+3+0+1}{4}
\]

\[
\boxed{Average\ WT=2.75}
\]

---

## Average Turnaround Time

\[
Average\ TAT=\frac{15+7+2+2}{4}
\]

\[
\boxed{Average\ TAT=6.5}
\]

---

# 18. Tie-Breaking in SRTF

Sometimes two or more processes have the same remaining time.

Example:

```text
P2 remaining = 3
P3 remaining = 3
```

The scheduling rule needs a tie-breaker.

A common classroom convention is:

> If remaining times are equal, use **FCFS** — select the process that arrived earlier.

Example:

```text
P2:
AT = 2
Remaining = 3

P3:
AT = 4
Remaining = 3
```

P2 is selected because P2 arrived earlier.

### Important

If the question specifies a different tie-breaking rule, **follow the question**.

---

# 19. What Happens When a New Process Arrives?

This is the most important decision point in SRTF.

Suppose:

```text
Current process:
P1 remaining = 5
```

New process arrives:

```text
P2 remaining = 3
```

Compare:

```text
P1 = 5
P2 = 3
```

Since:

```text
3 < 5
```

P1 is preempted.

---

## Case 1: New process is shorter

```text
P1 remaining = 6
P2 BT = 2

6 > 2
```

### Action:

```text
Preempt P1
Run P2
```

---

## Case 2: New process is longer

```text
P1 remaining = 3
P2 BT = 6

3 < 6
```

### Action:

```text
Continue P1
```

No preemption is required.

---

## Case 3: Equal remaining time

```text
P1 remaining = 4
P2 BT = 4
```

Use the specified tie-breaking rule.

If FCFS is used, the earlier-arriving process gets priority.

---

# 20. Gantt Chart in SRTF

A process can appear multiple times.

Example:

```text
0    2    5    7    10
|----|----|----|-----|
 P1   P2   P1   P3
```

If a process is interrupted:

```text
P1 → P2 → P1
```

then P1 will appear in multiple blocks.

### This is not an error.

It is a direct result of **preemption**.

---

# 21. CPU Idle Time

Sometimes no process has arrived yet.

Example:

| Process | AT | BT |
|---|---:|---:|
| P1 | 3 | 4 |
| P2 | 5 | 2 |

From:

```text
0 → 3
```

no process is available.

Therefore CPU remains idle.

Gantt chart:

```text
0       3       5       7
|-------|-------|-------|
 IDLE    P1      P2
```

Do not assign idle time to a process.

---

# 22. Important Formulas

### Completion Time

```text
CT = time at which process finally completes
```

### Turnaround Time

\[
TAT=CT-AT
\]

### Waiting Time

\[
WT=TAT-BT
\]

### Response Time

\[
RT=First\ CPU\ Start-AT
\]

### Average Waiting Time

\[
Average\ WT=
\frac{\sum WT}{Number\ of\ Processes}
\]

### Average Turnaround Time

\[
Average\ TAT=
\frac{\sum TAT}{Number\ of\ Processes}
\]

---

# 23. Important SRTF Trick

When solving a numerical, maintain a small **remaining-time table**.

Example:

At time = 2:

| Process | Original BT | Executed | Remaining |
|---|---:|---:|---:|
| P1 | 8 | 2 | 6 |
| P2 | 4 | 0 | 4 |
| P3 | 2 | 0 | 2 |

Therefore:

```text
P1 → 6
P2 → 4
P3 → 2
```

Select:

```text
P3
```

because:

```text
2 < 4 < 6
```

---

# 24. Common Student Mistakes

## Mistake 1: Comparing original BT

Wrong:

```text
P1 BT = 8
P2 BT = 5

Choose P2
```

This may be wrong if P1 has already executed.

Correct:

```text
P1 remaining = 2
P2 remaining = 5

Choose P1
```

---

## Mistake 2: Forgetting preemption

If a shorter process arrives, SRTF may interrupt the current process.

Do not continue the current process automatically.

---

## Mistake 3: Using the first completion time as CT

If:

```text
P2 runs 1 → 2
P2 runs 5 → 8
```

then:

```text
CT(P2) = 8
```

not 2.

---

## Mistake 4: Forgetting repeated Gantt blocks

This is valid:

```text
P1 | P2 | P1
```

It means P1 was preempted and later resumed.

---

## Mistake 5: Incorrect WT formula

Do not use:

```text
WT = CT - BT
```

Use:

\[
WT=TAT-BT
\]

or:

\[
WT=CT-AT-BT
\]

---

## Mistake 6: Ignoring Arrival Time

A process cannot be selected before its arrival.

If:

```text
P1 AT = 0
P2 AT = 5
```

P2 cannot run at time 2.

---

# 25. SRTF vs FCFS vs SJF

| Feature | FCFS | SJF | SRTF |
|---|---|---|---|
| Type | Non-preemptive | Non-preemptive | Preemptive |
| Selection | Earliest arrival | Smallest BT | Smallest remaining time |
| Preemption | No | No | Yes |
| New shorter process can interrupt? | No | No | Yes |
| Gantt chart repetition | Usually no | Usually no | Possible |

---

# 26. Advantages of SRTF

### 1. Short jobs can finish quickly

Processes with small remaining times are prioritized.

### 2. Can reduce average waiting time

By prioritizing short remaining jobs, SRTF can reduce waiting for some workloads.

### 3. Dynamic decision making

The scheduler considers newly arriving processes.

### 4. Better responsiveness for short processes

A newly arrived short process may get CPU quickly.

---

# 27. Disadvantages of SRTF

### 1. Starvation

A long process may wait for a long time if short processes keep arriving.

Example:

```text
Long process
     ↓
Waiting
     ↓
Short process arrives
     ↓
Short process runs
     ↓
Another short process arrives
     ↓
Long process keeps waiting
```

This can lead to **starvation**.

### 2. More context switches

Frequent preemption can increase context-switch overhead.

### 3. Remaining time must be tracked

The scheduler must continuously maintain process state.

### 4. Burst-time information is required

The algorithm needs an estimate of how much CPU time processes require.

---

# 28. Starvation in SRTF

Consider:

```text
P1:
BT = 20
```

Suppose many short processes continuously arrive:

```text
P2 = 2
P3 = 1
P4 = 2
P5 = 1
...
```

P1 may repeatedly lose CPU selection.

Therefore:

```text
Long process
     ↓
waits
     ↓
short process
     ↓
waits
     ↓
another short process
     ↓
continues waiting
```

This is called **starvation**.

### Possible solution

**Aging** can be used in scheduling systems to gradually increase the priority of waiting processes.

---

# 29. Context Switch Overhead

A context switch occurs when the CPU changes from one process to another.

Example:

```text
P1
 ↓
Context Switch
 ↓
P2
 ↓
Context Switch
 ↓
P1
```

SRTF can cause more context switches because it is preemptive.

In theoretical numerical questions, context-switch overhead is usually ignored unless the question explicitly provides it.

---

# 30. The Main Difference: SJF vs SRTF

This is the line students should remember:

> **SJF selects the shortest job. SRTF continuously selects the process with the shortest remaining job.**

### SJF

```text
Shortest BT
    ↓
Select
    ↓
Run until completion
```

### SRTF

```text
Shortest Remaining BT
    ↓
Select
    ↓
New process arrives?
    ↓
Compare again
    ↓
Preempt if necessary
```

---

# 31. Numerical Practice Set

The following questions are designed for classroom practice.

For each question, calculate:

- Gantt Chart
- Completion Time (CT)
- Turnaround Time (TAT)
- Waiting Time (WT)
- Average Waiting Time
- Average Turnaround Time

Unless specified otherwise:

> **Use FCFS as the tie-breaking rule when remaining times are equal.**

---

# Question 1 — Basic SRTF

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 8 |
| P2 | 2 | 3 |

### Answer

Gantt Chart:

```text
0       2       5       11
|-------|-------|--------|
   P1      P2      P1
```

| Process | CT | TAT | WT |
|---|---:|---:|---:|
| P1 | 11 | 11 | 3 |
| P2 | 5 | 3 | 0 |

```text
Average WT  = 1.5
Average TAT = 7
```

---

# Question 2 — Multiple Processes

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 7 |
| P2 | 2 | 4 |
| P3 | 4 | 1 |

### Answer

Gantt Chart:

```text
0       2       4    5       7        12
|-------|-------|----|-------|---------|
   P1      P2     P3    P2       P1
```

| Process | CT | TAT | WT |
|---|---:|---:|---:|
| P1 | 12 | 12 | 5 |
| P2 | 7 | 5 | 1 |
| P3 | 5 | 1 | 0 |

```text
Average WT  = 2
Average TAT = 6
```

---

# Question 3 — Full SRTF Practice

Consider:

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 8 |
| P2 | 1 | 4 |
| P3 | 2 | 2 |
| P4 | 3 | 1 |

### Answer

Gantt Chart:

```text
0    1    2    4    5    8         15
|----|----|----|----|----|-----------|
 P1   P2   P3   P4   P2      P1
```

| Process | CT | TAT | WT |
|---|---:|---:|---:|
| P1 | 15 | 15 | 7 |
| P2 | 8 | 7 | 3 |
| P3 | 4 | 2 | 0 |
| P4 | 5 | 2 | 1 |

```text
Average WT  = 2.75
Average TAT = 6.5
```

---

# Question 4

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 6 |
| P2 | 1 | 3 |
| P3 | 2 | 5 |
| P4 | 4 | 2 |

### Answer

Gantt Chart:

```text
0    1       4      6          11      16
|----|-------|------|-----------|-------|
 P1     P2      P4       P1        P3
```

| Process | CT | TAT | WT |
|---|---:|---:|---:|
| P1 | 11 | 11 | 5 |
| P2 | 4 | 3 | 0 |
| P3 | 16 | 14 | 9 |
| P4 | 6 | 2 | 0 |

```text
Average WT  = 3.5
Average TAT = 7.5
```

---

# Question 5

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 5 |
| P2 | 1 | 2 |
| P3 | 2 | 4 |
| P4 | 3 | 1 |
| P5 | 5 | 3 |

### Answer

Gantt Chart:

```text
0    1       3    4        8        11       15
|----|-------|----|--------|---------|---------|
 P1     P2    P4     P1       P5        P3
```

| Process | CT | TAT | WT |
|---|---:|---:|---:|
| P1 | 8 | 8 | 3 |
| P2 | 3 | 2 | 0 |
| P3 | 15 | 13 | 9 |
| P4 | 4 | 1 | 0 |
| P5 | 11 | 6 | 3 |

```text
Average WT  = 3
Average TAT = 6
```

---

# Question 6

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 10 |
| P2 | 3 | 2 |
| P3 | 5 | 1 |
| P4 | 6 | 4 |

### Answer

Gantt Chart:

```text
0       3      5    6        10             17
|-------|------|----|--------|---------------|
   P1     P2    P3      P4         P1
```

| Process | CT | TAT | WT |
|---|---:|---:|---:|
| P1 | 17 | 17 | 7 |
| P2 | 5 | 2 | 0 |
| P3 | 6 | 1 | 0 |
| P4 | 10 | 4 | 0 |

```text
Average WT  = 1.75
Average TAT = 6
```

---

# Question 7

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 4 |
| P2 | 1 | 5 |
| P3 | 2 | 1 |
| P4 | 4 | 2 |

### Answer

Gantt Chart:

```text
0      2    3      5        7         12
|------|----|------|--------|----------|
  P1    P3    P1      P4        P2
```

| Process | CT | TAT | WT |
|---|---:|---:|---:|
| P1 | 5 | 5 | 1 |
| P2 | 12 | 11 | 6 |
| P3 | 3 | 1 | 0 |
| P4 | 7 | 3 | 1 |

```text
Average WT  = 2
Average TAT = 5
```

---

# Question 8 — Same Arrival Time

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 3 |
| P2 | 0 | 2 |
| P3 | 1 | 4 |
| P4 | 2 | 1 |

### Answer

Using FCFS for the initial tie between P1 and P2:

```text
0      2    3    6          10
|------|----|----|-----------|
  P2    P4   P1      P3
```

| Process | CT | TAT | WT |
|---|---:|---:|---:|
| P1 | 6 | 6 | 3 |
| P2 | 2 | 2 | 0 |
| P3 | 10 | 9 | 5 |
| P4 | 3 | 1 | 0 |

```text
Average WT  = 2
Average TAT = 4.5
```

---

# Question 9

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 9 |
| P2 | 1 | 3 |
| P3 | 2 | 5 |
| P4 | 6 | 2 |

### Answer

Gantt Chart:

```text
0    1       4          6      8          11              19
|----|-------|----------|------|----------|----------------|
 P1    P2        P3        P4       P3             P1
```

| Process | CT | TAT | WT |
|---|---:|---:|---:|
| P1 | 19 | 19 | 10 |
| P2 | 4 | 3 | 0 |
| P3 | 11 | 9 | 4 |
| P4 | 8 | 2 | 0 |

```text
Average WT  = 3.5
Average TAT = 8.25
```

---

# Question 10 — Multiple Preemptions

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 6 |
| P2 | 2 | 2 |
| P3 | 4 | 3 |
| P4 | 5 | 1 |
| P5 | 7 | 2 |

### Answer

Gantt Chart:

```text
0       2       4     5      6       8      10        14
|-------|-------|-----|------|-------|------|----------|
   P1      P2     P3    P4     P3      P5       P1
```

| Process | CT | TAT | WT |
|---|---:|---:|---:|
| P1 | 14 | 14 | 8 |
| P2 | 4 | 2 | 0 |
| P3 | 8 | 4 | 1 |
| P4 | 6 | 1 | 0 |
| P5 | 10 | 3 | 1 |

```text
Average WT  = 2
Average TAT = 4.8
```

---

# 32. Quick Comparison of the 10 Numericals

| Q | Processes | Key Concept | Avg WT | Avg TAT |
|---|---:|---|---:|---:|
| 1 | 2 | Basic preemption | 1.5 | 7 |
| 2 | 3 | Multiple processes | 2 | 6 |
| 3 | 4 | Multiple preemptions | 2.75 | 6.5 |
| 4 | 4 | New arrivals | 3.5 | 7.5 |
| 5 | 5 | Several arrivals | 3 | 6 |
| 6 | 4 | Long process + short arrivals | 1.75 | 6 |
| 7 | 4 | Preemption after arrival | 2 | 5 |
| 8 | 4 | Same arrival time + tie | 2 | 4.5 |
| 9 | 4 | Multiple preemptions | 3.5 | 8.25 |
| 10 | 5 | Multiple preemptions | 2 | 4.8 |

---

# 33. SRTF Numerical Checklist

Before finalizing your answer, check:

- [ ] Did I consider Arrival Time?
- [ ] Did I compare **remaining time**, not original BT?
- [ ] Did I check newly arriving processes?
- [ ] Did I preempt when a shorter process arrived?
- [ ] Did I update the remaining time correctly?
- [ ] Did I split the Gantt chart when a process was preempted?
- [ ] Did I allow a process to continue when a newly arrived process was longer?
- [ ] Did I handle ties consistently?
- [ ] Did I calculate CT using the **final completion time**?
- [ ] Did I use `TAT = CT - AT`?
- [ ] Did I use `WT = TAT - BT`?
- [ ] Did I calculate Average WT?
- [ ] Did I calculate Average TAT?
- [ ] Did I account for CPU idle time if no process was available?

---

# 34. One-Minute Revision

```text
SRTF
 ↓
Preemptive version of SJF
 ↓
Choose smallest Remaining Burst Time
 ↓
New process arrives?
 ↓
Compare remaining times again
 ↓
If new process is shorter
 ↓
Preempt current process
 ↓
Continue
 ↓
Draw Gantt Chart
 ↓
Calculate CT
 ↓
TAT = CT - AT
 ↓
WT = TAT - BT
 ↓
Calculate averages
```

---

# 35. Final Memory Trick

Remember these three lines:

```text
FCFS → Who came first?

SJF  → Who has the shortest total job?

SRTF → Who has the shortest job remaining?
```

The most important difference:

> **SJF:** Once selected, the process keeps the CPU until it finishes.

> **SRTF:** The running process can lose the CPU whenever another available process has a shorter remaining time.

---

## End of Lecture 15.0

**Next focus:** More CPU Scheduling Algorithms and comparison of scheduling techniques.
