# Lec 16 : Scheduling Algorithms - II - Round Robin

## 1. What is Round Robin Scheduling?

**Round Robin (RR)** is a **preemptive CPU scheduling algorithm** mainly designed for **time-sharing systems**.

In Round Robin, every ready process gets a fixed amount of CPU time called the **Time Quantum (TQ)** or **Time Slice**.

If the process finishes within the time quantum, it leaves the Ready Queue.

If it does not finish, it is **preempted** and placed at the **back of the Ready Queue**.

### Simple Idea

```text
Ready Queue
     ↓
   P1 → P2 → P3 → P4
    ↓
  CPU gets P1
    ↓
  TQ expires?
   /      \
 No        Yes
 ↓          ↓
Finish    Put P1 at
          back of queue
```

### One-line definition

> Round Robin is a preemptive CPU scheduling algorithm in which each ready process is given a fixed time quantum in cyclic order. If a process does not finish within its quantum, it is preempted and placed at the back of the Ready Queue.

---

# 2. Why do we need Round Robin?

Algorithms such as FCFS can allow one long process to occupy the CPU for a long time.

Round Robin solves this by dividing CPU time into small portions.

Example:

```text
P1 = 10 ms
P2 = 3 ms
P3 = 4 ms

TQ = 2 ms
```

Instead of:

```text
P1 → P1 → P1 → P1 → P1 → ...
```

Round Robin gives everyone a turn:

```text
P1 → P2 → P3 → P1 → P2 → P3 → P1 → ...
```

This gives better fairness and response time for interactive systems.

---

# 3. Important Terms

| Term | Meaning |
|---|---|
| AT | Arrival Time |
| BT | Burst Time |
| CT | Completion Time |
| TAT | Turnaround Time |
| WT | Waiting Time |
| TQ | Time Quantum |
| Ready Queue | Queue containing processes waiting for CPU |
| Context Switch | Switching CPU from one process to another |
| Remaining BT | CPU time still required by a process |

---

# 4. Time Quantum

Time Quantum is the maximum amount of CPU time given to a process during one turn.

For example:

```text
TQ = 3 ms
```

A process can execute for at most 3 ms in one turn.

### Case 1: Process finishes before TQ

```text
BT = 2
TQ = 3
```

The process executes for 2 ms and finishes.

```text
P1: |------|
     2 ms
```

No preemption is required.

### Case 2: Process does not finish within TQ

```text
BT = 8
TQ = 3
```

Execution:

```text
3 ms → remaining 5
3 ms → remaining 2
2 ms → finish
```

---

# 5. What if Time Quantum is NOT Given?

This is an important point for numerical problems.

## There is no unique TQ that can be calculated from AT and BT alone.

For example:

```text
P1 BT = 5
P2 BT = 4
P3 BT = 2
```

Different TQs produce different schedules.

```text
TQ = 2 → many turns and context switches

TQ = 4 → fewer turns

TQ = 10 → processes may finish in their first turn
```

Therefore, if a university question does not specify TQ, write an assumption:

> Time Quantum is not specified. Assume TQ = 2 units.

Then solve the numerical using that TQ.

---

# 6. Is There a Best Time Quantum?

There is **no universally best Time Quantum**.

There is a trade-off.

## Small Time Quantum

```text
Small TQ
   ↓
More preemptions
   ↓
More context switches
   ↓
More overhead
```

Example:

```text
TQ = 1
```

A process may be interrupted very frequently.

---

## Large Time Quantum

```text
Large TQ
   ↓
Fewer preemptions
   ↓
Fewer context switches
   ↓
RR starts behaving like FCFS
```

If the TQ is extremely large compared with the CPU bursts, most processes finish in their first turn.

---

# 7. How to Choose a Practical TQ?

There is no single formula that always gives the best TQ.

In a real system, the TQ is chosen according to:

- Response-time requirements
- Average CPU burst lengths
- Context-switch cost
- Number of processes
- Workload characteristics

A useful practical principle is:

> Choose a TQ large enough to avoid excessive context-switch overhead, but small enough to provide good response time and fairness.

If the question explicitly asks to find the best TQ, compare multiple candidate values.

For example:

```text
TQ = 2
TQ = 4
TQ = 6
```

For each value, calculate:

- Average Waiting Time
- Average Turnaround Time
- Response Time
- Number of Context Switches

Then discuss the trade-off rather than assuming that one metric alone determines the best TQ.

---

# 8. Round Robin Ready Queue

Round Robin uses a **FIFO / circular Ready Queue**.

Example:

```text
Initial Ready Queue:

[P1] [P2] [P3] [P4]
 ↑
 CPU
```

After P1 gets its quantum and is not finished:

```text
[P2] [P3] [P4] [P1]
 ↑
 CPU
```

Then P2 gets the CPU.

```text
[P3] [P4] [P1] [P2]
 ↑
 CPU
```

This continues until all processes finish.

---

# 9. Basic Round Robin Algorithm

```text
1. Find the first process that has arrived.
2. Put available processes into the Ready Queue.
3. Select the process at the front.
4. Run it for:
      min(Time Quantum, Remaining Burst Time)
5. If the process finishes:
      Remove it from the queue.
6. Otherwise:
      Reduce its remaining burst time.
      Put it at the back of the Ready Queue.
7. Add newly arrived processes to the Ready Queue.
8. Repeat until every process finishes.
9. Draw the Gantt Chart.
10. Calculate CT, TAT and WT.
```

---

# 10. Important Rule About Arrival Times

Suppose:

```text
P1 starts at 0
TQ = 4
P2 arrives at time 2
```

P2 does **not normally immediately preempt P1**.

P1 continues until:

- it finishes, or
- its quantum expires.

During this time, P2 enters the Ready Queue.

```text
0       2       4
|-------|-------|
   P1      P1

       P2 arrives
       and waits
```

At time 4, the queue is considered according to the problem's stated convention.

### Important

If a new process arrives exactly when a quantum expires, different implementations/textbooks can define the enqueue order differently.

For university numericals, follow the convention given in the question. If none is given, state your assumption.

---

# 11. CPU Idle Time in Round Robin

If no process is available, the CPU becomes idle.

Example:

```text
P1: AT = 3, BT = 4
P2: AT = 8, BT = 2
TQ = 2
```

CPU is idle from:

```text
0 → 3
```

Then P1 runs.

After P1 finishes at 7, there is no process until P2 arrives at 8.

Therefore:

```text
0-3   IDLE
3-5   P1
5-7   P1
7-8   IDLE
8-10  P2
```

---

# 12. Important Formulas

## Completion Time

The time at which a process finishes.

```text
CT = Completion Time
```

---

## Turnaround Time

```text
TAT = CT - AT
```

---

## Waiting Time

```text
WT = TAT - BT
```

or:

```text
WT = CT - AT - BT
```

---

## Average Waiting Time

```text
Average WT = Sum of all WT / Number of Processes
```

---

## Average Turnaround Time

```text
Average TAT = Sum of all TAT / Number of Processes
```

---

# 13. Solved Numerical 1 — All Processes Arrive at 0

### Given

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 5 |
| P2 | 0 | 4 |
| P3 | 0 | 2 |

```text
TQ = 2
```

### Step 1: Initial Ready Queue

```text
[P1, P2, P3]
```

### Step 2: Scheduling

P1 executes for 2:

```text
P1: 5 → 3
```

Queue:

```text
[P2, P3, P1]
```

P2 executes for 2:

```text
P2: 4 → 2
```

Queue:

```text
[P3, P1, P2]
```

P3 executes for 2 and finishes.

Queue:

```text
[P1, P2]
```

P1 executes for 2:

```text
P1: 3 → 1
```

P2 executes for 2 and finishes.

P1 executes for 1 and finishes.

### Gantt Chart

```text
0    2    4    6    8    10   11
| P1 | P2 | P3 | P1 | P2 | P1 |
```

### Completion Time

| Process | CT |
|---|---:|
| P1 | 11 |
| P2 | 10 |
| P3 | 6 |

### Turnaround Time

Since all AT = 0:

```text
TAT = CT - AT
```

| Process | CT | AT | TAT |
|---|---:|---:|---:|
| P1 | 11 | 0 | 11 |
| P2 | 10 | 0 | 10 |
| P3 | 6 | 0 | 6 |

### Waiting Time

```text
WT = TAT - BT
```

| Process | TAT | BT | WT |
|---|---:|---:|---:|
| P1 | 11 | 5 | 6 |
| P2 | 10 | 4 | 6 |
| P3 | 6 | 2 | 4 |

### Average Waiting Time

```text
Average WT = (6 + 6 + 4) / 3
           = 5.33
```

### Average Turnaround Time

```text
Average TAT = (11 + 10 + 6) / 3
            = 9
```

---

# 14. Solved Numerical 2 — Different Arrival Times

### Given

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 5 |
| P2 | 1 | 3 |
| P3 | 2 | 4 |
| P4 | 4 | 2 |

```text
TQ = 2
```

### Gantt Chart

```text
0-2 P1
2-4 P2
4-6 P3
6-8 P1
8-10 P4
10-11 P2
11-13 P3
13-14 P1
```

Combined:

```text
0    2    4    6    8    10   11   13   14
| P1 | P2 | P3 | P1 | P4 | P2 | P3 | P1 |
```

### Completion Time

| Process | CT |
|---|---:|
| P1 | 14 |
| P2 | 11 |
| P3 | 13 |
| P4 | 10 |

### Turnaround Time

```text
P1 = 14 - 0 = 14
P2 = 11 - 1 = 10
P3 = 13 - 2 = 11
P4 = 10 - 4 = 6
```

### Waiting Time

```text
P1 = 14 - 0 - 5 = 9
P2 = 11 - 1 - 3 = 7
P3 = 13 - 2 - 4 = 7
P4 = 10 - 4 - 2 = 4
```

### Final Table

| Process | AT | BT | CT | TAT | WT |
|---|---:|---:|---:|---:|---:|
| P1 | 0 | 5 | 14 | 14 | 9 |
| P2 | 1 | 3 | 11 | 10 | 7 |
| P3 | 2 | 4 | 13 | 11 | 7 |
| P4 | 4 | 2 | 10 | 6 | 4 |

```text
Average WT = 6.75
Average TAT = 10.25
```

---

# 15. Round Robin and Starvation

### Does Round Robin have starvation?

Standard Round Robin generally avoids starvation because processes in the Ready Queue get turns in cyclic order.

Example:

```text
P1 → P2 → P3 → P1 → P2 → P3 → ...
```

Even if P1 has a large burst time, it continues receiving CPU time.

### Compare

| Algorithm | Starvation Possibility |
|---|---|
| FCFS | Generally No |
| SJF | Yes |
| SRTF | Yes |
| Priority | Yes |
| Preemptive Priority | Yes |
| Round Robin | Generally No |

---

# 16. Round Robin vs FCFS

| Feature | FCFS | Round Robin |
|---|---|---|
| Type | Non-preemptive | Preemptive |
| Time Quantum | No | Yes |
| Preemption | No | Yes |
| Fairness | Lower | Higher |
| Context Switches | Usually fewer | More |
| Interactive Systems | Less suitable | Suitable |
| Starvation | Generally no | Generally no |
| Queue | FIFO | Circular/FIFO |

### Important relationship

If:

```text
TQ is very large
```

Round Robin starts behaving like:

```text
FCFS
```

---

# 17. Advantages of Round Robin

1. Fair CPU allocation.
2. Good response time.
3. Suitable for interactive/time-sharing systems.
4. Simple to implement.
5. Standard RR generally avoids starvation.
6. Every ready process gets regular CPU opportunities.

---

# 18. Disadvantages of Round Robin

1. Too-small TQ causes many context switches.
2. Context switching creates overhead.
3. Performance depends heavily on TQ.
4. Average waiting time can be higher than some other algorithms for certain workloads.
5. Choosing TQ requires a trade-off between responsiveness and overhead.

---

# 19. Common Mistakes in Round Robin Numericals

### Mistake 1: Forgetting Remaining Burst Time

If:

```text
BT = 7
TQ = 3
```

After first execution:

```text
Remaining BT = 4
```

Not 7.

---

### Mistake 2: Treating RR as FCFS

RR does not let a process run until completion unless:

```text
BT <= TQ
```

---

### Mistake 3: Forgetting new arrivals

While a process is running, other processes may arrive.

Always check arrival times.

---

### Mistake 4: Immediately preempting for a new arrival

A newly arrived process normally waits for the current quantum to expire or for the running process to finish.

---

### Mistake 5: Wrong formulas

Remember:

```text
TAT = CT - AT

WT = TAT - BT

WT = CT - AT - BT
```

---

### Mistake 6: Not mentioning the TQ assumption

If TQ is not given:

```text
Assume TQ = ___ units
```

Write it before solving.

---

# 20. Exam Problem-Solving Strategy

For every Round Robin numerical:

### Step 1

Create the process table.

```text
P | AT | BT
```

### Step 2

Write the Time Quantum.

```text
TQ = ___
```

If missing:

```text
Assume TQ = ___
```

### Step 3

Create the Ready Queue.

### Step 4

Track Remaining Burst Time.

### Step 5

Create the Gantt Chart.

### Step 6

Find Completion Time.

### Step 7

Calculate:

```text
TAT = CT - AT
WT = TAT - BT
```

### Step 8

Calculate averages.

```text
Average WT
Average TAT
```

---

# 21. Quick Mental Model

Remember:

```text
ROUND ROBIN

Circular Queue
      +
Time Quantum
      +
Preemption
      ↓
Fair CPU Sharing
```

---

# 22. Numerical Assignment — Questions Only With Answers

## Question 1 — Basic Round Robin

Given:

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 5 |
| P2 | 0 | 4 |
| P3 | 0 | 3 |

```text
TQ = 2
```

Find:

- Gantt Chart
- CT
- TAT
- WT
- Average WT
- Average TAT

### Answer

```text
Gantt:
0-2 P1 | 2-4 P2 | 4-6 P3 | 6-8 P1 |
8-10 P2 | 10-11 P3 | 11-12 P1 | 12-14 P2

CT:
P1 = 12
P2 = 14
P3 = 11

TAT:
P1 = 12
P2 = 14
P3 = 11

WT:
P1 = 7
P2 = 10
P3 = 8

Average WT = 8.33
Average TAT = 12.33
```

---

## Question 2 — Different Arrival Times

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 6 |
| P2 | 1 | 4 |
| P3 | 2 | 5 |
| P4 | 4 | 2 |

```text
TQ = 2
```

### Answer

```text
Gantt:
0-2 P1 | 2-4 P2 | 4-6 P3 | 6-8 P1 |
8-10 P4 | 10-12 P2 | 12-14 P3 | 14-16 P1 |
16-17 P3

CT:
P1 = 16
P2 = 12
P3 = 17
P4 = 10

TAT:
P1 = 16
P2 = 11
P3 = 15
P4 = 6

WT:
P1 = 10
P2 = 7
P3 = 10
P4 = 4

Average WT = 7.75
Average TAT = 12.00
```

---

## Question 3 — Small Time Quantum

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 4 |
| P2 | 0 | 3 |
| P3 | 0 | 5 |

```text
TQ = 1
```

### Answer

```text
Gantt:
0-1 P1 | 1-2 P2 | 2-3 P3 | 3-4 P1 |
4-5 P2 | 5-6 P3 | 6-7 P1 | 7-8 P2 |
8-9 P3 | 9-10 P3 | 10-11 P3 | 11-12 P3

CT:
P1 = 7
P2 = 8
P3 = 12

TAT:
P1 = 7
P2 = 8
P3 = 12

WT:
P1 = 3
P2 = 5
P3 = 7

Average WT = 5.00
Average TAT = 9.00
```

---

## Question 4 — CPU Idle Time

| Process | AT | BT |
|---|---:|---:|
| P1 | 2 | 5 |
| P2 | 7 | 3 |
| P3 | 10 | 4 |

```text
TQ = 2
```

### Answer

```text
Gantt:
0-2 IDLE | 2-4 P1 | 4-6 P1 | 6-7 P1 |
7-9 P2 | 9-10 P2 | 10-12 P3 |
12-14 P3

CT:
P1 = 7
P2 = 10
P3 = 14

TAT:
P1 = 5
P2 = 3
P3 = 4

WT:
P1 = 0
P2 = 0
P3 = 0

Average WT = 0
Average TAT = 4.00
```

---

## Question 5 — Longer Bursts

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 8 |
| P2 | 1 | 5 |
| P3 | 2 | 7 |
| P4 | 3 | 4 |

```text
TQ = 3
```

### Answer

```text
Gantt:
0-3 P1 | 3-6 P2 | 6-9 P3 | 9-12 P4 |
12-15 P1 | 15-17 P2 | 17-20 P3 |
20-21 P4 | 21-23 P1 | 23-24 P3

CT:
P1 = 23
P2 = 17
P3 = 24
P4 = 21

TAT:
P1 = 23
P2 = 16
P3 = 22
P4 = 18

WT:
P1 = 15
P2 = 11
P3 = 15
P4 = 14

Average WT = 13.75
Average TAT = 19.75
```

---

## Question 6 — Different Arrival Times

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 3 |
| P2 | 2 | 6 |
| P3 | 4 | 4 |
| P4 | 5 | 5 |

```text
TQ = 2
```

### Answer

```text
Gantt:
0-2 P1 | 2-3 P1 | 3-5 P2 | 5-7 P3 |
7-9 P4 | 9-11 P2 | 11-13 P3 | 13-15 P4 |
15-17 P2 | 17-19 P4

CT:
P1 = 3
P2 = 17
P3 = 13
P4 = 19

TAT:
P1 = 3
P2 = 15
P3 = 9
P4 = 14

WT:
P1 = 0
P2 = 9
P3 = 5
P4 = 9

Average WT = 5.75
Average TAT = 10.25
```

---

## Question 7 — Large Time Quantum

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 4 |
| P2 | 1 | 3 |
| P3 | 2 | 2 |
| P4 | 3 | 5 |

```text
TQ = 5
```

### Answer

```text
Gantt:
0-4 P1 | 4-7 P2 | 7-9 P3 | 9-14 P4

CT:
P1 = 4
P2 = 7
P3 = 9
P4 = 14

TAT:
P1 = 4
P2 = 6
P3 = 7
P4 = 11

WT:
P1 = 0
P2 = 3
P3 = 5
P4 = 6

Average WT = 3.50
Average TAT = 7.00
```

---

## Question 8 — Challenge Numerical

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 7 |
| P2 | 1 | 4 |
| P3 | 2 | 6 |
| P4 | 4 | 3 |
| P5 | 6 | 5 |

```text
TQ = 2
```

### Answer

```text
Gantt:
0-2 P1 | 2-4 P2 | 4-6 P3 | 6-8 P1 |
8-10 P4 | 10-12 P5 | 12-14 P2 | 14-16 P3 |
16-18 P1 | 18-19 P4 | 19-21 P5 | 21-22 P3 |
22-23 P1 | 23-24 P5 | 24-26 P3

CT:
P1 = 23
P2 = 14
P3 = 26
P4 = 19
P5 = 24

TAT:
P1 = 23
P2 = 13
P3 = 24
P4 = 15
P5 = 18

WT:
P1 = 16
P2 = 9
P3 = 18
P4 = 12
P5 = 13

Average WT = 13.60
Average TAT = 18.60
```

---

## Question 9 — Challenge Numerical

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 10 |
| P2 | 2 | 3 |
| P3 | 3 | 6 |
| P4 | 5 | 2 |

```text
TQ = 3
```

### Answer

```text
Gantt:
0-3 P1 | 3-6 P2 | 6-9 P3 | 9-12 P1 |
12-14 P4 | 14-17 P3 | 17-20 P1 |
20-23 P1

CT:
P1 = 23
P2 = 6
P3 = 17
P4 = 14

TAT:
P1 = 23
P2 = 4
P3 = 14
P4 = 9

WT:
P1 = 13
P2 = 1
P3 = 8
P4 = 7

Average WT = 7.25
Average TAT = 12.50
```

---

## Question 10 — Time Quantum Comparison

Given:

| Process | AT | BT |
|---|---:|---:|
| P1 | 0 | 6 |
| P2 | 0 | 4 |
| P3 | 0 | 2 |

Solve the same workload using:

```text
TQ = 1
TQ = 2
TQ = 4
```

Compare:

- Gantt Chart
- Average Waiting Time
- Average Turnaround Time
- Number of context switches

### Answer

```text
TQ = 1
Average WT = 5.33
Average TAT = 9.33

TQ = 2
Average WT = 5.33
Average TAT = 9.33

TQ = 4
Average WT = 5.00
Average TAT = 9.00
```

The number of context switches decreases as the TQ increases.

---

# 23. Quick Revision

```text
Round Robin
     ↓
Preemptive CPU Scheduling
     ↓
Circular Ready Queue
     ↓
Fixed Time Quantum
     ↓
Process runs for TQ
     ↓
Finished?
 ┌───────┴───────┐
Yes              No
 ↓                ↓
Remove       Remaining BT
             ↓
        Back of Queue
```

### Remember these four things

```text
1. RR is PREEMPTIVE.
2. RR uses TIME QUANTUM.
3. Unfinished process goes to the BACK of the queue.
4. WT = TAT - BT
```

### Most important exam formulas

```text
TAT = CT - AT

WT = TAT - BT

Average WT = ΣWT / n

Average TAT = ΣTAT / n
```

### Time Quantum rule

```text
Small TQ
→ More context switches
→ More overhead
→ Better responsiveness

Large TQ
→ Fewer context switches
→ Less overhead
→ RR approaches FCFS
```

---

# 24. Exam Definition

> **Round Robin is a preemptive CPU scheduling algorithm in which each process in the Ready Queue is assigned a fixed Time Quantum. After using its quantum, an unfinished process is preempted and placed at the end of the Ready Queue, allowing other processes to receive CPU time.**

