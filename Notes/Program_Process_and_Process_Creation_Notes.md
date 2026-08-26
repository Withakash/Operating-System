# Operating System Notes: Program, Process & Process Creation

## Lecture 11: Program, Process & Process States

### 1. Program vs Process

#### Program
A **program** is a passive set of instructions stored on secondary storage (disk).

Examples:
- `chrome`
- `java`
- `python`
- `a.out`

A program is **not executing** by itself.

#### Process
When a program is loaded into memory and starts executing, it becomes a **process**.

```text
Program on Disk
      ↓
User Executes It
      ↓
Loaded into RAM
      ↓
Process Created
      ↓
CPU Executes Instructions
```

### Simple Example

Compile a C program:

```bash
gcc hello.c -o hello
```

The file `hello` is a **program**.

Run it:

```bash
./hello
```

Now it becomes a **process**.

### Program vs Process

| Program | Process |
|---|---|
| Passive | Active |
| Stored on disk | Exists in memory |
| Static | Dynamic |
| No execution state | Has execution state |
| One program can create many processes | Each process has its own PID |

---

## 2. Process Components

A process is generally divided into different memory sections.

```text
+----------------------+
|        Stack         |
| Local variables      |
| Function calls       |
+----------------------+
|                      |
|      Free Space      |
|                      |
+----------------------+
|        Heap          |
| Dynamic memory       |
+----------------------+
|        Data          |
| Global/Static vars   |
+----------------------+
|        Code          |
| Program instructions |
+----------------------+
```

### Code / Text Segment
Contains the executable instructions of the program.

### Data Segment
Stores:
- Global variables
- Static variables

Example:

```c
int count = 10;
static int x = 20;
```

### Heap
Used for dynamic memory allocation.

C example:

```c
int *arr = malloc(10 * sizeof(int));
```

Java example:

```java
Student s = new Student();
```

### Stack
Used for:
- Local variables
- Function or method calls
- Parameters
- Return addresses

Example:

```c
void test() {
    int x = 10;
}
```

---

## 3. Process Control Block (PCB)

The Operating System maintains information about every process using a data structure called the **Process Control Block (PCB)**.

```text
+---------------------------+
|   PROCESS CONTROL BLOCK   |
+---------------------------+
| PID                       |
| Process State             |
| Program Counter           |
| CPU Registers             |
| Scheduling Information    |
| Memory Information        |
| Open Files                |
| Parent Process ID         |
+---------------------------+
```

### Important Terms

#### PID
**Process ID** — a unique identifier for a process.

Example:

```text
PID = 2456
```

#### PPID
**Parent Process ID** — identifies the process that created the current process.

```text
Parent Process
PID = 100
     |
     └── Child Process
         PID = 250
         PPID = 100
```

---

## 4. Process States

A process moves through different states during its lifetime.

```text
             +-------+
             |  New  |
             +---+---+
                 |
                 v
             +---+---+
             | Ready |
             +---+---+
                 |
                 | CPU Scheduler
                 v
             +---+---+
             |Running|
             +---+---+
              /      \
             /        \
     I/O Request      Process Finished
           /             \
          v               v
    +-----+-----+      +------+
    |   Waiting |      | Term |
    +-----+-----+      +------+
          |
          | I/O Complete
          v
        Ready
```

### States

**New**  
The process is being created.

**Ready**  
The process is ready to execute but is waiting for CPU allocation.

**Running**  
The process is currently executing on the CPU.

**Waiting / Blocked**  
The process is waiting for an event or I/O operation.

Example:

```text
Process requests keyboard input
        ↓
Moves to Waiting
        ↓
Input is received
        ↓
Moves back to Ready
```

**Terminated**  
The process has completed execution.

---

## 5. Real Process Observation

### `ps`

Shows process information.

```bash
ps
```

Detailed process information:

```bash
ps -ef
```

Important columns:

```text
UID     PID     PPID     CMD
```

### `top`

Run:

```bash
top
```

It displays:
- PID
- CPU usage
- Memory usage
- Running processes
- System load

Useful keys:

```text
q → Quit
```

---

# Lecture 12: Process Creation & Operations

## 1. Process Creation Concept

A process can create another process.

The original process is called the **Parent Process**.

The newly created process is called the **Child Process**.

```text
Parent Process
       |
       v
Child Process
```

In Linux/Unix systems, process creation is commonly performed using:

```c
fork()
```

---

## 2. `fork()` System Call

The `fork()` system call creates a new process.

Before `fork()`:

```text
Process A
PID = 100
```

After `fork()`:

```text
             fork()
               |
       +-------+-------+
       |               |
       v               v
 Parent Process     Child Process
 PID = 100          PID = 101
```

Both processes continue execution from the statement after `fork()`.

### Simple Example

```c
#include <stdio.h>
#include <unistd.h>

int main() {
    fork();

    printf("Hello\n");

    return 0;
}
```

Possible output:

```text
Hello
Hello
```

This happens because after `fork()` there are two processes, and both execute `printf()`.

### Return Value of `fork()`

```c
pid_t pid = fork();
```

| Return Value | Meaning |
|---|---|
| `0` | Code is executing in the child process |
| Positive value | Code is executing in the parent; value is child's PID |
| `-1` | Process creation failed |

### Parent and Child Example

```c
#include <stdio.h>
#include <unistd.h>

int main() {

    pid_t pid = fork();

    if (pid == 0) {
        printf("I am the Child Process\n");
    } else if (pid > 0) {
        printf("I am the Parent Process\n");
    } else {
        printf("Fork failed\n");
    }

    return 0;
}
```

---

## 3. `exec()` Family

The `fork()` system call creates a new process.

The `exec()` family is used to **replace the current process program with another program**.

```text
Parent
   |
   | fork()
   v
Child
   |
   | exec()
   v
Runs Another Program
```

### Example: Shell Concept

When a shell runs a command such as:

```bash
ls
```

Conceptually:

```text
Shell
  |
  | fork()
  v
Child Process
  |
  | exec()
  v
ls Program Executes
```

### Key Difference

> `fork()` creates a new process.

> `exec()` replaces the program currently running inside a process.

---

## 4. `wait()`

A parent process may need to wait until its child process completes.

```text
Parent
   |
   | fork()
   v
Child
   |
   | Doing Work
   v
Finished
   |
   v
Parent Continues
```

Example:

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {

    int pid = fork();

    if (pid == 0) {
        printf("Child is running\n");
    } else {
        wait(NULL);
        printf("Parent continues after child finishes\n");
    }

    return 0;
}
```

`wait(NULL)` makes the parent wait for a child process to terminate.

---

## 5. Process Termination

A process can terminate when:

- The program completes normally.
- `exit()` is called.
- An error occurs.
- The OS terminates the process.
- A termination signal is received.

```text
Running
   |
   | Program Finished
   v
Terminated
```

---

## 6. Zombie Process

A **zombie process** is a process that has completed execution, but its entry still remains in the process table because its parent has not yet collected its exit status.

```text
Child Process
     |
     | Finishes
     v
Exit
     |
     | Parent has not called wait()
     v
Zombie
```

A zombie is not executing. It only retains limited process information until the parent collects its status.

---

## 7. Orphan Process

An **orphan process** occurs when the parent process terminates before its child process.

```text
Parent Process
      |
      +---- Child Process
               |
Parent Terminates
      X        |
               v
          Still Running
```

The operating system reassigns the orphaned child to another system process so it can eventually be managed and reaped.

---

# Useful Linux Commands

## View Processes

```bash
ps
```

## Detailed Process List

```bash
ps -ef
```

## Interactive Process Monitor

```bash
top
```

## Display Process Tree

```bash
pstree
```

---

# Practical Learning Flow

## Lecture 11

```text
Program vs Process
       ↓
Run a Program
       ↓
Observe using ps
       ↓
Understand PID and PPID
       ↓
Process Memory Components
       ↓
PCB
       ↓
Process States
       ↓
Observe using top
```

## Lecture 12

```text
One Process
     ↓
fork()
     ↓
Parent + Child
     ↓
Observe PID and PPID
     ↓
exec()
     ↓
Run Another Program
     ↓
wait()
     ↓
Process Termination
     ↓
Zombie and Orphan
```

# Quick Revision

- **Program** → A passive set of instructions stored on disk.
- **Process** → A program currently executing.
- **PID** → Unique Process ID.
- **PPID** → Parent Process ID.
- **PCB** → OS data structure containing process information.
- **Ready** → Waiting for CPU.
- **Running** → Currently using CPU.
- **Waiting/Blocked** → Waiting for an event or I/O.
- **fork()** → Creates a child process.
- **exec()** → Replaces the current process program with another program.
- **wait()** → Parent waits for child termination.
- **Zombie** → Finished process whose exit status has not yet been collected.
- **Orphan** → Child process whose parent has terminated.
