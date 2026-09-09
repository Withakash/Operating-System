# Lec 14 — CPU Scheduling: Preemptive vs Non-Preemptive & FCFS

**Course:** Operating Systems
**Lecture:** 14
**Topic:** CPU Scheduling — Introduction, Preemptive vs Non-Preemptive, FCFS
**Recommended Duration:** 60–75 Minutes

---

# 1. Learning Objectives

By the end of this lecture, students should be able to:

1. Explain why CPU scheduling is required.
2. Understand the role of the CPU Scheduler.
3. Differentiate between preemptive and non-preemptive scheduling.
4. Explain why FCFS is a non-preemptive scheduling algorithm.
5. Explain how FCFS works using a Ready Queue.
6. Draw a Gantt Chart for FCFS.
7. Calculate:

   * Arrival Time (AT)
   * Burst Time (BT)
   * Completion Time (CT)
   * Turnaround Time (TAT)
   * Waiting Time (WT)
   * Response Time (RT)
8. Calculate Average Waiting Time and Average Turnaround Time.
9. Handle FCFS problems with:

   * Same arrival time
   * Different arrival times
   * CPU idle time
10. Explain the Convoy Effect.
11. Discuss advantages and disadvantages of FCFS.

---

# 2. Introduction to CPU Scheduling

## 2.1 Why Do We Need CPU Scheduling?

A computer system may have multiple processes waiting to execute.

However:

> **A CPU core can execute only one process/thread at a time.**

Therefore, the Operating System must decide:

> **Which process should get the CPU next?**

This decision is called **CPU Scheduling**.

### Simple Example

Suppose the Ready Queue contains:

```text
READY QUEUE

┌────┬────┬────┬────┐
│ P1 │ P2 │ P3 │ P4 │
└────┴────┴────┴────┘
        │
        ▼
   CPU Scheduler
        │
        ▼
       CPU
```

The CPU Scheduler selects one process from the Ready Queue and allocates the CPU to it.

---

# 3. Definition of CPU Scheduling

> **CPU Scheduling is the process by which the Operating System selects a process from the Ready Queue and allocates the CPU to it.**

The main purpose is to use the CPU efficiently while trying to provide good performance to processes.

Important goals include:

* Maximizing CPU Utilization
* Improving Throughput
* Minimizing Waiting Time
* Minimizing Turnaround Time
* Minimizing Response Time
* Providing fairness

CPU scheduling is important because many processes may be ready while only one can use a CPU core at a particular instant.

---

# 4. CPU Scheduler

The **CPU Scheduler** is the OS component responsible for selecting the next process to run.

```text
                  PROCESSES
                      │
                      ▼
               ┌─────────────┐
               │ Ready Queue │
               └─────────────┘
                      │
                      ▼
              ┌──────────────┐
              │ CPU Scheduler│
              └──────────────┘
                      │
                      ▼
                    CPU
```

### CPU Scheduler answers:

> **"Who gets the CPU next?"**

---

# 5. What is a Scheduling Algorithm?

The CPU Scheduler needs a rule to decide which process should execute next.

That rule is called a:

> **CPU Scheduling Algorithm**

Examples:

* FCFS
* SJF
* SRTF
* Round Robin
* Priority Scheduling
* Multilevel Queue
* Multilevel Feedback Queue

---

# 6. Important Question Before Learning Algorithms

Before learning FCFS, SJF and Round Robin, understand this question:

> **Can the Operating System take the CPU away from a currently running process?**

There are two possibilities.

```text
                    CPU SCHEDULING
                          │
              ┌───────────┴───────────┐
              │                       │
              ▼                       ▼
        NON-PREEMPTIVE           PREEMPTIVE
              │                       │
       CPU is not forcibly       CPU can be taken
       taken from process         from running process
```

---

# 7. Preemptive vs Non-Preemptive Scheduling

## 7.1 Non-Preemptive Scheduling

In **non-preemptive scheduling**, once a process gets the CPU, the OS does not forcibly remove it.

The process normally continues until:

* Its CPU burst finishes, or
* It voluntarily enters a waiting/blocked state, such as waiting for I/O.

### Basic Flow

```text
Process
   │
   ▼
READY
   │
   │ Scheduler selects process
   ▼
RUNNING
   │
   ├───────────────┐
   │               │
   ▼               ▼
FINISH          WAIT / BLOCK
   │               │
   ▼               ▼
CPU available   CPU available
```

### Simple Rule

> **Once the process gets the CPU, let it continue until it finishes its CPU burst or blocks.**

---

# 8. Why is it Called "Non-Preemptive"?

The word **preempt** means:

> To take something away before it is finished.

Therefore:

### Non-Preemptive

The OS does **not preempt** the currently running process.

Example:

```text
Time →

0             5             8
│-------------│-------------│
      P1             P2
```

Suppose P1 is running.

If P2 arrives at time 2:

```text
Time
0        2                  5
│--------│------------------│
   P1 arrives
        P2 arrives
```

P2 does **not** interrupt P1.

P1 continues:

```text
P1: RUNNING
     │
     │ P2 arrives
     │
     │ P1 continues
     │
     ▼
   FINISH
     │
     ▼
     P2
```

---

# 9. Preemptive Scheduling

In **preemptive scheduling**, the OS can interrupt a running process and allocate the CPU to another process.

Example:

```text
Time →

0          3          6
│----------│----------│
    P1         P2
```

Suppose:

* P1 is running.
* P2 arrives.
* Scheduling policy decides P2 should execute.

The OS can interrupt P1.

```text
P1
│
▼
RUNNING
│
│ PREEMPTED
▼
READY
│
│
└──────────────► CPU later
```

CPU is given to P2.

---

# 10. Real-Life Analogy

## Non-Preemptive — Single Service Counter

Imagine a normal bank counter.

Customer A is being served.

Customer B arrives.

Customer B cannot normally force Customer A to leave halfway through the transaction.

```text
Customer A
    │
    ▼
  SERVICE
    │
    ▼
 COMPLETE
    │
    ▼
Customer B
```

This is similar to **non-preemptive scheduling**.

---

## Preemptive — Emergency Service

Imagine an emergency situation where a higher-priority customer must be served immediately.

Current service may be interrupted.

```text
Customer A
    │
    ▼
  SERVICE
    │
    ▼
INTERRUPTED
    │
    ▼
Customer B
    │
    ▼
  SERVICE
```

This is similar to **preemptive scheduling**.

---

# 11. Comparison: Preemptive vs Non-Preemptive

| Feature                                      | Non-Preemptive           | Preemptive        |
| -------------------------------------------- | ------------------------ | ----------------- |
| Can running process be forcibly interrupted? | No                       | Yes               |
| CPU can be taken before completion?          | No                       | Yes               |
| Context switching                            | Generally lower          | Generally higher  |
| Implementation                               | Simpler                  | More complex      |
| Response to newly arrived processes          | Usually slower           | Usually faster    |
| Example                                      | FCFS, Non-Preemptive SJF | SRTF, Round Robin |
| Scheduling overhead                          | Lower                    | Higher            |

> **Important:** Preemptive scheduling is not automatically "better". It can improve responsiveness but introduces additional context-switching overhead.

---

# 12. Classification of CPU Scheduling Algorithms

A useful high-level classification:

```text
                  CPU SCHEDULING
                        │
          ┌─────────────┴─────────────┐
          │                           │
          ▼                           ▼
   NON-PREEMPTIVE                PREEMPTIVE
          │                           │
    ┌─────┴─────┐              ┌─────┴─────┐
    │           │              │           │
   FCFS        SJF            SRTF      Round Robin
    │
    │
    ▼
 Priority
 Non-Preemptive
```

Common scheduling algorithms include FCFS, SJF, SRTF, Round Robin and Priority Scheduling.

---

# 13. First Algorithm — FCFS

# FCFS — First Come First Serve

FCFS stands for:

> **First Come, First Serve**

The name itself explains the basic idea.

> **The process that arrives first gets the CPU first.**

It works similarly to a normal queue.

```text
Arrival Order

P1 → P2 → P3 → P4

CPU Order

P1 → P2 → P3 → P4
```

FCFS is a **non-preemptive CPU scheduling algorithm**. Once a process starts executing, it normally continues until its CPU burst completes or it blocks.

---

# 14. FCFS Real-Life Example

Imagine a queue at a grocery store.

```text
┌────┐
│ P1 │ ← First
├────┤
│ P2 │
├────┤
│ P3 │
├────┤
│ P4 │
└────┘
```

The service order is:

```text
P1 → P2 → P3 → P4
```

The same basic principle is used by FCFS.

---

# 15. How FCFS Works

FCFS follows a simple process:

### Step 1

Processes arrive and enter the Ready Queue.

### Step 2

The process at the front of the queue is selected.

### Step 3

The selected process gets the CPU.

### Step 4

The process runs until its CPU burst is completed or it blocks.

### Step 5

The next process in the queue gets the CPU.

### Step 6

Repeat until all processes finish.

```text
             PROCESS ARRIVES
                    │
                    ▼
              READY QUEUE
                    │
                    ▼
              FRONT PROCESS
                    │
                    ▼
                   CPU
                    │
                    ▼
                COMPLETE
                    │
                    ▼
            NEXT PROCESS
```

This queue-based working is the fundamental mechanism of FCFS.

---

# 16. Why is FCFS Non-Preemptive?

Suppose:

```text
P1 = 10 ms
P2 = 2 ms
```

P1 starts first.

```text
0                  10        12
│-------------------│---------│
         P1              P2
```

Even if P2 arrives while P1 is running:

```text
P1 starts at 0

P2 arrives at 2
       ↓
P1 continues
       ↓
P1 finishes at 10
       ↓
P2 starts
```

P2 cannot interrupt P1.

Therefore:

> **FCFS is non-preemptive.**

---

# 17. Important CPU Scheduling Terminology

Before solving numerical problems, understand these terms.

---

## 17.1 Arrival Time — AT

> The time at which a process enters the Ready Queue.

Example:

```text
P1 arrives at time 3

AT(P1) = 3
```

---

## 17.2 Burst Time — BT

> The amount of CPU time required by a process.

Example:

```text
P1 needs 5 ms CPU time

BT(P1) = 5 ms
```

Also called:

> **CPU Burst Time**

---

## 17.3 Completion Time — CT

> The time at which a process finishes execution.

Example:

```text
P1 finishes at time 10

CT(P1) = 10
```

---

## 17.4 Turnaround Time — TAT

> The total time spent by a process in the system from arrival until completion.

### Formula

```text
TAT = CT - AT
```

It includes:

```text
Turnaround Time
       │
       ├── Waiting Time
       │
       └── CPU Burst Time
```

Therefore:

```text
TAT = WT + BT
```

The standard scheduling definition is completion time minus arrival time.

---

# 18. Waiting Time — WT

> **Waiting Time is the total time a process spends waiting in the Ready Queue.**

### Formula

```text
WT = TAT - BT
```

Since:

```text
TAT = CT - AT
```

we can also write:

```text
WT = CT - AT - BT
```

---

# 19. Response Time — RT

> **Response Time is the time from process arrival until the process gets the CPU for the first time.**

### Formula

```text
RT = First CPU Start Time - Arrival Time
```

Example:

```text
Process arrives at 2 ms
Process first gets CPU at 7 ms

RT = 7 - 2
   = 5 ms
```

### Important

Do not confuse:

```text
Response Time ≠ Turnaround Time
```

Response time only considers the time until the **first CPU response**.

This distinction becomes particularly important in interactive/time-sharing systems.

---

# 20. Formula Summary

```text
┌───────────────────────────────────────────┐
│          CPU SCHEDULING FORMULAS          │
├───────────────────────────────────────────┤
│                                           │
│ TAT = CT - AT                             │
│                                           │
│ WT  = TAT - BT                            │
│                                           │
│ WT  = CT - AT - BT                        │
│                                           │
│ RT  = First Start Time - AT               │
│                                           │
│ Average WT  = Σ WT / Number of Processes  │
│                                           │
│ Average TAT = Σ TAT / Number of Processes │
│                                           │
└───────────────────────────────────────────┘
```

---

# 21. Gantt Chart

A **Gantt Chart** represents the execution of processes over time.

Example:

```text
        P1          P2          P3
     ┌─────────┬──────────┬──────────┐
     │         │          │          │
─────┴─────────┴──────────┴──────────┴────
     0         5          8         10
```

Interpretation:

```text
P1 → executes from 0 to 5
P2 → executes from 5 to 8
P3 → executes from 8 to 10
```

### Important

The numbers below the Gantt Chart represent:

> **Time boundaries**

They are not process numbers.

---

# 22. FCFS Numerical Example 1

## All Processes Arrive at the Same Time

Consider:

| Process | Arrival Time (AT) | Burst Time (BT) |
| ------- | ----------------: | --------------: |
| P1      |                 0 |               5 |
| P2      |                 0 |               3 |
| P3      |                 0 |               8 |

Since all processes arrive at time 0, use the given order:

```text
P1 → P2 → P3
```

This type of same-arrival example is also used in the GeeksforGeeks FCFS explanation.

---

# 23. Step 1 — Draw Gantt Chart

P1:

```text
0 → 5
```

P2:

```text
5 → 8
```

P3:

```text
8 → 16
```

Therefore:

```text
        P1          P2              P3
     ┌─────────┬───────────┬─────────────┐
     │         │           │             │
─────┴─────────┴───────────┴─────────────┴──
     0         5           8             16
```

---

# 24. Step 2 — Completion Time

Completion time is the time at which each process finishes.

| Process | CT |
| ------- | -: |
| P1      |  5 |
| P2      |  8 |
| P3      | 16 |

---

# 25. Step 3 — Turnaround Time

Formula:

```text
TAT = CT - AT
```

### P1

```text
TAT = 5 - 0
    = 5
```

### P2

```text
TAT = 8 - 0
    = 8
```

### P3

```text
TAT = 16 - 0
    = 16
```

---

# 26. Step 4 — Waiting Time

Formula:

```text
WT = TAT - BT
```

### P1

```text
WT = 5 - 5
   = 0
```

### P2

```text
WT = 8 - 3
   = 5
```

### P3

```text
WT = 16 - 8
   = 8
```

---

# 27. Final Table

| Process | AT | BT | CT | TAT | WT |
| ------- | -: | -: | -: | --: | -: |
| P1      |  0 |  5 |  5 |   5 |  0 |
| P2      |  0 |  3 |  8 |   8 |  5 |
| P3      |  0 |  8 | 16 |  16 |  8 |

### Average Turnaround Time

```text
Average TAT
= (5 + 8 + 16) / 3

= 29 / 3

= 9.67 ms
```

### Average Waiting Time

```text
Average WT
= (0 + 5 + 8) / 3

= 13 / 3

= 4.33 ms
```

These values match the standard same-arrival FCFS example.

---

# 28. FCFS Numerical Example 2

## Different Arrival Times

Consider:

| Process | AT | BT |
| ------- | -: | -: |
| P1      |  2 |  5 |
| P2      |  0 |  3 |
| P3      |  4 |  4 |

---

# 29. Step 1 — Determine Arrival Order

Arrival times:

```text
P2 → P1 → P3

AT:
P2 = 0
P1 = 2
P3 = 4
```

Therefore FCFS order:

```text
P2 → P1 → P3
```

---

# 30. Step 2 — Execute P2

P2 arrives at:

```text
t = 0
```

BT:

```text
3 ms
```

Therefore:

```text
P2: 0 → 3
```

CT:

```text
CT(P2) = 3
```

---

# 31. Step 3 — Execute P1

P1 arrived at:

```text
t = 2
```

But P2 is running.

P1 starts at:

```text
t = 3
```

BT = 5

Therefore:

```text
P1: 3 → 8
```

CT:

```text
CT(P1) = 8
```

---

# 32. Step 4 — Execute P3

P3 arrived at:

```text
t = 4
```

But P1 is running.

P3 starts at:

```text
t = 8
```

BT = 4

Therefore:

```text
P3: 8 → 12
```

CT:

```text
CT(P3) = 12
```

---

# 33. Gantt Chart

```text
        P2          P1              P3
     ┌─────────┬────────────┬────────────┐
     │         │            │            │
─────┴─────────┴────────────┴────────────┴──
     0         3            8            12
```

---

# 34. Calculate TAT

### P2

```text
TAT = CT - AT
    = 3 - 0
    = 3 ms
```

### P1

```text
TAT = 8 - 2
    = 6 ms
```

### P3

```text
TAT = 12 - 4
    = 8 ms
```

---

# 35. Calculate WT

### P2

```text
WT = TAT - BT
   = 3 - 3
   = 0 ms
```

### P1

```text
WT = 6 - 5
   = 1 ms
```

### P3

```text
WT = 8 - 4
   = 4 ms
```

---

# 36. Final Table

| Process | AT | BT | CT | TAT | WT |
| ------- | -: | -: | -: | --: | -: |
| P2      |  0 |  3 |  3 |   3 |  0 |
| P1      |  2 |  5 |  8 |   6 |  1 |
| P3      |  4 |  4 | 12 |   8 |  4 |

### Average TAT

```text
= (3 + 6 + 8) / 3

= 17 / 3

= 5.67 ms
```

### Average WT

```text
= (0 + 1 + 4) / 3

= 5 / 3

= 1.67 ms
```

This is the same numerical result presented in the referenced FCFS material.

---

# 37. FCFS With CPU Idle Time

Students must understand that the CPU is not necessarily busy from time 0.

Consider:

| Process | AT | BT |
| ------- | -: | -: |
| P1      |  2 |  4 |
| P2      |  5 |  3 |
| P3      |  6 |  2 |

At:

```text
t = 0
```

No process has arrived.

Therefore:

```text
CPU = IDLE
```

P1 arrives at t = 2.

---

# 38. Gantt Chart With Idle Time

```text
       IDLE        P1          P2       P3
    ┌────────┬──────────┬─────────┬────────┐
    │        │          │         │        │
────┴────────┴──────────┴─────────┴────────┴──
    0        2          6         9       11
```

Important:

> **Never assume the CPU starts executing a process at time 0 unless a process is actually available at time 0.**

---

# 39. FCFS Problem-Solving Method

For exams, teach students this fixed process.

## Step 1 — Arrange According to Arrival Time

```text
AT ↑
```

Earliest arrival first.

---

## Step 2 — Draw the Gantt Chart

Determine exactly when each process starts and finishes.

---

## Step 3 — Find Completion Time

Read the right boundary of each process.

```text
CT = End Time of Process
```

---

## Step 4 — Calculate Turnaround Time

```text
TAT = CT - AT
```

---

## Step 5 — Calculate Waiting Time

```text
WT = TAT - BT
```

---

## Step 6 — Calculate Response Time

```text
RT = First Start Time - AT
```

---

## Step 7 — Calculate Averages

```text
Average WT
= Total WT / Number of Processes
```

```text
Average TAT
= Total TAT / Number of Processes
```

---

# 40. Response Time in FCFS

For standard non-preemptive FCFS problems:

```text
RT = First Start Time - AT
```

Since a process gets CPU once and then continues until completion:

```text
RT = WT
```

for the usual single-CPU-burst FCFS numerical problem.

### Example

P2:

```text
AT = 1
First Start = 5

RT = 5 - 1
   = 4 ms
```

Its waiting time is also:

```text
WT = 4 ms
```

Therefore:

```text
RT = WT
```

### Important

Do **not** memorize:

```text
RT = WT
```

as a universal CPU-scheduling formula.

The general formula is:

```text
RT = First Start Time - Arrival Time
```

The equality with waiting time is a consequence of the standard FCFS/non-preemptive setup.

---

# 41. Convoy Effect

One of the most important disadvantages of FCFS is the:

# Convoy Effect

## Definition

> **Convoy Effect occurs when a long CPU-bound process holds the CPU while many shorter processes wait behind it.**

Consider:

| Process |    BT |
| ------- | ----: |
| P1      | 20 ms |
| P2      |  2 ms |
| P3      |  3 ms |

All arrive at time 0.

FCFS executes:

```text
P1 → P2 → P3
```

Gantt Chart:

```text
        P1                         P2     P3
     ┌────────────────────┬──────────┬───────┐
     │                    │          │       │
─────┴────────────────────┴──────────┴───────┴──
     0                   20         22      25
```

P2 requires only 2 ms.

But P2 waits:

```text
20 ms
```

before starting.

P3 also waits:

```text
22 ms
```

---

# 42. Why is this Called a Convoy?

Think of a convoy:

```text
🚚 Long Vehicle
🚗 Short Vehicle
🚗 Short Vehicle
🚗 Short Vehicle
```

If the long vehicle moves slowly at the front, all the smaller vehicles behind it are forced to wait.

Similarly:

```text
Long CPU Process
       ↓
       P1
       ↓
Short Processes
P2  P3  P4
```

The short processes form a "convoy" behind the long process.

---

# 43. Why Convoy Effect is a Problem

It can cause:

* High waiting time
* Poor response time
* Poor user experience
* Inefficient execution of short jobs
* Poor performance when workloads contain both long and short processes

FCFS's long-process-before-short-process problem and convoy effect are specifically identified as disadvantages in the referenced FCFS material.

---

# 44. Advantages of FCFS

## 1. Simple

FCFS is one of the simplest CPU scheduling algorithms.

---

## 2. Easy to Implement

It can be implemented naturally using a queue.

```text
ENQUEUE → Process Arrival
DEQUEUE  → Process Execution
```

---

## 3. Fair in Arrival Order

Processes are served according to their arrival order.

No process is arbitrarily skipped in favor of a later-arriving process.

---

## 4. No Starvation

Under normal FCFS operation, a process will eventually get the CPU if the system continues making progress.

---

## 5. Low Scheduling Overhead

The scheduler does not need to repeatedly calculate which process has the shortest remaining time or highest priority.

---

## 6. Suitable for Batch-Type Workloads

FCFS can be reasonable for batch systems where immediate response is less important.

These advantages are consistent with the referenced FCFS discussion.

---

# 45. Disadvantages of FCFS

## 1. Convoy Effect

A long process can make many short processes wait.

---

## 2. High Average Waiting Time

FCFS can produce significantly larger waiting times than algorithms designed to favor shorter jobs.

---

## 3. Poor Response Time

A short or interactive process may have to wait behind a long process.

---

## 4. Not Suitable for Time-Sharing Systems

Interactive systems generally need processes to receive CPU time frequently rather than waiting behind a long process.

Round Robin is more suitable for this type of workload.

---

## 5. Order Matters

The same set of processes can produce very different waiting times depending on their arrival/order.

---

# 46. FCFS vs SJF — Introduction

At the end of the lecture, introduce the motivation for the next algorithm.

FCFS asks:

> **"Who arrived first?"**

SJF asks:

> **"Who has the shortest CPU burst?"**

Example:

```text
P1 = 20 ms
P2 = 2 ms
P3 = 3 ms
```

### FCFS

```text
P1 → P2 → P3
```

### SJF

```text
P2 → P3 → P1
```

This can drastically reduce the waiting time of shorter processes.

Therefore:

> **The weakness of FCFS motivates the Shortest Job First algorithm.**

---

# 47. FCFS Quick Revision

```text
FCFS
│
├── Full Form
│   └── First Come First Serve
│
├── Type
│   └── Non-Preemptive
│
├── Selection Criteria
│   └── Arrival Order
│
├── Data Structure
│   └── Queue
│
├── Preemption
│   └── No
│
├── Starvation
│   └── Normally No
│
├── Major Problem
│   └── Convoy Effect
│
└── Suitable For
    └── Simple / Batch-oriented workloads
```

---

# 48. One-Line Definitions for Exams

### CPU Scheduling

> CPU scheduling is the process of selecting a process from the Ready Queue and allocating the CPU to it.

### FCFS

> FCFS is a non-preemptive CPU scheduling algorithm in which processes are executed in the order of their arrival.

### Arrival Time

> The time at which a process enters the Ready Queue.

### Burst Time

> The CPU execution time required by a process.

### Completion Time

> The time at which a process completes its execution.

### Turnaround Time

> The total time between process arrival and process completion.

```text
TAT = CT - AT
```

### Waiting Time

> The time spent by a process waiting in the Ready Queue.

```text
WT = TAT - BT
```

### Response Time

> The time between process arrival and its first allocation of CPU.

```text
RT = First Start Time - AT
```

### Convoy Effect

> A condition in which a long-running process causes several short processes to wait behind it.

---

# 49. Important Viva Questions

### Q1. What is FCFS?

**Answer:** First Come First Serve.

### Q2. Is FCFS preemptive?

**Answer:** No. FCFS is non-preemptive.

### Q3. Why is FCFS non-preemptive?

**Answer:** Once a process gets the CPU, it is not forcibly removed until its CPU burst finishes or it blocks.

### Q4. What determines execution order in FCFS?

**Answer:** Arrival order.

### Q5. Which data structure naturally represents FCFS?

**Answer:** Queue.

### Q6. What is the major disadvantage of FCFS?

**Answer:** Convoy effect and potentially high waiting time.

### Q7. Does FCFS normally cause starvation?

**Answer:** No.

### Q8. What is the formula for TAT?

```text
TAT = CT - AT
```

### Q9. What is the formula for WT?

```text
WT = TAT - BT
```

### Q10. What is the formula for RT?

```text
RT = First Start Time - AT
```

### Q11. Can CPU be idle in FCFS?

**Answer:** Yes, if no process is available in the Ready Queue.

### Q12. What happens if a new process arrives while another process is executing?

**Answer:** It waits in the Ready Queue; it does not preempt the currently running process.

---

# 50. Practice Problem 1 — Basic

Given:

| Process | AT | BT |
| ------- | -: | -: |
| P1      |  0 |  6 |
| P2      |  0 |  4 |
| P3      |  0 |  2 |

Using FCFS, calculate:

1. Gantt Chart
2. CT
3. TAT
4. WT
5. RT
6. Average WT
7. Average TAT

---

# 51. Practice Problem 2 — Different Arrival Times

Given:

| Process | AT | BT |
| ------- | -: | -: |
| P1      |  0 |  7 |
| P2      |  2 |  4 |
| P3      |  4 |  3 |
| P4      |  5 |  2 |

Find:

* Gantt Chart
* CT
* TAT
* WT
* RT
* Average WT
* Average TAT

---

# 52. Practice Problem 3 — CPU Idle Time

Given:

| Process | AT | BT |
| ------- | -: | -: |
| P1      |  3 |  4 |
| P2      |  6 |  2 |
| P3      | 10 |  5 |

Find:

1. CPU idle period
2. Gantt Chart
3. CT
4. TAT
5. WT
6. RT
7. Average WT

---

# 53. Practice Problem 4 — Convoy Effect

Consider:

| Process | AT | BT |
| ------- | -: | -: |
| P1      |  0 | 15 |
| P2      |  1 |  2 |
| P3      |  2 |  1 |
| P4      |  3 |  3 |

Using FCFS:

1. Draw the Gantt Chart.
2. Calculate WT for every process.
3. Calculate average WT.
4. Identify the process causing the convoy effect.
5. Explain why P2 and P3 experience poor waiting time.

---

# 54. Practice Problem 5 — Conceptual

Answer:

### A.

Why is FCFS called non-preemptive?

### B.

Why does FCFS not consider burst time?

### C.

Why can FCFS have poor average waiting time?

### D.

What is the convoy effect?

### E.

Why is FCFS not ideal for interactive systems?

---

# 55. Classroom Summary

At the end of the lecture, students should remember:

```text
                 CPU SCHEDULING
                       │
                       ▼
             Who gets the CPU?
                       │
                       ▼
             Preemptive / Non-Preemptive
                       │
                       ▼
                  FCFS
                       │
                       ▼
             Arrival Order
                       │
                       ▼
             Non-Preemptive
                       │
                       ▼
               Gantt Chart
                       │
                       ▼
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
       TAT             WT             RT
        │              │              │
        ▼              ▼              ▼
   CT - AT         TAT - BT      First Start - AT
```

---

# 56. Final Takeaway

## FCFS in One Sentence

> **FCFS gives the CPU to processes in the order they arrive and does not forcibly interrupt a running process.**

### Remember:

```text
FCFS
 ↓
First Come
 ↓
First Serve
 ↓
Arrival Order
 ↓
Non-Preemptive
 ↓
Simple Queue
 ↓
Possible Convoy Effect
```

---

# 57. Next Lecture Connection — SJF

FCFS has a clear weakness:

```text
Long Process
     ↓
Blocks
     ↓
Short Processes
     ↓
High Waiting Time
```

So the next question is:

> **"Can we reduce waiting time by executing shorter processes first?"**

This leads to:

# SJF — Shortest Job First

```text
FCFS
"Who arrived first?"

        ↓

SJF
"Who has the shortest CPU burst?"
```

**Next Topic:** SJF — Non-Preemptive Shortest Job First
