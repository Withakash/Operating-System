# Banker's Algorithm --- Complete Student Notes & Assignment

> **Subject:** Operating Systems\
> **Topic:** Deadlock Avoidance --- Banker's Algorithm\
> **Focus:** How to solve numerical problems step-by-step

------------------------------------------------------------------------

# 1. What is Banker's Algorithm?

The **Banker's Algorithm** is a **deadlock avoidance algorithm** used by
an operating system.

Its main purpose is to determine whether the system can allocate
resources to processes while keeping the system in a **safe state**.

The basic idea is:

> **Before allowing a resource allocation, check whether the system can
> still finish all processes in some order.**

Think of the OS as a banker and processes as customers requesting loans.

The banker does not give resources in a way that could leave everyone
waiting forever.

------------------------------------------------------------------------

# 2. Important Terms

## 2.1 Allocation

`Allocation` tells us how many resources are currently allocated to each
process.

Example:

``` text
P0 = (2, 1, 0)
```

This means P0 currently has:

-   2 units of A
-   1 unit of B
-   0 units of C

------------------------------------------------------------------------

## 2.2 Max

`Max` tells us the maximum total resources a process may need during its
execution.

Example:

``` text
P0 = (5, 3, 2)
```

This means P0 may need at most:

-   5 units of A
-   3 units of B
-   2 units of C

------------------------------------------------------------------------

## 2.3 Need

`Need` tells us how many additional resources a process still requires
to reach its maximum.

The most important formula is:

``` text
Need = Max - Allocation
```

Example:

``` text
Max        = (5, 3, 2)
Allocation = (2, 1, 1)
-----------------------
Need       = (3, 2, 1)
```

Therefore:

``` text
Need = (3, 2, 1)
```

------------------------------------------------------------------------

## 2.4 Available

`Available` tells us how many resources are currently free and can
potentially be allocated.

Example:

``` text
Available = (3, 2, 1)
```

------------------------------------------------------------------------

## 2.5 Work

During the safety algorithm, we use a temporary vector called `Work`.

Initially:

``` text
Work = Available
```

When a process finishes, its currently allocated resources are released:

``` text
Work = Work + Allocation
```

> **Important:** We add `Allocation`, NOT `Need`.

------------------------------------------------------------------------

# 3. The Most Important Formula

Memorize this:

\[ `\boxed{Need = Max - Allocation}`{=tex} \]

For every process and every resource:

``` text
Need[A] = Max[A] - Allocation[A]

Need[B] = Max[B] - Allocation[B]

Need[C] = Max[C] - Allocation[C]

Need[D] = Max[D] - Allocation[D]
```

------------------------------------------------------------------------

# 4. Safety Condition

A process can finish if:

\[ `\boxed{Need_i \leq Work}`{=tex} \]

This comparison must be done **for every resource**.

For example:

``` text
Need P0 = (2, 1, 0, 3)
Work     = (3, 2, 1, 3)
```

Check:

``` text
A: 2 <= 3 ✓
B: 1 <= 2 ✓
C: 0 <= 1 ✓
D: 3 <= 3 ✓
```

Therefore:

``` text
P0 CAN FINISH
```

------------------------------------------------------------------------

# 5. Example Where a Process Cannot Finish

Suppose:

``` text
Need P0 = (2, 1, 0, 3)
Work     = (3, 2, 1, 2)
```

Check:

``` text
A: 2 <= 3 ✓
B: 1 <= 2 ✓
C: 0 <= 1 ✓
D: 3 <= 2 ✗
```

Therefore:

``` text
P0 CANNOT FINISH
```

Even though A, B and C are sufficient, D is not sufficient.

> **All resources must satisfy Need \<= Work.**

------------------------------------------------------------------------

# 6. Complete Steps to Solve Banker's Algorithm

Use these steps in every numerical.

## Step 1 --- Calculate Need

For every process:

``` text
Need = Max - Allocation
```

Create a Need table.

------------------------------------------------------------------------

## Step 2 --- Set Work

Initially:

``` text
Work = Available
```

------------------------------------------------------------------------

## Step 3 --- Find a Process

Find any unfinished process satisfying:

``` text
Need <= Work
```

Compare every resource.

------------------------------------------------------------------------

## Step 4 --- Assume the Process Completes

If a process can finish, assume it completes and releases its allocated
resources.

Update:

``` text
Work = Work + Allocation
```

------------------------------------------------------------------------

## Step 5 --- Add Process to Safe Sequence

For example:

``` text
Safe Sequence:

P1
```

Then continue checking the remaining processes.

------------------------------------------------------------------------

## Step 6 --- Repeat

Continue until:

### Case 1 --- All processes finish

``` text
SAFE STATE
```

Example:

``` text
P1 -> P3 -> P0 -> P2 -> P4
```

### Case 2 --- No remaining process can finish

``` text
UNSAFE STATE
```

------------------------------------------------------------------------

# 7. Standard Exam Table

A clean way to solve the numerical is:

  Process   Allocation   Max   Need
  --------- ------------ ----- ------
  P0        ...          ...   ...
  P1        ...          ...   ...
  P2        ...          ...   ...
  P3        ...          ...   ...

Then:

``` text
Initial Work = Available
```

After each successful process:

``` text
Process completed:
Work = Work + Allocation
```

Finally:

``` text
Safe Sequence = P? -> P? -> P? -> ...
```

------------------------------------------------------------------------

# 8. Complete Mini Example

Consider:

  Process   Allocation A B C   Max A B C
  --------- ------------------ -----------
  P0        1 0 0              2 1 1
  P1        0 1 0              1 2 1
  P2        1 1 1              2 2 1

Available:

``` text
(1, 1, 1)
```

## Step 1 --- Need

### P0

``` text
Max        = (2,1,1)
Allocation = (1,0,0)

Need       = (1,1,1)
```

### P1

``` text
Max        = (1,2,1)
Allocation = (0,1,0)

Need       = (1,1,1)
```

### P2

``` text
Max        = (2,2,1)
Allocation = (1,1,1)

Need       = (1,1,0)
```

Need table:

  Process     Need A   Need B   Need C
  --------- -------- -------- --------
  P0               1        1        1
  P1               1        1        1
  P2               1        1        0

------------------------------------------------------------------------

## Step 2 --- Initial Work

``` text
Work = (1,1,1)
```

P0:

``` text
Need = (1,1,1)
Work = (1,1,1)

P0 can finish.
```

Update:

``` text
Work = Work + Allocation(P0)

     = (1,1,1) + (1,0,0)

     = (2,1,1)
```

Safe sequence:

``` text
P0
```

------------------------------------------------------------------------

## Step 3

Now:

``` text
Work = (2,1,1)
```

P1:

``` text
Need = (1,1,1)
```

P1 can finish.

``` text
Work = (2,1,1) + (0,1,0)

     = (2,2,1)
```

Safe sequence:

``` text
P0 -> P1
```

------------------------------------------------------------------------

## Step 4

Now P2:

``` text
Need = (1,1,0)
Work = (2,2,1)
```

P2 can finish.

``` text
Work = (2,2,1) + (1,1,1)

     = (3,3,2)
```

All processes finished.

Therefore:

``` text
SAFE STATE
```

One safe sequence is:

``` text
P0 -> P1 -> P2
```

------------------------------------------------------------------------

# 9. Very Important: Safe Sequence Is Not Always Unique

Suppose two processes satisfy:

``` text
Need <= Work
```

at the same time.

You can select either one.

For example:

``` text
P1 -> P3 -> P0 -> P2
```

and

``` text
P3 -> P1 -> P0 -> P2
```

may both be safe sequences.

Therefore:

> Banker's Algorithm may have multiple valid safe sequences.

You only need to provide **one valid safe sequence** unless the question
specifically asks for all possible sequences.

------------------------------------------------------------------------

# 10. Common Mistakes

## Mistake 1 --- Using Max instead of Need

Wrong:

``` text
Compare Max with Available
```

Correct:

``` text
Compare Need with Work
```

------------------------------------------------------------------------

## Mistake 2 --- Adding Need after a process finishes

Wrong:

``` text
Work = Work + Need
```

Correct:

``` text
Work = Work + Allocation
```

------------------------------------------------------------------------

## Mistake 3 --- Checking only one resource

Wrong:

``` text
A fits, therefore process can finish.
```

Correct:

``` text
A, B, C, D must ALL satisfy:

Need <= Work
```

------------------------------------------------------------------------

## Mistake 4 --- Forgetting to update Work

After every completed process:

``` text
Work = Work + Allocation
```

must be performed.

------------------------------------------------------------------------

## Mistake 5 --- Calling a State Unsafe Too Early

Suppose:

``` text
P0 cannot finish
P1 can finish
P2 cannot finish
P3 can finish
```

You should not immediately say unsafe.

First complete P1 or P3, update Work, and check again.

------------------------------------------------------------------------

# 11. Quick Exam Shortcut

Write this on the side of your answer sheet:

``` text
1. Need = Max - Allocation

2. Work = Available

3. Find:
      Need <= Work

4. If yes:
      Work = Work + Allocation

5. Add process to Safe Sequence

6. Repeat

7. All finish = SAFE
   Stuck = UNSAFE
```

------------------------------------------------------------------------

# 12. How to Identify an Unsafe State

Suppose after some processes finish:

``` text
Work = (5, 11, 4, 2)
```

Remaining:

``` text
P0 Need = (2,1,0,3)
P4 Need = (2,1,1,3)
```

Both need:

``` text
D = 3
```

But:

``` text
Work D = 2
```

Therefore:

``` text
P0 cannot finish
P4 cannot finish
```

No process can proceed.

Therefore:

``` text
UNSAFE STATE
```

------------------------------------------------------------------------

# Banker’s Algorithm — Complete Notes & Assignment

> **Subject:** Operating Systems  
> **Topic:** Deadlock Avoidance — Banker’s Algorithm  
> **Level:** Student Notes + Numerical Practice

---

# 1. What is Banker’s Algorithm?

The **Banker's Algorithm** is a **deadlock avoidance algorithm** used by an operating system.

Its purpose is to determine whether a system can allocate resources to processes in such a way that the system remains in a **safe state**.

The main idea is:

> Before allowing resource allocation, check whether the system can still complete all processes in some possible order.

The algorithm tries to avoid entering a situation where processes may wait forever for resources.

---

# 2. Why is it called Banker’s Algorithm?

Imagine a bank that has a limited amount of money.

Several customers request loans.

The bank does not give money blindly. Before giving a loan, it checks:

> "After giving this money, will I still be able to satisfy all customers eventually?"

Similarly, the operating system checks:

> "If resources are allocated, can all processes still complete?"

If yes:

```text
SAFE STATE
```

If no:

```text
UNSAFE STATE
```

---

# 3. Important Terms

Banker's Algorithm uses the following important terms:

1. Allocation
2. Max
3. Need
4. Available
5. Work
6. Safe Sequence

---

# 4. Allocation

`Allocation` tells us how many resources are currently allocated to each process.

Example:

```text
P0 = (2, 1, 0)
```

This means P0 currently has:

```text
A = 2
B = 1
C = 0
```

---

# 5. Max

`Max` tells us the maximum number of resources that a process may need during its execution.

Example:

```text
P0 = (5, 3, 2)
```

This means P0 may need at most:

```text
A = 5
B = 3
C = 2
```

---

# 6. Need

`Need` tells us how many additional resources a process still requires to complete.

The most important formula is:

```text
Need = Max - Allocation
```

For example:

```text
Max        = (5, 3, 2)
Allocation = (2, 1, 1)

Need       = (3, 2, 1)
```

Therefore:

```text
Need = (3, 2, 1)
```

---

# 7. Available

`Available` tells us how many resources are currently free.

Example:

```text
Available = (3, 2, 1)
```

This means the system currently has:

```text
A = 3 free resources
B = 2 free resources
C = 1 free resource
```

---

# 8. Work

`Work` is a temporary vector used while checking whether the system is safe.

Initially:

```text
Work = Available
```

When a process finishes, it releases its allocated resources.

Therefore:

```text
Work = Work + Allocation
```

## Important

Do **NOT** do:

```text
Work = Work + Need
```

Correct:

```text
Work = Work + Allocation
```

---

# 9. Safe State

A system is in a **safe state** if there is at least one order in which all processes can complete successfully.

For example:

```text
P1 -> P3 -> P0 -> P2 -> P4
```

If all processes can complete in this order, the system is safe.

---

# 10. Unsafe State

A system is in an **unsafe state** if the safety algorithm gets stuck before all processes can complete.

For example:

```text
P1 -> P3
```

After P1 and P3 finish, suppose no remaining process satisfies:

```text
Need <= Work
```

Then the state is unsafe.

---

# 11. Safe Sequence

A **safe sequence** is an order of processes in which every process can obtain its remaining resources, complete, and release its allocated resources.

Example:

```text
Safe Sequence:

P1 -> P3 -> P0 -> P2 -> P4
```

The exact safe sequence does not always have to be unique.

There can be multiple safe sequences.

---

# 12. Most Important Formula

Always remember:

```text
Need = Max - Allocation
```

For every resource:

```text
Need[A] = Max[A] - Allocation[A]

Need[B] = Max[B] - Allocation[B]

Need[C] = Max[C] - Allocation[C]

Need[D] = Max[D] - Allocation[D]
```

---

# 13. Main Condition

A process can complete if:

```text
Need <= Work
```

The comparison must be done for **all resources**.

For example:

```text
Need P0 = (2, 1, 0, 3)
Work     = (3, 2, 1, 3)
```

Check:

```text
A: 2 <= 3 ✓
B: 1 <= 2 ✓
C: 0 <= 1 ✓
D: 3 <= 3 ✓
```

Therefore:

```text
P0 CAN FINISH
```

---

# 14. Example Where a Process Cannot Finish

Suppose:

```text
Need P0 = (2, 1, 0, 3)
Work     = (3, 2, 1, 2)
```

Check:

```text
A: 2 <= 3 ✓
B: 1 <= 2 ✓
C: 0 <= 1 ✓
D: 3 <= 2 ✗
```

Therefore:

```text
P0 CANNOT FINISH
```

Even though A, B and C are sufficient, D is not sufficient.

> All resources must satisfy `Need <= Work`.

---

# 15. Complete Steps to Solve Banker’s Algorithm

Use these steps for every numerical problem.

---

## Step 1 — Calculate Need Matrix

For every process:

```text
Need = Max - Allocation
```

Create the Need table.

---

## Step 2 — Set Work

Initially:

```text
Work = Available
```

---

## Step 3 — Find a Process

Find any unfinished process for which:

```text
Need <= Work
```

Compare every resource.

---

## Step 4 — Assume the Process Completes

If a process can complete, assume it finishes.

It releases its currently allocated resources.

Update:

```text
Work = Work + Allocation
```

---

## Step 5 — Add the Process to Safe Sequence

For example:

```text
Safe Sequence:

P1
```

---

## Step 6 — Repeat

Use the new Work value to check the remaining processes.

Continue until one of the following happens.

### Case 1 — All processes finish

```text
SAFE STATE
```

### Case 2 — No remaining process can finish

```text
UNSAFE STATE
```

---

# 16. Standard Solving Format

For an exam question, use this structure.

## Step 1 — Need Matrix

| Process | A | B | C | D |
|---|---:|---:|---:|---:|
| P0 |  |  |  |  |
| P1 |  |  |  |  |
| P2 |  |  |  |  |
| P3 |  |  |  |  |
| P4 |  |  |  |  |

---

## Step 2 — Initial Work

```text
Work = Available
```

Example:

```text
Work = (1, 2, 1, 2)
```

---

## Step 3 — Check Processes

Example:

```text
P0:

Need = (2, 1, 0, 3)
Work = (1, 2, 1, 2)

Need <= Work ?

A: 2 <= 1 ✗
```

Therefore:

```text
P0 cannot finish.
```

Check the next process.

---

## Step 4 — Process Completes

Suppose P1 can finish.

```text
Need P1       = (1, 0, 1, 1)
Work          = (1, 2, 1, 2)
```

Therefore:

```text
P1 can finish.
```

Now release P1's allocation.

```text
Work = Work + Allocation(P1)
```

---

## Step 5 — Update Work

Example:

```text
Old Work          = (1, 2, 1, 2)
Allocation(P1)    = (1, 1, 0, 1)
--------------------------------
New Work          = (2, 3, 1, 3)
```

---

## Step 6 — Write Safe Sequence

```text
Safe Sequence:

P1
```

Continue with the remaining processes.

---

# 17. Complete Mini Example

Consider:

| Process | Allocation A | Allocation B | Allocation C | Max A | Max B | Max C |
|---|---:|---:|---:|---:|---:|---:|
| P0 | 1 | 0 | 0 | 2 | 1 | 1 |
| P1 | 0 | 1 | 0 | 1 | 2 | 1 |
| P2 | 1 | 1 | 1 | 2 | 2 | 1 |

Available:

```text
(1, 1, 1)
```

---

## Step 1 — Calculate Need

### P0

```text
Max        = (2, 1, 1)
Allocation = (1, 0, 0)

Need       = (1, 1, 1)
```

### P1

```text
Max        = (1, 2, 1)
Allocation = (0, 1, 0)

Need       = (1, 1, 1)
```

### P2

```text
Max        = (2, 2, 1)
Allocation = (1, 1, 1)

Need       = (1, 1, 0)
```

Need matrix:

| Process | A | B | C |
|---|---:|---:|---:|
| P0 | 1 | 1 | 1 |
| P1 | 1 | 1 | 1 |
| P2 | 1 | 1 | 0 |

---

## Step 2 — Initial Work

```text
Work = Available

Work = (1, 1, 1)
```

---

## Step 3 — Check P0

```text
Need P0 = (1, 1, 1)
Work    = (1, 1, 1)
```

Therefore:

```text
P0 can finish.
```

Release P0's allocation:

```text
Work = Work + Allocation(P0)

     = (1, 1, 1)
     + (1, 0, 0)

     = (2, 1, 1)
```

Safe sequence:

```text
P0
```

---

## Step 4 — Check P1

```text
Need P1 = (1, 1, 1)
Work    = (2, 1, 1)
```

P1 can finish.

```text
Work = (2, 1, 1)
     + (0, 1, 0)

     = (2, 2, 1)
```

Safe sequence:

```text
P0 -> P1
```

---

## Step 5 — Check P2

```text
Need P2 = (1, 1, 0)
Work    = (2, 2, 1)
```

P2 can finish.

```text
Work = (2, 2, 1)
     + (1, 1, 1)

     = (3, 3, 2)
```

All processes completed.

Therefore:

```text
SAFE STATE
```

One safe sequence is:

```text
P0 -> P1 -> P2
```

---

# 18. Important: Safe Sequence May Not Be Unique

Suppose:

```text
P1 can finish
P2 can also finish
```

at the same time.

We may choose either one.

Therefore, a problem can have multiple safe sequences.

Example:

```text
P1 -> P3 -> P0 -> P2
```

and:

```text
P3 -> P1 -> P0 -> P2
```

may both be valid safe sequences.

Unless the question specifically asks for **all possible safe sequences**, finding **one valid safe sequence** is normally sufficient.

---

# 19. Common Mistakes

## Mistake 1 — Comparing Max with Work

Wrong:

```text
Max <= Work
```

Correct:

```text
Need <= Work
```

---

## Mistake 2 — Adding Need to Work

Wrong:

```text
Work = Work + Need
```

Correct:

```text
Work = Work + Allocation
```

---

## Mistake 3 — Checking Only One Resource

Wrong:

```text
A is sufficient, therefore process can finish.
```

Correct:

```text
A, B, C and D must ALL satisfy:

Need <= Work
```

---

## Mistake 4 — Forgetting to Update Work

After every completed process:

```text
Work = Work + Allocation
```

must be performed.

---

## Mistake 5 — Declaring Unsafe Too Early

Suppose:

```text
P0 cannot finish
P1 can finish
P2 cannot finish
P3 can finish
```

Do not immediately say:

```text
UNSAFE
```

First complete P1 or P3, update Work, and check again.

---

# 20. How to Identify an Unsafe State

Suppose after some processes finish:

```text
Work = (5, 11, 4, 2)
```

Remaining processes:

```text
P0 Need = (2, 1, 0, 3)
P4 Need = (2, 1, 1, 3)
```

Both processes need:

```text
D = 3
```

But:

```text
Work D = 2
```

Therefore:

```text
P0 cannot finish.
P4 cannot finish.
```

No remaining process can finish.

Therefore:

```text
UNSAFE STATE
```

---

# 21. Quick Exam Shortcut

Write this on the side of your answer sheet:

```text
1. Need = Max - Allocation

2. Work = Available

3. Find:
      Need <= Work

4. If YES:
      Work = Work + Allocation

5. Add process to Safe Sequence

6. Repeat

7. All processes finish = SAFE

8. No process can finish = UNSAFE
```

---

# 22. Classroom Solving Practice — 1

> **Solve this in class with the instructor.**

Consider:

| Process | Allocation A | Allocation B | Allocation C | Max A | Max B | Max C |
|---|---:|---:|---:|---:|---:|---:|
| P0 | 1 | 0 | 1 | 2 | 1 | 2 |
| P1 | 1 | 1 | 0 | 2 | 2 | 1 |
| P2 | 1 | 0 | 0 | 1 | 2 | 1 |

Available:

```text
(1, 1, 1)
```

### Solve:

1. Calculate Need.
2. Find the initial Work.
3. Check which processes can finish.
4. Update Work.
5. Find one safe sequence.
6. Determine whether the state is safe.

### Working Space

```text
Need Matrix:

| Process | A | B | C |
|---|---:|---:|---:|
| P0 |   |   |   |
| P1 |   |   |   |
| P2 |   |   |   |

Initial Work:

Work = ( , , )

Safe Sequence:

P__ -> P__ -> P__
```

---

# 23. Classroom Solving Practice — 2

> **Solve this in class. Try without looking at previous examples.**

| Process | Allocation A | Allocation B | Allocation C | Allocation D | Max A | Max B | Max C | Max D |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| P0 | 1 | 0 | 1 | 1 | 3 | 1 | 2 | 2 |
| P1 | 1 | 1 | 0 | 0 | 2 | 2 | 1 | 1 |
| P2 | 0 | 1 | 1 | 0 | 1 | 2 | 2 | 1 |
| P3 | 1 | 0 | 0 | 1 | 2 | 1 | 1 | 2 |

Available:

```text
(1, 1, 1, 1)
```

### Solve:

1. Calculate Need matrix.
2. Set Work = Available.
3. Check every process.
4. Select a process that satisfies Need <= Work.
5. Release its Allocation.
6. Update Work.
7. Continue until all processes finish or the algorithm gets stuck.
8. Write the safe sequence.
9. State whether the system is safe or unsafe.

### Working Table

| Step | Process Completed | Work Before | Allocation Released | Work After |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |

---

# 24. Classroom Solving Practice — 3

> **Challenge Question**

Consider:

| Process | Allocation A | Allocation B | Allocation C | Allocation D | Max A | Max B | Max C | Max D |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| P0 | 2 | 0 | 1 | 1 | 4 | 2 | 2 | 3 |
| P1 | 1 | 1 | 0 | 0 | 2 | 2 | 1 | 1 |
| P2 | 1 | 0 | 1 | 0 | 3 | 1 | 2 | 1 |
| P3 | 0 | 1 | 1 | 1 | 1 | 2 | 2 | 2 |
| P4 | 1 | 1 | 0 | 1 | 2 | 2 | 1 | 2 |

Available:

```text
(1, 1, 1, 1)
```

### Tasks

1. Calculate the Need matrix.
2. Write the initial Work.
3. Check P0.
4. Check P1.
5. Check P2.
6. Check P3.
7. Check P4.
8. Select a process that can finish.
9. Update Work.
10. Continue until all processes finish or the system becomes stuck.
11. Write the safe sequence.
12. Determine SAFE or UNSAFE.

### Important

Do not simply check the first process and stop.

You must continue until:

```text
All processes finish
```

or:

```text
No remaining process can finish
```

---

# 25. Assignment

> **Instructions:** For every question:
>
> 1. Calculate the Need matrix.
> 2. Write the initial Work/Available vector.
> 3. Compare Need with Work.
> 4. Update Work after every completed process.
> 5. Write the safe sequence if one exists.
> 6. Clearly explain why the state is unsafe if the algorithm gets stuck.

---

# Assignment 1 — Basic 3 Resources

| Process | Allocation A | Allocation B | Allocation C | Max A | Max B | Max C |
|---|---:|---:|---:|---:|---:|---:|
| P0 | 1 | 0 | 0 | 2 | 1 | 1 |
| P1 | 0 | 1 | 0 | 1 | 2 | 1 |
| P2 | 1 | 1 | 1 | 2 | 2 | 1 |

Available:

```text
(1, 1, 1)
```

### Find:

1. Need matrix
2. Whether the system is safe
3. One safe sequence

---

# Assignment 2 — Find Safe Sequence

| Process | Allocation A | Allocation B | Allocation C | Max A | Max B | Max C |
|---|---:|---:|---:|---:|---:|---:|
| P0 | 1 | 1 | 0 | 2 | 2 | 1 |
| P1 | 1 | 0 | 1 | 2 | 1 | 2 |
| P2 | 1 | 2 | 1 | 3 | 3 | 2 |
| P3 | 0 | 1 | 1 | 1 | 2 | 2 |

Available:

```text
(1, 1, 1)
```

### Find:

- Need matrix
- Safe or unsafe
- Safe sequence

---

# Assignment 3 — Four Resources

| Process | Allocation A | Allocation B | Allocation C | Allocation D | Max A | Max B | Max C | Max D |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| P0 | 1 | 0 | 2 | 1 | 3 | 2 | 2 | 2 |
| P1 | 1 | 1 | 0 | 0 | 2 | 2 | 1 | 1 |
| P2 | 1 | 0 | 1 | 1 | 1 | 2 | 2 | 2 |
| P3 | 0 | 1 | 1 | 0 | 2 | 2 | 2 | 1 |

Available:

```text
(1, 1, 1, 1)
```

Determine whether the state is safe.

---

# Assignment 4 — Unsafe State

| Process | Allocation A | Allocation B | Allocation C | Max A | Max B | Max C |
|---|---:|---:|---:|---:|---:|---:|
| P0 | 1 | 0 | 1 | 2 | 1 | 2 |
| P1 | 0 | 1 | 0 | 1 | 2 | 1 |
| P2 | 1 | 1 | 0 | 2 | 2 | 1 |

Available:

```text
(0, 0, 0)
```

### Find:

1. Need matrix
2. Can any process execute?
3. Is the system safe or unsafe?
4. Explain why.

---

# Assignment 5 — Five Processes

| Process | Allocation A | Allocation B | Allocation C | Allocation D | Max A | Max B | Max C | Max D |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| P0 | 0 | 1 | 0 | 2 | 1 | 2 | 1 | 3 |
| P1 | 2 | 0 | 1 | 0 | 3 | 2 | 1 | 1 |
| P2 | 1 | 1 | 1 | 1 | 2 | 2 | 2 | 2 |
| P3 | 1 | 0 | 0 | 1 | 3 | 1 | 1 | 2 |
| P4 | 0 | 1 | 1 | 0 | 1 | 2 | 2 | 1 |

Available:

```text
(1, 1, 1, 1)
```

### Determine:

- Need matrix
- Safe or unsafe
- Safe sequence

---

# Assignment 6 — Multiple Possible Safe Sequences

| Process | Allocation A | Allocation B | Allocation C | Max A | Max B | Max C |
|---|---:|---:|---:|---:|---:|---:|
| P0 | 1 | 0 | 0 | 2 | 1 | 1 |
| P1 | 0 | 1 | 0 | 1 | 2 | 1 |
| P2 | 0 | 0 | 1 | 1 | 1 | 2 |
| P3 | 1 | 1 | 0 | 2 | 2 | 1 |

Available:

```text
(1, 1, 1)
```

### Find:

1. Need matrix
2. All possible safe sequences, if possible
3. Is there only one safe sequence?

---

# Assignment 7 — Exam-Level 5 × 4

| Process | Allocation A | Allocation B | Allocation C | Allocation D | Max A | Max B | Max C | Max D |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| P0 | 3 | 0 | 1 | 1 | 5 | 1 | 1 | 3 |
| P1 | 1 | 2 | 0 | 0 | 2 | 3 | 2 | 1 |
| P2 | 1 | 0 | 2 | 1 | 1 | 2 | 2 | 2 |
| P3 | 0 | 1 | 1 | 0 | 1 | 2 | 1 | 1 |
| P4 | 1 | 1 | 0 | 1 | 2 | 2 | 1 | 2 |

Available:

```text
(1, 1, 1, 1)
```

### Solve completely.

---

# Assignment 8 — Two Different Available States

Use the same Allocation and Max matrices:

| Process | Allocation A | Allocation B | Allocation C | Max A | Max B | Max C |
|---|---:|---:|---:|---:|---:|---:|
| P0 | 1 | 0 | 0 | 2 | 1 | 1 |
| P1 | 0 | 1 | 0 | 1 | 2 | 1 |
| P2 | 1 | 1 | 1 | 2 | 2 | 1 |
| P3 | 0 | 0 | 1 | 1 | 1 | 2 |

## (a)

```text
Available = (1, 1, 0)
```

## (b)

```text
Available = (0, 0, 1)
```

Determine whether each state is safe or unsafe.

---

# Assignment 9 — Find Where the Algorithm Gets Stuck

| Process | Allocation A | Allocation B | Allocation C | Max A | Max B | Max C |
|---|---:|---:|---:|---:|---:|---:|
| P0 | 2 | 0 | 0 | 3 | 1 | 1 |
| P1 | 0 | 1 | 1 | 1 | 2 | 2 |
| P2 | 1 | 0 | 1 | 2 | 1 | 2 |
| P3 | 0 | 1 | 0 | 1 | 2 | 1 |

Available:

```text
(0, 1, 1)
```

Show:

```text
Initial Work = ?

P0 -> Can/Cannot?
P1 -> Can/Cannot?
P2 -> Can/Cannot?
P3 -> Can/Cannot?

If one completes:

New Work = ?

Continue...
```

Do not simply write SAFE/UNSAFE.

Show the complete process.

---

# Assignment 10 — University Exam Style

| Process | Allocation | Max |
|---|---|---|
| P0 | (0, 1, 0, 2) | (1, 2, 1, 3) |
| P1 | (2, 0, 0, 0) | (3, 2, 1, 1) |
| P2 | (1, 1, 1, 1) | (2, 2, 2, 2) |
| P3 | (1, 0, 1, 0) | (1, 1, 2, 1) |
| P4 | (0, 0, 2, 0) | (1, 1, 3, 1) |

Available:

```text
(1, 1, 1, 1)
```

### Questions

**a)** Calculate the Need matrix.

**b)** Apply Banker's Safety Algorithm.

**c)** Determine whether the system is safe.

**d)** If safe, give a safe sequence.

**e)** Show Work and Allocation after each process completes.

---

# 23. Recommended Answer Format for Students

For every numerical question, use this structure.

## Step 1 — Need Matrix

| Process | A | B | C | D |
|---|---:|---:|---:|---:|
| P0 |  |  |  |  |
| P1 |  |  |  |  |
| P2 |  |  |  |  |
| P3 |  |  |  |  |
| P4 |  |  |  |  |

---

## Step 2 — Initial Work

```text
Work = Available

Work = ( , , , )
```

---

## Step 3 — Process Selection

Write the comparison:

```text
P0:

Need = ( , , , )
Work = ( , , , )

Need <= Work ?
YES / NO
```

---

## Step 4 — Update Work

If P0 completes:

```text
New Work = Old Work + Allocation(P0)
```

Example:

```text
Old Work       = (2, 3, 1, 2)
Allocation P0  = (1, 0, 1, 1)
--------------------------------
New Work       = (3, 3, 2, 3)
```

---

## Step 5 — Safe Sequence

```text
Safe Sequence:

P__ -> P__ -> P__ -> P__ -> P__
```

---

## Step 6 — Final Result

If all processes finish:

```text
All processes completed.

Therefore:

SAFE STATE
```

If the algorithm gets stuck:

```text
No remaining process can satisfy:

Need <= Work

Therefore:

UNSAFE STATE
```

---

# 24. Work Table Format

For difficult questions, students should use a Work table.

| Step | Process Completed | Work Before | Allocation Released | Work After |
|---|---|---|---|---|
| 1 | P__ | ( , , , ) | ( , , , ) | ( , , , ) |
| 2 | P__ | ( , , , ) | ( , , , ) | ( , , , ) |
| 3 | P__ | ( , , , ) | ( , , , ) | ( , , , ) |
| 4 | P__ | ( , , , ) | ( , , , ) | ( , , , ) |
| 5 | P__ | ( , , , ) | ( , , , ) | ( , , , ) |

---

# 25. Final Cheat Sheet

```text
                BANKER'S ALGORITHM

                       START
                         |
                         v
              Calculate Need Matrix
                         |
                         v
             Need = Max - Allocation
                         |
                         v
                  Work = Available
                         |
                         v
             Find unfinished process
                  Need <= Work
                         |
              +----------+----------+
              |                     |
             YES                    NO
              |                     |
              v                     v
        Process finishes       Are all processes
              |                   finished?
              v                     |
       Work = Work +               +-------+
          Allocation               |       |
              |                   YES      NO
              v                    |       |
       Add to safe sequence        v       v
              |                  SAFE   UNSAFE
              |
              v
           Repeat
```

---

# 26. Three Formulas to Memorize

## Formula 1 — Need

```text
Need = Max - Allocation
```

## Formula 2 — Initial Work

```text
Work = Available
```

## Formula 3 — Work After Process Completion

```text
Work_new = Work_old + Allocation
```

---

# 27. One-Line Memory Trick

Remember:

```text
MAX
 ↓
How much can the process need?

ALLOCATION
 ↓
How much does the process currently have?

NEED
 ↓
How much more does the process need?

AVAILABLE
 ↓
How much is currently free?

WORK
 ↓
How much can we currently give?

SAFE SEQUENCE
 ↓
Which processes can finish one by one?
```

---

# 28. Final Rules

> **Rule 1:** First calculate `Need`.

> **Rule 2:** Start with `Work = Available`.

> **Rule 3:** Check `Need <= Work`.

> **Rule 4:** Check ALL resources.

> **Rule 5:** If a process finishes, release its `Allocation`.

> **Rule 6:** Update `Work = Work + Allocation`.

> **Rule 7:** Continue until all processes finish or no process can finish.

> **Rule 8:** All processes finish → `SAFE`.

> **Rule 9:** Algorithm gets stuck → `UNSAFE`.

> **Rule 10:** Never add `Need` to `Work`; add `Allocation`.

---

# 29. Final Exam Answer Pattern

For a 5-mark or 10-mark numerical, write your answer in this order:

```text
1. Given Available

2. Calculate Need:
   
   Need = Max - Allocation

3. Initial Work:
   
   Work = Available

4. Check processes:
   
   Need <= Work

5. Select process that can finish.

6. Release Allocation:
   
   Work = Work + Allocation

7. Repeat.

8. Write Safe Sequence.

9. Final conclusion:
   
   SAFE STATE / UNSAFE STATE
```

### Example conclusion:

```text
Since all processes can complete in the order

P1 -> P3 -> P0 -> P2 -> P4

the system is in a SAFE STATE.
```

Or:

```text
After completing the possible processes, no remaining
process satisfies Need <= Work.

Therefore, the system is in an UNSAFE STATE.
```

---

# End of Banker’s Algorithm Notes

## Remember:

```text
Need = Max - Allocation

Work = Available

If Need <= Work:

    Process can finish

    Work = Work + Allocation

    Add process to Safe Sequence

Repeat.
```

**Banker's Algorithm = Check before allocation → find safe sequence → avoid unsafe state.**
