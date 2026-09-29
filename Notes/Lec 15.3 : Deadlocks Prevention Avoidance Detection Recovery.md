# Deadlocks in Operating Systems

## 1. Introduction

A **deadlock** is a situation in an operating system where a group of processes are permanently waiting for resources held by each other.

As a result:

- No process in the group can continue.
- Resources remain occupied.
- The processes keep waiting indefinitely.

### Simple real-life example

Imagine two people:

```text
Person A has Pen
Person B has Notebook

A needs Notebook to continue
B needs Pen to continue
```

So:

```text
A → waiting for Notebook → held by B
B → waiting for Pen      → held by A
```

Neither person can continue.

This is similar to a deadlock in an operating system.

---

# 2. Deadlock Example in Operating System

Consider two processes:

```text
P1 requires:
    Printer
    Scanner

P2 requires:
    Scanner
    Printer
```

Suppose the following happens:

```text
P1 gets Printer
P2 gets Scanner
```

Now:

```text
P1 → waiting for Scanner
P2 → waiting for Printer
```

But:

```text
Scanner → held by P2
Printer → held by P1
```

Therefore:

```text
P1 waits for P2
P2 waits for P1
```

Neither can proceed.

This is a **deadlock**.

---

# 3. Resources

A resource is anything that a process needs to perform its work.

Examples:

- CPU
- Main memory
- Printer
- Scanner
- Files
- Database locks
- I/O devices
- Network devices

Resources can be:

### Reusable Resources

Resources that can be used again after being released.

Examples:

```text
CPU
Printer
Memory
File
```

### Consumable Resources

Resources that are produced and consumed.

Examples:

```text
Messages
Signals
Data
```

Most classical deadlock problems focus on **reusable resources**.

---

# 4. Resource Allocation

A process normally follows:

```text
Request
   ↓
Allocate
   ↓
Use
   ↓
Release
```

Example:

```text
P1
 |
 | Request Printer
 ↓
Printer allocated
 |
 | Use Printer
 ↓
Release Printer
```

A deadlock can occur when processes hold some resources while waiting for others.

---

# 5. Four Necessary Conditions for Deadlock

According to the classical deadlock model, four conditions must exist simultaneously for deadlock to occur:

1. Mutual Exclusion
2. Hold and Wait
3. No Preemption
4. Circular Wait

These are called the **Necessary Conditions for Deadlock**.

```text
                 DEADLOCK
                    |
       +------------+------------+
       |            |            |
       ↓            ↓            ↓
Mutual Exclusion  Hold & Wait  No Preemption
                    +
                Circular Wait
```

If even one condition is prevented, deadlock cannot occur.

---

# 6. Mutual Exclusion

## Meaning

At least one resource must be **non-shareable**.

Only one process can use that resource at a time.

Example:

```text
Printer
   ↓
P1 is using it
   ↓
P2 must wait
```

If P1 has the printer:

```text
Printer → P1
```

P2 cannot simultaneously use it.

### Why can this cause deadlock?

Suppose:

```text
P1 holds Printer
P2 waits for Printer
```

If another resource is involved, this can contribute to a circular dependency.

---

# 7. Hold and Wait

## Meaning

A process is holding at least one resource while waiting for another resource.

Example:

```text
P1
 |
 +---- Holds Printer
 |
 +---- Waiting for Scanner
```

At the same time:

```text
P2
 |
 +---- Holds Scanner
 |
 +---- Waiting for Printer
```

This creates:

```text
P1 → Scanner
P2 → Printer
```

---

# 8. No Preemption

## Meaning

A resource cannot be forcibly taken away from a process.

The process must release the resource voluntarily.

Example:

```text
P1 owns Printer
```

The OS cannot simply take the printer away from P1 while P1 is using it.

P1 must:

```text
Finish
  ↓
Release Printer
```

---

# 9. Circular Wait

## Meaning

A circular chain of processes exists where each process is waiting for a resource held by the next process.

Example:

```text
P1 → waiting for resource held by P2

P2 → waiting for resource held by P3

P3 → waiting for resource held by P1
```

Graphically:

```text
P1 → P2
↑     ↓
P3 ←--
```

Or:

```text
P1 → P2 → P3 → P1
```

This is a circular wait.

---

# 10. All Four Conditions Together

Consider:

```text
Resources:

R1 = Printer
R2 = Scanner

P1:
    Holds R1
    Requests R2

P2:
    Holds R2
    Requests R1
```

Now check the four conditions.

### Mutual Exclusion

Printer and Scanner cannot be simultaneously allocated to multiple processes.

### Hold and Wait

P1 holds R1 and waits for R2.

P2 holds R2 and waits for R1.

### No Preemption

Resources cannot be forcibly taken away.

### Circular Wait

```text
P1 → R2 → P2 → R1 → P1
```

All four conditions exist.

Therefore:

```text
DEADLOCK
```

---

# 11. Resource Allocation Graph

A **Resource Allocation Graph (RAG)** is used to represent relationships between processes and resources.

We use:

```text
Circle     → Process
Rectangle  → Resource
```

### Request Edge

```text
P1 → R1
```

means:

```text
P1 is requesting R1
```

### Assignment Edge

```text
R1 → P1
```

means:

```text
R1 is allocated to P1
```

---

# 12. Resource Allocation Graph Example

Suppose:

```text
P1 holds R1
P1 requests R2

P2 holds R2
P2 requests R1
```

Graph:

```text
R1 → P1 → R2 → P2 → R1
```

There is a cycle:

```text
R1 → P1 → R2 → P2 → R1
```

For the simple case where each resource has a single instance, this cycle indicates a deadlock.

---

# 13. Deadlock Handling Strategies

Operating systems can deal with deadlocks using four major approaches:

```text
                  DEADLOCK HANDLING
                         |
       +-----------------+------------------+
       |                 |                  |
       ↓                 ↓                  ↓
 Prevention          Avoidance          Detection
                                             |
                                             ↓
                                          Recovery
```

Main approaches:

1. Deadlock Prevention
2. Deadlock Avoidance
3. Deadlock Detection
4. Deadlock Recovery

---

# 14. Deadlock Prevention

## Definition

**Deadlock prevention** means designing the system so that at least one of the four necessary conditions for deadlock can never occur.

```text
Deadlock requires:

Mutual Exclusion
Hold and Wait
No Preemption
Circular Wait

Prevention:
Break at least ONE condition
```

---

# 15. Preventing Mutual Exclusion

If possible, make resources shareable.

Example:

Instead of giving a printer directly to each process, use **spooling**.

```text
P1 ─┐
P2 ─┼──→ Print Queue ──→ Printer
P3 ─┘
```

Processes write their output to a queue.

The printer processes jobs one by one.

This reduces direct resource competition.

### Limitation

Some resources cannot be made shareable.

For example:

```text
Printer
Mutex lock
Certain hardware devices
```

Therefore, mutual exclusion cannot always be eliminated.

---

# 16. Preventing Hold and Wait

One method is:

> A process must request all required resources before it starts execution.

Example:

```text
P1 requires:
Printer + Scanner
```

Instead of:

```text
Request Printer
Use Printer
Request Scanner
```

it must request:

```text
Request:
Printer + Scanner
```

Only when both are available:

```text
Allocate both
   ↓
Execute
   ↓
Release both
```

### Advantage

The process never holds one resource while waiting for another.

### Disadvantages

- Resources may remain unused for a long time.
- A process may request resources it does not immediately need.
- Can reduce resource utilization.

---

# 17. Preventing No Preemption

Allow the OS to take resources away under certain conditions.

Example:

```text
P1 holds R1
P1 requests R2

R2 unavailable
```

The system may force P1 to release R1.

```text
P1
 |
 | Holds R1
 |
 | Requests R2
 ↓
R2 unavailable
 ↓
Release R1
```

The process can retry later.

### Limitation

This works only for resources whose state can safely be saved and restored.

It is difficult for some hardware resources.

---

# 18. Preventing Circular Wait

Assign an ordering to resources.

Example:

```text
R1 < R2 < R3
```

Processes must request resources only in increasing order.

For example:

```text
Correct:

Request R1
   ↓
Request R2
   ↓
Request R3
```

But:

```text
Incorrect:

Request R2
   ↓
Request R1
```

because it violates the ordering.

### Why does this work?

If every process follows the same ordering, a circular chain cannot be formed.

---

# 19. Prevention Summary

| Deadlock Condition | Prevention Idea |
|---|---|
| Mutual Exclusion | Make resources shareable when possible |
| Hold and Wait | Request all resources together |
| No Preemption | Allow resource preemption where possible |
| Circular Wait | Impose resource ordering |

---

# 20. Deadlock Avoidance

## Definition

**Deadlock avoidance** does not permanently break a deadlock condition.

Instead, the OS examines each resource request and decides:

> "If I grant this request, will the system remain in a safe state?"

If yes:

```text
Grant request
```

If no:

```text
Delay request
```

---

# 21. Safe State

A system is in a **safe state** if there exists at least one sequence in which all processes can complete without causing deadlock.

This sequence is called a:

**Safe Sequence**

Example:

```text
<P2, P1, P3>
```

means:

```text
P2 can finish first
then P1
then P3
```

If such a sequence exists, the system is safe.

---

# 22. Unsafe State

An **unsafe state** means the system cannot guarantee that all processes can finish without deadlock.

Important:

```text
Unsafe State ≠ Guaranteed Deadlock
```

An unsafe state means:

> The OS can no longer guarantee deadlock-free execution.

Deadlock may occur later.

---

# 23. Safe vs Unsafe vs Deadlock

```text
Safe State
    ↓
Safe sequence exists
    ↓
System can guarantee completion
```

```text
Unsafe State
    ↓
No safe sequence can be guaranteed
    ↓
Deadlock may occur
```

```text
Deadlock State
    ↓
Processes are already permanently waiting
```

So:

```text
Safe → Safe execution possible
Unsafe → Risk exists
Deadlock → Deadlock has occurred
```

---

# 24. Banker's Algorithm

The **Banker's Algorithm** is a famous deadlock avoidance algorithm.

It is used when:

- There are multiple instances of resources.
- Each process declares its maximum resource requirement.
- The OS knows the available resources.
- The OS checks whether granting a request keeps the system safe.

The algorithm is called "Banker's Algorithm" because it is similar to a bank deciding whether it can safely give loans while still being able to satisfy all customers eventually.

---

# 25. Data Structures in Banker's Algorithm

The algorithm uses:

### Available

Number of currently available instances of each resource.

### Max

Maximum resources each process may need.

### Allocation

Resources currently allocated to each process.

### Need

Remaining resources a process may still require.

Formula:

```text
Need = Max - Allocation
```

This formula is extremely important.

---

# 26. Banker's Algorithm Example

Suppose there are three resource types:

```text
A, B, C
```

Available:

```text
Available = [3, 3, 2]
```

Processes:

```text
P0
P1
P2
P3
P4
```

### Allocation

| Process | A | B | C |
|---|---:|---:|---:|
| P0 | 0 | 1 | 0 |
| P1 | 2 | 0 | 0 |
| P2 | 3 | 0 | 2 |
| P3 | 2 | 1 | 1 |
| P4 | 0 | 0 | 2 |

### Max

| Process | A | B | C |
|---|---:|---:|---:|
| P0 | 7 | 5 | 3 |
| P1 | 3 | 2 | 2 |
| P2 | 9 | 0 | 2 |
| P3 | 2 | 2 | 2 |
| P4 | 4 | 3 | 3 |

---

# 27. Calculate Need Matrix

Use:

```text
Need = Max - Allocation
```

### P0

```text
Max        = [7,5,3]
Allocation = [0,1,0]

Need = [7,4,3]
```

### P1

```text
Max        = [3,2,2]
Allocation = [2,0,0]

Need = [1,2,2]
```

### P2

```text
Max        = [9,0,2]
Allocation = [3,0,2]

Need = [6,0,0]
```

### P3

```text
Max        = [2,2,2]
Allocation = [2,1,1]

Need = [0,1,1]
```

### P4

```text
Max        = [4,3,3]
Allocation = [0,0,2]

Need = [4,3,1]
```

Therefore:

| Process | Need A | Need B | Need C |
|---|---:|---:|---:|
| P0 | 7 | 4 | 3 |
| P1 | 1 | 2 | 2 |
| P2 | 6 | 0 | 0 |
| P3 | 0 | 1 | 1 |
| P4 | 4 | 3 | 1 |

---

# 28. Finding the Safe Sequence

Initial:

```text
Work = Available
     = [3,3,2]
```

We need to find a process whose:

```text
Need <= Work
```

component by component.

---

## Step 1: Check P0

```text
Need P0 = [7,4,3]

Work = [3,3,2]
```

Check:

```text
7 <= 3 ❌
```

Cannot execute P0.

---

## Step 2: Check P1

```text
Need P1 = [1,2,2]

Work = [3,3,2]
```

Check:

```text
1 <= 3 ✓
2 <= 3 ✓
2 <= 2 ✓
```

P1 can finish.

After P1 finishes, its allocated resources are released:

```text
Work = Work + Allocation(P1)

     = [3,3,2] + [2,0,0]

     = [5,3,2]
```

Safe sequence:

```text
<P1>
```

---

## Step 3: Check P3

```text
Need P3 = [0,1,1]

Work = [5,3,2]
```

All conditions satisfy:

```text
0 <= 5 ✓
1 <= 3 ✓
1 <= 2 ✓
```

P3 can finish.

Release Allocation(P3):

```text
Work = [5,3,2] + [2,1,1]

     = [7,4,3]
```

Safe sequence:

```text
<P1, P3>
```

---

## Step 4: Check P4

```text
Need P4 = [4,3,1]

Work = [7,4,3]
```

All conditions satisfy.

P4 finishes.

```text
Work = [7,4,3] + [0,0,2]

     = [7,4,5]
```

Safe sequence:

```text
<P1, P3, P4>
```

---

## Step 5: Check P0

```text
Need P0 = [7,4,3]

Work = [7,4,5]
```

All conditions satisfy.

P0 finishes.

```text
Work = [7,4,5] + [0,1,0]

     = [7,5,5]
```

Safe sequence:

```text
<P1, P3, P4, P0>
```

---

## Step 6: Check P2

```text
Need P2 = [6,0,0]

Work = [7,5,5]
```

All conditions satisfy.

P2 finishes.

Therefore:

```text
Safe Sequence:

<P1, P3, P4, P0, P2>
```

The system is in a **safe state**.

---

# 29. Banker's Algorithm Procedure

For exam numericals:

### Step 1

Calculate:

```text
Need = Max - Allocation
```

### Step 2

Set:

```text
Work = Available
```

### Step 3

Find a process satisfying:

```text
Need <= Work
```

### Step 4

Assume that process finishes.

Update:

```text
Work = Work + Allocation
```

### Step 5

Repeat.

### Step 6

If all processes finish:

```text
Safe State
```

If no unfinished process can satisfy:

```text
Need <= Work
```

then:

```text
Unsafe State
```

---

# 30. Deadlock Detection

## Definition

Deadlock detection allows deadlocks to occur.

The OS periodically checks:

> "Has a deadlock occurred?"

If yes:

```text
Detect
   ↓
Identify involved processes/resources
   ↓
Recover
```

This is different from prevention and avoidance.

---

# 31. Detection vs Avoidance

### Avoidance

Before allocating resources:

```text
Request
   ↓
Check safety
   ↓
Safe?
 /   \
Yes   No
 |     |
Grant  Wait
```

### Detection

The OS allows allocations:

```text
Request
   ↓
Grant
   ↓
Later check
   ↓
Deadlock?
```

So:

```text
Avoidance → Prevent entering unsafe state

Detection → Find deadlock after it occurs
```

---

# 32. Wait-For Graph

A **Wait-For Graph** is commonly used for deadlock detection when each resource has a single instance.

Only processes are represented.

Example:

```text
P1 → P2
```

means:

```text
P1 is waiting for a resource held by P2
```

If we have:

```text
P1 → P2 → P3 → P1
```

there is a cycle.

For single-instance resource systems:

```text
Cycle in Wait-For Graph
        ↓
Deadlock
```

---

# 33. Detection Example

Suppose:

```text
P1 holds R1 and waits for R2
P2 holds R2 and waits for R3
P3 holds R3 and waits for R1
```

Wait-for relationships:

```text
P1 → P2
P2 → P3
P3 → P1
```

Therefore:

```text
P1 → P2 → P3 → P1
```

Cycle exists.

Hence:

```text
Deadlock detected
```

---

# 34. Deadlock Recovery

After detecting a deadlock, the OS needs to recover.

Two major methods are:

1. Process Termination
2. Resource Preemption

---

# 35. Recovery Method 1: Process Termination

The OS can terminate one or more processes involved in the deadlock.

Example:

```text
P1 → P2 → P3 → P1
```

The OS may terminate P2.

Then:

```text
P2 terminates
   ↓
Resources held by P2 are released
   ↓
P1/P3 may continue
```

---

# 36. Terminating All Deadlocked Processes

The simplest approach:

```text
Terminate all processes involved
```

Example:

```text
Deadlocked:
P1, P2, P3

Terminate:
P1
P2
P3
```

### Advantage

Simple and guaranteed to break the deadlock.

### Disadvantage

Large amount of work may be lost.

---

# 37. Terminating One Process at a Time

Another approach:

```text
Terminate one process
       ↓
Check whether deadlock remains
       ↓
If yes:
Terminate another process
       ↓
Repeat
```

This may preserve more work but requires repeated detection.

---

# 38. Selecting Which Process to Terminate

The OS may consider:

- Process priority
- How long the process has executed
- How much work remains
- Number of resources held
- Number of resources still required
- Whether the process can be restarted easily
- Cost of terminating the process

There is no universal rule that is always optimal.

---

# 39. Recovery Method 2: Resource Preemption

The OS can take a resource away from one process and give it to another.

Example:

```text
P1 holds R1
P2 holds R2

P1 needs R2
P2 needs R1
```

The OS may preempt R1 from P1.

```text
R1 → taken from P1
```

Then P2 may proceed.

Eventually P1 can resume.

---

# 40. Problems with Resource Preemption

Resource preemption creates several problems:

### 1. Victim Selection

Which process should lose its resource?

### 2. Rollback

The process may need to return to an earlier safe state.

### 3. Starvation

The same process might repeatedly be selected as the victim.

Therefore, recovery needs a careful policy.

---

# 41. Rollback

Rollback means returning a process to an earlier state.

Example:

```text
Process P1
   ↓
Execution
   ↓
Checkpoint
   ↓
More execution
   ↓
Deadlock
```

The system can:

```text
Deadlock
   ↓
Rollback P1
   ↓
Release resources
   ↓
Restart P1 later
```

---

# 42. Starvation During Recovery

Suppose:

```text
P1 repeatedly becomes the victim.
```

Every time a deadlock occurs:

```text
P1 → preempted
```

This can cause P1 to wait indefinitely.

This is a form of **starvation**.

A recovery algorithm should consider how many times a process has already been selected as a victim.

---

# 43. Prevention vs Avoidance vs Detection

| Feature | Prevention | Avoidance | Detection |
|---|---|---|---|
| Basic idea | Break a necessary condition | Stay in safe state | Allow deadlock and detect it |
| Deadlock allowed? | No | No, if correctly avoided | Yes |
| Main concept | Conditions | Safe state | Detection algorithm |
| Banker's Algorithm | No | Yes | No |
| Resource knowledge | Less dynamic information needed | Maximum needs usually known | Current allocation/waiting information |
| Overhead | Can reduce resource utilization | Higher runtime checking | Periodic detection |
| Recovery needed | Normally no | Normally no | Yes |

---

# 44. Prevention vs Avoidance

This distinction is important for exams.

### Prevention

```text
Break a condition
```

Example:

```text
Prevent Circular Wait
```

by imposing:

```text
R1 < R2 < R3
```

### Avoidance

```text
Check every allocation
```

and ask:

```text
Will the system remain safe?
```

Example:

```text
Banker's Algorithm
```

---

# 45. Avoidance vs Detection

### Avoidance

The OS checks **before** granting a request.

```text
Request
   ↓
Safety Check
   ↓
Grant / Delay
```

### Detection

The OS may grant the request and later checks for deadlock.

```text
Request
   ↓
Grant
   ↓
Run
   ↓
Detection
   ↓
Deadlock?
```

---

# 46. Deadlock Recovery Example

Suppose:

```text
P1 holds R1
P1 waits for R2

P2 holds R2
P2 waits for R1
```

Deadlock:

```text
P1 ↔ P2
```

Detection finds:

```text
Cycle
```

Recovery option 1:

```text
Terminate P1
```

Then:

```text
R1 released
   ↓
P2 gets R1
   ↓
P2 finishes
```

Recovery option 2:

```text
Preempt R1 from P1
```

Then give R1 to P2.

---

# 47. Real-Life Analogy

Imagine two cars on a very narrow road.

```text
Car A → needs Car B to move
Car B → needs Car A to move
```

Neither can move.

This is similar to:

```text
Process A → Resource held by Process B
Process B → Resource held by Process A
```

### Prevention

Design the road so that cars cannot enter from opposite ends.

### Avoidance

Allow a car to enter only if enough space exists to safely complete the movement.

### Detection

Allow the situation to happen and check whether traffic is stuck.

### Recovery

Move one car backward or remove one car.

---

# 48. Important Numerical Concepts

For deadlock numericals, remember:

## Need Matrix

```text
Need = Max - Allocation
```

## Work

Initially:

```text
Work = Available
```

When process Pi can finish:

```text
Work = Work + Allocation[i]
```

## Safe Condition

For a process:

```text
Need[i] <= Work
```

If true:

```text
Assume process finishes
```

Then release its allocated resources.

---

# 49. Common Mistakes in Banker's Algorithm

### Mistake 1

Using:

```text
Max
```

instead of:

```text
Need
```

Correct:

```text
Need = Max - Allocation
```

### Mistake 2

Forgetting to update Work.

Correct:

```text
Work = Work + Allocation
```

after assuming a process finishes.

### Mistake 3

Comparing only one resource.

All resources must satisfy:

```text
Need A <= Work A
Need B <= Work B
Need C <= Work C
```

### Mistake 4

Thinking unsafe always means deadlock.

Remember:

```text
Unsafe ≠ Deadlock
```

An unsafe state means the system cannot guarantee that deadlock will be avoided.

---

# 50. Exam Diagram

A useful diagram for a 5/10-mark answer:

```text
                         DEADLOCK
                            |
             +--------------+--------------+
             |              |              |
             ↓              ↓              ↓
          Prevention     Avoidance      Detection
             |              |              |
             ↓              ↓              ↓
       Break one of     Safe State      Detect Cycle
       four conditions      |              |
                            ↓              ↓
                     Banker's Algo      Recovery
                                           |
                                  +--------+--------+
                                  |                 |
                                  ↓                 ↓
                            Termination       Preemption
```

---

# 51. Four Conditions — Easy Memory Trick

Remember:

```text
M H N C
```

### M — Mutual Exclusion

Resource cannot be shared.

### H — Hold and Wait

Hold one resource, wait for another.

### N — No Preemption

Resource cannot be forcibly taken.

### C — Circular Wait

Processes wait in a circle.

Remember:

> **M H N C → all four together can create Deadlock.**

---

# 52. Complete Deadlock Flow

```text
Process requests resources
          |
          ↓
Resources allocated
          |
          ↓
Does process hold some
resource while waiting?
          |
          ↓
       Conditions
          |
    +-----+-----+
    |           |
   Safe       Risk
    |           |
    ↓           ↓
Continue    Deadlock handling
                |
       +--------+--------+
       |        |        |
       ↓        ↓        ↓
 Prevention  Avoidance  Detection
                         |
                         ↓
                      Recovery
                         |
                +--------+--------+
                |                 |
                ↓                 ↓
           Termination      Preemption
```

---

# 53. Quick Revision Table

| Topic | Key Idea |
|---|---|
| Deadlock | Processes wait forever for resources |
| Mutual Exclusion | Resource is non-shareable |
| Hold and Wait | Hold one resource, wait for another |
| No Preemption | Resource cannot be forcibly taken |
| Circular Wait | Processes form a waiting cycle |
| Prevention | Break at least one necessary condition |
| Avoidance | Allocate only if system remains safe |
| Safe State | Safe sequence exists |
| Unsafe State | No safe sequence can be guaranteed |
| Banker's Algorithm | Deadlock avoidance algorithm |
| Detection | Find deadlock after it may occur |
| Recovery | Remove deadlock after detection |
| Termination | Kill one/all processes |
| Preemption | Take resources from processes |
| Rollback | Return process to earlier state |
| Wait-For Graph | Detect cycles for single-instance resources |

---

# 54. Most Important Exam Questions

## 2-Mark Questions

1. Define deadlock.
2. List the four necessary conditions for deadlock.
3. What is mutual exclusion?
4. What is hold and wait?
5. What is circular wait?
6. Define safe state.
7. Define unsafe state.
8. What is Banker's Algorithm?
9. What is deadlock detection?
10. What is deadlock recovery?

---

## 5-Mark Questions

1. Explain the four necessary conditions for deadlock.
2. Explain deadlock prevention techniques.
3. Explain deadlock avoidance.
4. Explain safe and unsafe states.
5. Explain resource allocation graphs.
6. Explain deadlock detection using a wait-for graph.
7. Explain deadlock recovery techniques.

---

## 10-Mark Questions

### Question 1

**Explain deadlock in detail. Discuss the four necessary conditions for deadlock with suitable examples.**

Answer structure:

```text
Definition
↓
Example
↓
Mutual Exclusion
↓
Hold and Wait
↓
No Preemption
↓
Circular Wait
↓
Resource Allocation Graph
```

### Question 2

**Explain deadlock prevention and avoidance.**

Answer structure:

```text
Deadlock
↓
Prevention
↓
Four conditions
↓
Avoidance
↓
Safe State
↓
Unsafe State
↓
Banker's Algorithm
↓
Example
```

### Question 3

**Explain Banker's Algorithm with a suitable example.**

Answer structure:

```text
Definition
↓
Available
↓
Max
↓
Allocation
↓
Need
↓
Need = Max - Allocation
↓
Work = Available
↓
Find safe process
↓
Release allocation
↓
Safe sequence
```

### Question 4

**Explain deadlock detection and recovery.**

Answer structure:

```text
Detection
↓
Wait-For Graph
↓
Cycle
↓
Deadlock
↓
Recovery
   ├── Process Termination
   ├── Resource Preemption
   └── Rollback
```

---

# 55. Final Concept Summary

The complete concept can be remembered as:

```text
DEADLOCK
   |
   | Four Conditions
   ↓
MUTUAL EXCLUSION
HOLD AND WAIT
NO PREEMPTION
CIRCULAR WAIT
   |
   ↓
How should OS handle it?
   |
   +-------------------+
   |                   |
   ↓                   ↓
PREVENTION          AVOIDANCE
   |                   |
Break condition     Safe state
                       |
                       ↓
                 Banker's Algorithm
   |
   +-------------------+
                       |
                       ↓
                  DETECTION
                       |
                       ↓
                Deadlock Found
                       |
                       ↓
                    RECOVERY
                  /           \
                 ↓             ↓
          Termination      Preemption
                 |
                 ↓
              Rollback
```

## One-Line Definitions

> **Deadlock:** A condition where processes wait indefinitely for resources held by each other.

> **Deadlock Prevention:** Prevent deadlock by ensuring at least one necessary condition can never occur.

> **Deadlock Avoidance:** Dynamically allocate resources only when the resulting state remains safe.

> **Safe State:** A state in which a safe sequence exists for all processes to complete.

> **Unsafe State:** A state where the OS cannot guarantee that all processes can complete without deadlock.

> **Banker's Algorithm:** A deadlock-avoidance algorithm that checks whether granting resources keeps the system in a safe state.

> **Deadlock Detection:** Allow resource allocation and periodically check whether deadlock has occurred.

> **Deadlock Recovery:** Actions taken after detection to break the deadlock.

> **Resource Preemption:** Temporarily taking a resource away from a process.

> **Rollback:** Returning a process to an earlier safe/checkpointed state.
