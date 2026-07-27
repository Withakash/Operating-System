# Process Management - Part 1 Notes
# Program vs Process | Process Lifecycle | Threads | Process vs Thread | Process States & Thread States

> **Subject:** Operating System
> **Module:** Process Management
> **Level:** Beginner to Industry Ready
> **Teaching Style:** Theory + Practical + Linux Commands + Java Examples + Interview Questions

---

# Learning Outcomes

After completing this chapter, you will be able to:

- Understand what a Program is.
- Understand what a Process is.
- Differentiate Program vs Process.
- Explain Process Lifecycle.
- Understand what Threads are.
- Differentiate Process vs Thread.
- Understand Process States.
- Understand Thread States.
- Observe running processes practically in Linux and Java.

---

# Real Life Analogy

Imagine a restaurant.

- Recipe Book = Program
- Chef cooking using recipe = Process
- Multiple chefs working on different dishes = Threads

A recipe book does nothing until someone starts cooking.

Similarly,

A **Program** does nothing until it is executed.

---

# What is a Program?

## Definition

A Program is a collection of instructions stored on a storage device.

It is a passive entity.

It does not execute by itself.

Examples

- Chrome.exe
- VLC.exe
- Java Compiler
- Calculator App
- MS Word

These files are simply stored on disk.

Until we execute them, they consume only storage.

---

## Characteristics of Program

- Stored in HDD/SSD
- Static
- Passive
- No CPU allocation
- No RAM allocation
- Cannot perform computation alone

---

## Example

```
calculator.exe
```

This is only a file.

Nothing happens until you double click it.

---

# What is a Process?

## Definition

A Process is a program in execution.

When a program starts executing, Operating System creates a Process.

It becomes an active entity.

---

## Example

Double click Chrome.

Operating System loads

- code
- libraries
- memory
- stack
- heap

and creates a process.

Now Chrome is executing.

---

## Components of Process

A process consists of

```
+----------------------------+
| Program Code (Text)        |
+----------------------------+
| Global Variables (Data)    |
+----------------------------+
| Heap                       |
| Dynamic Memory             |
+----------------------------+
| Stack                      |
| Function Calls             |
+----------------------------+
| Registers                  |
+----------------------------+
| Program Counter            |
+----------------------------+
```

---

# Process in Memory

```
RAM

+------------------------------------+
| Process A                          |
|                                    |
| Code                               |
| Data                               |
| Heap                               |
| Stack                              |
+------------------------------------+

+------------------------------------+
| Process B                          |
+------------------------------------+

+------------------------------------+
| Process C                          |
+------------------------------------+
```

Each process occupies separate memory.

---

# Program vs Process

| Feature | Program | Process |
|----------|----------|----------|
| Meaning | Set of Instructions | Program in Execution |
| Nature | Passive | Active |
| Stored In | HDD / SSD | RAM |
| CPU | No | Yes |
| Memory | No | Yes |
| State | Static | Dynamic |
| Scheduling | No | Yes |
| Resources | No | Uses CPU, Memory, Files |

---

# Example

Program

```
Photoshop.exe
```

↓

User opens Photoshop

↓

Operating System creates Process

```
Photoshop.exe

↓

PID : 2145
RAM : 450 MB
CPU : Running
```

---

# Practical Example

Windows

Open Task Manager

```
Ctrl + Shift + Esc
```

Observe

Chrome

One program

Many processes

---

Linux

```
ps
```

or

```
ps -ef
```

or

```
top
```

Observe

```
PID
CPU
Memory
State
```

---

# Multiple Processes Example

Program

```
chrome.exe
```

User opens

```
Tab 1
Tab 2
Tab 3
```

Operating System may create

```
Chrome Process 1

Chrome Process 2

Chrome Process 3

GPU Process

Extension Process
```

One Program

↓

Many Processes

---

# What is a Process Control Block (PCB)?

Whenever OS creates a process,

it creates one data structure called

PCB

(Process Control Block)

PCB stores all information about process.

---

## PCB Contains

- PID
- Process State
- Program Counter
- CPU Registers
- Memory Information
- Scheduling Information
- Priority
- Open Files
- I/O Status

---

## PCB Diagram

```
+----------------------+
| PID                  |
+----------------------+
| State                |
+----------------------+
| Program Counter      |
+----------------------+
| CPU Registers        |
+----------------------+
| Scheduling Info      |
+----------------------+
| Memory Info          |
+----------------------+
| File Info            |
+----------------------+
```

---

# Process Lifecycle

Every process goes through several stages.

---

## Diagram

```
                New
                 |
                 ▼
              Ready
                 |
                 ▼
             Running
            /   |    \
           /    |     \
          ▼     ▼      ▼
 Waiting  Exit  Ready
```

---

# 1 New State

Process has just been created.

OS allocates PCB.

Memory allocation starts.

---

Example

```
Double Click Calculator
```

Calculator enters

```
New
```

---

# 2 Ready State

Process is ready to execute.

Waiting for CPU.

CPU not assigned yet.

---

Example

Many applications waiting.

Only one CPU.

Scheduler decides.

---

```
Ready Queue

Chrome

VS Code

Spotify

Calculator
```

---

# 3 Running State

CPU assigned.

Instructions executing.

---

Example

You type in Word.

Word process is Running.

---

# 4 Waiting (Blocked)

Process waiting for

- Keyboard
- Mouse
- Disk
- Network
- File
- Printer

---

Example

Downloading file

Waiting for network

---

# 5 Terminated

Execution finished.

Resources released.

PCB deleted.

---

Example

Close Calculator

↓

Exit

---

# Complete Lifecycle Diagram

```
              New
               |
               ▼
            Ready
               |
          Scheduler
               |
               ▼
            Running
          /    |     \
         /     |      \
        ▼      ▼       ▼
 Waiting Exit Ready
        |
        |
        ▼
      Ready
```

---

# Practical Observation

Linux

Run

```
sleep 20
```

Open another terminal

```
ps -ef
```

Observe process.

Kill process

```
kill PID
```

Observe terminated.

---

# What is a Thread?

## Definition

A Thread is the smallest unit of execution inside a process.

A process may contain

- one thread
- multiple threads

---

Example

Chrome

One process

Contains

```
UI Thread

Rendering Thread

Network Thread

Audio Thread
```

---

# Real Life Example

Restaurant

Restaurant = Process

Workers = Threads

All workers

Share

Kitchen

Gas

Utensils

Fridge

Similarly

Threads share

Memory

Heap

Files

Resources

---

# Single Thread Process

```
Process

+----------------------+
| Thread               |
+----------------------+
```

---

# Multithread Process

```
+--------------------------------+

          Process

+--------------------------------+

Thread 1

Thread 2

Thread 3

Thread 4

Shared Memory

Shared Heap

Open Files

+--------------------------------+
```

---

# Why Threads?

Without threads

Application freezes.

With threads

UI remains responsive.

---

Example

Downloading movie

Without Thread

```
Application Freezes
```

With Threads

```
UI Thread

Download Thread

Audio Thread

Notification Thread
```

Everything works simultaneously.

---

# Java Example

Single Thread

```java
public class Demo {

    public static void main(String[] args) {

        System.out.println("Main Thread");

    }

}
```

---

Creating Thread

```java
class MyThread extends Thread {

    public void run() {

        System.out.println("Thread Running");

    }

}

public class Demo {

    public static void main(String[] args) {

        MyThread t = new MyThread();

        t.start();

    }

}
```

---

# Process vs Thread

| Feature | Process | Thread |
|----------|----------|----------|
| Definition | Program in execution | Smallest execution unit |
| Memory | Separate | Shared |
| Communication | IPC Required | Easy |
| Context Switch | Costly | Faster |
| Isolation | High | Low |
| Failure | Independent | Can affect process |
| Resource Sharing | No | Yes |

---

# Example

Chrome

```
Chrome Process

├── UI Thread

├── Rendering Thread

├── GPU Thread

├── Audio Thread

└── Download Thread
```

---

# Process States

Operating systems maintain state for every process.

---

## Five State Model

```
New

↓

Ready

↓

Running

↓

Waiting

↓

Ready

↓

Running

↓

Terminated
```

---

# State Explanation

## New

Created

---

## Ready

Waiting for CPU

---

## Running

Currently executing

---

## Waiting

Waiting for I/O

---

## Terminated

Execution completed

---

# Thread States (Java)

Java defines several thread states.

---

## NEW

Created

Not started

```java
Thread t = new Thread();
```

---

## RUNNABLE

Ready or Running

```java
t.start();
```

---

## BLOCKED

Waiting to acquire lock

---

## WAITING

Waiting indefinitely

```java
wait();
join();
```

---

## TIMED_WAITING

Waiting for specific time

```java
Thread.sleep(5000);
```

---

## TERMINATED

Finished execution

---

# Java Thread Lifecycle

```
NEW

↓

RUNNABLE

↓

RUNNING

↓

BLOCKED

↓

RUNNABLE

↓

TERMINATED
```

---

# Practical Linux Commands

## Show Processes

```bash
ps
```

---

## Detailed Processes

```bash
ps -ef
```

---

## Live Monitoring

```bash
top
```

---

## Better Monitor

```bash
htop
```

---

## Kill Process

```bash
kill PID
```

---

## Show Current Process

```bash
echo $$
```

---

## Background Process

```bash
sleep 100 &
```

---

## List Background Jobs

```bash
jobs
```

---

# Practical Activity 1

Open

```
Calculator

Chrome

VS Code
```

Observe

Task Manager

Questions

- How many processes?
- Which consumes most memory?
- Which consumes most CPU?

---

# Practical Activity 2

Linux

```
sleep 60 &
```

Find PID

```
ps
```

Kill

```
kill PID
```

Observe

Process disappears.

---

# Practical Activity 3 (Java)

Create

5 Threads

Print

```
Current Thread Name
```

Observe

Execution order.

---

# Interview Questions

### Q1 What is Program?

A collection of instructions stored on secondary storage.

---

### Q2 What is Process?

Program in execution.

---

### Q3 Can one Program create multiple Processes?

Yes.

Example

Chrome

---

### Q4 Can one Process have multiple Threads?

Yes.

---

### Q5 Why Threads are faster?

Because they share memory and have lower context-switch overhead.

---

### Q6 Which is expensive?

Process Creation

because

Separate memory allocation

Separate PCB

Separate address space

---

### Q7 Why do browsers use multiple processes and threads?

To improve security, stability, responsiveness, and performance. If one tab crashes, the others can continue running.

---

# Common Mistakes

❌ Program and Process are the same.

✔ A Program is passive; a Process is an executing instance of that program.

❌ One Process has only one Thread.

✔ Modern applications often use many threads.

❌ Waiting State means CPU is executing.

✔ Waiting means the process is blocked until an event (I/O, network, etc.) occurs.

❌ Threads have separate memory.

✔ Threads within the same process share heap, code, and open resources, but each has its own stack and program counter.

---

# Summary

- **Program** = Static instructions stored on disk.
- **Process** = Active program executing in memory.
- **PCB** stores all information about a process.
- Every process follows the lifecycle: **New → Ready → Running → Waiting → Ready → Running → Terminated**.
- **Thread** is the smallest unit of execution inside a process.
- Multiple threads share the same process resources but execute independently.
- **Processes** provide isolation; **Threads** provide lightweight parallelism.
- Linux tools like `ps`, `top`, `htop`, and `kill` help observe and manage processes.
- Java provides thread support using the `Thread` class and the `Runnable` interface.







# Operating System - Process Management (Part 1)

> **Module:** Process Management  
> **Part 1 Topics Covered**
>
> - What is a Thread?
> - Process vs Thread
> - Introduction to Process States & Thread States
>
> **Level:** Beginner → Advanced
>
> **Language Used:** Theory + Practical + Interview + Java Examples

---

# Table of Contents

1. What is a Thread?
2. Why Threads?
3. Thread Memory Structure
4. Thread Components
5. User Threads vs Kernel Threads
6. Multithreading
7. Concurrency vs Parallelism
8. Process vs Thread
9. Process Memory Layout
10. Thread Memory Layout
11. Process vs Thread Comparison
12. Process Lifecycle
13. Process States
14. Thread States
15. Java Thread States
16. Practical Examples
17. Java Programs
18. Interview Questions
19. Summary

---

# Chapter 1 : What is a Thread?

## Definition

A **Thread** is the **smallest unit of execution** inside a process.

A process can contain one or more threads.

A thread performs actual execution of instructions.

> **Remember**
>
> Process = Resource Container
>
> Thread = Execution Unit

---

## Real Life Example

### Restaurant

```
Restaurant (Process)

├── Chef (Thread)

├── Waiter (Thread)

├── Cashier (Thread)

└── Cleaner (Thread)
```

Everything is shared

- Kitchen
- Electricity
- Ingredients

Each worker performs different work.

---

## Another Example

Chrome Browser

```
Chrome Process

│

├── UI Thread

├── JavaScript Thread

├── Download Thread

├── Audio Thread

├── Rendering Thread

└── Network Thread
```

One application

Multiple threads

---

# Why Threads?

Suppose downloading a 5GB file.

Without Threads

```
Download Starts

↓

Application Freezes

↓

Cannot Click Anything

↓

Wait
```

Poor User Experience.

---

With Threads

```
Main Thread

↓

User Interface

------------------

Download Thread

↓

Downloading

------------------

Background Thread

↓

Auto Save
```

Everything works simultaneously.

---

# Thread Memory Structure

Threads share

```
Code

Heap

Global Variables

Files

Sockets
```

Each thread owns

```
Stack

Registers

Program Counter

Thread State
```

---

## Memory Diagram

```
+--------------------------------+

Process

---------------------------------

Code

Heap

Global Variables

---------------------------------

Thread 1 Stack

Thread 2 Stack

Thread 3 Stack

+--------------------------------+
```

---

# Why Separate Stack?

Example

```java
Thread A

int x = 10;
```

```java
Thread B

int x = 50;
```

If stack were shared

Variables would overwrite each other.

Hence

Every thread has its own stack.

---

# Components of Thread

Every thread contains

- Thread ID
- Program Counter
- Registers
- Stack
- Thread State

---

# User Threads vs Kernel Threads

## User Thread

Managed by Application.

Examples

- Java Thread
- POSIX Thread

Advantages

- Faster creation
- Less OS overhead

---

## Kernel Thread

Managed by Operating System.

Advantages

- Better CPU utilization
- Can run on multiple cores

---

# Multithreading

Running multiple threads inside one process.

Example

```
Music Player

├── UI Thread

├── Audio Thread

├── Lyrics Thread

└── Download Thread
```

---

# Single Thread vs Multi Thread

## Single Thread

```
Task A

↓

Task B

↓

Task C
```

Everything waits.

---

## Multi Thread

```
Thread 1 → Task A

Thread 2 → Task B

Thread 3 → Task C
```

Runs concurrently.

---

# Concurrency vs Parallelism

## Concurrency

One CPU

```
A

↓

B

↓

A

↓

B
```

Looks simultaneous.

---

## Parallelism

Multiple CPU Cores

```
Core 1 → Task A

Core 2 → Task B

Core 3 → Task C
```

Actually simultaneous.

---

# Advantages of Threads

- Faster execution
- Better CPU utilization
- Better responsiveness
- Lower memory usage
- Shared memory communication

---

# Disadvantages

- Race Condition
- Deadlock
- Difficult debugging
- Synchronization issues

---

# Java Example

```java
class MyThread extends Thread {

    @Override
    public void run() {

        System.out.println("Thread Running");

    }

    public static void main(String[] args) {

        MyThread t = new MyThread();

        t.start();

    }

}
```

---

# Runnable Example

```java
class Task implements Runnable {

    @Override
    public void run() {

        System.out.println("Task Running");

    }

    public static void main(String[] args) {

        Thread t = new Thread(new Task());

        t.start();

    }

}
```

---

# Practical Activity

Linux / macOS

```bash
top
```

or

```bash
htop
```

Observe

- CPU Usage
- Running Processes
- Memory Usage

---

# Java Practical

```java
public class Demo {

    public static void main(String[] args) {

        Thread t = Thread.currentThread();

        System.out.println(t.getName());

    }

}
```

Output

```
main
```

---

# Chapter 2 : Process vs Thread

---

# Process

A process is an independent program in execution.

Examples

- Chrome
- VS Code
- Spotify
- Word

Each process has

- Own Memory
- Heap
- Stack
- Resources

---

# Thread

Smallest execution unit inside a process.

A process may contain many threads.

---

# Memory Layout

## Process

```
+---------------------------+

Code

Heap

Stack

Data

+---------------------------+
```

---

## Threads

```
+----------------------------------------+

Code (Shared)

Heap (Shared)

Global Data (Shared)

-----------------------------------------

Thread 1 Stack

Thread 2 Stack

Thread 3 Stack

+----------------------------------------+
```

---

# Resource Sharing

## Process

Separate

- Memory
- Heap
- Variables

Need IPC.

---

## Thread

Shared

- Heap
- Code
- Files
- Resources

Private

- Stack
- Registers
- Program Counter

---

# Process vs Thread

| Feature | Process | Thread |
|----------|----------|---------|
| Execution Unit | Independent | Inside Process |
| Memory | Separate | Shared |
| Heap | Separate | Shared |
| Stack | Separate | Separate |
| Creation | Slow | Fast |
| Context Switch | Slow | Fast |
| Communication | IPC | Shared Memory |
| Failure | Independent | Can affect process |
| Memory Usage | High | Low |

---

# Context Switching

## Process Switching

Save

- PCB
- Registers
- Memory Mapping
- Cache

Heavy operation.

---

## Thread Switching

Save

- Registers
- Program Counter
- Stack Pointer

Much faster.

---

# Java Example

```java
class Download extends Thread {

    public void run() {

        for(int i=1;i<=5;i++)
            System.out.println("Downloading");

    }

}

class Music extends Thread {

    public void run() {

        for(int i=1;i<=5;i++)
            System.out.println("Playing");

    }

}

public class Demo {

    public static void main(String[] args) {

        new Download().start();

        new Music().start();

    }

}
```

Possible Output

```
Downloading

Playing

Downloading

Playing
```

---

# Advantages of Process

- Better Isolation
- Secure
- Independent

---

# Advantages of Thread

- Faster
- Less Memory
- Better Performance

---

# Disadvantages

## Process

- Slow
- Heavy
- High Memory

---

## Thread

- Race Condition
- Deadlock
- Synchronization Problems

---

# Chapter 3 : Process States & Thread States

---

# Why Process States?

CPU cannot execute all processes simultaneously.

OS Scheduler decides

- Who Runs
- Who Waits
- Who Stops

---

# Process Lifecycle

```
              New

               │

               ▼

             Ready

               │

               ▼

            Running

          /     |      \

         /      |       \

 Waiting      Ready   Terminated

      │

      ▼

    Ready
```

---

# Process States

## 1. New

Process is created.

OS

- Creates PCB
- Allocates Memory
- Loads Program

---

## 2. Ready

Everything prepared.

Waiting for CPU.

Stored inside Ready Queue.

---

## 3. Running

Currently executing on CPU.

---

## 4. Waiting / Blocked

Waiting for

- Keyboard
- Mouse
- File
- Network
- Database

Cannot continue until event completes.

---

## 5. Terminated

Execution finished.

OS releases

- Memory
- PCB
- Resources

---

# Complete Example

Opening Calculator

```
New

↓

Ready

↓

Running

↓

Waiting

↓

Ready

↓

Running

↓

Terminated
```

---

# State Transition Diagram

```
New

↓

Ready

↓

Running

├──────────────┐

│              │

│          Terminated

│

↓

Waiting

↓

Ready
```

---

# Thread Lifecycle

```
New

↓

Runnable

↓

Running

↓

Waiting / Blocked

↓

Runnable

↓

Running

↓

Terminated
```

---

# Java Thread States

Java defines

| State | Description |
|---------|-------------|
| NEW | Object Created |
| RUNNABLE | Ready / Running |
| BLOCKED | Waiting for Lock |
| WAITING | Waiting Forever |
| TIMED_WAITING | Waiting for Time |
| TERMINATED | Finished |

---

# Java Example

```java
class Demo extends Thread {

    public void run() {

        System.out.println("Running");

    }

    public static void main(String[] args) {

        Demo t = new Demo();

        System.out.println(t.getState());

        t.start();

        System.out.println(t.getState());

    }

}
```

Possible Output

```
NEW

RUNNABLE
```

---

# Practical Activity

View Running Processes

```bash
ps -ef
```

or

```bash
top
```

Observe

- PID
- CPU Usage
- Memory Usage

---

Thread State

```java
public class Demo {

    public static void main(String[] args) {

        Thread t = Thread.currentThread();

        System.out.println(t.getName());

        System.out.println(t.getState());

    }

}
```

Output

```
main

RUNNABLE
```

---

# Frequently Asked Interview Questions

## Thread

### What is a Thread?

Smallest unit of execution inside a process.

---

### Why use Threads?

To improve responsiveness and CPU utilization.

---

### What is Multithreading?

Running multiple threads within one process.

---

## Process vs Thread

### Which is faster?

Thread.

---

### Which consumes more memory?

Process.

---

### Which communicates faster?

Thread.

---

### Can a process exist without a thread?

No.

---

### Can a thread exist without a process?

No.

---

## Process States

### List Process States

- New
- Ready
- Running
- Waiting
- Terminated

---

### Why does Waiting occur?

Because process is waiting for I/O or another external event.

---

### Difference between Ready and Running?

Ready

Waiting for CPU.

Running

Currently executing.

---

# Complete Summary

## Thread

- Smallest execution unit
- Shares process memory
- Own Stack
- Fast
- Lightweight

---

## Process

- Independent program
- Own memory
- Heavyweight
- Secure
- Better isolation

---

## Process States

```
New

↓

Ready

↓

Running

↓

Waiting

↓

Ready

↓

Running

↓

Terminated
```

---

## Java Thread States

- NEW
- RUNNABLE
- BLOCKED
- WAITING
- TIMED_WAITING
- TERMINATED

---

# Next Module

The next Process Management topics are:

- Why CPU Scheduling?
- Scheduler Types
- Context Switching
- CPU Burst & I/O Burst
- Scheduling Algorithms Overview (FCFS, SJF, Priority, Round Robin)
