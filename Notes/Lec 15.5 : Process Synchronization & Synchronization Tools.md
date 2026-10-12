# Operating Systems --- Student Notes

## Lecture 21: Process Synchronization \| Lecture 22: Synchronization Tools

**Level:** Beginner-friendly, university exam preparation\
**Learning goal:** Understand why concurrent processes need
coordination, how critical sections are protected, and how mutexes,
semaphores, and monitors help.

------------------------------------------------------------------------

# Part A --- Lecture 21: Process Synchronization

## 1. What is process synchronization?

A modern operating system can run many processes and threads
concurrently. Even on a single CPU core, the operating system can switch
rapidly between tasks, making them appear to run at the same time. On
multicore systems, tasks may genuinely execute in parallel.

When two or more tasks use the same data or resource, their operations
may overlap. **Process synchronization** is the coordination of these
tasks so that shared data remains correct and operations happen in a
safe order.

### Everyday analogy

Imagine two students editing the same marks spreadsheet at the same
time. If both read an old value and then save different updates, one
student's change may overwrite the other's. A coordination rule---such
as allowing only one editor to update a particular record at a
time---helps prevent this problem.

### Important terms

-   **Concurrency:** Multiple tasks make progress during overlapping
    periods.
-   **Parallelism:** Multiple tasks execute at the same instant, usually
    on different cores.
-   **Shared resource:** Data or an object used by more than one
    process/thread, such as a variable, file, printer, or bank balance.
-   **Atomic operation:** An operation that appears indivisible to other
    concurrent tasks.
-   **Synchronization:** Coordination that controls access to shared
    resources or the order of events.

## 2. Race condition

A **race condition** occurs when the result of a program depends on the
timing or interleaving of concurrent operations that access shared
state.

### Example: lost update

Suppose a shared variable `balance` is initially `100`. Two threads each
add `50`.

A simplified increment is not necessarily one indivisible action. It may
involve: 1. Read the current value. 2. Calculate the new value. 3. Write
the new value.

If both threads read `100` before either writes, both calculate `150`.
Both may then write `150`, although the correct final value should be
`200`.

``` mermaid
sequenceDiagram
    participant T1 as Thread 1
    participant M as Shared balance
    participant T2 as Thread 2
    Note over M: Initial value = 100
    T1->>M: Read 100
    T2->>M: Read 100
    T1->>T1: Calculate 100 + 50 = 150
    T2->>T2: Calculate 100 + 50 = 150
    T1->>M: Write 150
    T2->>M: Write 150
    Note over M: Final value = 150 (incorrect)
```

The problem is not that addition is mathematically incorrect. The
problem is that the **read--modify--write sequence was not protected**.

### How can it be prevented?

-   Protect the shared update with a mutex/lock.
-   Use an appropriate atomic operation.
-   Use a semaphore or another synchronization mechanism where suitable.
-   Design the program to avoid unnecessary shared mutable state.

### Demonstration idea

Ask students to imagine two threads incrementing one shared counter
100,000 times each. Without synchronization, the final count may be less
than 200,000. With a correct lock or atomic increment, the expected
result is 200,000. In real code, the result depends on the language,
compiler, and runtime; a race demonstration should use a properly
constructed concurrent program rather than relying on a particular
output every run.

## 3. The critical section problem

A **critical section** is the part of a program where a process or
thread accesses shared data or a shared resource in a way that requires
coordination.

A typical process structure is:

``` text
do {
    Entry section       // request permission to enter
    Critical section   // access/update shared resource
    Exit section       // release permission
    Remainder section  // other work
} while (true);
```

``` mermaid
flowchart TD
    A[Process performs other work] --> B[Entry section]
    B --> C{Permission granted?}
    C -- No --> B
    C -- Yes --> D[Critical section]
    D --> E[Exit section]
    E --> A
```

### Example

A program has a shared variable:

``` c
int seats_available = 1;
```

Two booking threads may both check that a seat is available and then
both reserve it unless checking and updating the seat are coordinated.
The protected operation should cover the logically connected
check-and-update, not merely one individual line.

### Four common parts

1.  **Entry section:** Requests entry into the critical section.
2.  **Critical section:** Uses or modifies the protected shared
    resource.
3.  **Exit section:** Releases access or updates synchronization state.
4.  **Remainder section:** Performs work that does not need the
    protected resource.

The critical-section problem asks us to design a protocol that
coordinates these parts correctly.

## 4. Three requirements for a correct solution

A classical solution to the critical-section problem is evaluated using
three requirements.

### 4.1 Mutual exclusion

At most one process can be inside the critical section for a particular
protected resource at a time.

**Why it matters:** If two threads update the same data simultaneously,
an update may be lost or the data may become inconsistent.

### 4.2 Progress

If no process is inside the critical section and some processes wish to
enter, the choice of the next process must not be postponed
indefinitely. Only processes that want to enter should participate in
deciding who goes next.

**Why it matters:** A process should not be blocked for no good reason
when the resource is free.

### 4.3 Bounded waiting

After a process requests entry, there must be a bound on how many times
other processes can enter their critical sections before the waiting
process gets its turn.

**Why it matters:** It prevents indefinite postponement (starvation)
under the stated assumptions.

``` mermaid
flowchart TD
    A[Correct critical-section solution] --> B[Mutual Exclusion]
    A --> C[Progress]
    A --> D[Bounded Waiting]
    B --> E[No unsafe simultaneous entry]
    C --> F[No unnecessary delay when free]
    D --> G[No indefinite postponement]
```

**Exam tip:** Remember the phrase **M-P-B**: Mutual Exclusion, Progress,
Bounded Waiting. All three are important; satisfying only mutual
exclusion is not enough.

## 5. Peterson's solution

Peterson's solution is a classic software algorithm that demonstrates
how **two processes** can coordinate access to a critical section using
shared variables, without relying on a special lock instruction in the
conceptual model.

It is mainly important for understanding synchronization principles.
Modern production code should normally use language/runtime
synchronization primitives rather than implementing Peterson's algorithm
directly.

### Shared variables

For two processes, `P0` and `P1`:

``` c
boolean flag[2] = {false, false};
int turn;
```

-   `flag[i] = true` means process `Pi` wants to enter the critical
    section.
-   `turn = j` gives the other process `Pj` priority when both processes
    want to enter.

### Pseudocode for process `Pi`

Let `j = 1 - i`.

``` c
do {
    flag[i] = true;   // I want to enter
    turn = j;         // Give the other process priority

    while (flag[j] && turn == j) {
        // Busy wait
    }

    /* CRITICAL SECTION */

    flag[i] = false;  // I am leaving

    /* REMAINDER SECTION */
} while (true);
```

### How it works

1.  A process raises its own flag to indicate interest.
2.  It sets `turn` to the other process, giving that process priority if
    both compete.
3.  It waits only when the other process is interested **and** it is the
    other process's turn.
4.  When it exits, it lowers its flag so the other process can proceed.

### Visual intuition

``` mermaid
flowchart TD
    A[Pi wants to enter] --> B[flag[i] = true]
    B --> C[turn = j]
    C --> D{flag[j] is true AND turn = j?}
    D -- Yes --> E[Busy wait]
    E --> D
    D -- No --> F[Enter critical section]
    F --> G[flag[i] = false]
    G --> H[Remainder section]
```

### Why it satisfies the classical requirements

-   **Mutual exclusion:** If both flags are true, the single shared
    `turn` value cannot give both processes priority simultaneously. At
    least one must wait.
-   **Progress:** If only one process wants to enter, the other
    process's flag is false, so the interested process does not wait on
    the loop condition.
-   **Bounded waiting:** If both want to enter, `turn` resolves the
    competition. Once a process has requested entry, the other process
    cannot repeatedly bypass it indefinitely under the algorithm's
    assumptions.

### Limitations and assumptions

-   It is designed for **two processes** in its basic form.
-   It uses **busy waiting**: a waiting process repeatedly checks a
    condition and consumes CPU time.
-   The classical proof assumes suitable atomic reads/writes and a
    sequentially consistent memory model. Modern compilers and
    processors may reorder memory operations; a literal implementation
    using ordinary variables in modern C/C++/Java is not automatically
    correct.
-   For real programs, use appropriate mutexes, semaphores, or
    language-specific concurrency tools.

**Exam tip:** Be able to write the shared variables, pseudocode, and
explain the three critical-section requirements.

------------------------------------------------------------------------

# Part B --- Lecture 22: Synchronization Tools

## 6. Mutex

A **mutex** (mutual-exclusion lock) is a synchronization primitive used
to protect a critical section. A thread must acquire the mutex before
accessing the protected region and release it when finished.

### Basic pattern

``` text
lock(mutex)
    // critical section
unlock(mutex)
```

If one thread already holds the mutex, another thread attempting to
acquire it must wait or otherwise be prevented from entering, depending
on the mutex implementation.

``` mermaid
flowchart TD
    A[Thread requests mutex] --> B{Mutex available?}
    B -- Yes --> C[Acquire mutex]
    C --> D[Enter critical section]
    D --> E[Finish protected work]
    E --> F[Release mutex]
    F --> G[Another waiting thread may proceed]
    B -- No --> H[Wait / block]
    H --> B
```

### Example

Two threads update a shared counter. The lock must cover the entire
read-and-update operation, not just the final write.

``` text
lock(counter_mutex)
counter = counter + 1
unlock(counter_mutex)
```

This is conceptual pseudocode. The exact API differs by programming
language.

### Important points

-   A mutex is commonly used for **mutual exclusion**.
-   A mutex generally has ownership: the thread that locks it is
    expected to unlock it.
-   Keep critical sections short to reduce waiting.
-   Always consider what happens if code exits early or an error occurs;
    many languages provide scoped-lock patterns to release locks safely.
-   Incorrect lock ordering can cause deadlock.

## 7. Semaphores

A **semaphore** is a synchronization primitive that maintains a counter
and supports atomic operations commonly called `wait()` and `signal()`.

Different textbooks use different names: - `wait()` may be called `P()`
or `down()`. - `signal()` may be called `V()` or `up()`.

### Conceptual operations

``` text
wait(S):
    atomically wait until the resource/condition can be acquired
    then decrement S

signal(S):
    atomically increment/release S
    and potentially wake a waiting process
```

The operations must be atomic. The pseudocode above describes the
concept, not a safe implementation using an ordinary integer.

### Two common uses

1.  **Resource counting:** Track how many identical resource instances
    are available.
2.  **Event/order synchronization:** Make one task wait until another
    task signals that an event has happened.

### Example: one printer

If the system has one printer available, a counting semaphore can be
initialized to `1`. A process performs `wait(printer)` before using the
printer and `signal(printer)` when it finishes.

### Mutex vs semaphore

  -----------------------------------------------------------------------
  Feature                 Mutex                   Semaphore
  ----------------------- ----------------------- -----------------------
  Main purpose            Mutual exclusion        Resource counting
                                                  and/or event
                                                  coordination

  State                   Typically               Counter
                          locked/unlocked         

  Ownership               Usually owned by the    Generally no ownership
                          locking thread          requirement in the same
                                                  sense

  Common operations       lock / unlock           wait / signal

  Typical use             Protect shared data     Limit concurrent access
                                                  or order tasks
  -----------------------------------------------------------------------

A binary semaphore may also be used for mutual exclusion, but it is not
identical to a mutex in every API or ownership rule.

## 8. Binary vs counting semaphore

### Binary semaphore

A binary semaphore has two logical states, usually `0` and `1`.

-   `1` can represent an available permit.
-   `0` can represent no permit available.

It can be used to control access to a single resource or signal an
event, depending on how it is designed.

### Counting semaphore

A counting semaphore tracks multiple available permits.

**Example:** A lab has five identical computers available for a
particular shared service. Initialize a semaphore to `5`. Each user must
acquire one permit before using a computer and return the permit
afterward. At most five users can hold permits simultaneously.

``` mermaid
flowchart LR
    A[Counting semaphore = 5] --> B[Acquire permit]
    B --> C[4 permits remain]
    C --> D[Another user acquires]
    D --> E[3 permits remain]
    E --> F[Users release permits when finished]
    F --> A
```

  -----------------------------------------------------------------------
  Feature                 Binary semaphore        Counting semaphore
  ----------------------- ----------------------- -----------------------
  Logical range           0 or 1                  Non-negative count

  Represents              One permit or an event  Number of available
                          state                   permits/resources

  Example                 One available slot      Five identical devices

  Maximum concurrent      Usually one             Up to the initial
  permit holders                                  count, if used as a
                                                  resource semaphore
  -----------------------------------------------------------------------

**Caution:** A semaphore is not a substitute for thoughtful design. If a
permit is not returned, tasks can remain blocked. If `signal()` is
called incorrectly, the program may allow too much concurrent access.

## 9. Producer--Consumer problem using semaphores

This is a classic synchronization problem. A **producer** creates items
and places them into a shared bounded buffer. A **consumer** removes
items from that buffer.

We must prevent three errors: 1. The producer inserting into a full
buffer. 2. The consumer removing from an empty buffer. 3. The producer
and consumer corrupting shared buffer metadata by modifying it
simultaneously.

### Required semaphores

For a buffer with capacity `N`:

``` text
mutex = 1   // mutual exclusion for buffer operations
empty = N   // number of empty slots
full  = 0   // number of occupied slots
```

### Producer pseudocode

``` text
while true:
    item = produce_item()

    wait(empty)       // reserve an empty slot
    wait(mutex)       // enter buffer critical section

    insert(item)

    signal(mutex)     // leave critical section
    signal(full)      // announce one more full slot
```

### Consumer pseudocode

``` text
while true:
    wait(full)        // reserve an available item
    wait(mutex)       // enter buffer critical section

    item = remove()

    signal(mutex)     // leave critical section
    signal(empty)     // announce one more empty slot

    consume_item(item)
```

### Diagram

``` mermaid
flowchart LR
    P[Producer] -->|wait empty, wait mutex| B[Shared bounded buffer]
    B -->|signal full| C[Consumer]
    C -->|wait full, wait mutex| B
    B -->|signal empty| P
```

### Why the order matters

The producer first waits for an empty slot, then locks the buffer. The
consumer first waits for a full slot, then locks the buffer. Both
release `mutex` after the buffer operation and update the relevant
counting semaphore.

If a process holds `mutex` while waiting for `empty` or `full`, it can
prevent the other side from entering to create the condition being
awaited. This can lead to deadlock. Therefore, follow the standard
ordering above.

### Worked state example

Suppose `N = 3`, so initially:

``` text
empty = 3, full = 0, mutex = 1
```

After one successful production: - `empty = 2` - `full = 1` -
`mutex = 1` after the operation completes

After one successful consumption: - `empty = 3` - `full = 0` -
`mutex = 1` after the operation completes

The invariant is:

``` text
empty + full = N
```

This invariant assumes the semaphores accurately track the buffer and
each operation completes correctly.

## 10. Monitors --- introduction

A **monitor** is a higher-level synchronization construct that
combines: - Shared data - Procedures that operate on that data -
Automatic mutual exclusion for monitor procedures under the standard
monitor model - Condition variables for waiting until a condition
becomes true

Rather than asking programmers to manage a mutex and several semaphores
manually at every access point, a monitor provides a structured way to
protect shared state.

### Conceptual structure

``` text
monitor Buffer:
    shared buffer data

    procedure insert(item):
        // check capacity and insert safely

    procedure remove():
        // check availability and remove safely
```

Only one thread at a time is permitted to execute inside the monitor in
the classic model. If a required condition is not satisfied, a thread
can wait on a condition variable and allow another thread to enter and
change the state.

### Condition variables

Common conceptual operations include: - `wait(condition)`: suspend until
signalled; typically releases the monitor lock while waiting and
reacquires it before continuing. - `signal(condition)`: notify a waiting
thread that it may be able to proceed.

The precise semantics vary across languages and monitor implementations.
In many practical APIs, a thread must re-check the condition after
waking, often by using a `while` loop.

### Monitor vs semaphore

  -----------------------------------------------------------------------
  Monitor                             Semaphore
  ----------------------------------- -----------------------------------
  Higher-level structured construct   Lower-level synchronization
                                      primitive

  Groups shared data and operations   Maintains a counter/permit state

  Mutual exclusion is typically built Correct locking and signaling order
  into monitor entry                  must be designed explicitly

  Condition variables coordinate      `wait` and `signal` operate on
  waiting for conditions              semaphore permits
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# Part C --- Common mistakes and exam revision

## 11. Common mistakes students should avoid

1.  **Race condition is not the same as deadlock.** A race condition is
    an incorrect timing-dependent outcome. Deadlock is a situation where
    tasks are stuck waiting for one another or for resources.
2.  **A critical section is not the entire program.** It is the specific
    region requiring protected access.
3.  **Mutual exclusion alone is insufficient.** Progress and bounded
    waiting are also classical requirements.
4.  **Peterson's algorithm is for two processes in its basic form.** Its
    correctness relies on classical memory assumptions.
5.  **`wait()` and `signal()` must be atomic semaphore operations.**
    They are not simply ordinary counter changes.
6.  **Binary semaphore and mutex are not always interchangeable.**
    Ownership and API semantics can differ.
7.  **Do not acquire `mutex` and then wait for a full/empty condition
    while holding it** in the standard producer--consumer solution.
8.  **A wake-up does not always guarantee the condition is still true.**
    Re-check condition-variable predicates where required.

## 12. Quick revision sheet

  -----------------------------------------------------------------------
  Term                                One-line meaning
  ----------------------------------- -----------------------------------
  Process synchronization             Coordinating concurrent tasks that
                                      share data/resources

  Race condition                      Result depends on
                                      timing/interleaving

  Critical section                    Code region that accesses protected
                                      shared state

  Mutual exclusion                    At most one task in a protected
                                      critical section at a time

  Progress                            Entry decision is not postponed
                                      unnecessarily when the section is
                                      free

  Bounded waiting                     A requester cannot be bypassed
                                      indefinitely

  Peterson's solution                 Two-process software algorithm
                                      using `flag[]` and `turn`

  Mutex                               Lock commonly used to protect a
                                      critical section

  Semaphore                           Atomic wait/signal mechanism using
                                      a counter

  Binary semaphore                    Semaphore with logical states 0 and
                                      1

  Counting semaphore                  Semaphore that tracks multiple
                                      permits

  Producer--Consumer                  Coordination of adding/removing
                                      items in a bounded buffer

  Monitor                             Structured synchronization
                                      construct with shared state and
                                      procedures
  -----------------------------------------------------------------------

## 13. University-style questions

### Short-answer questions (2--3 marks)

1.  Define process synchronization.
2.  What is a race condition? Give one example.
3.  Define a critical section.
4.  List the three requirements of a critical-section solution.
5.  What is Peterson's solution?
6.  Define mutex.
7.  What are `wait()` and `signal()` in a semaphore?
8.  Differentiate binary and counting semaphores.
9.  What is a monitor?
10. What are `empty`, `full`, and `mutex` in the Producer--Consumer
    problem?

### Long-answer questions (5--10 marks)

1.  Explain the race condition with a suitable example and diagram.
2.  Explain the critical-section problem and its four program sections.
3.  Explain mutual exclusion, progress, and bounded waiting.
4.  Write Peterson's algorithm for two processes and explain how it
    works.
5.  Compare mutexes and semaphores.
6.  Explain binary and counting semaphores with examples.
7.  State the Producer--Consumer problem and give a semaphore-based
    solution.
8.  Explain monitors and condition variables. Compare monitors with
    semaphores.

## 14. Suggested practical/classroom activities

-   **Race condition demonstration:** Use two threads incrementing a
    shared counter; compare an unsynchronized version with a protected
    version.
-   **Peterson dry run:** Let two students represent `P0` and `P1`;
    track `flag[0]`, `flag[1]`, and `turn` on the board.
-   **Semaphore simulation:** Use five tokens to represent five permits.
    Students must acquire a token before using a resource and return it
    afterward.
-   **Producer--Consumer trace:** For buffer capacity 3, have the class
    update `empty`, `full`, and `mutex` after each
    production/consumption.

------------------------------------------------------------------------

## Final takeaway

Synchronization is about preserving correctness when execution overlaps.
Race conditions explain **why** coordination is needed; critical-section
requirements define **what a correct solution must guarantee**;
Peterson's algorithm demonstrates a classical software approach;
mutexes, semaphores, and monitors provide practical synchronization
mechanisms.
