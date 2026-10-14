# Process Synchronization, Critical Section & Synchronization Tools
## Student Notes — Operating Systems

These notes follow the examples and explanations from the process synchronization and critical section lessons, with additional topics on Peterson's solution, mutexes, semaphores, binary/counting semaphores, and the producer–consumer problem.

---

# Lec 21 — Process Synchronization and Critical Section

## 1. Process Synchronization

A modern operating system runs many processes. On a single-CPU system, the CPU can switch quickly between processes; on a multi-core system, processes may also run in parallel.

Some processes are independent, while others cooperate by sharing data or resources.

### Types of processes

**Independent processes**
- The execution of one process does not affect another.
- They do not share data or resources in a way that affects each other's results.
- Example: two processes working with unrelated data.

**Cooperating processes**
- The execution of one process can affect another because they share something.
- They may share a variable, memory, a buffer, code, or a resource such as a printer.
- Example: two processes updating the same `count` variable.

### Why is synchronization needed?

If cooperating processes access and update shared data without coordination, the result may become incorrect. **Process synchronization** coordinates their access so that shared data remains consistent.

Think of an ATM: customers use the same machine in an orderly way. Shared data in a computer also needs controlled access.

### Diagram: Types of processes

```text
                         Processes
                             |
                +------------+------------+
                |                         |
          Independent                Cooperating
           Processes                  Processes
                |                         |
       One does not affect       Share data/resources
          the other              (variable, memory,
                                  buffer, code, device)
                                           |
                                  Need synchronization
```

---

## 2. Race Condition

A **race condition** occurs when the result depends on the order or timing in which concurrent processes access and update shared data.

### Example

Assume a shared variable starts at `5`. Two processes use it:

```text
Process P1                         Process P2
---------                         ---------
X = shared                        Y = shared
X = X + 1                         Y = Y - 1
sleep(1)                          sleep(1)
shared = X                        shared = Y
```

- `shared` is common to both processes.
- `X` and `Y` are local variables.
- P1 adds 1; P2 subtracts 1.
- The expected final value is `5`, because adding 1 and subtracting 1 should cancel out.

### One interleaving that produces the wrong result

Initial value: `shared = 5`

| Step | Process | Operation | Result |
|---:|---|---|---|
| 1 | P1 | `X = shared` | `X = 5` |
| 2 | P1 | `X = X + 1` | `X = 6` |
| 3 | P1 | `sleep(1)` | P1 pauses |
| 4 | P2 | `Y = shared` | `Y = 5` |
| 5 | P2 | `Y = Y - 1` | `Y = 4` |
| 6 | P2 | `sleep(1)` | P2 pauses |
| 7 | P1 | `shared = X` | `shared = 6` |
| 8 | P2 | `shared = Y` | `shared = 4` |

Final value: `4` — incorrect. With a different interleaving, the final value may be `6`. The expected value is `5`.

```text
shared = 5
   |
   +--> P1 reads 5 --> local X = 5 --> increments X to 6 --+
   |                                                       |
   +--> P2 reads 5 --> local Y = 5 --> decrements Y to 4 --+
                                                           |
                                         writes may overlap
                                                           |
                                       final value: 4 or 6
                                       expected value: 5
```

**Note:** `sleep(1)` is used in this example to demonstrate pausing and switching between processes. It is not a synchronization solution by itself.

### Why is it called a race condition?

Both processes are effectively racing to write their result to the shared variable. The final result depends on which write happens last.

**Remember:** Shared data + concurrent access + no proper coordination can lead to a race condition.

---

## 3. Critical Section

A **critical section** is the part of a process's code where it accesses or updates shared data or shared resources.

For example, if P1 and P2 both use the same `count` variable, the instructions that read or update `count` belong in their critical sections. Code that only uses private variables can be part of the non-critical (remainder) section.

### Diagram: Structure of a process

```text
Process P1                         Process P2
+----------------------+           +----------------------+
| Non-critical section |           | Non-critical section |
| private work (x, y)  |           | private work (a, b)  |
+----------------------+           +----------------------+
            |                                  |
            v                                  v
+----------------------+           +----------------------+
|     Entry section    |           |     Entry section    |
| request permission   |           | request permission   |
+----------------------+           +----------------------+
            |                                  |
            v                                  v
+----------------------+           +----------------------+
|   Critical section   |           |   Critical section   |
| shared data/resource |           | shared data/resource |
+----------------------+           +----------------------+
            |                                  |
            v                                  v
+----------------------+           +----------------------+
|      Exit section    |           |      Exit section    |
| release permission   |           | release permission   |
+----------------------+           +----------------------+
            |                                  |
            v                                  v
      remainder work                    remainder work
```

### Four parts of a process structure

1. **Entry section:** Requests permission to enter the critical section. A lock or synchronization mechanism may be used here.
2. **Critical section:** Accesses or modifies shared data/resources.
3. **Exit section:** Releases the lock or signals that the process has left the critical section.
4. **Remainder section:** The rest of the process's work that does not require access to the shared resource.

A process should not enter a shared-data critical section without following the synchronization rules. Otherwise, two processes may update the same data unsafely and cause a race condition.

### Everyday example: One washroom

- The washroom is the critical section.
- Checking/unlocking the door is like the entry section.
- Using the washroom is the critical section.
- Locking/unlocking appropriately when leaving represents releasing access in the exit section.
- Other activities are the remainder section.

---

## 4. Requirements for a Correct Synchronization Solution

A synchronization solution is evaluated using four important requirements.

### 4.1 Mutual Exclusion

At most **one process at a time** may be inside the critical section for a particular shared resource.

Example: If P1 is using the shared resource in its critical section, P2 must wait outside until P1 leaves.

```text
P1: [ ENTRY ] ---> [ CRITICAL SECTION ] ---> [ EXIT ]
                           |
                     resource in use
                           |
P2: [ ENTRY ] ---> WAIT --------------------> [ ENTER later ]
```

If P1 and P2 enter the same protected critical section at the same time, mutual exclusion has been violated.

### 4.2 Progress

If no process is inside the critical section and some processes want to enter, the decision about who enters next should not be delayed indefinitely by processes that are not trying to enter.

Example: The shared resource is free and P1 wants to use it. A process that is not interested in entering should not block P1 unnecessarily.

### 4.3 Bounded Waiting

After a process requests entry, there must be a limit on how many times other processes can enter before that request is granted.

Example: P2 is waiting. P1 should not be allowed to re-enter forever while P2 waits. This prevents indefinite waiting, also called starvation.

### 4.4 No Assumption About Hardware or Relative Speed

A correct general solution should not rely on a particular CPU speed or on one process always being faster than another. It should work based on its synchronization rules, not timing guesses.

### Quick summary

| Requirement | Easy meaning |
|---|---|
| Mutual exclusion | Only one process in the critical section at a time |
| Progress | If the section is free, an interested process can be selected |
| Bounded waiting | A waiting process will not wait forever |
| No hardware/speed assumption | Do not rely on a particular CPU speed or process timing |

---

## 5. Peterson's Solution

Peterson's solution is a classic software method for demonstrating mutual exclusion between **two processes**, usually called P0 and P1. It uses two shared variables:

- `flag[2]`: `flag[i] = true` means process Pi wants to enter its critical section.
- `turn`: indicates which process should wait when both processes want to enter at the same time.

### Pseudocode

```c
// Shared variables
bool flag[2] = {false, false};
int turn;

// Code for process Pi; j is the other process (0 or 1)
do {
    flag[i] = true;  // Pi wants to enter
    turn = j;        // give the other process priority
    while (flag[j] && turn == j) {
        ;             // wait
    }

    // CRITICAL SECTION

    flag[i] = false; // Pi leaves; no longer interested

    // REMAINDER SECTION

} while (true);
```

Here, `i` is the current process and `j` is the other process.

### How it works

1. A process raises its flag to announce that it wants to enter.
2. It sets `turn` to the other process, giving the other process priority if both compete.
3. It waits only when the other process also wants to enter and it is the other process's turn.
4. After leaving the critical section, it lowers its flag.

**Limitation:** The classic solution is for two processes and relies on assumptions about shared-memory operations and memory ordering. Modern systems generally use operating-system or hardware-supported synchronization primitives instead.

---

# Lec 22 — Synchronization Tools

## 6. Mutex

A **mutex** (mutual-exclusion lock) is a synchronization tool that protects a critical section so only one thread/process holding the lock can access the protected section at a time.

### Basic working

```text
          Process wants the resource
                      |
                      v
                 [ lock mutex ]
                    /      \
             Available     Locked
                 |            |
                 v            v
          Enter critical    Wait until
              section       available
                 |
                 v
          Use shared data
                 |
                 v
               unlock
                 |
                 v
          Another can enter
```

### Basic pseudocode

```text
lock(mutex);

// Critical section
shared_count = shared_count + 1;

unlock(mutex);
```

- `lock(mutex)`: acquire the lock; if another process holds it, wait.
- Critical section: access the shared resource safely.
- `unlock(mutex)`: release the lock so another process can acquire it.

### Analogy

Imagine a single key to a room. The person holding the key can enter. Others wait until the key is returned.

**Important:** The program must release the mutex correctly. A mutex protects access; it does not automatically make unrelated code safe.

---

## 7. Semaphores

A **semaphore** is a synchronization tool represented by a counter and accessed through atomic operations. It can coordinate processes and control access to shared resources.

The two classic operations are:

- **`wait(S)`** — also called `P` or acquire: attempts to take a permit. If no permit is available, the process waits.
- **`signal(S)`** — also called `V` or release: returns a permit or wakes a waiting process, depending on the implementation.

Semaphore operations must be atomic, meaning the operation cannot be interrupted halfway through in a way that breaks synchronization.

### Conceptual working

```text
             wait(S)
                |
                v
       Is permission available?
            /          \
          Yes            No
           |              |
           v              v
     Continue into      Wait/block
     protected work     until signalled
           |
           v
        signal(S)
           |
           v
    Release permission
```

### Example: Protecting a critical section with a binary semaphore

```text
semaphore S = 1;

wait(S);

// Critical section
shared_count = shared_count + 1;

signal(S);
```

With an initial value of `1`, one process can acquire permission. Other processes must wait until the permission is released. This is conceptual pseudocode; actual API syntax may differ.

### Mutex vs semaphore

| Mutex | Semaphore |
|---|---|
| Primarily used for mutual exclusion | Used for signalling and/or resource counting |
| Usually has ownership: the thread that locks it should unlock it | Generally does not have the same ownership rule |
| Typically represents locked/unlocked | Has a counter or permits |
| Often protects one critical section | Can control access to one or multiple resource instances |

---

## 8. Binary vs Counting Semaphore

### Binary semaphore

- Its value is restricted to `0` or `1`.
- It can represent one available permission/resource.
- Commonly used for simple signalling or mutual-exclusion-like coordination.
- It is not identical to a mutex because ownership rules may differ.

```text
S = 1  --> permission available
wait(S) --> S becomes 0; one process proceeds
S = 0  --> another process must wait
signal(S) --> permission is released
```

### Counting semaphore

- Its counter represents multiple available instances of a resource.
- It is initialized to the number of available resources.
- A successful `wait` consumes one permit; `signal` returns a permit.

**Example:** A lab has four identical printers. Initialize `printer = 4`. Up to four processes can use a printer at once. When all four are busy, another process waits until one becomes available.

```text
Counting semaphore: printer = 4

P1 acquires --> 3 permits left
P2 acquires --> 2 permits left
P3 acquires --> 1 permit left
P4 acquires --> 0 permits left
P5 requests --> waits
One process finishes and signals --> a permit becomes available
P5 can then proceed
```

### Comparison

| Feature | Binary semaphore | Counting semaphore |
|---|---|---|
| Counter values | 0 or 1 | 0 or more, normally |
| Represents | One permission/resource or a signal | Multiple identical resource instances |
| Initial value | Often 0 or 1, depending on purpose | Usually the number of available resources |
| Example | One shared access permission | Four printers available |
| Waiting | Wait when permission is unavailable | Wait when count reaches zero |

---

## 9. Producer–Consumer Problem Using Semaphores

The **Producer–Consumer Problem** is a classic process synchronization problem. A producer creates items and places them in a shared buffer. A consumer removes items from that buffer.

The processes must be coordinated so:
1. The producer does not insert an item when the buffer is full.
2. The consumer does not remove an item when the buffer is empty.
3. Both processes do not modify the shared buffer unsafely at the same time.

For a basic bounded-buffer solution, use three semaphores:

| Semaphore | Initial value | Purpose |
|---|---:|---|
| `mutex` | 1 | Protects the shared buffer |
| `empty` | N | Counts empty slots |
| `full` | 0 | Counts filled slots |

Here, `N` is the buffer capacity. `mutex` is a binary semaphore; `empty` and `full` are counting semaphores.

### Producer process

```text
while (true) {
    item = produce_item();

    wait(empty);     // wait for an empty slot
    wait(mutex);     // lock buffer access

    insert_item(item);

    signal(mutex);   // release buffer access
    signal(full);    // one more filled slot
}
```

### Consumer process

```text
while (true) {
    wait(full);      // wait for an available item
    wait(mutex);     // lock buffer access

    item = remove_item();

    signal(mutex);   // release buffer access
    signal(empty);   // one more empty slot

    consume_item(item);
}
```

### Diagram

```text
                 PRODUCER
                    |
              produce item
                    |
               wait(empty)
                    |
               wait(mutex)
                    |
                    v
          +---------------------+
          |    SHARED BUFFER    |
          | [item][item][ empty]|
          +---------------------+
                    |
               signal(mutex)
                    |
                signal(full)
                    |
                    v
                 CONSUMER
                    |
                 wait(full)
                    |
                wait(mutex)
                    |
              remove item
                    |
               signal(mutex)
                    |
               signal(empty)
```

### Why are three semaphores needed?

- `mutex`: stops the producer and consumer from modifying the buffer simultaneously.
- `empty`: prevents the producer from inserting into a full buffer.
- `full`: prevents the consumer from removing from an empty buffer.

This is a standard conceptual bounded-buffer solution. Actual implementations use the synchronization API provided by the programming language or operating system.

---

# Quick Revision

- **Process synchronization:** Coordinates cooperating processes that share data/resources.
- **Cooperating processes:** One process can affect another because they share something.
- **Race condition:** Output depends on the order/timing of concurrent access to shared data.
- **Critical section:** Code that accesses shared data or resources.
- **Entry section:** Requests permission to enter the critical section.
- **Exit section:** Releases permission after leaving.
- **Mutual exclusion:** At most one process enters the protected critical section at a time.
- **Progress:** Interested processes should not be blocked indefinitely by processes that are not trying to enter.
- **Bounded waiting:** A request should not be postponed forever.
- **Peterson's solution:** Classic two-process software solution using `flag[]` and `turn`.
- **Mutex:** Lock used to protect a critical section.
- **Semaphore:** Coordination mechanism using atomic `wait` and `signal`.
- **Binary semaphore:** Counter restricted to 0 or 1.
- **Counting semaphore:** Counter represents multiple resource permits.
- **Producer–consumer:** Uses `mutex`, `empty`, and `full` to coordinate a bounded shared buffer.

---

# Practice Questions

1. Define process synchronization. Why is it needed?
2. Differentiate independent and cooperating processes.
3. Explain race condition using the shared variable example.
4. Define a critical section and draw its four-part structure.
5. Explain mutual exclusion, progress, bounded waiting, and the no-hardware/speed-assumption requirement.
6. Explain Peterson's solution and the purpose of `flag[]` and `turn`.
7. What is a mutex? Explain its working with a simple example.
8. Define semaphore. Explain `wait()` and `signal()`.
9. Differentiate binary and counting semaphores with examples.
10. State the Producer–Consumer Problem and give a solution using semaphores.
