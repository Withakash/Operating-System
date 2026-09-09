# Lec 13 : CPU Scheduling Fundamentals

## Operating System – Process Management

---

# 1. Introduction

In a multiprogramming operating system, multiple processes may exist in the system at the same time.

For example, a user may simultaneously run:

* Google Chrome
* VS Code
* A Java program
* Music player
* File manager
* Background system services

However, the CPU cannot execute all processes simultaneously on a single CPU core.

Therefore, the Operating System must decide:

> **Which process should get the CPU and when?**

This decision-making process is called **CPU Scheduling**.

---

# 2. What is CPU Scheduling?

**CPU Scheduling** is the process by which the Operating System selects one process from the **Ready Queue** and allocates the CPU to it.

```text
Ready Queue
     │
     ▼
CPU Scheduler
     │
     ▼
Selected Process
     │
     ▼
    CPU
```

The component responsible for selecting the next process is called the:

> **CPU Scheduler** or **Short-Term Scheduler**

---

# 3. Why Do We Need CPU Scheduling?

A computer system may have multiple processes waiting to execute.

Consider the following processes:

```text
P1 → Web Browser
P2 → VS Code
P3 → Music Player
P4 → Java Program
P5 → Operating System Service
```

All of these processes may need CPU time.

But on a single CPU core:

```text
Only one process can execute at a particular instant.
```

Therefore, the Operating System needs a mechanism to decide:

* Which process should execute first?
* How long should it execute?
* When should another process get the CPU?
* What happens when a process requests I/O?

This mechanism is called **CPU Scheduling**.

---

# 4. Main Objectives of CPU Scheduling

CPU scheduling algorithms are designed to improve system performance.

The major objectives are:

1. CPU Utilization
2. Throughput
3. Turnaround Time
4. Waiting Time
5. Response Time

---

# 5. CPU Utilization

CPU Utilization refers to the percentage of time during which the CPU is busy doing useful work.

The Operating System tries to keep the CPU busy as much as possible.

### Bad Situation

```text
CPU → Working
CPU → Idle
CPU → Idle
CPU → Working
```

This means CPU resources are being wasted.

### Better Situation

```text
CPU → Working
CPU → Working
CPU → Working
CPU → Working
```

The goal is:

> **Keep the CPU busy whenever possible.**

---

# 6. Throughput

**Throughput** refers to the number of processes completed in a given amount of time.

Example:

```text
System A:

10 processes completed in 1 minute
```

```text
System B:

25 processes completed in 1 minute
```

System B has higher throughput.

### Formula

```text
Throughput = Number of Completed Processes / Unit Time
```

---

# 7. Turnaround Time

Turnaround Time is the total time taken by a process from its arrival until its completion.

### Formula

```text
Turnaround Time = Completion Time - Arrival Time
```

### Example

```text
Process Arrival Time = 2 ms
Process Completion Time = 12 ms
```

Therefore:

```text
Turnaround Time = 12 - 2
                = 10 ms
```

---

# 8. Waiting Time

Waiting Time is the total amount of time a process spends waiting in the Ready Queue.

### Formula

```text
Waiting Time = Turnaround Time - CPU Burst Time
```

### Example

```text
Turnaround Time = 20 ms
CPU Burst Time = 5 ms
```

Therefore:

```text
Waiting Time = 20 - 5
             = 15 ms
```

---

# 9. Response Time

Response Time measures how quickly the system gives the first response after receiving a request.

It is especially important in:

* Interactive systems
* Web applications
* Operating systems
* Gaming systems

### Example

```text
User Clicks Application
        │
        ▼
Application Starts Responding
        │
        └──► Response Time
```

Response Time is different from Completion Time.

```text
Response Time
      ↓
First response from the system


Completion Time
      ↓
Entire process finishes
```

---

# 10. CPU Burst and I/O Burst

A process does not continuously use the CPU.

Most processes alternate between:

```text
CPU Burst
    ↓
I/O Burst
    ↓
CPU Burst
    ↓
I/O Burst
```

This is called the **CPU-I/O Burst Cycle**.

---

# 11. What is CPU Burst?

A **CPU Burst** is the period during which a process is actively executing instructions on the CPU.

Example:

```java
int sum = 0;

for(int i = 0; i < 1000000; i++) {
    sum = sum + i;
}
```

During this operation, the processor continuously executes instructions.

This period is called:

> **CPU Burst**

---

# 12. What is I/O Burst?

An **I/O Burst** is the period during which a process performs or waits for an Input/Output operation.

Examples of I/O operations:

* Reading from disk
* Writing to disk
* Keyboard input
* Mouse input
* Network communication
* Database access
* Printer operations

Example:

```java
Scanner sc = new Scanner(System.in);

int number = sc.nextInt();
```

The process may wait for the user to enter a value.

This waiting period is related to an:

> **I/O Burst**

---

# 13. CPU-I/O Burst Cycle

A typical process follows this cycle:

```text
        ┌────────────┐
        │ CPU Burst  │
        └─────┬──────┘
              │
              ▼
        ┌────────────┐
        │ I/O Burst  │
        └─────┬──────┘
              │
              ▼
        ┌────────────┐
        │ CPU Burst  │
        └─────┬──────┘
              │
              ▼
        ┌────────────┐
        │ I/O Burst  │
        └────────────┘
```

The Operating System takes advantage of this behavior.

When one process is waiting for I/O:

```text
P1 → Waiting for Disk
```

The CPU can execute another process:

```text
CPU → Execute P2
```

This improves CPU utilization.

---

# 14. CPU-Bound Process

A **CPU-Bound Process** spends most of its time performing CPU operations.

Characteristics:

* Long CPU bursts
* Less frequent I/O
* Heavy computation

Examples:

* Video rendering
* Scientific calculations
* Large mathematical computations
* Machine learning processing

```text
CPU → CPU → CPU → CPU
```

---

# 15. I/O-Bound Process

An **I/O-Bound Process** spends a significant amount of time performing or waiting for I/O operations.

Characteristics:

* Short CPU bursts
* Frequent I/O operations
* Frequently enters the waiting state

Examples:

* Web browser
* File downloading
* Database applications
* User input programs

```text
CPU → I/O → CPU → I/O → CPU
```

---

# 16. CPU-Bound vs I/O-Bound Process

| CPU-Bound Process        | I/O-Bound Process           |
| ------------------------ | --------------------------- |
| Uses CPU heavily         | Frequently performs I/O     |
| Long CPU bursts          | Short CPU bursts            |
| Less frequent I/O        | Frequent I/O                |
| Heavy computation        | Heavy input/output activity |
| Example: Video rendering | Example: Web browser        |

A good operating system tries to maintain a balanced mixture of CPU-bound and I/O-bound processes.

---

# 17. Scheduling Queues

The Operating System maintains different queues to organize processes.

A process moves between different queues depending on its current state.

The major queues are:

1. Job Queue
2. Ready Queue
3. Device Queue

---

# 18. Job Queue

The **Job Queue** contains processes submitted to the system.

```text
JOB QUEUE

P1 → P2 → P3 → P4 → P5
```

These processes may wait before being admitted for execution.

The Long-Term Scheduler is associated with selecting jobs for the system.

---

# 19. Ready Queue ⭐

The **Ready Queue** contains processes that are:

* In memory
* Ready to execute
* Waiting for CPU allocation

```text
READY QUEUE

┌────┐    ┌────┐    ┌────┐    ┌────┐
│ P1 │ → │ P2 │ → │ P3 │ → │ P4 │
└────┘    └────┘    └────┘    └────┘
```

The Short-Term Scheduler selects a process from the Ready Queue.

```text
READY QUEUE
      │
      ▼
SHORT-TERM SCHEDULER
      │
      ▼
     CPU
```

---

# 20. Device Queue / I/O Queue

Processes waiting for an I/O device are placed in a Device Queue.

Example:

```text
DISK QUEUE

P1 → P3 → P7
```

Another device may have its own queue:

```text
PRINTER QUEUE

P2 → P5
```

When the I/O operation is completed:

```text
I/O Complete
     │
     ▼
Ready Queue
```

---

# 21. Complete Process Movement

```text
                NEW PROCESS
                     │
                     ▼
                ┌──────────┐
                │ JOB QUEUE│
                └────┬─────┘
                     │
                     ▼
                ┌──────────┐
                │ READY    │
                │ QUEUE    │
                └────┬─────┘
                     │
                     ▼
              CPU SCHEDULER
                     │
                     ▼
                    CPU
                     │
        ┌────────────┼────────────┐
        │            │            │
        ▼            ▼            ▼
   TERMINATED     I/O REQUEST   PREEMPTED
                     │            │
                     ▼            │
                 I/O QUEUE        │
                     │            │
                     │            │
                     └────────────┴────► READY QUEUE
```

---

# 22. Schedulers

The Operating System uses different schedulers to manage processes.

There are three major schedulers:

1. Long-Term Scheduler
2. Short-Term Scheduler
3. Medium-Term Scheduler

---

# 23. Long-Term Scheduler

The **Long-Term Scheduler** is also called the:

> **Job Scheduler**

It selects processes from the Job Queue and admits them into the system.

```text
JOB QUEUE

P1  P2  P3  P4  P5
          │
          ▼
LONG-TERM SCHEDULER
          │
          ▼
READY QUEUE
```

---

## Main Responsibility

The Long-Term Scheduler controls the:

> **Degree of Multiprogramming**

### Degree of Multiprogramming

It refers to the number of processes currently present in memory.

Example:

```text
100 Processes Submitted

Only 10 Processes Allowed
Into Main Memory
```

The Long-Term Scheduler controls how many processes enter the system.

---

## Frequency

The Long-Term Scheduler runs:

```text
Infrequently
```

because new jobs do not need to be admitted every few milliseconds.

---

# 24. Short-Term Scheduler ⭐⭐⭐

The **Short-Term Scheduler** is also called:

* CPU Scheduler

It selects the next process from the Ready Queue and allocates the CPU.

```text
READY QUEUE

P1 → P2 → P3 → P4
          │
          ▼
SHORT-TERM SCHEDULER
          │
          ▼
         CPU
```

---

## Why Must It Be Fast?

The Short-Term Scheduler runs very frequently.

It may run when:

* A process finishes
* A process requests I/O
* An I/O operation completes
* A process is interrupted
* A time quantum expires

Therefore:

> **The Short-Term Scheduler must be extremely fast.**

---

# 25. Scheduling Algorithms

The Short-Term Scheduler uses scheduling algorithms to decide which process should execute next.

Major algorithms include:

1. FCFS
2. SJF
3. SRTF
4. Priority Scheduling
5. Round Robin
6. Multilevel Queue Scheduling
7. Multilevel Feedback Queue Scheduling

These algorithms will be studied in the upcoming lectures.

---

# 26. Medium-Term Scheduler

The **Medium-Term Scheduler** is responsible for temporarily suspending and later resuming processes.

It is closely related to:

> **Swapping**

---

# 27. Swapping

When memory becomes overloaded, the Operating System may temporarily remove a process from main memory.

This is called:

> **Swap Out**

```text
MAIN MEMORY
      │
      │ Swap Out
      ▼
SECONDARY STORAGE
```

Later, the process may return:

> **Swap In**

```text
SECONDARY STORAGE
      │
      │ Swap In
      ▼
MAIN MEMORY
```

---

# 28. Why Do We Need Medium-Term Scheduling?

Suppose:

```text
RAM = Limited

Processes = Too Many
```

The Operating System can temporarily suspend some processes.

```text
P1 → Running
P2 → Ready
P3 → Suspended
P4 → Ready
```

This helps manage memory and reduces the degree of multiprogramming.

---

# 29. Comparison of Schedulers

| Feature        | Long-Term                  | Short-Term           | Medium-Term                |
| -------------- | -------------------------- | -------------------- | -------------------------- |
| Other Name     | Job Scheduler              | CPU Scheduler        | Swapper                    |
| Main Work      | Admits processes           | Selects next process | Suspends/Resumes processes |
| Related Queue  | Job Queue                  | Ready Queue          | Suspended Processes        |
| Frequency      | Low                        | Very High            | Medium                     |
| Speed Required | Moderate                   | Very Fast            | Moderate                   |
| Main Control   | Degree of Multiprogramming | CPU Allocation       | Memory Management          |

---

# 30. Scheduler vs Dispatcher ⭐⭐⭐

Students often confuse these two concepts.

They are different.

## Scheduler

The Scheduler decides:

> **Which process should run next?**

Example:

```text
Ready Queue:

P1 → P2 → P3

Scheduler selects:

P2
```

---

## Dispatcher

The Dispatcher performs the actual switching.

It gives CPU control to the selected process.

```text
Scheduler
    │
    │ Select P2
    ▼
Dispatcher
    │
    │ Give CPU to P2
    ▼
P2 Running
```

---

# 31. Dispatcher Responsibilities

The Dispatcher is responsible for:

1. Context Switching
2. Switching to User Mode
3. Jumping to the correct instruction of the selected process

---

# 32. Dispatch Latency

The time taken by the Dispatcher to stop one process and start another process is called:

> **Dispatch Latency**

```text
P1 Running
     │
     ▼
Stop P1
     │
     ▼
Save P1 Context
     │
     ▼
Load P2 Context
     │
     ▼
Start P2
```

The time required for this entire operation is:

```text
Dispatch Latency
```

Lower dispatch latency generally improves system performance.

---

# 33. Context Switching ⭐⭐⭐

A **Context Switch** happens when the CPU switches from one process to another.

Example:

```text
CPU is executing P1
        │
        ▼
Interrupt Occurs
        │
        ▼
Save P1 Information
        │
        ▼
Load P2 Information
        │
        ▼
CPU Executes P2
```

---

# 34. What is Process Context?

The **Context** of a process is the information required to stop the process and later resume it correctly.

This information is generally stored in the:

> **PCB – Process Control Block**

---

# 35. Information Stored During Context Switching

Important information may include:

* Program Counter
* CPU Registers
* Stack Pointer
* Process State
* Scheduling Information
* Memory Management Information

Example:

```text
PCB

┌────────────────────────────┐
│ Process ID                 │
│ Process State              │
│ Program Counter            │
│ CPU Registers              │
│ Stack Pointer              │
│ Scheduling Information     │
│ Memory Information         │
└────────────────────────────┘
```

---

# 36. Context Switching Process

```text
             P1 RUNNING
                  │
                  │ Interrupt
                  ▼
        ┌──────────────────┐
        │ Save P1 Context  │
        │    into PCB      │
        └────────┬─────────┘
                 │
                 ▼
          Scheduler Selects
                 │
                 ▼
                P2
                 │
                 ▼
        ┌──────────────────┐
        │ Load P2 Context  │
        │   from PCB       │
        └────────┬─────────┘
                 │
                 ▼
             P2 RUNNING
```

---

# 37. Simple Context Switching Example

Suppose Process P1 is executing:

```java
int a = 10;
int b = 20;
int sum = a + b;
```

At a particular moment:

```text
Program Counter = Instruction 50

Register R1 = 10
Register R2 = 20
```

Now the Operating System decides to execute another process.

Before stopping P1, the OS saves:

```text
Program Counter = 50

R1 = 10

R2 = 20
```

Later, when P1 gets the CPU again, the Operating System restores this information.

P1 can continue from exactly where it stopped.

---

# 38. Context Switching is Overhead

During a Context Switch, the CPU is not performing useful work for the user process.

The CPU spends time:

```text
Saving Process State
        +
Loading Another Process State
        +
Switching Execution
```

Therefore:

> **Context Switching creates overhead.**

---

# 39. Too Many Context Switches

Suppose:

```text
P1 executes for 1 ms
Switch takes 0.5 ms

P2 executes for 1 ms
Switch takes 0.5 ms
```

A large amount of CPU time is spent switching between processes.

Therefore:

```text
More Context Switching
        ↓
More Overhead
        ↓
Lower Efficiency
```

---

# 40. Context Switching and Time Quantum

This concept becomes very important in **Round Robin Scheduling**.

Suppose:

```text
Time Quantum = 100 ms
```

Processes switch less frequently.

But if:

```text
Time Quantum = 1 ms
```

Processes switch very frequently.

Therefore:

```text
Small Time Quantum
        │
        ├──► Better Responsiveness
        │
        └──► More Context Switching
                  │
                  ▼
                Overhead
```

The Operating System must maintain a balance between:

```text
Responsiveness

        VS

Context Switching Overhead
```

---

# 41. Complete CPU Scheduling Flow

```text
                    NEW PROCESSES
                         │
                         ▼
                   ┌───────────┐
                   │ JOB QUEUE │
                   └─────┬─────┘
                         │
                 LONG-TERM SCHEDULER
                         │
                         ▼
                   ┌───────────┐
                   │ READY     │
                   │ QUEUE     │
                   └─────┬─────┘
                         │
                SHORT-TERM SCHEDULER
                         │
                         ▼
                   ┌───────────┐
                   │ DISPATCHER│
                   └─────┬─────┘
                         │
                         ▼
                        CPU
                         │
           ┌─────────────┼─────────────┐
           │             │             │
           ▼             ▼             ▼
       TERMINATED    I/O REQUEST    PREEMPTED
                         │             │
                         ▼             │
                     I/O QUEUE         │
                         │             │
                         └─────────────┴──────► READY QUEUE


             MEDIUM-TERM SCHEDULER

                         │
                         ▼

              Suspend / Swap Out Process

                         │
                         ▼

                 SECONDARY STORAGE

                         │
                         ▼

                    Swap In Process

                         │
                         ▼

                    READY QUEUE
```

---

# 42. Important Differences

## CPU Scheduler vs Dispatcher

| CPU Scheduler                    | Dispatcher                            |
| -------------------------------- | ------------------------------------- |
| Selects the next process         | Gives CPU control to selected process |
| Makes the decision               | Performs the switching                |
| Works with scheduling algorithms | Performs context switching            |

---

## CPU Burst vs I/O Burst

| CPU Burst                              | I/O Burst                                |
| -------------------------------------- | ---------------------------------------- |
| Process uses CPU                       | Process performs/waits for I/O           |
| Executing instructions                 | Waiting for input/output                 |
| CPU-bound processes have longer bursts | I/O-bound processes have frequent bursts |

---

# 43. Quick Revision

| Topic                 | Meaning                               |
| --------------------- | ------------------------------------- |
| CPU Scheduling        | Selecting the next process for CPU    |
| CPU Burst             | Time spent executing on CPU           |
| I/O Burst             | Time spent performing/waiting for I/O |
| Job Queue             | Submitted processes                   |
| Ready Queue           | Processes waiting for CPU             |
| Device Queue          | Processes waiting for an I/O device   |
| Long-Term Scheduler   | Admits processes into the system      |
| Short-Term Scheduler  | Selects the next process for CPU      |
| Medium-Term Scheduler | Suspends and resumes processes        |
| Dispatcher            | Transfers CPU control                 |
| Context Switch        | Switch from one process to another    |
| PCB                   | Stores process-related information    |
| Dispatch Latency      | Time required to perform dispatching  |

---

# 44. Important Exam Questions

### Short Questions

1. What is CPU Scheduling?
2. What is a CPU Burst?
3. What is an I/O Burst?
4. What is the Ready Queue?
5. What is a Job Queue?
6. What is a Device Queue?
7. What is Dispatch Latency?
8. What is Context Switching?
9. What is a CPU-Bound Process?
10. What is an I/O-Bound Process?

---

### Long Questions

1. Explain the need for CPU Scheduling in an Operating System.
2. Explain CPU Burst and I/O Burst with suitable examples.
3. Explain different Scheduling Queues in an Operating System.
4. Explain Long-Term, Short-Term, and Medium-Term Schedulers.
5. Differentiate between Scheduler and Dispatcher.
6. Explain Context Switching with a suitable diagram.
7. Explain Dispatch Latency and its importance.
8. Compare CPU-Bound and I/O-Bound processes.

---

# 45. Key Takeaway ⭐

The complete idea of CPU Scheduling can be summarized as:

```text
Multiple Processes
        │
        ▼
Processes Wait in Ready Queue
        │
        ▼
CPU Scheduler Selects a Process
        │
        ▼
Dispatcher Gives CPU Control
        │
        ▼
Process Executes
        │
        ├──► Completes → Terminated
        │
        ├──► Requests I/O → Waiting
        │
        └──► Preempted → Ready Queue
```

The main goal is:

> **Keep the CPU efficiently utilized while providing fair and responsive execution to processes.**

---

# Next Lecture

## CPU Scheduling Algorithms

We will now study how the CPU Scheduler decides **which process should execute next**.

Topics:

1. FCFS Scheduling
2. SJF Scheduling
3. SRTF Scheduling
4. Priority Scheduling
5. Round Robin Scheduling

> **First understand the fundamentals. Then the scheduling algorithms become much easier to learn.**
