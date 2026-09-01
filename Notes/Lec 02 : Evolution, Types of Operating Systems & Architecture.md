# Operating System – Lecture 2

## Evolution of Operating Systems | Types of Operating Systems | Real-Time, Distributed, Network & Embedded OS | OS Architecture | Kernel vs User Space

> **Subject:** Operating System
> **Lecture:** 2
> **Duration:** 50 Minutes
> **Level:** Beginner
> **Approach:** Concept + Historical Evolution + Real-World Examples + Diagrams + Practical Observation

---

# 1. Learning Objectives

After completing this lecture, students should be able to:

* Explain why Operating Systems evolved over time.
* Understand major stages in OS evolution.
* Identify different types of operating systems.
* Differentiate:

  * Batch OS
  * Multiprogramming OS
  * Multitasking OS
  * Time-Sharing OS
  * Real-Time OS
  * Distributed OS
  * Network OS
  * Embedded OS
* Explain operating system architecture.
* Understand the difference between Kernel Space and User Space.
* Understand why applications cannot directly access hardware.
* Observe basic kernel information practically using Linux.

---

# 2. Quick Revision from Lecture 1

We learned:

```text
User
  ↓
Application
  ↓
Operating System
  ↓
Hardware
```

The OS manages:

* CPU
* Memory
* Files
* Devices
* Processes
* Security
* Networking

Today we will understand:

> **How Operating Systems evolved and how different OS designs are used for different requirements.**

---

# 3. Why Did Operating Systems Evolve?

The earliest computers were very different from today's systems.

Initially:

* Computers were extremely expensive.
* Users interacted directly with hardware or very low-level software.
* Programs were loaded manually.
* CPU time was often wasted.
* Only one job could be processed at a time.

As technology improved, computers became capable of handling more programs and more users.

This created new requirements:

```text
More programs
     ↓
More users
     ↓
More hardware
     ↓
More complexity
     ↓
Better Operating Systems
```

---

# 4. Evolution of Operating Systems

A simplified evolution is:

```text
No OS
  ↓
Batch Systems
  ↓
Multiprogramming
  ↓
Multitasking
  ↓
Time-Sharing
  ↓
Personal Computer OS
  ↓
Network OS
  ↓
Distributed Systems
  ↓
Real-Time / Embedded / Mobile / Cloud Systems
```

This evolution was driven by one major goal:

> **Use computer resources more efficiently while making computers easier and safer to use.**

---

# 5. Stage 1 – No Operating System

In the earliest computers, there was no modern OS.

Users or operators had to:

* Load programs manually.
* Configure hardware.
* Start execution.
* Collect output.
* Prepare the next job.

Conceptually:

```text
Human
  ↓
Machine Setup
  ↓
Program
  ↓
Execution
  ↓
Output
```

### Main Problem

A lot of human effort was required.

The CPU could remain idle while the operator prepared the next job.

---

# 6. Stage 2 – Batch Operating Systems

Batch systems were introduced to reduce manual interaction.

Instead of executing one job and manually preparing the next:

```text
Job 1
Job 2
Job 3
Job 4
```

Jobs were collected into a batch.

The system executed them one after another.

```text
+-------+
| Job 1 |
+-------+
    ↓
+-------+
| Job 2 |
+-------+
    ↓
+-------+
| Job 3 |
+-------+
    ↓
+-------+
| Job 4 |
+-------+
```

---

## Characteristics

* Jobs are grouped into batches.
* Minimal user interaction.
* Jobs execute sequentially.
* Good for repetitive workloads.

---

## Advantages

* Reduces manual setup.
* Improves overall system utilization.
* Suitable for large repetitive jobs.

## Disadvantages

* Poor user interaction.
* Long waiting times.
* Difficult to immediately observe job output.

---

## Real-Life Example

Suppose a company needs to process:

```text
10,000 salary records
```

A batch system can process them automatically.

---

# 7. Stage 3 – Multiprogramming

Batch systems still had a major problem.

Suppose Process A is waiting for disk I/O.

The CPU is idle.

Instead of waiting:

```text
Process A
   ↓
Waiting for I/O

CPU
   ↓
Idle
```

the OS loads another process.

```text
Process A
   ↓
Waiting
   |
   +------> Process B uses CPU
```

This concept is called:

# Multiprogramming

---

## Definition

Multiprogramming is the technique of keeping multiple programs in memory so the CPU can switch to another program when the current program is waiting for I/O.

---

## Example

```text
RAM

+----------------+
| Process A       |
+----------------+
| Process B       |
+----------------+
| Process C       |
+----------------+
```

CPU:

```text
Process A
   ↓
I/O wait
   ↓
Process B
   ↓
Process C
```

---

## Main Goal

> **Keep the CPU busy as much as possible.**

---

# 8. Stage 4 – Multitasking

As interactive computing became important, users wanted multiple applications to appear to run at the same time.

For example:

```text
Browser
Music Player
VS Code
Terminal
```

The OS rapidly switches CPU time between them.

```text
Browser
  ↓
VS Code
  ↓
Music
  ↓
Terminal
  ↓
Browser
```

This happens so quickly that users perceive simultaneous execution.

---

## Multitasking

Multitasking allows multiple tasks to make progress during overlapping periods by sharing CPU time.

---

# 9. Stage 5 – Time-Sharing Systems

Time-sharing extends multitasking for interactive users.

The CPU gives a small time slice to each process/user.

Example:

```text
P1 → P2 → P3 → P4
↑               |
+---------------+
```

Each process gets CPU time.

The duration assigned is often called:

# Time Quantum

---

## Why Time-Sharing?

Users expect:

* Quick response
* Fair CPU allocation
* Interactive systems

This became very important for terminals and multi-user systems.

---

# 10. Stage 6 – Personal Computer Operating Systems

Computers eventually became affordable for individuals.

Operating systems such as:

* MS-DOS
* Windows
* Classic Mac OS
* Later Linux distributions
* Modern macOS

were designed for personal computers.

The emphasis shifted toward:

* User friendliness
* Graphical interfaces
* Multitasking
* Hardware support
* Security
* Networking

---

# 11. Stage 7 – Network and Distributed Systems

As computers became connected:

```text
Computer A
      |
    Network
      |
Computer B
      |
    Network
      |
Computer C
```

Operating systems needed better:

* Network communication
* Resource sharing
* Remote access
* File sharing
* Authentication

This led to Network OS concepts and later distributed computing.

---

# 12. Modern Operating Systems

Today's operating systems may support:

* Multicore CPUs
* Virtual memory
* Virtualization
* Networking
* Security
* Containers
* Cloud computing
* Mobile applications
* Real-time processing
* Power management
* Hardware acceleration

Examples:

```text
Windows
Linux
macOS
Android
iOS
```

---

# 13. Timeline Summary

```text
Early Computers
      ↓
No OS / Manual Operation
      ↓
Batch OS
      ↓
Multiprogramming
      ↓
Multitasking
      ↓
Time-Sharing
      ↓
PC Operating Systems
      ↓
Network & Distributed Computing
      ↓
Modern OS
      ↓
Cloud + Mobile + Embedded + Real-Time
```

---

# 14. Types of Operating Systems

There is no single "best" operating system design.

Different environments need different features.

For example:

```text
ATM
```

has very different requirements from:

```text
Gaming PC
```

and:

```text
Airbag Controller
```

Therefore, different types of OS evolved.

---

# 15. Batch Operating System

## Definition

An OS that executes a group of jobs in batches with minimal user interaction.

### Example

Large-scale data processing.

### Best suited for

* Payroll processing
* Billing
* Report generation
* Large data processing

---

# 16. Multiprogramming Operating System

Multiple programs are kept in memory.

When one waits for I/O, another gets CPU.

```text
Process A → Waiting
Process B → Running
Process C → Ready
```

### Main Objective

Increase CPU utilization.

---

# 17. Multitasking Operating System

Allows multiple tasks to make progress by rapidly sharing CPU time.

Examples:

* Windows
* Linux desktop
* macOS
* Android

A user can:

```text
Watch YouTube
+
Download File
+
Use Browser
+
Run VS Code
```

at the same time.

---

# 18. Time-Sharing Operating System

A time-sharing system divides CPU time among processes/users to provide interactive response.

Example:

```text
P1 → 10 ms
P2 → 10 ms
P3 → 10 ms
P4 → 10 ms
```

Then the cycle repeats.

---

# 19. Real-Time Operating System (RTOS)

One of the most important specialized OS categories.

## Definition

A Real-Time Operating System is designed to provide predictable responses to events, often within specified timing constraints.

The important idea is:

> **Correctness can depend on both the result and when the result is produced.**

---

# 20. Real-Time Example – Airbag System

Suppose a car detects a severe collision.

The controller must react within a very small time window.

```text
Collision Sensor
      ↓
RTOS
      ↓
Decision
      ↓
Airbag Trigger
```

A delayed response could be dangerous.

---

# 21. Types of Real-Time Systems

## Hard Real-Time

Missing the deadline may cause system failure or unacceptable consequences.

Examples may include:

* Certain safety-critical control systems
* Some avionics systems
* Certain industrial control systems

---

## Soft Real-Time

Missing an occasional deadline reduces quality but is not necessarily catastrophic.

Examples:

* Multimedia playback
* Audio/video processing
* Some communication systems

---

# 22. RTOS Characteristics

Typical RTOS characteristics include:

* Deterministic behavior
* Predictable scheduling
* Low interrupt latency
* Fast response to events
* Priority-based scheduling
* Reliability

Examples of RTOS families include:

* FreeRTOS
* QNX
* VxWorks
* Zephyr

---

# 23. Distributed Operating System

A Distributed OS conceptually manages a collection of networked computers and attempts to provide a more unified system environment.

```text
+---------+      +---------+
|Computer |------|Computer |
|    A    |      |    B    |
+---------+      +---------+
      \             /
       \           /
        \         /
        +---------+
        |Computer |
        |    C    |
        +---------+
```

The machines are physically separate but cooperate.

---

## Goals

* Resource sharing
* Load distribution
* Fault tolerance
* Communication
* Transparency

---

# 24. Important Concept – Transparency

Ideally, users should not always need to know where a resource physically exists.

For example:

```text
User
 ↓
Request File
 ↓
Distributed System
 ↓
Resource located on another machine
```

The system may make this interaction feel local.

---

# 25. Network Operating System

A Network OS focuses on enabling computers to communicate and share resources over a network.

Typical features:

* File sharing
* Printer sharing
* User management
* Remote login
* Centralized authentication
* Network administration

Examples include server-oriented operating environments such as:

* Windows Server
* Linux server systems
* UNIX server environments

---

# 26. Network OS vs Distributed OS

These two concepts are often confused.

| Network OS                           | Distributed OS                         |
| ------------------------------------ | -------------------------------------- |
| Machines remain visibly separate     | Tries to provide a unified environment |
| Network resource sharing is central  | System-wide coordination is emphasized |
| Users often know where resources are | Location can be more transparent       |
| Examples include server OS platforms | More specialized design                |

---

# 27. Embedded Operating System

An embedded OS is designed for a dedicated device or specific-purpose computing system.

Examples:

```text
Smart Watch
Router
Printer
Smart TV
Car Controller
IoT Device
```

---

## Embedded System

```text
+-----------------------+
| Dedicated Hardware    |
+-----------------------+
          |
          ↓
+-----------------------+
| Embedded OS / Runtime |
+-----------------------+
          |
          ↓
+-----------------------+
| Specific Application  |
+-----------------------+
```

---

# 28. Embedded OS Characteristics

Usually optimized for:

* Limited memory
* Limited CPU resources
* Low power
* Reliability
* Specific functionality
* Small footprint

Some embedded devices use full operating systems, while others use lightweight RTOSes or specialized firmware.

---

# 29. Real-World Comparison

| System                     | Suitable OS Type                   |
| -------------------------- | ---------------------------------- |
| Payroll processing         | Batch                              |
| Desktop PC                 | Multitasking                       |
| Multi-user terminal system | Time-Sharing                       |
| Airbag controller          | Real-Time                          |
| Router                     | Embedded / Network-oriented        |
| Smartwatch                 | Embedded / Mobile-oriented         |
| Large network service      | Network / Distributed architecture |

---

# 30. Operating System Architecture

Architecture describes:

> **How the different components of an operating system are organized and how they interact.**

A simplified view:

```text
+----------------------------------+
|            USER                  |
+----------------------------------+
                |
                ↓
+----------------------------------+
|       APPLICATION PROGRAMS       |
+----------------------------------+
                |
                ↓
+----------------------------------+
|         SYSTEM CALLS            |
+----------------------------------+
                |
                ↓
+----------------------------------+
|             KERNEL               |
|                                  |
| Process Management               |
| Memory Management                |
| File System                      |
| Device Management                |
| Networking                       |
| Security                         |
+----------------------------------+
                |
                ↓
+----------------------------------+
|           HARDWARE               |
| CPU | RAM | Disk | Devices       |
+----------------------------------+
```

---

# 31. Why Do We Need Architecture?

Operating systems are extremely large software systems.

Without proper organization, the code would become:

* Difficult to maintain
* Difficult to debug
* Difficult to secure
* Difficult to extend

Architecture separates responsibilities.

---

# 32. Major OS Architecture Styles

Important architectural approaches include:

1. Monolithic Architecture
2. Layered Architecture
3. Microkernel Architecture
4. Modular Architecture
5. Hybrid Architecture

---

# 33. Monolithic Architecture

In a monolithic kernel, many OS services run in kernel space.

Conceptually:

```text
+-------------------------+
|       Applications      |
+-------------------------+
           |
           ↓
+-------------------------+
|      MONOLITHIC KERNEL  |
|                         |
| Process Management      |
| Memory Management       |
| File System             |
| Drivers                 |
| Networking              |
+-------------------------+
           |
           ↓
+-------------------------+
|        Hardware         |
+-------------------------+
```

---

## Advantages

* Good performance
* Direct communication between kernel components
* Efficient

## Disadvantages

* Large and complex kernel
* Bugs in kernel-space code can have serious consequences
* Harder to isolate components

Linux uses a **monolithic kernel design with a modular architecture**, so calling Linux simply "pure monolithic" misses an important detail.

---

# 34. Layered Architecture

The OS can be organized into layers.

```text
+----------------------+
| User Applications    |
+----------------------+
| User Interface       |
+----------------------+
| File Management      |
+----------------------+
| Memory Management    |
+----------------------+
| CPU Management       |
+----------------------+
| Hardware             |
+----------------------+
```

Each layer generally uses services from lower layers.

---

## Advantage

* Easier to understand
* Easier to test
* Better separation of responsibility

## Disadvantage

* Additional overhead
* Defining perfectly clean layers can be difficult

---

# 35. Microkernel Architecture

A microkernel keeps only essential functionality in kernel space.

Other services may run as user-space processes.

```text
+--------------------------+
| Applications             |
+--------------------------+
| User-Space Services      |
| File System              |
| Drivers                  |
| Networking               |
+--------------------------+
|      Microkernel         |
| IPC                      |
| Scheduling               |
| Basic Memory Management  |
+--------------------------+
| Hardware                 |
+--------------------------+
```

---

## Advantages

* Better isolation
* Smaller kernel
* Easier to extend some services
* Potentially better fault isolation

## Disadvantages

* Communication between services can add overhead
* More complex system coordination

Examples of systems using microkernel designs include:

* QNX
* seL4
* MINIX 3

---

# 36. Modular Architecture

A modular kernel keeps a core kernel but allows components to be added or removed as modules.

Linux is a major example.

```text
Core Kernel
   |
   +---- Network Module
   |
   +---- USB Module
   |
   +---- File System Module
   |
   +---- Device Driver Module
```

This allows functionality to be extended without rebuilding the entire kernel every time.

---

# 37. Hybrid Architecture

Modern operating systems may combine ideas from different architecture styles.

A hybrid design may include:

* Monolithic-like performance
* Modular components
* Microkernel-inspired isolation for selected subsystems

Windows and Apple platforms use hybrid approaches with different implementation details.

---

# 38. Kernel vs User Space

This is a very important concept.

Modern operating systems separate execution into privilege levels.

The two broad conceptual areas are:

```text
USER SPACE
     |
     ↓
KERNEL SPACE
     |
     ↓
HARDWARE
```

---

# 39. User Space

User Space is where normal applications execute.

Examples:

* Chrome
* VS Code
* VLC
* Java programs
* Python programs
* Shells

Applications in user space have restricted access to critical system resources.

---

# 40. Kernel Space

Kernel Space is where the OS kernel and highly privileged code execute.

It can perform operations such as:

* Managing memory
* Scheduling processes
* Accessing hardware
* Handling interrupts
* Managing devices
* Managing filesystems

---

# 41. Why Separate User Space and Kernel Space?

Imagine every application had unrestricted access to hardware.

A buggy application could:

* Overwrite another application's memory
* Modify kernel data
* Access sensitive information
* Crash the whole system
* Control devices directly

Therefore:

> **The OS uses privilege boundaries to protect the system.**

---

# 42. User Space vs Kernel Space

| User Space                            | Kernel Space                           |
| ------------------------------------- | -------------------------------------- |
| Applications run here                 | Kernel runs here                       |
| Restricted privileges                 | High privileges                        |
| Less direct hardware access           | Direct hardware/resource control       |
| Application crash is usually isolated | Kernel failure can affect whole system |
| Java, Python, Chrome, etc.            | Kernel, drivers, core OS services      |

---

# 43. How Does a User Program Access Kernel Services?

Through:

# System Calls

A program running in user space cannot simply execute every privileged operation directly.

Instead, it requests a service from the kernel.

Conceptually:

```text
Application
    |
    | System Call
    ↓
Kernel
    |
    ↓
Hardware / Resource
```

---

# 44. Example – Reading a File

Suppose a program wants to read:

```text
data.txt
```

Conceptually:

```text
Java Program
     ↓
read/open request
     ↓
System Call
     ↓
Kernel
     ↓
File System
     ↓
Storage Device
     ↓
Data returned
     ↓
Program
```

The application does not directly manipulate SSD hardware.

---

# 45. Example – Printing

```text
Application
     ↓
Print Request
     ↓
System Call
     ↓
Kernel
     ↓
Printer Driver
     ↓
Printer
```

---

# 46. Example – Creating a Process

On Linux, system calls such as:

```text
fork()
execve()
clone()
```

are involved in process creation/execution workflows.

The exact behavior differs depending on the system call and platform.

---

# 47. Privileged vs Unprivileged Operations

CPU architectures provide privilege mechanisms.

Conceptually:

```text
High Privilege
     ↓
Kernel
     ↓
Low Privilege
     ↓
Applications
```

An application normally runs with fewer privileges than the kernel.

This reduces the damage that a buggy or malicious application can cause.

---

# 48. Practical Linux – Identify the Kernel

Run:

```bash
uname -s
```

Example:

```text
Linux
```

---

Run:

```bash
uname -r
```

This displays the kernel release.

Example:

```text
6.x.x-...
```

Your exact version will depend on your Ubuntu installation.

---

# 49. Detailed Kernel Information

Run:

```bash
uname -a
```

This may show:

* Kernel name
* Kernel release
* Machine architecture
* Hostname
* Other system information

Example structure:

```text
Linux hostname 6.x.x ... x86_64 GNU/Linux
```

---

# 50. Practical – CPU Architecture

Run:

```bash
uname -m
```

Common result:

```text
x86_64
```

or on ARM-based systems:

```text
aarch64
```

---

# 51. Practical – CPU Information

Linux:

```bash
lscpu
```

Look for:

* Architecture
* CPU(s)
* Core(s)
* Thread(s)
* Model name

This helps connect OS concepts with actual hardware.

---

# 52. Practical – Kernel Messages

On many Linux systems:

```bash
dmesg | head
```

This displays the first few kernel messages.

You may need elevated privileges for some information depending on system configuration.

This provides a glimpse into interactions between the kernel and hardware.

---

# 53. Practical – User Space Observation

Run:

```bash
ps
```

This shows processes associated with the current terminal/session.

Try:

```bash
ps -ef
```

This provides a broader process listing.

You are observing user-space processes managed by the kernel.

---

# 54. Practical – Process and Kernel Connection

Run:

```bash
sleep 30 &
```

Then:

```bash
ps -ef | grep sleep
```

You will see the process.

Conceptually:

```text
Your Command
    ↓
Shell
    ↓
System Calls
    ↓
Kernel
    ↓
Process Created
```

This is one of the first ways to connect theory with actual OS behavior.

---

# 55. Live Classroom Demonstration

### Demo 1 – Identify OS

Ubuntu:

```bash
uname -a
```

macOS:

```bash
uname -a
```

Ask students:

> "Are both systems using the same kernel family?"

Discuss the difference between the OS as a whole and the kernel underneath.

---

### Demo 2 – Explore Processes

Run:

```bash
ps
```

Then:

```bash
ps -ef
```

Ask:

* How many processes are visible?
* Which process is the shell?
* Why does every process have a PID?

---

### Demo 3 – Observe Kernel Information

Run:

```bash
uname -r
```

Ask:

> "Who manages these processes and hardware resources underneath the applications?"

Answer:

**The kernel.**

---

# 56. Important Comparison

## Batch vs Multiprogramming vs Multitasking vs Time-Sharing

| Feature             | Batch           | Multiprogramming            | Multitasking          | Time-Sharing                   |
| ------------------- | --------------- | --------------------------- | --------------------- | ------------------------------ |
| User Interaction    | Very Low        | Low                         | High                  | High                           |
| Main Goal           | Process batches | CPU utilization             | Multiple active tasks | Interactive response           |
| CPU Switching       | Limited         | When waiting                | Frequent              | Time slices                    |
| Typical Environment | Large jobs      | Early multi-program systems | Desktops              | Multi-user interactive systems |

---

# 57. Important Comparison – Specialized OS

| Type        | Main Goal                         | Example Scenario    |
| ----------- | --------------------------------- | ------------------- |
| Real-Time   | Predictable timing                | Control system      |
| Distributed | Coordinated distributed resources | Cluster environment |
| Network OS  | Resource/network sharing          | Server environment  |
| Embedded    | Dedicated function                | Router, IoT device  |

---

# 58. Common Confusions

## Confusion 1

**Is every multitasking OS a real-time OS?**

No.

A normal desktop OS may support multitasking without providing hard real-time guarantees.

---

## Confusion 2

**Is Linux an embedded OS?**

Linux itself is a kernel. It can be used in many environments, including servers, desktops, mobile/embedded systems, and appliances.

---

## Confusion 3

**Is Android a kernel?**

No.

Android is an operating system platform built around a Linux kernel plus Android-specific frameworks, runtime, system services, and applications.

---

## Confusion 4

**Is Kernel the same as OS?**

Not exactly.

The kernel is the core part of an OS. The operating system as a whole includes the kernel plus system software, libraries, services, utilities, and often user-facing components.

---

## Confusion 5

**Can an application directly access hardware?**

Normally, applications use OS-provided interfaces and system calls. The kernel and drivers control privileged hardware access.

---

# 59. Real-World Mapping

Think about these examples:

### Laptop

```text
Windows / Linux / macOS
        ↓
Multitasking + Networking + Security
```

### ATM

```text
Dedicated device
        ↓
Embedded system
        ↓
Strong reliability requirements
```

### Car Controller

```text
Sensor
  ↓
Controller
  ↓
Real-Time response
```

### Data Center

```text
Many connected machines
        ↓
Network + Distributed Computing
```

### Smartwatch

```text
Battery constrained
        ↓
Embedded / Mobile OS
```

---

# 60. Memory Trick for Students

Remember the evolution using:

```text
B → M → M → T
```

### B

Batch

### M

Multiprogramming

### M

Multitasking

### T

Time-Sharing

Then specialized systems:

```text
Real-Time
Distributed
Network
Embedded
```

---

# 61. Viva Questions

### Q1. Why did operating systems evolve?

To improve resource utilization, reduce manual work, support multiple programs/users, improve interaction, and manage increasingly complex hardware and software.

### Q2. What is Batch OS?

An OS that processes jobs in batches with minimal interaction.

### Q3. What is Multiprogramming?

Keeping multiple programs in memory so the CPU can execute another program when one is waiting for I/O.

### Q4. What is Multitasking?

Allowing multiple tasks to make progress through CPU sharing and rapid switching.

### Q5. What is Time-Sharing?

A technique where CPU time is divided among processes/users to provide interactive response.

### Q6. What is an RTOS?

An operating system designed for predictable and timely responses to events.

### Q7. Difference between Hard and Soft Real-Time?

Hard real-time systems have strict deadlines where missing one can be unacceptable; soft real-time systems tolerate occasional deadline misses with reduced quality.

### Q8. What is an Embedded OS?

An OS designed for a dedicated or resource-constrained device.

### Q9. What is Kernel Space?

A privileged execution environment used by the kernel and other highly privileged components.

### Q10. What is User Space?

The restricted execution environment where normal applications run.

### Q11. Why do we need system calls?

To allow user-space programs to request controlled services from the kernel.

### Q12. Is Linux only a desktop operating system?

No. Linux-based systems are used in servers, cloud platforms, embedded devices, networking equipment, Android, and many other environments.

---

# 62. Practical Assignment

Run the following commands on Ubuntu or Linux:

```bash
uname -a
uname -r
uname -m
lscpu
ps
ps -ef
```

Write down:

```text
1. Kernel Name
2. Kernel Version
3. CPU Architecture
4. Number of CPUs
5. Number of Cores
6. Number of Threads
7. Number of Running Processes
```

---

# 63. Think and Answer

### Question 1

Your music player is running while you use VS Code.

Which OS concept allows both applications to make progress?

**Multitasking / CPU scheduling.**

---

### Question 2

A process waits for disk I/O.

Should the CPU remain idle?

**No. The OS can schedule another ready process.**

---

### Question 3

Why should Chrome not be allowed to modify kernel memory directly?

Because unrestricted access would break isolation and system security.

---

### Question 4

Why is an airbag controller different from a normal desktop?

Because timing predictability and reliability can be much more critical.

---

# 64. Final Mental Model

```text
                OPERATING SYSTEM
                       |
        +--------------+--------------+
        |              |              |
    Management       Interface     Protection
        |              |              |
    CPU / RAM       GUI / CLI       Security
    Processes       System Calls    Isolation
    Files           APIs            Permissions
    Devices
        |
        ↓
      KERNEL
        |
        ↓
    HARDWARE
```

And the historical journey:

```text
Manual Operation
       ↓
Batch
       ↓
Multiprogramming
       ↓
Multitasking
       ↓
Time-Sharing
       ↓
Networked Systems
       ↓
Distributed Systems
       ↓
Modern Specialized Systems
       |
       +---- Real-Time
       +---- Embedded
       +---- Mobile
       +---- Cloud
```

---

# Key Takeaways

* Operating systems evolved because computers became more powerful, complex, interactive, and connected.
* Batch systems reduced manual work.
* Multiprogramming improved CPU utilization.
* Multitasking enabled multiple active applications.
* Time-sharing improved interactive response and fairness.
* Real-time systems focus on predictable timing.
* Distributed systems coordinate multiple machines.
* Network OS designs emphasize network resource sharing and administration.
* Embedded OS designs target dedicated devices and constrained resources.
* OS architecture defines how components are organized.
* Kernel space provides privileged execution.
* User space runs normal applications with restricted privileges.
* System calls provide the controlled bridge between applications and the kernel.
* Linux provides an excellent practical environment for observing OS concepts.

> **Core idea to remember:**
> An operating system evolved from a simple way to run programs into a sophisticated resource manager that safely coordinates CPU, memory, storage, devices, networking, and applications.
