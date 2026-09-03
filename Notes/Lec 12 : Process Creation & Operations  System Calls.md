# Operating System Notes

# Process Creation, `fork()`, `exec()`, `wait()` and Process Termination

---

## Table of Contents

1. Introduction to Process Creation
2. Parent and Child Processes
3. Process Hierarchy
4. Process ID (PID) and Parent Process ID (PPID)
5. `fork()` System Call
6. Return Values of `fork()`
7. Process Execution After `fork()`
8. Multiple `fork()` Calls
9. Copy-on-Write Concept
10. `exec()` Family
11. Difference Between `fork()` and `exec()`
12. Using `fork()` and `exec()` Together
13. `wait()` System Call
14. Process Termination
15. Zombie Process
16. Orphan Process
17. Zombie vs Orphan Process
18. Complete Process Creation Flow
19. Important Exam Questions
20. Quick Revision

---

# 1. Introduction to Process Creation

A **process** is a program that is currently executing.

Whenever a program is executed, the Operating System creates a process to manage its execution.

The creation of a new process is called:

> **Process Creation**

A process may create one or more additional processes. These newly created processes may also create other processes.

Therefore, processes in an Operating System can form a hierarchy.

## Basic Process Creation Diagram

```text
        Parent Process
              |
              |
      Creates New Process
              |
              v
        Child Process
```

The process that creates another process is called the **Parent Process**, while the newly created process is called the **Child Process**.

---

# 2. Parent and Child Processes

In most modern operating systems, processes can create other processes.

```text
        Parent Process
              |
              |
            fork()
              |
              v
        Child Process
```

### Important Terms

| Term              | Meaning                                         |
| ----------------- | ----------------------------------------------- |
| Parent Process    | Process that creates another process            |
| Child Process     | Newly created process                           |
| Process Creation  | Creating a new process                          |
| Process Hierarchy | Relationship between parent and child processes |

A parent process may create:

* One child process
* Multiple child processes

A child process may also create its own child processes.

---

# 3. Process Hierarchy

Processes are commonly organized in the form of a **tree structure**.

This is called a:

> **Process Tree**

## Process Tree Diagram

```text
                    System Process
                         |
        ---------------------------------
        |               |               |
      Process A       Process B       Process C
        |
    -----------
    |         |
 Child 1    Child 2
```

Each process may have:

* Its own Process ID
* A Parent Process ID

---

# 4. Process ID (PID) and Parent Process ID (PPID)

Every process in the operating system is assigned a unique number called:

> **Process ID (PID)**

Example:

```text
Process: Program A
PID: 4500
```

The process that creates another process is identified using:

> **Parent Process ID (PPID)**

## Example

```text
PID      PPID      Process
--------------------------------
100       1        bash
250      100       gcc
300      100       ls
```

Here:

```text
bash
 |
 +---- gcc
 |
 +---- ls
```

Therefore:

* `bash` is the parent process.
* `gcc` and `ls` are child processes.

---

# 5. `fork()` System Call

In UNIX and Linux systems, a new process is commonly created using:

```c
fork()
```

The `fork()` system call creates a new process.

After `fork()` executes successfully:

* The original process continues as the **Parent Process**.
* A new **Child Process** is created.
* Both processes continue execution.

---

## Before `fork()`

```text
        +----------------+
        | Parent Process |
        +----------------+
```

## After `fork()`

```text
               fork()
                 |
                 v

        +----------------+
        | Parent Process |
        +----------------+

                 +

        +----------------+
        | Child Process  |
        +----------------+
```

---

# 6. Return Values of `fork()`

The `fork()` system call returns different values to the parent and child processes.

| Return Value   | Meaning                                                               |
| -------------- | --------------------------------------------------------------------- |
| `-1`           | Process creation failed                                               |
| `0`            | Currently executing in the Child Process                              |
| Positive Value | Currently executing in the Parent Process; value represents Child PID |

## Diagram

```text
                    fork()
                      |
          -------------------------
          |                       |
          v                       v

    Parent Process           Child Process

     pid > 0                   pid = 0

 Gets Child PID            Gets value 0
```

---

# 7. First `fork()` Program

```c
#include <stdio.h>
#include <unistd.h>

int main() {

    fork();

    printf("Hello\n");

    return 0;
}
```

## What Happens?

Before `fork()`:

```text
Only One Process
```

After `fork()`:

```text
            Original Process
                   |
                 fork()
                   |
          -----------------
          |               |
          v               v

       Parent           Child
          |               |
          |               |
       printf()        printf()
```

Therefore:

```text
Hello
Hello
```

The statement after `fork()` is executed by both the Parent and Child processes.

---

# 8. Identifying Parent and Child

Example:

```c
#include <stdio.h>
#include <unistd.h>

int main() {

    int pid = fork();

    if (pid == 0) {

        printf("I am the Child Process\n");

    } else if (pid > 0) {

        printf("I am the Parent Process\n");

    } else {

        printf("Process Creation Failed\n");
    }

    return 0;
}
```

## Execution Flow

```text
                  Program Starts
                        |
                        v
                     fork()
                        |
             ---------------------
             |                   |
             v                   v

       Parent Process       Child Process

          pid > 0              pid = 0
             |                   |
             v                   v

       Parent Code         Child Code
```

---

# 9. Parent and Child Execute Concurrently

After `fork()`, both processes can execute independently.

However, the order of execution is not guaranteed.

For example, sometimes the output may be:

```text
Parent Process
Child Process
```

Another time:

```text
Child Process
Parent Process
```

This happens because the Operating System scheduler decides which process receives CPU time.

## Important Point

> After `fork()`, the parent and child processes execute concurrently, and their execution order is determined by the CPU scheduler.

---

# 10. Parent and Child Have Different Process IDs

After `fork()`:

```text
Parent Process
PID = 100
```

```text
Child Process
PID = 101
```

Although the child is created from the parent, both are separate processes.

## Conceptual Diagram

```text
                fork()
                  |
        ---------------------
        |                   |
        v                   v

     Parent              Child

     PID 100             PID 101
```

Each process has its own execution context.

---

# 11. Multiple `fork()` Calls

Consider the following program:

```c
#include <stdio.h>
#include <unistd.h>

int main() {

    fork();
    fork();

    printf("Process\n");

    return 0;
}
```

## Initially

```text
        P
```

Total processes:

```text
1
```

## After First `fork()`

```text
        P
       / \
      P   C
```

Total processes:

```text
2
```

## After Second `fork()`

Both existing processes execute the second `fork()`.

```text
            P
          /   \
         P     C
        / \   / \
```

Total processes:

```text
4
```

---

## Formula

If all processes execute every `fork()` successfully:

```text
Maximum Number of Processes = 2ⁿ
```

Where:

```text
n = Number of fork() calls
```

### Example

| Number of `fork()` Calls | Maximum Processes |
| ------------------------ | ----------------: |
| 1                        |                 2 |
| 2                        |                 4 |
| 3                        |                 8 |
| 4                        |                16 |

---

# 12. Copy-on-Write (COW)

Creating a complete copy of the parent's memory immediately can be expensive.

Modern UNIX/Linux systems optimize process creation using:

> **Copy-on-Write (COW)**

Initially, the Parent and Child may share memory pages.

```text
Parent Process --------\
                        \
                         > Shared Memory Pages
                        /
Child Process --------/
```

When one process modifies a memory page:

```text
Before Modification

Parent --------> Memory Page
Child  --------> Memory Page
```

After modification:

```text
Parent --------> Original Page

Child  --------> New Copy
```

The actual copy is created only when modification becomes necessary.

Therefore:

> **Copy-on-Write improves the efficiency of process creation.**

---

# 13. `exec()` Family

The `exec()` family of functions is used to:

> **Replace the currently executing program with another program.**

Unlike `fork()`, `exec()` does not create a new process.

---

# 14. Understanding `exec()`

Suppose a process is running:

```text
PID = 500

Running Program A
```

After executing `exec()`:

```text
PID = 500

Running Program B
```

The program has changed.

The process is now executing a new program.

---

## Visual Representation

### Before `exec()`

```text
+---------------------------+
| Process                   |
| Running Program A         |
+---------------------------+
```

### After `exec()`

```text
+---------------------------+
| Process                   |
| Running Program B         |
+---------------------------+
```

Therefore:

```text
fork() → Creates a New Process

exec() → Replaces the Current Program
```

---

# 15. `exec()` Family Functions

The `exec()` family includes several functions:

```text
execl()
execv()
execlp()
execvp()
execle()
execve()
```

For beginners, the most important concept is:

> All `exec()` family functions are used to execute another program by replacing the current program image.

---

# 16. Difference Between `fork()` and `exec()`

| `fork()`                                   | `exec()`                      |
| ------------------------------------------ | ----------------------------- |
| Creates a new process                      | Does not create a new process |
| Parent and child exist after execution     | Current program is replaced   |
| Creates a child process                    | Loads another program         |
| Parent and child can execute independently | Old program is replaced       |
| Used for process creation                  | Used for program execution    |

---

# 17. Using `fork()` and `exec()` Together

This is one of the most important concepts in UNIX/Linux Operating Systems.

A common process creation model is:

```text
Parent Process
       |
       |
     fork()
       |
       +--------------------+
       |                    |
       v                    v

    Parent                Child
    continues               |
                             |
                           exec()
                             |
                             v

                     Executes New Program
```

---

# 18. Real-Life Example: Linux Shell

Suppose you type:

```bash
ls
```

The shell conceptually performs the following steps:

```text
                Shell
                  |
                  |
                fork()
                  |
          ----------------
          |              |
          v              v

       Parent          Child
       Shell             |
                         |
                       exec()
                         |
                         v

                    ls Program
```

After the child finishes:

```text
ls completes
     |
     v
Child terminates
     |
     v
Shell continues
```

This is the basic idea behind how command execution works in UNIX-like systems.

---

# 19. Example: `fork()` + `exec()`

```c
#include <stdio.h>
#include <unistd.h>

int main() {

    int pid = fork();

    if (pid == 0) {

        execl("/bin/ls", "ls", NULL);

    } else {

        printf("Parent Process\n");
    }

    return 0;
}
```

## Execution Diagram

```text
                  Program Starts
                        |
                        |
                      fork()
                        |
            -----------------------
            |                     |
            v                     v

       Parent Process        Child Process
            |                     |
            |                   execl()
            |                     |
            |                     v
            |                ls Program
            |
            v
      Parent Continues
```

---

# 20. What Happens After Successful `exec()`?

Consider:

```c
printf("Before exec\n");

execl("/bin/ls", "ls", NULL);

printf("After exec\n");
```

If `exec()` succeeds:

```text
Before exec
```

Then:

```text
ls program executes
```

The statement:

```text
After exec
```

normally does not execute.

Why?

Because the current program has been replaced.

---

# 21. `wait()` System Call

The `wait()` system call is used by a Parent Process to:

> **Wait for a Child Process to terminate.**

The parent may need to wait until the child completes its execution.

---

## Without `wait()`

```text
Parent
   |
 fork()
   |
   +----------> Child
   |
   |
Parent continues
```

Both processes can continue independently.

---

## With `wait()`

```text
Parent
   |
 fork()
   |
   +----------> Child Executes
   |
   |
 wait()
   |
   |
Parent waits
   |
   |
Child finishes
   |
   v
Parent continues
```

---

# 22. Parent Waiting for Child

```text
            Parent
               |
             fork()
               |
       ----------------
       |              |
       v              v

    Parent          Child
       |              |
     wait()        Executes
       |              |
       |           Terminates
       |              |
       <--------------
       |
       v
 Parent Continues
```

---

# 23. Example of `wait()`

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {

    int pid = fork();

    if (pid == 0) {

        printf("Child Process is Running\n");

    } else {

        wait(NULL);

        printf("Child has finished\n");
    }

    return 0;
}
```

## Expected Logical Flow

```text
Child Process is Running
          |
          v
Child Terminates
          |
          v
Parent receives notification
          |
          v
Child has finished
```

---

# 24. Why is `wait()` Important?

The `wait()` system call is important because:

* It synchronizes Parent and Child processes.
* It allows the Parent to know when the Child finishes.
* It allows the Parent to collect the Child's termination status.
* It helps prevent zombie processes from remaining unnecessarily.

---

# 25. Process Termination

A process terminates when it completes its execution.

Process termination can occur in different ways.

---

## Normal Termination

A program completes successfully.

Example:

```c
return 0;
```

or:

```c
exit(0);
```

Diagram:

```text
Process Running
       |
       |
Program Completes
       |
       v
Process Terminates
```

---

# 26. `exit()` System Call

A process can explicitly terminate using:

```c
exit()
```

Example:

```c
#include <stdlib.h>

int main() {

    exit(0);
}
```

The process sends termination information to the operating system.

---

# 27. Abnormal Process Termination

A process may terminate abnormally because of:

* Runtime errors
* Illegal instructions
* Memory access violations
* External signals
* Forced termination
* Resource limitations

Conceptually:

```text
Process Running
       |
       |
Unexpected Error
       |
       v
Process Terminates
```

---

# 28. What Happens When a Child Process Terminates?

When a Child Process terminates, it may not immediately disappear completely.

The Operating System may temporarily retain information such as:

* Process ID
* Exit status
* Termination information

Why?

Because the Parent Process may need to collect this information.

This leads to the concept of a:

# Zombie Process

---

# 29. Zombie Process

A **Zombie Process** is:

> A process that has completed execution but still has an entry in the process table because its Parent Process has not yet collected its termination status.

---

## Zombie Process Diagram

```text
Child Process Running
        |
        |
        v
Child Finishes Execution
        |
        |
        v

+--------------------------+
|      ZOMBIE PROCESS      |
|                          |
| Process has terminated   |
| Status information       |
| still remains            |
+--------------------------+
        |
        |
Parent calls wait()
        |
        v

Process Entry Removed
```

---

# 30. Important Points About Zombie Processes

A Zombie Process:

* Is already terminated.
* Does not execute instructions.
* Does not perform useful computation.
* Temporarily retains process information.
* Exists until the Parent collects its termination status.

## Key Concept

```text
Child is DEAD
Parent is ALIVE
Status not collected

        ↓

      ZOMBIE
```

---

# 31. Zombie Process Example

Conceptually:

```c
#include <stdio.h>
#include <unistd.h>

int main() {

    int pid = fork();

    if (pid == 0) {

        printf("Child finished\n");

    } else {

        sleep(20);
    }

    return 0;
}
```

## Flow

```text
Parent starts
     |
   fork()
     |
     +--------> Child
     |            |
     |         Finishes
     |            |
Parent sleeps      v
     |          ZOMBIE
     |
Parent wakes up
```

During the period when the Parent has not collected the Child's status, the Child may remain as a Zombie Process.

---

# 32. How is a Zombie Process Removed?

The Parent Process calls:

```c
wait()
```

or:

```c
waitpid()
```

Then:

```text
Zombie Process
       |
       |
Parent collects status
       |
       v
Process Completely Removed
```

---

# 33. Orphan Process

An **Orphan Process** occurs when:

> The Parent Process terminates while the Child Process is still running.

---

## Orphan Process Diagram

Initially:

```text
        Parent
           |
           |
         Child
```

Then:

```text
Parent Terminates
        X


Child Continues Running
        |
        v
     ORPHAN
```

---

# 34. What Happens to an Orphan Process?

The Operating System does not simply stop the Child Process.

The process is typically **re-parented** to an appropriate system process.

Conceptually:

```text
Parent Terminates
       |
       v
Child becomes Orphan
       |
       v
Operating System adopts Child
       |
       v
Child continues execution
```

---

# 35. Zombie Process vs Orphan Process

| Zombie Process                              | Orphan Process                |
| ------------------------------------------- | ----------------------------- |
| Child has terminated                        | Child is still running        |
| Parent is alive                             | Parent has terminated         |
| Process is not executing                    | Process continues executing   |
| Termination information remains temporarily | Child is re-parented/adopted  |
| Removed when status is collected            | Continues until it terminates |

---

# 36. Easy Memory Trick

## Zombie

```text
Child = Dead
Parent = Alive
```

Therefore:

> **Dead Child + Alive Parent = Zombie**

---

## Orphan

```text
Parent = Dead
Child = Alive
```

Therefore:

> **Dead Parent + Alive Child = Orphan**

---

# 37. Zombie vs Orphan Diagram

## Zombie Process

```text
Parent: ALIVE
       |
       |
Child: TERMINATED
       |
       v
    ZOMBIE
```

## Orphan Process

```text
Parent: TERMINATED
       X

Child: RUNNING
       |
       v
    ORPHAN
```

---

# 38. Complete Process Creation Flow

```text
                       PROGRAM STARTS
                              |
                              v
                         Parent Process
                              |
                              |
                            fork()
                              |
                   -----------------------
                   |                     |
                   v                     v

             Parent Process         Child Process
                   |                     |
                   |                     |
                   |                   exec()
                   |                     |
                   |                     v
                   |                New Program
                   |                     |
                   |                  Executes
                   |
                 wait()                  |
                   |                     |
              Parent waits             exit()
                   |                     |
                   |                     v
                   |                Terminates
                   |                     |
                   <---------------------
                     Status Collected
                              |
                              v
                       Parent Continues
```

---

# 39. Complete Concept Map

```text
                         PROCESS
                            |
              ----------------------------
              |                          |
              v                          v

         PROCESS CREATION          PROCESS TERMINATION
              |                          |
            fork()                    exit()
              |
       Creates Child
              |
              v
           exec()
              |
     Executes New Program
              |
              v
           wait()
              |
      Parent waits for Child
              |
      ---------------------
      |                   |
      v                   v

   Normal              Special Cases
Termination
                          |
                -------------------
                |                 |
                v                 v

             Zombie            Orphan
```

---

# 40. Summary of System Calls

| System Call | Purpose                           |
| ----------- | --------------------------------- |
| `fork()`    | Creates a new process             |
| `exec()`    | Replaces the current program      |
| `wait()`    | Parent waits for child            |
| `waitpid()` | Parent waits for a specific child |
| `exit()`    | Terminates a process              |

---

# 41. Important Comparison: `fork()`, `exec()` and `wait()`

| System Call | Main Purpose           |
| ----------- | ---------------------- |
| `fork()`    | Create a Child Process |
| `exec()`    | Run a new program      |
| `wait()`    | Wait for Child Process |
| `exit()`    | Terminate Process      |

## Easy Flow

```text
fork()
  ↓
Create Child
  ↓
exec()
  ↓
Run New Program
  ↓
exit()
  ↓
Child Terminates
  ↓
wait()
  ↓
Parent Collects Status
```

---

# 42. Final Blackboard Diagram

```text
                    PROCESS CREATION

                         Parent
                            |
                          fork()
                            |
                 ---------------------
                 |                   |
                 v                   v

              Parent               Child
                 |                   |
                 |                 exec()
                 |                   |
                 |              New Program
                 |                   |
               wait()               exit()
                 |                   |
                 |                   v

                 |                ZOMBIE
                 |                   |
                 -------- wait() -----
                 |
                 v
           Parent Continues
```

---

# 43. Important Exam Questions

## Short Answer Questions

1. What is Process Creation?
2. Define Parent Process and Child Process.
3. What is a Process ID?
4. What is the purpose of the `fork()` system call?
5. What are the return values of `fork()`?
6. What is the purpose of the `exec()` family?
7. What is the `wait()` system call?
8. Define Zombie Process.
9. Define Orphan Process.
10. Differentiate between Zombie and Orphan Process.

---

## Long Answer Questions

### Q1. Explain Process Creation in UNIX/Linux Operating Systems.

Include:

* Parent Process
* Child Process
* Process Hierarchy
* PID and PPID
* `fork()` system call
* Diagram

---

### Q2. Explain the `fork()` System Call with a suitable example.

Include:

* Definition
* Process creation
* Return values
* Parent and Child
* Program
* Diagram

---

### Q3. Explain the `exec()` Family of Functions.

Include:

* Definition
* Program replacement
* `exec()` family
* Difference between `fork()` and `exec()`
* Diagram

---

### Q4. Explain `fork()` and `exec()` Together.

Use this diagram:

```text
Parent
   |
 fork()
   |
   +-------- Parent
   |
   +-------- Child
               |
             exec()
               |
          New Program
```

---

### Q5. Explain the `wait()` System Call.

Include:

* Parent-child synchronization
* Waiting mechanism
* Child termination
* Collection of termination status

---

### Q6. Explain Zombie and Orphan Processes with diagrams.

Include:

```text
Zombie:
Parent Alive + Child Dead
```

```text
Orphan:
Parent Dead + Child Alive
```

---

# 44. Quick Revision Notes

### Process Creation

> Creating a new process in the Operating System.

### Parent Process

> A process that creates another process.

### Child Process

> A process created by another process.

### `fork()`

> Creates a new child process.

### `exec()`

> Replaces the current program with a new program.

### `wait()`

> Makes the parent wait for the child process.

### Zombie Process

> Child has terminated, but the Parent has not collected its termination status.

### Orphan Process

> Parent has terminated, but the Child is still running.

---

# 45. Final Memory Formula

```text
fork()  → CREATE

exec()  → REPLACE

wait()  → WAIT

exit()  → TERMINATE
```

And remember:

```text
Parent Alive + Child Dead = ZOMBIE

Parent Dead + Child Alive = ORPHAN
```


# 🔍 Process Detective Lab

## Exploring Linux Processes Practically

**Subject:** Operating System
**Topic:** Process Investigation and Visualization in Linux
**Prerequisite:** Process, PID, PPID, `fork()`

---

# 🎯 Objective

In this practical lab, we will become **Process Detectives**. 🕵️

Instead of only learning theoretical definitions, we will investigate real processes running inside Linux.

We will learn how to:

* View running processes
* Find a process using its name
* Find PID and PPID
* Visualize process trees
* See only our own process tree
* Run programs in the background
* Observe parent and child processes
* Monitor processes
* Kill a process
* Observe process states
* Understand Zombie and Orphan processes

---

# 1. What is a Process?

A **process** is a program that is currently executing.

For example:

When you run:

```bash
firefox
```

Linux creates one or more processes to execute the program.

Every process has important information such as:

```text
Process Name
PID
PPID
State
CPU Usage
Memory Usage
```

---

# 2. Important Terms

## PID — Process ID

Every process has a unique identification number.

Example:

```text
PID = 12345
```

---

## PPID — Parent Process ID

Every process is usually created by another process.

The ID of the parent process is called:

```text
PPID
```

Example:

```text
PID = 12346
PPID = 12345
```

This means:

```text
Process 12345
      |
      └── Process 12346
```

---

# 3. First Process Detective Command — `ps`

The `ps` command displays information about processes.

Run:

```bash
ps
```

Example:

```text
PID TTY          TIME CMD
1250 pts/0    00:00:00 bash
1400 pts/0    00:00:00 ps
```

Here:

| Column | Meaning       |
| ------ | ------------- |
| PID    | Process ID    |
| TTY    | Terminal      |
| TIME   | CPU time used |
| CMD    | Command       |

---

# 4. View All Processes

Run:

```bash
ps -ef
```

This displays detailed information about running processes.

Example:

```text
UID        PID    PPID   CMD
akash     1250      1    bash
akash     1400   1250    ps
```

Notice:

```text
PID = 1400
PPID = 1250
```

Therefore:

```text
bash(1250)
     |
     └── ps(1400)
```

---

# 5. See PID, PPID and Process State

Run:

```bash
ps -o pid,ppid,state,cmd
```

Example:

```text
PID    PPID   S   CMD
1250      1   S   bash
1450   1250   R   ps
```

---

# 6. Understanding Process States

The `state` column shows the current state of a process.

Common states include:

| Symbol | Meaning               |
| ------ | --------------------- |
| R      | Running               |
| S      | Sleeping              |
| D      | Uninterruptible Sleep |
| T      | Stopped               |
| Z      | Zombie                |

Example:

```text
Z
```

means:

> 🧟 Zombie Process

---

# 7. Process Tree Using `pstree`

One of the most interesting Linux commands is:

```bash
pstree
```

It displays processes in a tree structure.

Example:

```text
systemd
 ├── bash
 │    └── program
 │         └── program
 └── firefox
```

This helps us understand:

```text
Parent
   |
   └── Child
         |
         └── Child
```

---

# 8. Display PID in Process Tree

Run:

```bash
pstree -p
```

Example:

```text
bash(1250)───program(1400)───program(1401)
```

Now we can clearly see:

```text
bash
 |
 └── Parent Process
       |
       └── Child Process
```

---

# ⭐ 9. See Only Your Requested Process Tree

Sometimes:

```bash
pstree
```

shows the entire system.

That can be confusing.

Suppose your program has PID:

```text
12345
```

Run:

```bash
pstree -p 12345
```

Example:

```text
program(12345)───program(12346)
```

This shows only:

```text
Your Process
     |
     └── Child Process
```

---

# 10. Find Your Process PID

Suppose your program is named:

```text
zombie
```

Run:

```bash
pgrep zombie
```

Example:

```text
12345
12346
```

These are the PIDs associated with the program.

---

# 11. Practical: Parent and Child Process Tree

Create a program:

```c
#include <stdio.h>
#include <unistd.h>

int main() {

    printf("Parent PID: %d\n", getpid());

    pid_t pid = fork();

    if (pid == 0) {

        printf("Child PID: %d\n", getpid());

        sleep(30);

    } else {

        sleep(30);
    }

    return 0;
}
```

Save as:

```text
process_tree.c
```

Compile:

```bash
gcc process_tree.c -o process_tree
```

Run:

```bash
./process_tree
```

Example output:

```text
Parent PID: 12345
Child PID: 12346
```

Open another terminal.

Run:

```bash
pstree -p 12345
```

Example:

```text
process_tree(12345)───process_tree(12346)
```

🎉 You are now watching your Parent and Child process relationship.

---

# ⭐ 12. See the Process Tree of Your Current Shell

A very useful command is:

```bash
pstree -p $$
```

The symbol:

```bash
$$
```

represents the PID of the current shell.

Example:

```text
bash(1200)
    |
    └── process_tree(12345)
             |
             └── process_tree(12346)
```

This is excellent for understanding:

```text
Terminal
   |
 Shell
   |
 Parent Process
   |
 Child Process
```

---

# 13. Run a Process in the Background

Normally, when you run:

```bash
./program
```

the terminal waits for the program.

To run it in the background:

```bash
./program &
```

Example:

```bash
sleep 100 &
```

Output:

```text
[1] 12345
```

Here:

```text
[1]      → Job Number
12345    → Process ID
```

---

# 14. Check Background Jobs

Run:

```bash
jobs
```

Example:

```text
[1]+ Running    sleep 100 &
```

---

# 15. Bring Background Process to Foreground

Use:

```bash
fg
```

If there are multiple jobs:

```bash
fg %1
```

Example:

```text
Job 1 → Foreground
```

---

# 16. Send a Process to the Background

Suppose a process is currently running.

Press:

```text
Ctrl + Z
```

This stops the process.

Then run:

```bash
bg
```

The process continues in the background.

---

# 17. Find a Process Using `pgrep`

Suppose you want to find:

```text
firefox
```

Run:

```bash
pgrep firefox
```

Example:

```text
2001
2005
2010
```

You can also display names:

```bash
pgrep -l firefox
```

Example:

```text
2001 firefox
2005 firefox
```

---

# 18. Monitor Processes Using `top`

Run:

```bash
top
```

This displays processes in real time.

You can observe:

* CPU usage
* Memory usage
* Running processes
* Process states

Press:

```text
q
```

to exit.

---

# 19. Using `htop`

If installed:

```bash
htop
```

This provides an easier visual interface.

Install it using:

```bash
sudo apt install htop
```

Then:

```bash
htop
```

You can:

* Search processes
* Kill processes
* View CPU usage
* View memory usage
* Monitor processes interactively

---

# 20. Kill a Process

Suppose a process has PID:

```text
12345
```

Run:

```bash
kill 12345
```

This sends a termination signal to the process.

---

# 21. Force Kill a Process

Sometimes a process does not terminate normally.

You can use:

```bash
kill -9 12345
```

Conceptually:

```text
Process
   |
 kill signal
   |
   v
Process Terminated
```

---

# 22. Practical: Observe a Running Process

Run:

```bash
sleep 100 &
```

Example:

```text
[1] 12345
```

Now investigate it.

Find the process:

```bash
ps -p 12345
```

See detailed information:

```bash
ps -o pid,ppid,state,cmd -p 12345
```

Example:

```text
PID    PPID   S   CMD
12345  1200   S   sleep 100
```

Now kill it:

```bash
kill 12345
```

Check jobs:

```bash
jobs
```

The process should no longer be running.

---

# 🧟 23. Practical: Investigating a Zombie Process

Create:

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {

    pid_t pid = fork();

    if (pid == 0) {

        printf("Child PID: %d\n", getpid());

        exit(0);

    } else {

        printf("Parent PID: %d\n", getpid());

        printf("Parent sleeping for 30 seconds...\n");

        sleep(30);
    }

    return 0;
}
```

Compile:

```bash
gcc zombie.c -o zombie
```

Run:

```bash
./zombie
```

Open another terminal.

Run:

```bash
ps -o pid,ppid,state,cmd
```

Look for:

```text
Z
```

Example concept:

```text
PID     PPID    STATE    CMD
12346   12345     Z      [zombie]
```

This is a real:

# 🧟 Zombie Process

---

# 👶 24. Practical: Investigating an Orphan Process

Create:

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {

    pid_t pid = fork();

    if (pid == 0) {

        sleep(5);

        printf("Child PID: %d\n", getpid());

        printf("New PPID: %d\n", getppid());

    } else {

        printf("Parent PID: %d\n", getpid());

        printf("Parent terminating...\n");

        exit(0);
    }

    return 0;
}
```

Compile:

```bash
gcc orphan.c -o orphan
```

Run:

```bash
./orphan
```

The parent terminates immediately.

The child continues running.

After a few seconds:

```text
Child PID: XXXX
New PPID: XXXX
```

This demonstrates that the child has been re-parented.

---

# 25. Zombie vs Orphan Investigation

## Zombie

```text
Parent = Alive
Child = Dead
```

```text
Parent
   |
   └── Zombie Child
```

The child has terminated.

Its status has not yet been collected.

---

## Orphan

```text
Parent = Dead
Child = Alive
```

```text
Parent
   X

Child continues
```

The operating system re-parents the child.

---

# 26. Process Detective Challenge 🕵️

Try the following.

## Challenge 1

Run:

```bash
sleep 60 &
```

Find:

* PID
* PPID
* Process State

Use:

```bash
ps -o pid,ppid,state,cmd
```

---

## Challenge 2

Display your shell process tree:

```bash
pstree -p $$
```

---

## Challenge 3

Create a Parent and Child using `fork()`.

Find both processes.

Display only their process tree.

Use:

```bash
pstree -p PID
```

---

## Challenge 4

Create a Zombie Process.

Find the process state:

```text
Z
```

---

## Challenge 5

Create an Orphan Process.

Observe:

```c
getppid()
```

before and after the parent terminates.

---

# 27. Most Useful Commands — Cheat Sheet

| Command                    | Purpose                      |
| -------------------------- | ---------------------------- |
| `ps`                       | Show processes               |
| `ps -ef`                   | Show detailed processes      |
| `ps -o pid,ppid,state,cmd` | Show PID, PPID and state     |
| `pstree`                   | Show process tree            |
| `pstree -p`                | Show tree with PID           |
| `pstree -p PID`            | Show a specific process tree |
| `pgrep name`               | Find process by name         |
| `pgrep -l name`            | Find PID and name            |
| `top`                      | Monitor processes            |
| `htop`                     | Interactive process monitor  |
| `jobs`                     | Show background jobs         |
| `fg`                       | Bring job to foreground      |
| `bg`                       | Continue job in background   |
| `kill PID`                 | Terminate process            |
| `kill -9 PID`              | Force terminate process      |
| `$$`                       | PID of current shell         |

---

# 28. Final Process Detective Diagram

```text
                    🖥️ LINUX SYSTEM
                           |
                           v
                        SHELL
                           |
                           |
                      ./program
                           |
                         fork()
                           |
                 -------------------
                 |                 |
                 v                 v

              Parent             Child
                 |                 |
                 |               exec()
                 |                 |
               wait()           Program
                 |
                 |
             ps / pstree
                 |
                 v

        🔍 PROCESS DETECTIVE
```

---

# 🎯 Final Learning Outcome

After completing this practical lab, students should be able to:

* Understand real processes running inside Linux.
* Identify PID and PPID.
* Find processes using `ps` and `pgrep`.
* Visualize Parent and Child relationships.
* Use `pstree` to investigate process trees.
* Display only a specific process tree.
* Monitor processes using `top`.
* Run programs in the background.
* Terminate processes using `kill`.
* Observe Zombie Processes.
* Understand Orphan Processes practically.

---

# 🔥 Final Message

Operating Systems become much more interesting when you stop asking:

> **"What is a process?"**

and start asking:

> **"Where is my process right now, who created it, what is its state, and what happens when I kill it?"** 🕵️💻

That is the beginning of becoming an **Operating System Process Detective**. 🔍🚀

---

# End of Notes

**Topic:** Process Creation, `fork()`, `exec()`, `wait()`, Process Termination, Zombie Process and Orphan Process
**Subject:** Operating Systems
