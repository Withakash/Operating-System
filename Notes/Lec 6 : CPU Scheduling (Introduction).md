# Operating System - Process Management (Part 2)
# CPU Scheduling, Scheduler Types, Context Switching, CPU Burst & I/O Burst, Overview of Scheduling Algorithms

> **Subject:** Operating System
> **Module:** Process Management
> **Level:** Beginner → Industry Ready
> **Teaching Style:** Theory + Practical + Linux Practice + Interview + Real World Examples

---

# 📚 Learning Outcomes

After completing this chapter, students will be able to:

- Understand why CPU Scheduling is required.
- Understand different Scheduler Types.
- Learn Context Switching in detail.
- Understand CPU Burst & I/O Burst.
- Understand how Operating Systems schedule processes.
- Get an overview of all scheduling algorithms.
- Observe scheduling practically in Linux.

---

# Table of Contents

1. Why CPU Scheduling?
2. CPU Scheduling Basics
3. Multiprogramming & CPU Utilization
4. Scheduler Types
5. Long Term Scheduler
6. Short Term Scheduler
7. Medium Term Scheduler
8. Context Switching
9. CPU Burst & I/O Burst
10. Scheduling Criteria
11. Scheduling Queue
12. Overview of Scheduling Algorithms
13. Real World Examples
14. Linux Practical
15. Java Practical
16. Interview Questions

---

# 1. Why CPU Scheduling?

---

## Imagine this Situation

You have only **one teacher** in a classroom.

But there are

- 50 Students

Everyone wants help.

Can the teacher help everyone simultaneously?

**No.**

The teacher gives time to each student one by one.

Exactly the same happens inside a computer.

---

## Computer Example

Computer has

```
CPU = 1
```

Running Applications

```
Chrome

VS Code

Spotify

Terminal

WhatsApp

Zoom

Calculator
```

All applications need CPU.

But CPU can execute only **one instruction per core at a time.**

So,

Operating System decides

```
Who gets CPU first?
Who waits?
For how long?
```

This decision-making process is called

# CPU Scheduling

---

# Definition

CPU Scheduling is the process of selecting one process from the Ready Queue and allocating the CPU to it.

---

## Diagram

```
Ready Queue

+---------+
| Chrome  |
+---------+

+---------+
| VSCode  |
+---------+

+---------+
| Spotify |
+---------+

+---------+
| Terminal|
+---------+

        |
        |
        ▼

CPU Scheduler

        |

        ▼

CPU
```

---

# Why CPU Scheduling is Important?

Without scheduling

```
One process

↓

Gets CPU forever

↓

Other processes never execute
```

This situation is called

**Starvation**

---

## Objectives

A good scheduler tries to

- Maximize CPU Utilization
- Reduce Waiting Time
- Reduce Response Time
- Increase Throughput
- Fair allocation

---

# Example

Imagine

Chrome

needs

```
2 seconds
```

VS Code

needs

```
5 seconds
```

Calculator

needs

```
1 second
```

Good scheduling finishes small tasks quickly.

User feels computer is fast.

---

# 2. CPU Scheduling Basics

Every process follows

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
```

Only

Processes in

```
Ready Queue
```

can be selected by scheduler.

---

## Ready Queue

```
Front

Chrome

↓

Spotify

↓

Calculator

↓

Terminal

↓

Back
```

Scheduler always chooses one process.

---

# 3. Multiprogramming and CPU Utilization

Suppose

Program A

waiting for disk.

CPU becomes idle.

Instead of wasting CPU,

Operating System executes

Program B.

```
Program A

Waiting

↓

CPU Free

↓

Program B executes
```

Result

CPU never sits idle.

This increases

CPU Utilization.

---

## CPU Utilization Formula

```
CPU Utilization

=

CPU Busy Time

------------------------

Total Time
```

Example

Busy = 90 seconds

Total =100 seconds

```
Utilization

=

90%

```

---

# 4. Scheduler Types

Operating Systems mainly use

Three schedulers.

```
Long Term Scheduler

↓

Medium Term Scheduler

↓

Short Term Scheduler
```

Each has a different responsibility.

---

# Overview Diagram

```
Program

↓

Long Term Scheduler

↓

Ready Queue

↓

Short Term Scheduler

↓

CPU

↓

Waiting Queue

↓

Medium Term Scheduler
```

---

# 5. Long Term Scheduler

## Definition

Controls

How many processes enter the system.

Also called

Admission Scheduler

---

## Responsibility

Chooses

Which process enters memory.

---

## Diagram

```
Disk

↓

Program A

Program B

Program C

↓

Long Term Scheduler

↓

RAM

↓

Ready Queue
```

---

## Characteristics

- Runs rarely
- Slow
- Controls Degree of Multiprogramming

---

## Example

100 users

submitted jobs.

Only

20

loaded into RAM.

---

# Real Life Example

Airport Security

Only limited passengers allowed inside.

---

# 6. Short Term Scheduler

Most important scheduler.

---

## Definition

Selects process

from Ready Queue

and gives CPU.

---

Runs

Thousands of times every second.

Must be extremely fast.

---

## Diagram

```
Ready Queue

↓

Short Term Scheduler

↓

CPU
```

---

## Example

Ready Queue

```
Chrome

VSCode

Spotify

Calculator
```

Scheduler chooses

```
Chrome
```

---

# Real Life Example

Traffic Police

Decides

Which vehicle crosses first.

---

# 7. Medium Term Scheduler

Sometimes

Memory becomes full.

Operating System temporarily removes

some processes from RAM.

This is called

Swapping.

---

## Diagram

```
RAM Full

↓

Medium Scheduler

↓

Swap Process

↓

Disk

↓

Later

↓

RAM
```

---

## Why?

To free memory.

---

## Real Life Example

Hostel

Room Full.

Some luggage moved to storage.

Later brought back.

---

# Comparison Table

| Scheduler | Works On | Speed | Purpose |
|------------|----------|-------|----------|
| Long Term | Disk → RAM | Slow | Admission |
| Short Term | Ready → CPU | Very Fast | CPU Allocation |
| Medium Term | RAM ↔ Disk | Medium | Swapping |

---

# 8. Context Switching

One of the most important OS topics.

---

## Definition

Switching CPU

from one process

to another.

---

Example

CPU executing

Chrome.

Suddenly

Spotify gets CPU.

Operating System

must save

Chrome's current state.

Then

load Spotify state.

This is

Context Switching.

---

## Diagram

```
CPU

↓

Chrome Running

↓

Save State

↓

Load Spotify

↓

Spotify Running
```

---

# What is Saved?

Operating System stores

inside PCB

- Program Counter
- CPU Registers
- Stack Pointer
- Memory Information
- Process State

---

## PCB Diagram

```
PCB

PID

Registers

Program Counter

Priority

Memory

State
```

---

# Steps of Context Switching

```
Running Process

↓

Save Context

↓

Scheduler

↓

Load Next Context

↓

Continue Execution
```

---

# Why Context Switching Takes Time?

Because OS performs

- Save Registers
- Save Program Counter
- Load Registers
- Load Memory Mapping
- Update PCB

CPU is not executing user programs during this time.

This is called

Context Switching Overhead.

---

# Example

```
Chrome

↓

Spotify

↓

VSCode

↓

Chrome
```

CPU continuously switches.

---

# Animation

```
CPU

Chrome

↓↓↓↓

Save

↓↓↓↓

Load

↓↓↓↓

Spotify

↓↓↓↓

Save

↓↓↓↓

Calculator
```

---

# 9. CPU Burst and I/O Burst

Processes do not always use CPU.

They alternate between computation and waiting.

---

## CPU Burst

Time during which a process uses CPU continuously.

Example

Calculating

```
100000 × 500000
```

Needs CPU.

---

## I/O Burst

Time during which process waits for

- Disk
- Keyboard
- Mouse
- Printer
- Network

CPU is not required.

---

## Diagram

```
CPU Burst

↓

I/O Burst

↓

CPU Burst

↓

I/O Burst

↓

CPU Burst
```

---

## Example

Microsoft Word

User types

↓

CPU processes keystrokes

↓

Saving file

↓

Disk access

↓

Waiting

↓

Continue typing

---

# CPU Bound Process

Uses CPU most of the time.

Examples

- Video Rendering
- AI Training
- Scientific Simulation
- Encryption
- Large Matrix Calculations

---

## Graph

```
CPU

███████████████

I/O

██
```

---

# I/O Bound Process

Mostly waits for I/O.

Examples

- Browser
- File Download
- Database Query
- Chat Application
- Cloud Storage Sync

---

## Graph

```
CPU

██

I/O

█████████████
```

---

# Comparison

| CPU Bound | I/O Bound |
|------------|------------|
| Long CPU Burst | Short CPU Burst |
| Less Waiting | More Waiting |
| Heavy Computation | Heavy I/O |
| Scientific Programs | Browsers |

---

# 10. Scheduling Criteria

Every scheduling algorithm tries to optimize these metrics.

---

## CPU Utilization

CPU should remain busy as much as possible.

---

## Throughput

Number of processes completed per unit time.

Higher throughput means more work completed.

---

## Turnaround Time

Total time taken from process submission to completion.

```
Turnaround Time = Completion Time - Arrival Time
```

---

## Waiting Time

Time spent waiting in the Ready Queue.

```
Waiting Time = Turnaround Time - CPU Burst Time
```

---

## Response Time

Time from request submission until the process gets CPU for the first time.

Important for interactive systems.

---

# 11. Scheduling Queue

```
          New Queue
               |
               ▼
          Ready Queue
               |
               ▼
             CPU
            /   \
           /     \
          ▼       ▼
 Waiting Queue   Exit
```

---

# 12. Overview of Scheduling Algorithms

> **Note:** This is only an introduction. Each algorithm will be covered in detail in later lectures.

---

## 1. FCFS (First Come First Serve)

Rule:

First process to arrive gets CPU first.

Example

```
A

↓

B

↓

C
```

Simple but can lead to long waiting times.

---

## 2. SJF (Shortest Job First)

Chooses the process with the smallest CPU burst.

Example

```
A = 8 ms
B = 2 ms
C = 5 ms

Order

B → C → A
```

Provides low average waiting time but requires knowledge of burst lengths.

---

## 3. Round Robin (RR)

Every process gets a fixed time slice (Time Quantum).

Example

Quantum = 2 ms

```
P1 → P2 → P3 → P1 → P2 ...
```

Very common in interactive operating systems.

---

## 4. Priority Scheduling

Each process has a priority.

Higher priority processes execute first.

Can suffer from starvation if low-priority processes never get CPU.

---

## 5. Multilevel Queue

Processes are divided into different queues.

Example

```
System Processes

Interactive Processes

Batch Processes
```

Each queue has its own scheduling policy.

---

## 6. Multilevel Feedback Queue (MLFQ)

Processes can move between queues based on behavior.

Frequently used in modern operating systems.

---

# Summary Table

| Algorithm | Main Idea | Typical Use |
|------------|-----------|-------------|
| FCFS | First come, first served | Batch jobs |
| SJF | Shortest job first | Minimize waiting time |
| Round Robin | Equal time quantum | Interactive systems |
| Priority | Highest priority first | Real-time & critical tasks |
| Multilevel Queue | Separate queues | Mixed workloads |
| MLFQ | Dynamic queue movement | Modern desktop OS |

---

# 13. Real World Examples

### ATM Queue

FCFS

First customer served first.

---

### Emergency Room

Priority Scheduling

Critical patient first.

---

### Classroom Viva

Round Robin

Each student gets equal time.

---

### Airport Security

Long-Term Scheduler

Only a limited number of passengers are admitted.

---

### Traffic Signal

Short-Term Scheduler

Decides which lane moves next.

---

### Warehouse Storage

Medium-Term Scheduler

Move items out temporarily and bring them back later.

---

# 14. Live Linux Practice

## Check Running Processes

```bash
ps
```

---

## Detailed Process Information

```bash
ps -ef
```

---

## Live CPU Usage

```bash
top
```

---

## Enhanced Monitor (install if needed)

```bash
htop
```

---

## Observe a CPU-Bound Process

Open a terminal and run:

```bash
yes > /dev/null
```

In another terminal:

```bash
top
```

Notice the `yes` process consuming nearly **100% CPU** on one core.

Stop it with:

```bash
Ctrl + C
```

---

## Observe an I/O-Bound Process

Create a large file:

```bash
dd if=/dev/zero of=testfile.bin bs=1M count=500
```

Copy the file:

```bash
cp testfile.bin copy.bin
```

Watch CPU usage:

```bash
top
```

Notice the process spends time waiting on disk I/O.

---

## Run a Background Process

```bash
sleep 60 &
```

View background jobs:

```bash
jobs
```

Find the process:

```bash
ps -ef | grep sleep
```

Terminate it:

```bash
kill <PID>
```

---

# 15. Java Practical

## CPU-Bound Example

```java
public class CpuBoundDemo {
    public static void main(String[] args) {
        long sum = 0;

        for (long i = 0; i < 5_000_000_000L; i++) {
            sum += i;
        }

        System.out.println(sum);
    }
}
```

This program performs heavy computation and continuously uses the CPU.

---

## I/O-Bound Example

```java
import java.util.Scanner;

public class IOBoundDemo {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        System.out.println("Enter your name:");

        String name = sc.nextLine();

        System.out.println("Hello " + name);

    }
}
```

The process waits for keyboard input, so it spends time in an I/O wait state.

---

# 16. Interview Questions

### 1. Why is CPU Scheduling required?

To decide which ready process gets CPU time, improving CPU utilization, responsiveness, and fairness.

---

### 2. Which scheduler allocates the CPU?

**Short-Term Scheduler.**

---

### 3. Which scheduler is called the Admission Scheduler?

**Long-Term Scheduler.**

---

### 4. What is Context Switching?

Saving the state of one process and restoring another so the CPU can switch execution.

---

### 5. Is Context Switching useful?

Yes, but it introduces **overhead** because no user process executes while the switch is occurring.

---

### 6. What is the difference between CPU Burst and I/O Burst?

- **CPU Burst:** Time spent executing instructions on the CPU.
- **I/O Burst:** Time spent waiting for input/output operations.

---

### 7. Which processes are CPU Bound?

Examples include video rendering, machine learning, scientific computing, encryption, and simulations.

---

### 8. Which processes are I/O Bound?

Examples include browsers, chat applications, database clients, and file download utilities.

---

# Key Takeaways

- CPU Scheduling ensures efficient and fair CPU allocation.
- Processes wait in the **Ready Queue** until selected by the **Short-Term Scheduler**.
- **Long-Term Scheduler** controls admission into memory, while the **Medium-Term Scheduler** swaps processes in and out.
- **Context Switching** enables multitasking by saving and restoring process state.
- Every process alternates between **CPU Bursts** and **I/O Bursts**.
- Scheduling algorithms aim to maximize CPU utilization, increase throughput, and reduce waiting, turnaround, and response times.
- Modern operating systems use advanced scheduling techniques, often based on **Multilevel Feedback Queues (MLFQ)**.
