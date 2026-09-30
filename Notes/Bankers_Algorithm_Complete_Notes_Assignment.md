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

# 13. Assignment

> **Instructions:** For every question:
>
> 1.  Calculate the Need matrix.
> 2.  Write the initial Work/Available vector.
> 3.  Compare Need with Work.
> 4.  Update Work after every completed process.
> 5.  Write the safe sequence if one exists.
> 6.  Clearly explain why the state is unsafe if the algorithm gets
>     stuck.

------------------------------------------------------------------------

## Assignment 1 --- Basic 3 Resources

  Process   Allocation A B C   Max A B C
  --------- ------------------ -----------
  P0        1 0 0              2 1 1
  P1        0 1 0              1 2 1
  P2        1 1 1              2 2 1

Available:

``` text
(1,1,1)
```

### Find:

1.  Need matrix
2.  Whether system is safe
3.  One safe sequence

------------------------------------------------------------------------

# Assignment 2 --- Find Safe Sequence

  Process   Allocation A B C   Max A B C
  --------- ------------------ -----------
  P0        1 1 0              2 2 1
  P1        1 0 1              2 1 2
  P2        1 2 1              3 3 2
  P3        0 1 1              1 2 2

Available:

``` text
(1,1,1)
```

### Find:

-   Need matrix
-   Safe or unsafe
-   Safe sequence

------------------------------------------------------------------------

# Assignment 3 --- Four Resources

  Process   Allocation A B C D   Max A B C D
  --------- -------------------- -------------
  P0        1 0 2 1              3 2 2 2
  P1        1 1 0 0              2 2 1 1
  P2        1 0 1 1              1 2 2 2
  P3        0 1 1 0              2 2 2 1

Available:

``` text
(1,1,1,1)
```

Determine whether the state is safe.

------------------------------------------------------------------------

# Assignment 4 --- Unsafe State

  Process   Allocation A B C   Max A B C
  --------- ------------------ -----------
  P0        1 0 1              2 1 2
  P1        0 1 0              1 2 1
  P2        1 1 0              2 2 1

Available:

``` text
(0,0,0)
```

### Find:

1.  Need matrix
2.  Can any process execute?
3.  Is the system safe or unsafe?
4.  Explain why.

------------------------------------------------------------------------

# Assignment 5 --- Five Processes

  Process   Allocation A B C D   Max A B C D
  --------- -------------------- -------------
  P0        0 1 0 2              1 2 1 3
  P1        2 0 1 0              3 2 1 1
  P2        1 1 1 1              2 2 2 2
  P3        1 0 0 1              3 1 1 2
  P4        0 1 1 0              1 2 2 1

Available:

``` text
(1,1,1,1)
```

### Determine:

-   Need matrix
-   Safe or unsafe
-   Safe sequence

------------------------------------------------------------------------

# Assignment 6 --- Multiple Possible Safe Sequences

  Process   Allocation A B C   Max A B C
  --------- ------------------ -----------
  P0        1 0 0              2 1 1
  P1        0 1 0              1 2 1
  P2        0 0 1              1 1 2
  P3        1 1 0              2 2 1

Available:

``` text
(1,1,1)
```

### Find:

1.  Need matrix
2.  All possible safe sequences, if possible
3.  Is there only one safe sequence?

------------------------------------------------------------------------

# Assignment 7 --- Exam-Level 5 × 4

  Process   Allocation A B C D   Max A B C D
  --------- -------------------- -------------
  P0        3 0 1 1              5 1 1 3
  P1        1 2 0 0              2 3 2 1
  P2        1 0 2 1              1 2 2 2
  P3        0 1 1 0              1 2 1 1
  P4        1 1 0 1              2 2 1 2

Available:

``` text
(1,1,1,1)
```

### Solve completely.

------------------------------------------------------------------------

# Assignment 8 --- Two Different Available States

Use the same Allocation and Max matrices:

  Process   Allocation A B C   Max A B C
  --------- ------------------ -----------
  P0        1 0 0              2 1 1
  P1        0 1 0              1 2 1
  P2        1 1 1              2 2 1
  P3        0 0 1              1 1 2

### (a)

``` text
Available = (1,1,0)
```

### (b)

``` text
Available = (0,0,1)
```

Determine whether each state is safe or unsafe.

------------------------------------------------------------------------

# Assignment 9 --- Find Where the Algorithm Gets Stuck

  Process   Allocation A B C   Max A B C
  --------- ------------------ -----------
  P0        2 0 0              3 1 1
  P1        0 1 1              1 2 2
  P2        1 0 1              2 1 2
  P3        0 1 0              1 2 1

Available:

``` text
(0,1,1)
```

Show:

``` text
Initial Work = ?

P0 -> Can/Cannot?
P1 -> Can/Cannot?
P2 -> Can/Cannot?
P3 -> Can/Cannot?

If one completes:
New Work = ?

Continue...
```

Do not simply write SAFE/UNSAFE. Show the complete process.

------------------------------------------------------------------------

# Assignment 10 --- University Exam Style

  Process   Allocation     Max
  --------- -------------- --------------
  P0        (0, 1, 0, 2)   (1, 2, 1, 3)
  P1        (2, 0, 0, 0)   (3, 2, 1, 1)
  P2        (1, 1, 1, 1)   (2, 2, 2, 2)
  P3        (1, 0, 1, 0)   (1, 1, 2, 1)
  P4        (0, 0, 2, 0)   (1, 1, 3, 1)

Available:

``` text
(1,1,1,1)
```

### Questions

**a)** Calculate the Need matrix.

**b)** Apply Banker's Safety Algorithm.

**c)** Determine whether the system is safe.

**d)** If safe, give a safe sequence.

**e)** Show Work and Allocation after each process completes.

------------------------------------------------------------------------

# 14. Recommended Answer Format for Students

For a numerical question, use this structure:

## Step 1 --- Need Matrix

  Process     A   B   C   D
  --------- --- --- --- ---
  P0                    
  P1                    
  P2                    
  P3                    
  P4                    

------------------------------------------------------------------------

## Step 2 --- Initial Work

``` text
Work = Available = ( , , , )
```

------------------------------------------------------------------------

## Step 3 --- Process Selection

Write the comparison:

``` text
P0:
Need  = ( , , , )
Work  = ( , , , )

Need <= Work ?
YES / NO
```

------------------------------------------------------------------------

## Step 4 --- Update Work

If P0 completes:

``` text
New Work = Old Work + Allocation(P0)
```

------------------------------------------------------------------------

## Step 5 --- Safe Sequence

``` text
Safe Sequence:

P__ -> P__ -> P__ -> P__ -> P__
```

------------------------------------------------------------------------

## Step 6 --- Final Result

``` text
All processes completed.

Therefore:
SAFE STATE
```

or:

``` text
No remaining process can satisfy Need <= Work.

Therefore:
UNSAFE STATE
```

------------------------------------------------------------------------

# 15. Final Cheat Sheet

``` text
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
             where Need <= Work
                      |
             +--------+--------+
             |                 |
            YES                NO
             |                 |
             v                 v
       Process finishes    Are all processes
             |               finished?
             v                 |
      Work = Work +            +------+
        Allocation             |      |
             |                YES     NO
             v                 |      |
      Add to safe sequence     v      v
             |              SAFE   UNSAFE
             |
             v
          Repeat
```

## The 3 formulas to memorize

\[ `\boxed{Need = Max - Allocation}`{=tex} \]

\[ `\boxed{Work = Available}`{=tex} \]

\[ `\boxed{Work_{new}=Work_{old}+Allocation}`{=tex} \]

> **Remember:** Compare **Need with Work**, and after completion add
> **Allocation** to Work.
