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
