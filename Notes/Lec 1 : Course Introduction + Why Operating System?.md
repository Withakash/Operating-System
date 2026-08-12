# Operating System – Lecture 1

## Course Introduction, Why Operating System?, Functions, Real-World Examples & Terminal Basics

> **Subject:** Operating System
> **Lecture:** 1
> **Duration:** 50 Minutes
> **Level:** Beginner
> **Approach:** Concept + Real-world examples + Live Terminal Practice

---

# 1. Course Roadmap & How We'll Learn OS

Operating System is not only a theoretical subject.

The goal of this course is to understand:

* How a computer actually works behind the applications.
* How the operating system manages CPU, memory, files, devices, and processes.
* How programs communicate with the operating system.
* How Linux commands and system calls interact with the OS.
* How concepts such as processes, threads, memory, files, and scheduling work internally.

## What We Will Study

### Unit 1 – OS Fundamentals

* What is an Operating System?
* Why do we need an OS?
* Functions of OS
* Evolution of Operating Systems
* Types of OS
* OS Architecture
* Kernel and User Space
* Booting Process
* Processes and Threads
* CPU Scheduling
* Inter-Process Communication
* Memory Management
* Virtual Memory

### Unit 2 – Process Management

* Process Lifecycle
* Process Control Block (PCB)
* Process States
* Threads
* CPU Scheduling Algorithms
* Context Switching
* Synchronization
* Mutex
* Semaphores
* Monitors
* Deadlocks

### Unit 3 – Memory Management

* Memory Hierarchy
* Contiguous Allocation
* Fragmentation
* Paging
* Segmentation
* Virtual Memory
* Demand Paging
* Page Replacement
* Thrashing

### Unit 4 – File & I/O Management

* File Systems
* File Organization
* Directory Structure
* File Access Methods
* File Protection
* Free Space Management
* Disk Scheduling
* RAID
* I/O Management

### Unit 5 – Protection & Security

* Protection Goals
* Access Matrix
* Authentication
* Security Threats
* Intrusion Detection
* Cryptography
* Operating System Security

---

# 2. How We Will Learn OS

The course will follow a simple pattern:

```text
CONCEPT
   ↓
REAL-WORLD EXAMPLE
   ↓
DIAGRAM
   ↓
LINUX COMMAND / PRACTICAL
   ↓
CODING / SYSTEM CALL
   ↓
INTERVIEW QUESTION
```

For example:

```text
Process
   ↓
Chrome is a process
   ↓
Process State Diagram
   ↓
ps command
   ↓
fork() program
   ↓
Process-related interview questions
```

This helps us understand both:

* **What the OS does**
* **How the OS actually behaves**

---

# 3. Why Do We Need an Operating System?

Before understanding an OS, imagine a computer **without an operating system**.

Suppose you switch on a computer.

You have:

* CPU
* RAM
* Keyboard
* Mouse
* SSD
* Monitor
* Network Card

But there is no Windows, Linux, macOS, Android, etc.

What would happen?

The hardware exists, but there is no convenient system to manage it for applications and users.

---

# 4. Life Without an Operating System

Consider opening a simple application such as a calculator.

Without an OS, the application would somehow need to directly handle:

* CPU instructions
* RAM allocation
* Keyboard input
* Display output
* Storage
* Hardware communication
* Device management

That would be extremely difficult.

Imagine every application having to know how every hardware device works.

For example:

```text
Application
    |
    +----> CPU
    |
    +----> RAM
    |
    +----> Keyboard
    |
    +----> SSD
    |
    +----> Network Card
    |
    +----> Display
```

Every program would need hardware-specific knowledge.

That is not practical.

---

# 5. Problems Without an OS

## Problem 1 – Resource Management

Many programs want to use the CPU.

Who gets the CPU?

```text
Chrome
VS Code
Spotify
Terminal
Game
```

An OS provides CPU scheduling.

---

## Problem 2 – Memory Management

Suppose the computer has:

```text
8 GB RAM
```

and several programs are running.

The OS decides:

* How much memory a process can use.
* Which memory is free.
* Which memory belongs to which process.
* What happens when memory becomes insufficient.

---

## Problem 3 – Device Management

Applications use:

* Keyboard
* Mouse
* Printer
* Disk
* Network
* Display

Applications should not need to directly control every hardware device.

The OS provides a controlled interface.

---

## Problem 4 – File Management

Suppose an application wants to save:

```text
photo.jpg
```

The application should not need to manually understand:

* Disk sectors
* SSD blocks
* File allocation
* Directory structures
* Permissions

The OS manages these details.

---

## Problem 5 – Security

Suppose Chrome wants to access another application's private memory.

Should it be allowed?

Usually:

```text
NO
```

The OS provides isolation and protection mechanisms.

---

# 6. What is an Operating System?

## Definition

An **Operating System (OS)** is system software that manages computer hardware and software resources and provides services to application programs and users.

In simple words:

> **The Operating System acts as a manager between applications, users, and hardware.**

---

# 7. Operating System as a Manager

Think of a college.

```text
                    PRINCIPAL
                       |
               Operating System
                       |
        +--------------+--------------+
        |              |              |
      CPU             RAM           Storage
        |              |              |
      Apps            Apps           Files
```

The OS coordinates the resources.

---

# 8. Operating System as an Interface

A user normally does not directly control hardware.

Instead:

```text
User
  ↓
Application
  ↓
Operating System
  ↓
Hardware
```

Example:

You click **Save** in a text editor.

```text
User
 ↓
Text Editor
 ↓
Operating System
 ↓
File System
 ↓
Storage Device
```

The application requests the service.

The OS handles the hardware details.

---

# 9. Two Important Roles of an OS

An operating system can be viewed in two major ways.

## 1. Resource Manager

It manages:

* CPU
* Memory
* Storage
* Devices
* Network resources

## 2. Control Program

It controls the execution of programs and prevents incorrect or unauthorized use of resources.

---

# 10. Functions of an Operating System

A modern OS performs many responsibilities.

---

## 10.1 Process Management

A process is a program in execution.

The OS:

* Creates processes
* Terminates processes
* Schedules processes
* Switches between processes
* Synchronizes processes
* Handles process communication

Example:

```text
Chrome
VS Code
Spotify
Terminal
```

The OS decides how CPU time is shared.

---

# 10.2 Memory Management

The OS manages RAM.

It tracks:

```text
Which memory is free?
Which process owns which memory?
How much memory is required?
Which memory should be released?
```

Example:

```text
RAM = 16 GB

Chrome      → 3 GB
VS Code     → 2 GB
OS          → 4 GB
Other Apps  → 5 GB
```

The OS keeps track of these allocations.

---

# 10.3 File Management

The OS manages files and directories.

Examples:

```text
document.txt
photo.jpg
program.java
video.mp4
```

It provides operations such as:

```text
Create
Open
Read
Write
Rename
Delete
```

---

# 10.4 Device Management

The OS manages hardware devices such as:

* Keyboard
* Mouse
* Printer
* Disk
* USB devices
* Network cards

Applications communicate with devices through OS-managed interfaces.

---

# 10.5 Security and Protection

The OS protects resources.

Examples:

```text
User permissions
File permissions
Process isolation
Memory protection
Authentication
```

Linux example:

```bash
-rw-r--r-- 1 user user file.txt
```

The OS uses permissions to determine who can read or modify the file.

---

# 10.6 CPU Scheduling

Multiple processes compete for CPU time.

The OS selects which process should run.

```text
Ready Queue

P1
P2
P3
P4
 |
 v
CPU Scheduler
 |
 v
CPU
```

---

# 10.7 Networking

Modern operating systems provide networking capabilities.

Examples:

* TCP/IP
* Network interfaces
* Sockets
* Wi-Fi
* Ethernet
* DNS interaction

Applications such as browsers depend heavily on these services.

---

# 10.8 User Interface

An OS provides ways for users to interact with the computer.

Two common interfaces are:

### GUI

Graphical User Interface

Examples:

* Windows Desktop
* macOS Finder
* Ubuntu Desktop
* Android UI

### CLI

Command Line Interface

Examples:

```bash
pwd
ls
cd
mkdir
rm
```

---

# 11. Operating System Examples

Common operating systems include:

| Operating System | Common Usage                     |
| ---------------- | -------------------------------- |
| Windows          | Desktop, Laptop, Enterprise      |
| Ubuntu           | Development, Servers, Education  |
| Linux            | Servers, Embedded Systems, Cloud |
| macOS            | Apple Desktop/Laptop             |
| Android          | Smartphones, Tablets             |
| iOS              | iPhone                           |
| ChromeOS         | Chromebooks                      |

---

# 12. Windows

Windows is a popular desktop operating system developed by Microsoft.

Common uses:

* Personal computers
* Office systems
* Gaming
* Enterprise systems

Examples:

```text
Windows 10
Windows 11
Windows Server
```

---

# 13. Ubuntu

Ubuntu is a Linux distribution based on Debian.

Popular for:

* Software development
* Servers
* Cloud
* Programming education
* DevOps

One major advantage for this course:

> Ubuntu provides an excellent environment for learning operating system concepts through the terminal.

---

# 14. macOS

macOS is Apple's desktop operating system.

It is based on Unix technologies and provides:

* GUI
* Terminal
* File system
* Process management
* Networking
* Security

The macOS terminal is especially useful for developers because many Unix-style commands are available.

---

# 15. Android

Android is a mobile operating system.

It is widely used on smartphones and tablets.

Android manages:

* Applications
* Memory
* CPU
* Sensors
* Camera
* Storage
* Network
* Power

---

# 16. iOS

iOS is Apple's mobile operating system for iPhone.

The OS provides controlled access to:

* CPU
* Memory
* Camera
* Storage
* Network
* Sensors

It also provides strong application isolation and security.

---

# 17. Common OS Architecture

A simplified model:

```text
+-----------------------------+
|           USER              |
+-----------------------------+
              |
              v
+-----------------------------+
|       APPLICATIONS          |
| Chrome | VS Code | VLC      |
+-----------------------------+
              |
              v
+-----------------------------+
|       OPERATING SYSTEM      |
| Process Management          |
| Memory Management            |
| File Management              |
| Device Management            |
| Security                     |
+-----------------------------+
              |
              v
+-----------------------------+
|          HARDWARE            |
| CPU | RAM | SSD | Devices   |
+-----------------------------+
```

---

# 18. Kernel

The **Kernel** is the core part of the operating system.

It directly manages important system resources.

Examples:

* CPU
* Memory
* Devices
* Processes
* System calls

Simplified view:

```text
Applications
     |
     v
System Calls
     |
     v
Kernel
     |
     v
Hardware
```

We will study the kernel in more detail later.

---

# 19. What Happens When You Open an Application?

Suppose you open a browser.

A simplified sequence is:

```text
User clicks browser
        ↓
OS receives request
        ↓
Program loaded into memory
        ↓
Process created
        ↓
CPU scheduled
        ↓
Browser starts execution
```

This single action involves several OS concepts.

Later in the course, we will study each part separately.

---

# 20. First Practical – Open Terminal

The terminal allows us to communicate with the operating system using commands.

## Ubuntu

Press:

```text
Ctrl + Alt + T
```

or open:

```text
Applications → Terminal
```

## macOS

Press:

```text
Command + Space
```

Search:

```text
Terminal
```

Open it.

---

# 21. Terminal Command #1 – pwd

## Meaning

`pwd` means:

```text
Print Working Directory
```

It shows the directory in which the terminal is currently working.

Run:

```bash
pwd
```

Example output:

```text
/home/akash
```

or on macOS:

```text
/Users/akash
```

---

# 22. Understanding the Output

Suppose:

```bash
pwd
```

returns:

```text
/home/akash
```

Break it down:

```text
/
|
+-- home
     |
     +-- akash
```

The first `/` represents the root directory.

---

# 23. Terminal Command #2 – ls

## Meaning

`ls` means:

```text
List
```

It displays files and directories in the current location.

Run:

```bash
ls
```

Example:

```text
Desktop
Documents
Downloads
Music
Pictures
Videos
```

---

# 24. Useful ls Variations

## Long Format

```bash
ls -l
```

Shows additional information such as:

* Permissions
* Owner
* Group
* Size
* Date
* Filename

Example:

```text
-rw-r--r--  1 akash akash  1250 Aug 12 10:30 notes.txt
```

---

## Show Hidden Files

```bash
ls -a
```

Example:

```text
.
..
.bashrc
.config
Desktop
Documents
```

---

## Long + Hidden

```bash
ls -la
```

This is one of the most commonly used variations.

---

# 25. Terminal Command #3 – cd

## Meaning

`cd` means:

```text
Change Directory
```

It is used to move from one directory to another.

Example:

```bash
cd Documents
```

Now verify:

```bash
pwd
```

You may see:

```text
/home/akash/Documents
```

---

# 26. Going Back One Directory

Use:

```bash
cd ..
```

Example:

```text
/home/akash/Documents
```

Run:

```bash
cd ..
```

Now:

```text
/home/akash
```

---

# 27. Go to Home Directory

Use:

```bash
cd ~
```

or simply:

```bash
cd
```

`~` represents the current user's home directory.

---

# 28. Absolute Path vs Relative Path

## Absolute Path

Starts from root.

Example:

```bash
cd /home/akash/Documents
```

This gives the complete path.

---

## Relative Path

Starts from the current directory.

Example:

```bash
cd Documents
```

The meaning depends on the current location.

---

# 29. Terminal Practice – Complete Flow

Run these commands one by one:

```bash
pwd
```

Then:

```bash
ls
```

Then:

```bash
cd Documents
```

Then:

```bash
pwd
```

Then:

```bash
ls
```

Then go back:

```bash
cd ..
```

Finally:

```bash
pwd
```

---

# 30. Practice Challenge

Without looking at the previous commands, perform this:

```text
1. Find your current directory.
2. List all files.
3. Enter Documents.
4. List files.
5. Return to the previous directory.
6. Show the current location again.
```

Expected commands:

```bash
pwd
ls
cd Documents
ls
cd ..
pwd
```

---

# 31. Understanding the Terminal Prompt

You may see something similar to:

```text
akash@ubuntu:~$
```

Let's understand it.

```text
akash
```

→ Username

```text
ubuntu
```

→ Host/Computer name

```text
~
```

→ Current user's home directory

```text
$
```

→ Shell prompt for a normal user

---

# 32. Why Are We Starting with Linux Terminal?

Because many Operating System concepts become easier to understand through practical observation.

For example:

### Process

```bash
ps
```

### Memory

```bash
free -h
```

### CPU

```bash
top
```

### Files

```bash
ls
```

### Current directory

```bash
pwd
```

### Users

```bash
whoami
```

So throughout this course, the terminal will be our practical laboratory.

---

# 33. First OS Practical Exercise

Open the terminal and execute:

```bash
whoami
```

What does it show?

Your current username.

---

Run:

```bash
pwd
```

What does it show?

Your current working directory.

---

Run:

```bash
ls
```

What does it show?

Files and directories.

---

Run:

```bash
ls -la
```

What additional entries do you see?

Look for:

```text
.
..
```

and hidden files such as:

```text
.bashrc
.config
```

---

# 34. Observation Exercise

Ask students:

### Question 1

When you execute:

```bash
ls
```

Who actually executes the command?

The shell and operating system work together to execute the request.

---

### Question 2

Who knows where the current directory is?

The shell maintains the current working directory context and interacts with the operating system.

---

### Question 3

Who ultimately accesses the storage device?

The operating system manages storage and filesystem operations.

---

# 35. Important Terminology

### Hardware

Physical components.

Examples:

```text
CPU
RAM
SSD
Keyboard
Mouse
```

### Software

Programs and instructions.

Examples:

```text
Chrome
VS Code
VLC
Java
```

### Operating System

System software that manages hardware and provides services to software.

### Kernel

Core component of the operating system.

### Process

A program that is currently executing.

### Shell

A program that provides a command-line interface to the operating system.

---

# 36. Quick Concept Map

```text
                    COMPUTER SYSTEM
                           |
              +------------+------------+
              |                         |
          Hardware                  Software
              |                         |
      +-------+-------+          +------+------+
      |   |    |      |          |             |
     CPU RAM  SSD   Devices   Applications     OS
                                               |
                                               |
                                             Kernel
                                               |
                                               |
                                          Hardware
```

---

# 37. Key Takeaways

* An operating system is system software that manages hardware and software resources.
* Without an OS, applications would have to directly manage many hardware details.
* The OS acts as a bridge between applications/users and hardware.
* Major OS responsibilities include:

  * Process Management
  * Memory Management
  * File Management
  * Device Management
  * CPU Scheduling
  * Security
  * Networking
  * User Interface
* Windows, Ubuntu, macOS, Android, and iOS are examples of operating systems.
* The kernel is the core component of an OS.
* The terminal provides a command-line interface for interacting with the system.
* Important first commands:

  * `pwd`
  * `ls`
  * `cd`

---

# 38. Interview / Viva Questions

### Q1. What is an Operating System?

An operating system is system software that manages computer hardware and software resources and provides services to applications.

### Q2. Why do we need an OS?

To manage resources, provide services to applications, provide security, and offer an interface between users/applications and hardware.

### Q3. Is an Operating System application software?

No. It is system software.

### Q4. What is the kernel?

The kernel is the core component of the operating system that manages important system resources and provides low-level services.

### Q5. Give examples of operating systems.

Windows, Linux, Ubuntu, macOS, Android, and iOS.

### Q6. What is a terminal?

A terminal is a command-line interface through which users can interact with the operating system using commands.

### Q7. What does `pwd` do?

It prints the current working directory.

### Q8. What does `ls` do?

It lists files and directories.

### Q9. What does `cd` do?

It changes the current working directory.

---

# 39. Homework / Practice

### Task 1

Run and understand:

```bash
pwd
ls
ls -l
ls -a
ls -la
cd
cd ..
cd ~
whoami
```

### Task 2

Create your own command notes.

For every command, write:

```text
Command
Purpose
Example
Output
```

### Task 3

Explore your home directory using only:

```text
pwd
ls
cd
```

Do not use a graphical file manager.

---

# Final Mental Model

Remember this diagram:

```text
                 USER
                   |
                   v
            APPLICATIONS
                   |
                   v
            OPERATING SYSTEM
                   |
          +--------+--------+
          |        |        |
         CPU      RAM     STORAGE
          |        |        |
          +--------+--------+
                   |
                DEVICES
```

The central idea of this entire subject is:

> **The Operating System manages resources and provides a controlled environment in which applications can execute.**

As we move forward, we will open this large box called **Operating System** and study each responsibility internally:

```text
Operating System
       |
       +---- Process Management
       |
       +---- CPU Scheduling
       |
       +---- Memory Management
       |
       +---- File Management
       |
       +---- I/O Management
       |
       +---- Protection & Security
```
