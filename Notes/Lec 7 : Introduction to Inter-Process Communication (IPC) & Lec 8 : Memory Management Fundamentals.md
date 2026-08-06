# Operating Systems - IPC & Memory Management (Overview)

> **Unit:** Process Communication & Memory Management  
> **Level:** Beginner  
> **Goal:** Understand why processes communicate and why memory management is important before learning implementation details.

---

# Table of Contents

1. Why Processes Need Communication
2. IPC (Inter-Process Communication) Overview
3. Pipes & Redirection
4. Client-Server Communication
5. Real-Life IPC Examples
6. Why Memory Management?
7. Memory Hierarchy
8. RAM, Cache & Registers
9. Memory Allocation & Deallocation
10. Fragmentation & Compaction
11. Quick Revision

---

# 1. Why Processes Need Communication?

## What is a Process?

A **process** is a program that is currently running.

Example:

- Google Chrome
- Spotify
- VS Code
- WhatsApp
- Calculator

Each application runs as one or more processes.

---

## Why Do Processes Need to Communicate?

Most applications cannot work alone.

They need to:

- Share information
- Coordinate tasks
- Exchange data
- Synchronize execution
- Request services from other processes

Without communication, modern applications would not function properly.

---

## Example 1: Google Chrome

```
Keyboard Input
      │
      ▼
Chrome UI Process
      │
      ▼
Renderer Process
      │
      ▼
Network Process
      │
      ▼
Google Server
```

Many processes cooperate to display a single webpage.

---

## Example 2: WhatsApp

```
Your Phone
     │
     ▼
WhatsApp Process
     │
     ▼
Internet
     │
     ▼
WhatsApp Server
     │
     ▼
Friend's Phone
```

---

## Uses of Process Communication

- File Sharing
- Data Sharing
- Sending Messages
- Client-Server Communication
- Process Synchronization
- Sharing Resources

---

# 2. IPC (Inter Process Communication) Overview

## Definition

**Inter-Process Communication (IPC)** is the mechanism that allows two or more processes to exchange information.

---

## Why IPC?

Imagine two separate applications:

```
Application A

Application B
```

Without IPC, they cannot exchange information.

With IPC:

```
Application A
      │
      │ IPC
      ▼
Application B
```

---

## Common IPC Mechanisms

```
Inter Process Communication

├── Pipes
├── Named Pipes
├── Message Queues
├── Shared Memory
├── Sockets
└── Signals
```

> We will study each mechanism in detail later.

---

## Real Example

When you click **Print**:

```
MS Word

↓

Print Service

↓

Printer Driver

↓

Printer
```

All these components communicate using IPC.

---

# 3. Pipes & Redirection

One of the simplest IPC mechanisms.

---

# Pipes

A **pipe** sends the output of one process directly to another process.

### Linux Example

```bash
ls | grep java
```

### Explanation

```
Process 1

ls

↓

Pipe

↓

Process 2

grep java
```

The output of `ls` becomes the input of `grep`.

---

## Why Pipes?

Instead of saving output to a file first,

```
Program A

↓

Temporary File

↓

Program B
```

we directly connect the programs.

```
Program A

↓

Pipe

↓

Program B
```

This is faster and simpler.

---

# Output Redirection

Redirect output into a file.

Example:

```bash
ls > files.txt
```

Instead of displaying on screen,

```
ls

↓

files.txt
```

---

# Append Output

```bash
echo Hello >> file.txt
```

- `>` → Overwrites the file
- `>>` → Appends to the file

---

# Input Redirection

Instead of typing from the keyboard:

```bash
sort < numbers.txt
```

The program reads input from the file.

```
numbers.txt

↓

sort

↓

Sorted Output
```

---

# 4. Client-Server Communication

Most software follows the **Client-Server Model**.

---

## Client

A client requests a service.

Examples:

- Browser
- Mobile App
- ATM
- Netflix App

---

## Server

A server provides the requested service.

Examples:

- Google Server
- Amazon Server
- Database Server
- University Portal

---

## Communication Flow

```
Client

↓

Request

↓

Server

↓

Processing

↓

Response

↓

Client
```

---

## Example: Opening Google

```
Browser

↓

Request

↓

Google Server

↓

HTML Response

↓

Browser Displays Website
```

---

## Real-Life Examples

| Client | Server |
|---------|---------|
| Browser | Google Server |
| ATM | Bank Server |
| Instagram App | Instagram Server |
| Netflix App | Netflix Server |
| PhonePe | Banking Server |

---

# 5. Real-Life IPC Examples

---

## ATM

```
ATM

↓

Bank Server

↓

Balance Check

↓

Cash Withdrawal
```

---

## Spotify

```
Spotify

↓

Audio Service

↓

Speaker Driver
```

---

## Chrome

```
Chrome UI

↓

Renderer

↓

GPU Process

↓

Network Process
```

---

## Printer

```
Word

↓

Print Service

↓

Printer Driver

↓

Printer
```

---

## VS Code

```
Editor

↓

Language Server

↓

Compiler

↓

Terminal
```

---

# 6. Why Memory Management?

Every running program requires memory.

The Operating System decides:

- Where to store programs
- How much memory to allocate
- When to release memory
- How to protect memory

Without memory management:

- Programs overwrite each other.
- System becomes unstable.
- Memory gets wasted.
- Applications crash.

---

## Hotel Analogy

Imagine RAM as a hotel.

```
RAM

+--------------------------+

Room 1 → Chrome

Room 2 → Spotify

Room 3 → VS Code

Room 4 → Zoom

+--------------------------+
```

The Operating System acts as the hotel manager.

---

# Responsibilities of Memory Management

- Memory Allocation
- Memory Deallocation
- Memory Protection
- Memory Tracking
- Efficient Memory Usage

---

# 7. Memory Hierarchy

Different types of memory have different speeds and sizes.

```
          Registers
               │
           L1 Cache
               │
           L2 Cache
               │
           L3 Cache
               │
              RAM
               │
             SSD
               │
              HDD
```

---

## As We Move Down

| Property | Changes |
|------------|---------|
| Speed | Decreases |
| Capacity | Increases |
| Cost | Decreases |

---

## Memory Hierarchy Table

| Memory | Speed | Capacity | Location |
|----------|--------|-----------|----------|
| Registers | Fastest | Very Small | CPU |
| Cache | Very Fast | Small | CPU |
| RAM | Fast | Medium | Motherboard |
| SSD | Slower | Large | Storage |
| HDD | Slowest | Very Large | Storage |

---

# 8. RAM, Cache & Registers

---

# Registers

- Inside CPU
- Fastest memory
- Stores immediate operands and results

Example

```
R1 = 20

R2 = 30

ADD
```

---

# Cache Memory

A small, high-speed memory between CPU and RAM.

```
CPU

↓

Cache

↓

RAM
```

Purpose:

- Reduce RAM access
- Improve performance

---

# RAM

Main memory used while programs are running.

Examples loaded into RAM:

- Chrome
- VS Code
- Spotify
- WhatsApp

Characteristics:

- Volatile
- Fast
- Temporary Storage

---

# 9. Allocation & Deallocation

---

## Allocation

When a process starts,

the Operating System allocates memory.

```
Program Starts

↓

Memory Allocated

↓

Program Executes
```

---

## Deallocation

When a process finishes,

the memory is released.

```
Program Ends

↓

Memory Freed

↓

Available Again
```

---

## Example

Open Calculator

↓

Memory Allocated

↓

Close Calculator

↓

Memory Released

---

# 10. Fragmentation & Compaction

After many allocations and deallocations,

memory develops empty gaps.

---

# Fragmentation

```
+----+------+----+------+----+------+
|App | Free |App | Free |App | Free |
+----+------+----+------+----+------+
```

Although enough total memory exists,

it is split into many small pieces.

---

## Types of Fragmentation

### Internal Fragmentation

Unused memory **inside** an allocated block.

Example:

```
Allocated Block = 100 KB

Program Uses = 70 KB

Unused = 30 KB
```

---

### External Fragmentation

Free memory exists,

but it is scattered.

```
Free

App

Free

App

Free
```

A large program cannot fit into any one free block.

---

# Compaction

The Operating System moves allocated blocks together.

Before:

```
+----+------+----+------+----+
|App | Free |App | Free |App |
+----+------+----+------+----+
```

After:

```
+----+----+----+----------------+
|App |App |App |      Free      |
+----+----+----+----------------+
```

Now one large continuous block is available.

---

# 11. Quick Revision

| Topic | Key Idea |
|--------|----------|
| Process Communication | Processes exchange information to work together. |
| IPC | Mechanism for communication between processes. |
| Pipes | Output of one process becomes input of another. |
| Redirection | Redirect input/output between files and programs. |
| Client-Server | Client requests services; server processes and responds. |
| Memory Management | OS allocates, tracks, protects, and frees memory. |
| Memory Hierarchy | Registers → Cache → RAM → SSD → HDD. |
| Registers | Fastest memory inside CPU. |
| Cache | Frequently accessed data stored for faster access. |
| RAM | Main memory used by running programs. |
| Allocation | OS assigns memory to processes. |
| Deallocation | OS releases memory after process completion. |
| Fragmentation | Free memory becomes scattered or partially unused. |
| Compaction | Rearranges memory to create larger continuous free space. |

---

# Key Takeaways

- Modern applications rely on **Inter-Process Communication (IPC)** to exchange data and coordinate tasks.
- **Pipes** and **redirection** are simple IPC mechanisms commonly used in operating systems like Linux.
- Most applications follow the **Client-Server architecture**, where clients request services and servers provide responses.
- **Memory Management** ensures efficient allocation, protection, and release of memory for running programs.
- The **Memory Hierarchy** balances speed, capacity, and cost using Registers, Cache, RAM, and Storage.
- **Fragmentation** wastes usable memory space, while **Compaction** reorganizes memory to improve allocation efficiency.

---
**Next Topics**

- Shared Memory
- Message Queues
- Sockets
- Signals
- Paging
- Segmentation
- Virtual Memory
- Page Replacement Algorithms
