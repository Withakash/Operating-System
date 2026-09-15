# Assignment: FCFS Scheduling — Numerical Problems

## Instructions

Solve all questions using the **First-Come, First-Served (FCFS)** CPU scheduling algorithm.

For each problem, calculate:

1. FCFS execution order
2. Gantt Chart
3. Completion Time (CT)
4. Turnaround Time (TAT)
5. Waiting Time (WT)
6. Average Waiting Time
7. Average Turnaround Time

### Formulas

- **TAT = CT − AT**
- **WT = TAT − BT**

> **Note:** FCFS is a **non-preemptive** scheduling algorithm. Always determine the execution order from the **Arrival Time (AT)**.

---

## Problem 1 — Basic Different Arrival Times

Consider the following processes:

| Process | Arrival Time (AT) | Burst Time (BT) |
|---|---:|---:|
| P1 | 0 | 5 |
| P2 | 1 | 3 |
| P3 | 2 | 2 |
| P4 | 4 | 4 |

**Tasks:**

- Determine the FCFS order.
- Draw the Gantt Chart.
- Calculate CT, TAT, and WT for every process.
- Calculate Average WT.
- Calculate Average TAT.

---

## Problem 2 — Arrival Order Is Different from Given Order

Consider:

| Process | Arrival Time (AT) | Burst Time (BT) |
|---|---:|---:|
| P1 | 3 | 4 |
| P2 | 0 | 5 |
| P3 | 2 | 3 |
| P4 | 5 | 2 |

**Tasks:**

- Determine the actual FCFS order using AT.
- Draw the Gantt Chart.
- Calculate CT, TAT, and WT.
- Calculate Average WT.
- Calculate Average TAT.

---

## Problem 3 — CPU Idle Time

Consider:

| Process | Arrival Time (AT) | Burst Time (BT) |
|---|---:|---:|
| P1 | 2 | 4 |
| P2 | 5 | 3 |
| P3 | 7 | 2 |
| P4 | 8 | 4 |

**Tasks:**

- Determine the FCFS order.
- Identify any CPU idle period.
- Draw the complete Gantt Chart including idle time.
- Calculate CT, TAT, and WT.
- Calculate Average WT.
- Calculate Average TAT.

---

## Problem 4 — More Processes

Consider:

| Process | Arrival Time (AT) | Burst Time (BT) |
|---|---:|---:|
| P1 | 0 | 7 |
| P2 | 2 | 4 |
| P3 | 4 | 3 |
| P4 | 5 | 2 |
| P5 | 6 | 5 |

**Tasks:**

- Determine the FCFS execution order.
- Draw the Gantt Chart.
- Calculate CT, TAT, and WT for all processes.
- Calculate Average WT.
- Calculate Average TAT.

---

## Problem 5 — Challenge Problem

Consider:

| Process | Arrival Time (AT) | Burst Time (BT) |
|---|---:|---:|
| P1 | 4 | 5 |
| P2 | 0 | 3 |
| P3 | 2 | 6 |
| P4 | 6 | 2 |
| P5 | 7 | 4 |
| P6 | 9 | 3 |

**Tasks:**

- Determine the correct FCFS execution order.
- Draw the Gantt Chart.
- Calculate CT for every process.
- Calculate TAT for every process.
- Calculate WT for every process.
- Calculate Average WT.
- Calculate Average TAT.

> **Challenge:** Do not arrange the processes according to the order in which they are listed. Carefully consider their Arrival Times.

---

## Submission Format

For every problem, present your answer in this format:

### FCFS Order

`P__ → P__ → P__ → ...`

### Gantt Chart

```text
|     |     |     |     |
0     __    __    __    __
```

### Calculation Table

| Process | AT | BT | CT | TAT | WT |
|---|---:|---:|---:|---:|---:|
| P1 | | | | | |
| P2 | | | | | |
| P3 | | | | | |
| P4 | | | | | |

### Final Result

- Average Waiting Time = ______
- Average Turnaround Time = ______
