# Operating System Fundamentals
## Kernel, System Calls, User Mode vs Kernel Mode, Linux Directory Structure & Terminal Exploration

> **Course:** Operating System  
> **Level:** B.Tech / BCA / MCA  
> **Goal:** Understand how the Operating System works internally through practical examples.

---

# Table of Contents

1. Kernel Responsibilities
2. System Calls
3. User Mode vs Kernel Mode
4. Linux Directory Structure
5. Terminal Exploration
6. Practical Commands
7. Interview Questions

---

# 1. Kernel Responsibilities

## What is a Kernel?

The **Kernel** is the **core (heart)** of an Operating System.

It acts as an intermediary between:

- Applications
- Hardware

Applications **cannot directly access hardware**. Every request goes through the Kernel.

```
Application
      │
      ▼
   Kernel
      │
      ▼
 Hardware
```

---

## Why Do We Need a Kernel?

Imagine multiple applications running simultaneously.

- Chrome
- VS Code
- Spotify
- Terminal
- Games

All require:

- CPU
- RAM
- Disk
- Network
- GPU

The Kernel decides:

- Who gets CPU?
- How much memory is allocated?
- Which device should respond?
- Which process runs first?

---

## Responsibilities of Kernel

### 1. Process Management

A **Process** is a running program.

Examples:

- Chrome
- VS Code
- Spotify

Kernel responsibilities:

- Create Process
- Destroy Process
- Schedule Process
- Context Switching
- Process Synchronization

Example:

```
Chrome

↓

VS Code

↓

Spotify

↓

Terminal
```

The Kernel rapidly switches between them using CPU Scheduling.

---

### 2. Memory Management

Kernel manages RAM.

Responsibilities:

- Allocate Memory
- Deallocate Memory
- Protect Memory
- Virtual Memory

Example:

```
RAM

+-------------+
Chrome
+-------------+

VS Code

+-------------+

Spotify

+-------------+
```

Without memory protection:

Chrome could access VS Code's memory.

---

### 3. Device Management

Hardware devices include:

- Keyboard
- Mouse
- Printer
- Camera
- GPU
- SSD

Kernel communicates through **Device Drivers**.

```
Application

↓

Kernel

↓

Driver

↓

Printer
```

---

### 4. File System Management

Kernel performs:

- Create File
- Delete File
- Read
- Write
- Rename
- Permissions

Applications never directly read the SSD.

```
Application

↓

Kernel

↓

SSD
```

---

### 5. CPU Scheduling

Kernel decides

- Which process runs?
- For how long?

Common Scheduling Algorithms:

- FCFS
- SJF
- Priority
- Round Robin

---

### 6. Interrupt Handling

Example:

```
Keyboard

↓

Interrupt

↓

Kernel

↓

Application
```

Interrupts occur when:

- Key Pressed
- Mouse Clicked
- USB Inserted
- Network Packet Arrives

---

### 7. Networking

Kernel manages

- TCP/IP
- Sockets
- Routing
- Ports

---

### 8. Security & Protection

Kernel checks:

- User Permission
- File Permission
- Process Isolation

---

## Summary

Kernel responsibilities include:

- Process Management
- Memory Management
- Device Management
- File System Management
- CPU Scheduling
- Interrupt Handling
- Networking
- Security

---

# 2. System Calls

## What is a System Call?

A **System Call** is the interface between:

Applications ↔ Kernel

Applications request services through System Calls.

```
Application

↓

System Call

↓

Kernel

↓

Hardware
```

---

## Why Are System Calls Needed?

Applications cannot:

- Read SSD
- Access RAM directly
- Use Printer
- Create Processes

They request the Kernel.

---

## Categories of System Calls

### Process Control

Examples

- fork()
- exec()
- wait()
- exit()

---

### File Management

Examples

- open()
- read()
- write()
- close()

---

### Device Management

Examples

- Access Printer
- Keyboard
- Camera
- USB

---

### Information Maintenance

Examples

- getpid()
- uname()
- time()

---

### Communication

Examples

- socket()
- pipe()
- shared memory

---

### Protection

Kernel checks permissions before granting access.

---

## How a System Call Works

```
Application

↓

System Call

↓

CPU switches to Kernel Mode

↓

Kernel Executes

↓

Hardware

↓

Return to User Mode
```

---

## Common Linux System Calls

| System Call | Purpose |
|-------------|----------|
| fork() | Create Process |
| exec() | Execute Program |
| wait() | Wait for Child |
| exit() | End Process |
| open() | Open File |
| read() | Read File |
| write() | Write File |
| close() | Close File |
| getpid() | Process ID |
| socket() | Networking |

---

# 3. User Mode vs Kernel Mode

Modern CPUs provide two execution modes.

```
User Mode

Kernel Mode
```

---

## User Mode

Applications run here.

Examples

- Chrome
- VS Code
- Java Program
- Python Program

Cannot

- Access Hardware
- Execute Privileged Instructions
- Access Physical Memory

---

## Kernel Mode

Operating System runs here.

Can access

- CPU
- RAM
- SSD
- GPU
- USB
- Printer

Everything.

---

## Mode Switching

Whenever an application needs a Kernel service

```
User Mode

↓

System Call

↓

Kernel Mode

↓

Kernel Executes

↓

User Mode
```

This is called **Mode Switching**.

---

## User Space vs Kernel Space

| User Space | Kernel Space |
|------------|--------------|
| Applications | Kernel |
| Limited Privileges | Full Privileges |
| Safe | Powerful |
| Cannot Access Hardware | Direct Hardware Access |

---

## Why Two Modes?

Without User Mode:

- Malware could format SSD.
- Applications could overwrite Kernel memory.
- One crash could crash the entire Operating System.

---

# 4. Linux Directory Structure

Linux starts from

```
/
```

Unlike Windows

```
C:
D:
E:
```

Linux has a **single root directory**.

---

## Directory Tree

```
/

├── boot
├── dev
├── etc
├── home
├── proc
├── sys
├── tmp
├── usr
├── var
```

---

## /boot

Contains

- Linux Kernel
- initramfs
- GRUB

Required during boot.

---

## /dev

Contains device files.

Examples

```
/dev/sda

/dev/null

/dev/random

/dev/tty
```

Everything in Linux is treated as a file.

---

## /proc

Virtual filesystem.

Stores

- Running Processes
- CPU Information
- Memory Information
- Kernel Information

Useful files

```
/proc/cpuinfo

/proc/meminfo

/proc/version
```

---

## /sys

Virtual filesystem.

Stores hardware information.

Examples

```
/sys/block

/sys/devices

/sys/kernel
```

---

## /home

User files.

Example

```
/home/akash
```

---

## /etc

Configuration files.

Examples

- hostname
- network
- users

---

## /usr

Installed applications

Libraries

Documentation

---

## /var

Variable files.

Examples

- Logs
- Cache
- Databases

---

## /tmp

Temporary files.

Automatically cleaned by the system.

---

## /proc vs /sys

| /proc | /sys |
|---------|-------|
| Process Information | Hardware Information |
| CPU Info | Device Info |
| Memory | Kernel Objects |

---

# 5. Terminal Exploration

Terminal is the primary interface with Linux.

Developers use Terminal for

- Git
- Docker
- AWS
- Kubernetes
- Linux Servers

---

## Basic Navigation

Show current directory

```bash
pwd
```

List files

```bash
ls
```

Long listing

```bash
ls -l
```

Hidden files

```bash
ls -la
```

---

## Directory Commands

Create directory

```bash
mkdir OS
```

Change directory

```bash
cd OS
```

Go back

```bash
cd ..
```

Go Home

```bash
cd
```

---

## File Commands

Create file

```bash
touch notes.txt
```

Read file

```bash
cat notes.txt
```

Copy

```bash
cp file1 file2
```

Move

```bash
mv file1 folder
```

Rename

```bash
mv old new
```

Delete

```bash
rm file
```

---

## Process Commands

Show running processes

```bash
ps
```

Interactive Monitor

```bash
top
```

Kill process

```bash
kill PID
```

---

## Memory

Linux

```bash
free -h
```

macOS

```bash
vm_stat
```

---

## CPU

```bash
lscpu
```

or

```bash
cat /proc/cpuinfo
```

---

## Disk Usage

```bash
df -h
```

Directory size

```bash
du -sh .
```

---

## Kernel Version

```bash
uname -r
```

Complete Information

```bash
uname -a
```

---

## System Information

Current User

```bash
whoami
```

Hostname

```bash
hostname
```

Date

```bash
date
```

---

## Networking

IP Address

```bash
ip addr
```

or

```bash
ifconfig
```

Test Connection

```bash
ping google.com
```

---

## System Calls Demonstration

Linux

```bash
strace ls
```

Shows

- open()
- read()
- write()
- close()

---

# Complete Flow

```
Power Button

↓

BIOS / UEFI

↓

Bootloader (GRUB)

↓

Kernel Starts

↓

Hardware Initialized

↓

User Login

↓

Shell Starts

↓

User Executes Command

↓

System Call

↓

Kernel

↓

Hardware

↓

Result

↓

Terminal
```

---

# Important Commands Summary

| Command | Purpose |
|----------|----------|
| pwd | Current Directory |
| ls | List Files |
| cd | Change Directory |
| mkdir | Create Directory |
| touch | Create File |
| cat | Read File |
| cp | Copy File |
| mv | Move/Rename |
| rm | Delete File |
| ps | Running Processes |
| top | Live Process Monitor |
| kill | Stop Process |
| free -h | Memory Usage |
| lscpu | CPU Information |
| uname -a | System Information |
| whoami | Current User |
| hostname | Machine Name |
| df -h | Disk Usage |
| du -sh | Directory Size |
| ping | Network Test |

---

# Interview Questions

1. What is a Kernel?
2. Explain Kernel responsibilities.
3. What is a System Call?
4. Why are System Calls required?
5. Difference between User Mode and Kernel Mode.
6. What is Mode Switching?
7. Explain `/boot`, `/proc`, `/sys`, and `/dev`.
8. Why does Linux treat hardware as files?
9. Explain the Linux directory hierarchy.
10. Which terminal commands do you use to monitor processes and memory?

---

# Key Takeaways

- The **Kernel** is the heart of the Operating System.
- **System Calls** are the bridge between applications and the Kernel.
- **User Mode** provides safety, while **Kernel Mode** has full privileges.
- Linux organizes everything under a **single root (`/`) directory**.
- `/boot`, `/dev`, `/proc`, and `/sys` are essential directories for understanding how Linux interacts with hardware and the kernel.
- The **Terminal** is the primary tool for interacting with Linux, making it an essential skill for developers, DevOps engineers, and system administrators.
