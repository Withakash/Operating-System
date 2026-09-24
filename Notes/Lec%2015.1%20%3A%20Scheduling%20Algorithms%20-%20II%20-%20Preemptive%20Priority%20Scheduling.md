# Lec 15.1 : Scheduling Algorithms - II — Preemptive Priority Scheduling

## 1. Introduction

**Priority Scheduling** assigns a priority to every process. The CPU selects the highest-priority process among the processes that have arrived.

In **Preemptive Priority Scheduling**, a running process can be interrupted when a newly arrived process has a higher priority.

> **Default convention used in these notes:** smaller priority number = higher priority.

Example:

```text
Priority 1 → Highest
Priority 2
Priority 3
Priority 4 → Lowest
```

Always check the question because some problems use larger numbers as higher priority.

---

## 2. What Does Preemptive Mean?

**Preemptive** means the operating system can interrupt a process that is currently using the CPU.

Example:

```text
P1 is running
Priority = 4

P2 arrives
Priority = 1
```

Because P2 has higher priority:

```text
P1 → PREEMPTED
P2 → RUNS
```

After P2 finishes, the scheduler can resume P1.

### Basic Flow

```text
Process is running
       ↓
New process arrives
       ↓
Compare priorities
    /         \
  YES          NO
   ↓            ↓
Preempt       Continue
current       current
process       process
   ↓
Run new process
```

---

## 3. Preemptive vs Non-Preemptive Priority

| Feature | Non-Preemptive Priority | Preemptive Priority |
|---|---|---|
| Type | Non-preemptive | Preemptive |
| Priority checked | When CPU becomes free | During scheduling decisions / arrivals |
| Running process interrupted? | No | Yes |
| Higher-priority process arrives | Waits | Can preempt |
| Context switches | Usually fewer | Usually more |
| Starvation | Possible | Possible |
| Aging | Can be used | Can be used |

### Simple Difference

**Non-Preemptive:**

```text
P1 starts
 ↓
P2 arrives with higher priority
 ↓
P1 CONTINUES
 ↓
P1 finishes
 ↓
P2 runs
```

**Preemptive:**

```text
P1 starts
 ↓
P2 arrives with higher priority
 ↓
P1 PREEMPTED
 ↓
P2 runs
```

---

## 4. Important Terms

| Term | Meaning |
|---|---|
| AT | Arrival Time |
| BT | Burst Time |
| Priority | Priority assigned to process |
| CT | Completion Time |
| TAT | Turnaround Time |
| WT | Waiting Time |
| Remaining BT | CPU time still required |

---

## 5. Important Formulas

### Completion Time

```text
CT = Time at which process finishes
```

### Turnaround Time

```text
TAT = CT - AT
```

### Waiting Time

```text
WT = TAT - BT
```

or:

```text
WT = CT - AT - BT
```

### Average Waiting Time

```text
Average WT = Sum of all WT / Number of Processes
```

### Average Turnaround Time

```text
Average TAT = Sum of all TAT / Number of Processes
```

---

## 6. Golden Rule

At every scheduling decision:

> **Look at all processes that have arrived and select the process with the highest priority.**

If smaller number means higher priority:

```text
Priority 1 > Priority 2 > Priority 3 > Priority 4
```

Do **not** simply sort all processes by priority before making the Gantt chart.

**Arrival Time matters.**

---

# 7. Step-by-Step Method for Solving Numericals

### Step 1 — Make the process table

Example:

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 0 | 7 | 3 |
| P2 | 1 | 4 | 2 |
| P3 | 2 | 2 | 1 |
| P4 | 4 | 3 | 4 |

Assume smaller priority number = higher priority.

### Step 2 — Start at the earliest arrival

At `t = 0`, only P1 has arrived.

So P1 starts.

### Step 3 — Check new arrivals

At `t = 1`, P2 arrives.

```text
P1 → Priority 3
P2 → Priority 2
```

P2 has higher priority:

```text
P1 → PREEMPTED
P2 → RUNS
```

### Step 4 — Check again when another process arrives

At `t = 2`, P3 arrives.

```text
P1 → Priority 3
P2 → Priority 2
P3 → Priority 1
```

P3 has the highest priority:

```text
P2 → PREEMPTED
P3 → RUNS
```

### Step 5 — Continue until every process finishes

Use this flow:

```text
Processes arrived
       ↓
Compare priorities
       ↓
Highest priority process
       ↓
Run it
       ↓
New process arrives?
       ↓
Compare again
```

---

# 8. Worked Numerical Example

Given:

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 0 | 7 | 3 |
| P2 | 1 | 4 | 2 |
| P3 | 2 | 2 | 1 |
| P4 | 4 | 3 | 4 |

Assumption: smaller number = higher priority.

### Time 0

Only P1 is available.

```text
0 → 1 : P1
```

P1 has executed 1 unit.

```text
P1 remaining = 7 - 1 = 6
```

### Time 1

P2 arrives.

```text
P1 → Priority 3
P2 → Priority 2
```

P2 has higher priority, so P1 is preempted.

### Time 2

P3 arrives.

```text
P1 → Priority 3
P2 → Priority 2
P3 → Priority 1
```

P3 has the highest priority, so P2 is preempted.

P3 runs from:

```text
2 → 4
```

P3 finishes.

```text
CT(P3) = 4
```

### Time 4

P4 arrives.

Available:

```text
P1 → Priority 3
P2 → Priority 2
P4 → Priority 4
```

P2 has the highest priority.

P2 already executed from 1 to 2, so:

```text
P2 remaining = 4 - 1 = 3
```

P2 runs:

```text
4 → 7
```

Therefore:

```text
CT(P2) = 7
```

### Time 7

Remaining:

```text
P1 → Priority 3
P4 → Priority 4
```

P1 has higher priority.

P1 has 6 units remaining:

```text
7 → 13
```

Therefore:

```text
CT(P1) = 13
```

### Time 13

Only P4 remains:

```text
13 → 16
```

Therefore:

```text
CT(P4) = 16
```

---

# 9. Gantt Chart

```text
0    1    2    4    7       13    16
| P1 | P2 | P3 | P2 |   P1   | P4 |
```

Notice that P1 and P2 appear more than once. This happens because the algorithm is preemptive.

---

# 10. CT, TAT and WT

| Process | AT | BT | Priority | CT | TAT | WT |
|---|---:|---:|---:|---:|---:|---:|
| P1 | 0 | 7 | 3 | 13 | 13 | 6 |
| P2 | 1 | 4 | 2 | 7 | 6 | 2 |
| P3 | 2 | 2 | 1 | 4 | 2 | 0 |
| P4 | 4 | 3 | 4 | 16 | 12 | 9 |

### Average Waiting Time

```text
Average WT = (6 + 2 + 0 + 9) / 4
           = 17 / 4
           = 4.25
```

### Average Turnaround Time

```text
Average TAT = (13 + 6 + 2 + 12) / 4
            = 33 / 4
            = 8.25
```

---

# 11. SRTF vs Preemptive Priority

Students often confuse these algorithms.

### SRTF

Selection is based on:

```text
Shortest Remaining Burst Time
```

Example:

```text
P1 → Remaining = 5
P2 → Remaining = 3

P2 runs
```

### Preemptive Priority

Selection is based on:

```text
Priority
```

Example:

```text
P1 → Priority = 3
P2 → Priority = 1

P2 runs
```

Remember:

```text
SRTF
↓
Shortest Remaining Time
```

```text
Preemptive Priority
↓
Highest Priority
```

---

# 12. What If the New Process Has Lower Priority?

Suppose:

```text
Current:
P1 → Priority 2

New:
P2 → Priority 5
```

P1 has higher priority, so:

```text
P1 continues
P2 waits
```

There is no preemption.

---

# 13. Equal Priority

Suppose:

```text
P1 → Priority 2
P2 → Priority 2
```

A tie-breaking rule is needed.

A common assumption is:

> **Use FCFS among processes having equal priority.**

Example:

```text
P1 AT = 0
P2 AT = 2
Both Priority = 1
```

P1 gets preference because it arrived earlier.

Always follow the rule specified in the question.

---

# 14. CPU Idle Case

Consider:

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 3 | 4 | 1 |
| P2 | 5 | 2 | 2 |

From 0 to 3, no process has arrived.

Therefore:

```text
0 → 3 : IDLE
```

Gantt chart:

```text
0     3        7
| IDLE |   P1   |
```

Always show CPU idle time.

---

# 15. Starvation

Preemptive Priority Scheduling can cause **starvation**.

Suppose:

```text
P1 → Priority 8
```

P1 is waiting while high-priority processes keep arriving:

```text
Priority 1
Priority 2
Priority 1
Priority 2
Priority 1
...
```

P1 may keep waiting.

This is called:

> **Starvation / Indefinite Blocking**

---

# 16. Aging

Aging helps prevent starvation.

The longer a process waits, the more its priority is gradually improved.

Example:

```text
Initial:
P1 → Priority 8

After waiting:
Priority 7
Priority 6
Priority 5
Priority 4
...
```

Eventually P1 gets CPU time.

> **Aging is a common solution to starvation.**

---

# 17. Context Switching

Preemptive scheduling can cause more context switches.

Example:

```text
P1
 ↓
P2
 ↓
P3
 ↓
P1
 ↓
P4
```

During a context switch:

```text
Save current process state
        ↓
Load next process state
        ↓
Run next process
```

If context-switch time is given in the question, include it. Otherwise, normally ignore it.

---

# 18. Advantages

1. High-priority processes get CPU quickly.
2. Useful when processes have different levels of urgency.
3. Newly arrived high-priority processes can get immediate CPU access.
4. Can improve response time for important processes.

---

# 19. Disadvantages

1. **Starvation** can occur.
2. More context switches may occur.
3. More complex than non-preemptive priority scheduling.
4. Priority assignment can be difficult.
5. Frequent preemption creates scheduling overhead.

---

# 20. Numerical Solving Checklist

Before solving:

```text
✓ Check whether smaller or larger number means higher priority
✓ Note AT, BT and Priority
✓ Start at the earliest arrival
✓ Identify all arrived processes
✓ Select highest-priority process
✓ Check new arrivals
✓ Preempt if a newly arrived process has higher priority
✓ Continue until all processes finish
✓ Draw Gantt chart
✓ Calculate CT
✓ Calculate TAT
✓ Calculate WT
✓ Calculate Average WT
✓ Calculate Average TAT
```

---

# 21. Quick Scheduling Table Method

While solving, maintain a table like this:

| Time | Arrived Processes | Priorities | Selected |
|---:|---|---|---|
| 0 | P1 | P1=3 | P1 |
| 1 | P1,P2 | P1=3, P2=2 | P2 |
| 2 | P1,P2,P3 | 3,2,1 | P3 |
| 4 | P1,P2,P4 | 3,2,4 | P2 |
| 7 | P1,P4 | 3,4 | P1 |
| 13 | P4 | 4 | P4 |

This makes preemption easy to track.

---

# 22. Numerical Assignment

## Instructions

For all questions:

> **Smaller priority number = Higher priority**

For each problem calculate:

1. Gantt Chart
2. Completion Time (CT)
3. Turnaround Time (TAT)
4. Waiting Time (WT)
5. Average Waiting Time
6. Average Turnaround Time

---

## Problem 1 — Basic

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 0 | 8 | 3 |
| P2 | 1 | 4 | 1 |
| P3 | 2 | 2 | 2 |
| P4 | 4 | 3 | 4 |

---

## Problem 2 — Multiple Preemptions

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 0 | 10 | 4 |
| P2 | 2 | 5 | 2 |
| P3 | 3 | 2 | 1 |
| P4 | 5 | 3 | 3 |

**Hint:** There will be multiple preemptions.

---

## Problem 3 — CPU Idle

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 2 | 5 | 2 |
| P2 | 4 | 3 | 1 |
| P3 | 7 | 2 | 3 |
| P4 | 9 | 4 | 2 |

Remember to show CPU idle time.

---

## Problem 4 — Same Priority

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 0 | 6 | 2 |
| P2 | 1 | 4 | 2 |
| P3 | 3 | 2 | 1 |
| P4 | 5 | 3 | 2 |

Assume **FCFS for equal priorities**.

---

## Problem 5 — Challenging

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 0 | 12 | 5 |
| P2 | 1 | 6 | 3 |
| P3 | 2 | 4 | 4 |
| P4 | 4 | 2 | 1 |
| P5 | 6 | 3 | 2 |
| P6 | 8 | 1 | 1 |

---

# 23. Extra Practice

## Problem 6

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 0 | 9 | 5 |
| P2 | 1 | 5 | 3 |
| P3 | 3 | 4 | 2 |
| P4 | 4 | 2 | 1 |
| P5 | 7 | 3 | 4 |

---

## Problem 7

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 0 | 15 | 5 |
| P2 | 2 | 7 | 3 |
| P3 | 4 | 4 | 2 |
| P4 | 5 | 2 | 1 |
| P5 | 7 | 1 | 4 |
| P6 | 9 | 3 | 2 |

---

## Problem 8 — Equal Priority + Preemption

| Process | AT | BT | Priority |
|---|---:|---:|---:|
| P1 | 0 | 8 | 3 |
| P2 | 2 | 5 | 2 |
| P3 | 3 | 3 | 2 |
| P4 | 5 | 2 | 1 |
| P5 | 8 | 4 | 3 |

Assume FCFS for equal priorities.

---

# 24. Quick Revision

```text
FCFS
→ First Come First Serve

SJF
→ Shortest Burst Time

SRTF
→ Shortest Remaining Time

Priority Non-Preemptive
→ Highest Priority, cannot interrupt

Priority Preemptive
→ Highest Priority, CAN interrupt
```

### Key formulas

```text
TAT = CT - AT

WT = TAT - BT

Average WT = ΣWT / n

Average TAT = ΣTAT / n
```

### One-line Definition

> **Preemptive Priority Scheduling is a CPU scheduling algorithm in which the highest-priority ready process executes, and a running process can be preempted when a higher-priority process arrives.**

### Golden Rule

> **At every scheduling decision, among all arrived processes, select the highest-priority process. If a newly arrived process has a higher priority than the running process, preempt the running process.**
