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
