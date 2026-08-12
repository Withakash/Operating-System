# Operating System — Processes: Practical Learning Notes

## Topics Covered

1. Program vs Process
2. Process Components — PCB, Code, Data, Stack, Heap
3. Process States & State Transition Diagram
4. Real Process Observation — `ps`, `top`
5. Process Creation Concept
6. `fork()` System Call
7. `exec()` Family
8. `wait()` & Process Termination
9. Zombie & Orphan Processes

---

# 1. Program vs Process

## 1.1 What is a Program?

A **program** is a passive set of instructions stored on secondary storage.

Example:

```c
#include <stdio.h>

int main() {
    printf("Hello OS\n");
    return 0;
}
```

When this C program is compiled, an executable file is created.

For example:

```bash
gcc hello.c -o hello
```

Now `hello` is a program/executable stored on disk.

A program itself is not executing.

---

## 1.2 What is a Process?

A **process** is a program that is currently being executed.

Think:

```text
Program
  |
  | Execution starts
  v
Process
```

If you run:

```bash
./hello
```

the operating system creates a process to execute the program.

### Important distinction

| Program | Process |
|---|---|
| Passive | Active |
| Stored on disk | Exists in memory while executing |
| Contains instructions | Executes instructions |
| No PID by itself | Has a PID |
| Static | Dynamic |
| Can exist without running | Represents a running/executing instance |

---

## 1.3 One Program Can Create Multiple Processes

Suppose we have:

```text
firefox
```

as one program.

If multiple instances are launched, the OS can have multiple processes.

```text
                 Program
              Firefox executable
                    |
          +---------+---------+
          |         |         |
          v         v         v
       Process A Process B Process C
       PID 1001   PID 1050   PID 1080
```

Each process has its own process-related resources and PID.

---

## 1.4 Practical Experiment

Create:

```bash
nano hello.c
```

Code:

```c
#include <stdio.h>
#include <unistd.h>

int main() {
    printf("Hello from process\n");
    sleep(20);
    return 0;
}
```

Compile:

```bash
gcc hello.c -o hello
```

Run:

```bash
./hello
```

While it is sleeping, open another terminal:

```bash
ps
```

or:

```bash
ps aux | grep hello
```

You should see the running process.

This demonstrates:

```text
Executable file on disk
        |
        | ./hello
        v
Operating System
        |
        v
Running Process
        |
        v
PID assigned
```

---

# 2. Process Components

A process is more than just program code.

A simplified process memory layout is:

```text
High Address
+----------------------+
|        Stack         |
| Local variables      |
| Function calls       |
+----------------------+
|          ↓           |
|                      |
|        Free          |
|       Space          |
|                      |
|          ↑           |
+----------------------+
|         Heap         |
| malloc/new memory    |
+----------------------+
|         Data         |
| Global variables     |
| Static variables     |
+----------------------+
|         Code         |
| Program instructions |
+----------------------+
Low Address
```

A process also has operating-system management information stored in its **PCB**.

---

# 3. PCB — Process Control Block

The OS needs to remember information about every process.

This information is maintained in a structure called the:

> **Process Control Block (PCB)**

A simplified PCB contains:

```text
+--------------------------------+
|       Process Control Block    |
+--------------------------------+
| PID                            |
| Process State                  |
| Program Counter                |
| CPU Registers                 |
| CPU Scheduling Information     |
| Memory Management Information  |
| Accounting Information         |
| I/O Status Information         |
+--------------------------------+
```

## Important PCB fields

### PID

Process Identifier.

Example:

```text
PID = 4215
```

The OS uses PID to identify a process.

---

### Process State

Examples:

```text
New
Ready
Running
Waiting
Terminated
```

---

### Program Counter

Stores the address of the next instruction to execute.

---

### CPU Registers

The PCB may store the CPU register state when the process is switched out.

This becomes important during **context switching**.

---

### Scheduling Information

Can include information such as:

- Priority
- Scheduling queue information
- CPU scheduling parameters

---

### Memory Information

The OS needs information about the process's memory.

Examples:

- Page tables
- Memory mappings
- Address-space information

---

### I/O Information

The process may have:

- Open files
- Devices
- I/O status

---

# 4. Code/Text Segment

The **code segment** contains executable instructions.

Example:

```c
int add(int a, int b) {
    return a + b;
}
```

The machine instructions generated from this code belong to the process's code/text area.

Conceptually:

```text
Process
|
+-- Code
|     |
|     +-- main()
|     +-- add()
|     +-- other functions
|
+-- Data
+-- Heap
+-- Stack
```

The code segment is generally not modified during normal execution.

---

# 5. Data Segment

The data segment contains global and static variables.

Example:

```c
int count = 10;

static int total = 50;

int main() {
    ...
}
```

Here:

```text
count
total
```

are stored in process data areas.

---

## Initialized vs Uninitialized Data

Conceptually:

```text
Data Segment
|
+-- Initialized data
|     int x = 10;
|
+-- BSS
      int y;
```

The exact memory layout is implementation-dependent, but this distinction is useful for understanding process memory.

---

# 6. Stack

The stack is used for function calls and automatic/local variables.

Example:

```c
void test() {
    int x = 10;
    int y = 20;
}
```

When `test()` executes, its stack frame contains information associated with the call, including local variables.

Conceptually:

```text
Stack
+-------------------+
| test() frame      |
| x = 10            |
| y = 20            |
+-------------------+
| main() frame      |
+-------------------+
```

Function calls push frames onto the stack.

Returning from functions removes those frames.

---

# 7. Heap

The heap is used for dynamically allocated memory.

Example:

```c
int *p = malloc(sizeof(int));
*p = 100;
```

The dynamically allocated integer is stored in heap memory.

Example:

```text
Stack
+------------------+
| p                |
| points to ...    |
+------------------+
         |
         v
Heap
+------------------+
|       100        |
+------------------+
```

Remember:

```text
Stack → automatic/function-call memory
Heap  → dynamic memory allocation
```

---

# 8. Complete Process Structure

A useful conceptual model:

```text
                  PROCESS
                     |
       +-------------+-------------+
       |                           |
       v                           v
      PCB                     Process Memory
       |                           |
       |              +------------+------------+
       |              |            |            |
       v              v            v            v
 PID, state         Code         Data         Heap
 registers                                      |
 scheduling                                     |
 I/O info                                      |
                                                |
                                             Stack
```

---

# 9. Process States

A process does not continuously execute on the CPU.

It moves between different states.

The basic five-state model is:

```text
New
 |
 | admitted
 v
Ready <----------+
 |
 | dispatch
 v
Running
 |  \
 |   \
 |    \ I/O request
 |     \
 |      v
 |    Waiting
 |      |
 |      | I/O complete
 |      v
 +---- Ready
 |
 | exit
 v
Terminated
```

---

# 10. New State

When the OS is creating a process:

```text
New
```

The process has been created but has not yet entered normal execution.

---

# 11. Ready State

The process is ready to execute but is waiting for CPU time.

Example:

```text
CPU
 |
 +-- Process A → Running

Ready Queue:
 +-- Process B
 +-- Process C
 +-- Process D
```

B, C, and D are ready but cannot all execute on a single CPU core simultaneously.

---

# 12. Running State

A process is in the **Running** state when its instructions are currently executing on a CPU.

```text
CPU
 |
 v
+-----------+
| Process A |
| RUNNING   |
+-----------+
```

---

# 13. Waiting / Blocked State

A process may need to wait for an event.

For example:

```c
read(fd, buffer, size);
```

If the required data is not immediately available, the process may block.

Conceptually:

```text
Running
   |
   | I/O request
   v
Waiting
   |
   | I/O completed
   v
Ready
```

The process is not using the CPU while waiting.

---

# 14. Terminated State

After a process finishes:

```text
Running
   |
   | exit()
   v
Terminated
```

The OS performs process cleanup and releases resources.

There are special cases such as zombie processes, discussed later.

---

# 15. Process State Transition Diagram

```text
                         +-------+
                         |  New  |
                         +---+---+
                             |
                         admitted
                             |
                             v
                       +-----+------+
                 +---->|   Ready    |
                 |     +-----+------+
                 |           |
                 |       dispatch
                 |           |
                 |           v
                 |     +-----+------+
                 |     |  Running   |
                 |     +--+-----+---+
                 |        |     |
      I/O done   |        |     | exit
                 |        |     |
                 |        |     v
                 |        |  +--+--------+
                 |        |  | Terminated|
                 |        |  +-----------+
                 |        |
                 |    I/O request
                 |        |
                 |        v
                 |   +----+-----+
                 +---| Waiting  |
                     +----------+
```

Another important transition is:

```text
Running → Ready
```

This happens when the OS preempts the process, for example because its time slice expires.

---

# 16. Practical Process Observation

Linux provides commands for observing processes.

Important commands:

```bash
ps
ps aux
top
pstree
```

---

# 17. `ps` Command

`ps` means **process status**.

Run:

```bash
ps
```

Example:

```text
    PID TTY          TIME CMD
   4210 pts/0    00:00:00 bash
   4352 pts/0    00:00:00 ps
```

Important columns:

| Column | Meaning |
|---|---|
| PID | Process ID |
| TTY | Terminal |
| TIME | CPU time used |
| CMD | Command/process |

---

# 18. `ps aux`

Run:

```bash
ps aux
```

This gives a broader process listing.

Useful columns include:

```text
USER
PID
%CPU
%MEM
STAT
START
TIME
COMMAND
```

Example:

```text
USER      PID  %CPU %MEM STAT COMMAND
student  4210  0.0  0.1 S    bash
student  4402  2.0  0.3 R    ./program
```

---

# 19. Understanding Process State in `ps`

The `STAT` column can show state information.

Common examples:

```text
R = Running / runnable
S = Interruptible sleep
D = Uninterruptible sleep
T = Stopped
Z = Zombie
```

For teaching, the key observation is:

```text
R → currently runnable
S → sleeping/waiting
Z → zombie
```

---

# 20. `top`

Run:

```bash
top
```

It continuously displays process information.

You can observe:

```text
PID
USER
PR
NI
VIRT
RES
SHR
S
%CPU
%MEM
TIME+
COMMAND
```

Students should identify:

- Which process uses the most CPU?
- Which process uses the most memory?
- What is its PID?
- What is its state?
- What happens when a process starts/stops?

Exit:

```text
q
```

---

# 21. Practical Experiment — Observe a Process

Create:

```c
#include <stdio.h>
#include <unistd.h>

int main() {
    printf("PID = %d\n", getpid());

    while (1) {
        sleep(2);
    }

    return 0;
}
```

Compile:

```bash
gcc process.c -o process
```

Run:

```bash
./process
```

Output:

```text
PID = 5312
```

From another terminal:

```bash
ps -p 5312
```

Or:

```bash
top -p 5312
```

This connects the concepts:

```text
C program
   ↓
Executable
   ↓
OS creates process
   ↓
PID assigned
   ↓
Process appears in ps/top
```

Stop it:

```bash
kill 5312
```

---

# 22. Process Creation

A process can create another process.

The process that creates another process is commonly called the:

> Parent process

The newly created process is called the:

> Child process

Conceptually:

```text
Parent Process
      |
      | create
      v
Child Process
```

This creates a process hierarchy.

```text
init/systemd
      |
      +---- Process A
      |       |
      |       +---- Process C
      |
      +---- Process B
```

---

# 23. Process ID and Parent Process ID

Linux provides:

```c
getpid()
```

to get the current process ID.

And:

```c
getppid()
```

to get the parent process ID.

Example:

```c
#include <stdio.h>
#include <unistd.h>

int main() {
    printf("PID  = %d\n", getpid());
    printf("PPID = %d\n", getppid());

    return 0;
}
```

Compile:

```bash
gcc ids.c -o ids
```

Run:

```bash
./ids
```

Possible output:

```text
PID  = 5100
PPID = 4900
```

Meaning:

```text
Parent Process
PID = 4900
      |
      v
Child Process
PID = 5100
```

---

# 24. `fork()` System Call

`fork()` is one of the most important Linux process-creation system calls.

It creates a new process by duplicating the calling process.

Conceptually:

```text
Before fork()

Parent
  |
  v
Running


After fork()

        Parent
       /      \
      /        \
 Parent        Child
```

---

# 25. Basic `fork()` Program

```c
#include <stdio.h>
#include <unistd.h>

int main() {

    fork();

    printf("Hello\n");

    return 0;
}
```

Compile:

```bash
gcc fork1.c -o fork1
```

Run:

```bash
./fork1
```

Possible output:

```text
Hello
Hello
```

Why two times?

Because after successful `fork()` there are two processes, and both continue executing from the point after `fork()`.

```text
Before fork:
       Parent
          |
        fork()
          |
          +-----------+
          |           |
          v           v
       Parent       Child
          |           |
          +-----+-----+
                |
             printf()
```

---

# 26. Return Value of `fork()`

This is extremely important.

```c
pid_t pid = fork();
```

`fork()` returns:

```text
< 0  → fork failed
= 0  → executing in child
> 0  → executing in parent; value is child's PID
```

Example:

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main() {

    pid_t pid = fork();

    if (pid < 0) {
        printf("fork failed\n");
    }
    else if (pid == 0) {
        printf("Child: PID = %d\n", getpid());
    }
    else {
        printf("Parent: PID = %d, Child PID = %d\n",
               getpid(), pid);
    }

    return 0;
}
```

Possible output:

```text
Parent: PID = 5000, Child PID = 5001
Child: PID = 5001
```

Notice:

```text
Parent gets child's PID
Child gets 0
```

---

# 27. Important `fork()` Mental Model

Students often think:

> `fork()` creates a child and then the child starts from `main()`.

That is not the right mental model.

The child continues from the point where `fork()` returned.

Example:

```c
printf("A\n");

fork();

printf("B\n");

fork();

printf("C\n");
```

Reason it out:

```text
Start:
1 process

After first fork:
2 processes

After second fork:
4 processes
```

Therefore `C` can be printed four times.

---

# 28. Practical Fork Experiment

```c
#include <stdio.h>
#include <unistd.h>

int main() {

    printf("Before fork\n");

    fork();

    printf("After fork\n");

    return 0;
}
```

Expected:

```text
Before fork
After fork
After fork
```

`Before fork` runs once.

`After fork` runs twice.

---

# 29. Fork with PID Observation

```c
#include <stdio.h>
#include <unistd.h>

int main() {

    printf("Before fork: PID=%d\n", getpid());

    pid_t pid = fork();

    if (pid == 0) {
        printf("Child: PID=%d PPID=%d\n",
               getpid(), getppid());
    }
    else {
        printf("Parent: PID=%d ChildPID=%d\n",
               getpid(), pid);
    }

    return 0;
}
```

This experiment demonstrates:

- Parent PID
- Child PID
- Parent-child relationship
- `fork()` return value

---

# 30. Does Parent and Child Share Everything?

After `fork()`, the child gets a logically separate process address space.

Conceptually:

```text
Parent Address Space
+----------------+
| Code           |
| Data           |
| Heap           |
| Stack          |
+----------------+

Child Address Space
+----------------+
| Code           |
| Data           |
| Heap           |
| Stack          |
+----------------+
```

Modern operating systems commonly optimize this using **Copy-on-Write (COW)**.

The pages do not necessarily get physically copied immediately.

They can initially be shared until one process modifies them.

---

# 31. Copy-on-Write — Basic Idea

```text
Before modification:

Parent ----+
           |
           v
       Physical Page
           ^
           |
Child -----+


Parent modifies page:

Parent ---> New/Private Page

Child ----> Original Page
```

This makes `fork()` more efficient.

---

# 32. `exec()` Family

`fork()` creates a new process.

`exec()` does something different.

The `exec` family replaces the current process's program image with another program.

Important:

> `exec()` does not normally create a new process.

Think:

```text
Process A
   |
   | exec()
   v
Same Process
New Program
```

The PID generally remains the same.

---

# 33. Why `fork()` + `exec()`?

A common Unix pattern is:

```text
Parent
  |
  | fork()
  v
Child
  |
  | exec()
  v
Run another program
```

This is extremely important.

For example, a shell can:

```text
User enters command
       |
       v
Shell
       |
     fork()
       |
       v
Child
       |
     exec()
       |
       v
Command
```

---

# 34. Simple `execl()` Example

```c
#include <stdio.h>
#include <unistd.h>

int main() {

    printf("Before exec\n");

    execl("/bin/ls", "ls", "-l", NULL);

    printf("After exec\n");

    return 0;
}
```

Run it.

You will see:

```text
Before exec
<ls output>
```

Normally you will **not** see:

```text
After exec
```

Why?

Because successful `exec()` replaces the current program.

The old program no longer continues.

---

# 35. `exec()` Does Not Create a New PID

Consider:

```c
printf("PID = %d\n", getpid());

execl("/bin/ls", "ls", NULL);
```

The `ls` program runs with the same process identity/PID.

Conceptually:

```text
PID 5000

Before exec:
myprogram

After exec:
ls

PID still = 5000
```

---

# 36. Common `exec` Functions

The exec family includes functions such as:

```text
execl()
execv()
execlp()
execvp()
execle()
execve()
```

The main differences involve:

- Argument format
- Whether a path is supplied
- Whether environment variables are supplied

For beginner practical work, focus first on:

```text
execl()
execvp()
```

---

# 37. `execl()` vs `execvp()`

### `execl()`

You provide the executable path explicitly.

```c
execl("/bin/ls", "ls", "-l", NULL);
```

### `execvp()`

You provide the command name and let the system search the `PATH`.

```c
char *args[] = {"ls", "-l", NULL};

execvp("ls", args);
```

---

# 38. Practical `fork()` + `exec()` Example

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main() {

    pid_t pid = fork();

    if (pid < 0) {
        perror("fork");
        return 1;
    }

    if (pid == 0) {

        printf("Child executing ls...\n");

        execlp("ls", "ls", "-l", NULL);

        perror("exec failed");
    }

    else {
        printf("Parent continues...\n");
    }

    return 0;
}
```

The child changes from your C program into the `ls` program.

---

# 39. `wait()` System Call

Suppose:

```text
Parent
   |
   +---- Child
```

If the child finishes before the parent has collected its termination status, the parent should normally call `wait()`/related APIs to reap the child.

Basic example:

```c
wait(NULL);
```

The parent waits for a child to terminate.

---

# 40. Why `wait()` Is Important

Without coordination:

```text
Parent
   |
   | continues
   |
   +---- Child
          |
          | finishes
          v
       Terminated
```

With `wait()`:

```text
Parent
   |
 wait()
   |
   | waits
   v
Child finishes
   |
   v
Parent continues
```

---

# 41. Practical `wait()` Program

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {

    pid_t pid = fork();

    if (pid == 0) {

        printf("Child running...\n");
        sleep(3);
        printf("Child finished\n");

    } else {

        printf("Parent waiting...\n");

        wait(NULL);

        printf("Parent continues after child finishes\n");
    }

    return 0;
}
```

Expected sequence:

```text
Parent waiting...
Child running...
Child finished
Parent continues after child finishes
```

The exact ordering of the first two lines can vary due to scheduling.

---

# 42. `wait()` Return Value

A basic form:

```c
pid_t child_pid = wait(NULL);
```

It returns the PID of the child that was collected.

For more detailed termination information:

```c
int status;

pid_t pid = wait(&status);
```

You can inspect the status using macros such as:

```c
WIFEXITED(status)
WEXITSTATUS(status)
```

Example:

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {

    pid_t pid = fork();

    if (pid == 0) {
        return 42;
    }

    int status;
    wait(&status);

    if (WIFEXITED(status)) {
        printf("Child exit status = %d\n",
               WEXITSTATUS(status));
    }

    return 0;
}
```

Output:

```text
Child exit status = 42
```

---

# 43. Process Termination

A process can terminate because:

1. It completes normally.
2. It calls `exit()`.
3. It returns from `main()`.
4. It receives a terminating signal.
5. It encounters certain fatal errors.

Example:

```c
#include <stdlib.h>

int main() {

    printf("Program ending\n");

    exit(0);
}
```

`0` conventionally indicates successful termination.

---

# 44. `return 0` vs `exit(0)`

Inside `main()`:

```c
return 0;
```

and:

```c
exit(0);
```

both result in normal process termination, although they have different language/runtime semantics.

For introductory OS practicals, remember:

```text
return from main
       ↓
normal process termination

exit()
       ↓
explicit process termination
```

---

# 45. Parent Waiting for a Specific Child

If a parent has multiple children:

```text
Parent
 |
 +---- Child A
 |
 +---- Child B
 |
 +---- Child C
```

The parent can use:

```c
waitpid()
```

to wait for a particular child.

Example:

```c
waitpid(child_pid, NULL, 0);
```

This is useful when process order matters.

---

# 46. Practical Parent-Child Synchronization

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {

    pid_t pid = fork();

    if (pid == 0) {

        printf("Child: doing work...\n");
        sleep(2);
        printf("Child: work complete\n");

    } else {

        waitpid(pid, NULL, 0);

        printf("Parent: child completed\n");
    }

    return 0;
}
```

This is a simple example of process synchronization.

---

# 47. Zombie Process

A **zombie process** is a child process that has terminated, but whose parent has not yet collected its termination status.

Conceptually:

```text
Child
  |
  | exit()
  v
Terminated
  |
  | parent has not wait()ed
  v
Zombie
```

The zombie is no longer executing.

It mainly remains as a process-table entry containing termination information until it is reaped.

---

# 48. Practical Zombie Example

Create:

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main() {

    pid_t pid = fork();

    if (pid == 0) {

        printf("Child exiting...\n");
        exit(0);

    } else {

        printf("Parent sleeping...\n");
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

Immediately open another terminal.

Find the process:

```bash
ps aux | grep zombie
```

You may see a state containing:

```text
Z
```

or a command indicating:

```text
<defunct>
```

The child has finished, but the parent has not called `wait()`.

---

# 49. Why Does Zombie Exist?

The OS needs to preserve some information about the terminated child so the parent can retrieve its termination status.

For example:

```text
Child exits with status 42

       |
       v

OS keeps termination information

       |
       v

Parent calls wait()

       |
       v

Information collected
Zombie removed
```

---

# 50. Fixing the Zombie

Modify the parent:

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/wait.h>

int main() {

    pid_t pid = fork();

    if (pid == 0) {

        printf("Child exiting...\n");
        exit(42);

    } else {

        sleep(5);

        wait(NULL);

        printf("Child reaped\n");
    }

    return 0;
}
```

The important operation is:

```c
wait(NULL);
```

The parent collects the child's termination status.

---

# 51. Orphan Process

An **orphan process** is a process whose original parent terminates before the child.

Conceptually:

```text
Parent
  |
  +---- Child
          |
          | Parent terminates
          v
       Orphan
```

The child continues running.

On Linux, an orphan is adopted/reparented to another suitable process, traditionally the `init` process; on modern systems this is commonly `systemd` as PID 1, or another configured subreaper where applicable.

---

# 52. Practical Orphan Example

```c
#include <stdio.h>
#include <unistd.h>

int main() {

    pid_t pid = fork();

    if (pid == 0) {

        printf("Child started\n");

        sleep(5);

        printf("Child PID  = %d\n", getpid());
        printf("New PPID   = %d\n", getppid());

    } else {

        printf("Parent exiting...\n");
        return 0;
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

The parent exits quickly.

The child continues sleeping.

After the parent terminates, the child's parent relationship can change.

You can observe it using:

```bash
ps -o pid,ppid,stat,cmd
```

---

# 53. Zombie vs Orphan

This distinction is extremely important.

| Zombie | Orphan |
|---|---|
| Child has already terminated | Child is still running |
| Parent still exists | Original parent has terminated |
| Parent has not collected child status | Child is reparented |
| Appears as `Z` / `<defunct>` | Usually continues normally |
| `wait()`/reaping removes zombie | Reparenting handles orphan relationship |

Remember:

```text
Zombie:
Child DEAD + Parent ALIVE + not reaped

Orphan:
Child ALIVE + Original Parent DEAD
```

---

# 54. Complete Practical Process Lifecycle

Combine everything:

```text
                 Program
                    |
                    | fork()
                    v
              +-----------+
              |   Child   |
              +-----------+
                    |
                  exec()
                    |
                    v
             Another Program
                    |
              +-----+-----+
              |           |
           Running       I/O
              |           |
              |        Waiting
              |           |
              +-----------+
                    |
                  exit()
                    |
                    v
               Terminated
                    |
            Parent wait()
                    |
                    v
                 Reaped
```

---

# 55. Shell: The Best Real-World Example

When you execute:

```bash
ls -l
```

a shell such as Bash commonly participates in a process-creation/exec workflow.

Conceptually:

```text
User
 |
 | types: ls -l
 v
Shell
 |
 | fork()
 v
Child Process
 |
 | exec()
 v
ls
 |
 | finishes
 v
exit
 |
 v
Parent shell
 |
 | wait/reap
 v
Prompt appears
```

This explains why:

```text
fork()
exec()
wait()
```

are fundamental Unix process concepts.

---

# 56. Observe the Shell's Process Tree

Run:

```bash
pstree -p
```

You may see a hierarchy similar to:

```text
systemd(1)
  └─ terminal
      └─ bash(4200)
          └─ pstree(4500)
```

The exact tree depends on the Linux distribution and desktop/terminal environment.

---

# 57. Practical Experiment — Build a Mini Shell Concept

A simplified program:

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {

    pid_t pid = fork();

    if (pid == 0) {

        execlp("ls", "ls", "-l", NULL);

        perror("exec");
        return 1;

    } else {

        wait(NULL);

        printf("Command finished.\n");
    }

    return 0;
}
```

This demonstrates the core idea behind command execution:

```text
fork → exec → wait
```

---

# 58. Practical Experiment Set

## Experiment 1 — Program vs Process

Create a program that sleeps:

```c
#include <unistd.h>

int main() {
    sleep(30);
    return 0;
}
```

Run it and inspect it using:

```bash
ps
ps aux
top
```

Goal:

```text
Executable → Process → PID
```

---

## Experiment 2 — PID and PPID

Use:

```c
getpid();
getppid();
```

Observe the process hierarchy.

---

## Experiment 3 — Basic `fork()`

Write:

```c
fork();
printf("Hello\n");
```

Predict the output before running.

---

## Experiment 4 — `fork()` Return Value

Use:

```c
pid_t pid = fork();
```

Print different messages for:

```text
pid < 0
pid == 0
pid > 0
```

---

## Experiment 5 — Multiple Forks

Predict:

```c
fork();
fork();
fork();

printf("Hello\n");
```

How many processes?

Answer:

```text
2^3 = 8
```

assuming every fork succeeds and no process exits before reaching later forks.

---

## Experiment 6 — `exec()`

Run:

```c
execlp("ls", "ls", "-l", NULL);
```

Observe that the old program does not continue after successful `exec()`.

---

## Experiment 7 — `fork()` + `exec()`

Create a child and execute:

```bash
ls
```

from the child.

---

## Experiment 8 — `wait()`

Make the child sleep:

```c
sleep(5);
```

and make the parent call:

```c
wait(NULL);
```

Observe the ordering.

---

## Experiment 9 — Zombie

Child:

```c
exit(0);
```

Parent:

```c
sleep(30);
```

Observe:

```bash
ps aux
```

Look for:

```text
Z
<defunct>
```

---

## Experiment 10 — Orphan

Make the parent terminate immediately and make the child sleep.

Observe the child's PPID before and after parent termination.

---

# 59. Debugging and Observation Commands

Useful commands for process practicals:

### List current processes

```bash
ps
```

### List all processes

```bash
ps aux
```

### Observe continuously

```bash
top
```

### Process hierarchy

```bash
pstree -p
```

### Find a process

```bash
pgrep process_name
```

### Inspect one process

```bash
ps -p PID -f
```

### Send termination signal

```bash
kill PID
```

### See process status

```bash
ps -o pid,ppid,stat,cmd -p PID
```

---

# 60. Important Safety Note for `kill`

Do not randomly run:

```bash
kill -9
```

on system processes.

First understand:

```text
PID
Process
Parent process
Signal
```

For your own test programs, normal termination is usually preferable:

```bash
kill PID
```

Use stronger signals only when necessary and when you understand the effect.

---

# 61. Common Student Confusions

## Confusion 1

**Is a program a process?**

No.

```text
Program = passive executable
Process = executing instance
```

---

## Confusion 2

**Does `fork()` run the program twice from the beginning?**

No.

Both parent and child continue from the point where `fork()` returns.

---

## Confusion 3

**Does `exec()` create a new process?**

Normally, no.

It replaces the current process image.

---

## Confusion 4

**Does the child get the same PID as the parent?**

No.

The child gets a different PID.

---

## Confusion 5

**What does `fork()` return?**

```text
Parent → child's PID
Child  → 0
Failure → -1
```

---

## Confusion 6

**Is a zombie using CPU?**

No.

A zombie is not executing.

It is a terminated process whose status has not yet been collected.

---

## Confusion 7

**Is an orphan dead?**

No.

An orphan is still running.

---

# 62. One Big Mental Model

Remember this entire chapter using:

```text
PROGRAM
   |
   | OS starts it
   v
PROCESS
   |
   +-----------------------+
   |                       |
   v                       v
PCB                    Memory
   |                 /    |    \
   |              Code   Data  Heap/Stack
   |
   +--> PID
   +--> State
   +--> Registers
   +--> Scheduling info
   +--> I/O info

PROCESS STATES
   |
   +--> New
   +--> Ready
   +--> Running
   +--> Waiting
   +--> Terminated

PROCESS CREATION
   |
   +--> fork()
          |
          +--> Parent
          |
          +--> Child
                 |
                 +--> exec()
                        |
                        v
                   New Program

PROCESS COMPLETION
   |
   +--> exit()
   |
   +--> Parent wait()
   |
   +--> Child reaped

SPECIAL CASES
   |
   +--> Zombie = terminated + not reaped
   |
   +--> Orphan = running + original parent terminated
```

---

# 63. Final Practical Challenge

Create a program that behaves like a tiny command runner.

Requirements:

1. Parent creates a child using `fork()`.
2. Child prints its PID and PPID.
3. Child uses `execvp()` to run:

```bash
ls -l
```

4. Parent waits using `waitpid()`.
5. Parent prints the child's exit status.
6. Run the program while observing it with:

```bash
ps
top
pstree -p
```

Expected conceptual flow:

```text
              Your Program
                   |
                 fork()
              /         \
             /           \
        Parent            Child
          |                 |
          |               execvp()
          |                 |
        waitpid()        ls -l
          |                 |
          |               exit
          |                 |
          +------<----------+
                 |
             reap child
                 |
          print status
```

This single experiment connects almost every concept in this chapter.

---

# 64. Quick Revision Table

| Concept | Key Idea | Practical API/Command |
|---|---|---|
| Program | Passive executable | `./program` |
| Process | Running instance | `ps` |
| PID | Process identifier | `getpid()` |
| PPID | Parent process ID | `getppid()` |
| PCB | OS process information | Conceptual |
| Code | Instructions | Program text |
| Data | Global/static data | Global variables |
| Heap | Dynamic memory | `malloc()` |
| Stack | Function-call memory | Local variables |
| New | Process being created | OS concept |
| Ready | Waiting for CPU | `ps`/scheduler |
| Running | Executing on CPU | `ps`, `top` |
| Waiting | Waiting for event/I/O | `ps` state |
| Terminated | Process finished | `exit()` |
| Process creation | Create child | `fork()` |
| Program replacement | Replace process image | `exec*()` |
| Parent synchronization | Wait for child | `wait()`, `waitpid()` |
| Normal termination | End process | `return`, `exit()` |
| Zombie | Terminated but unreaped child | `ps` → `Z` |
| Orphan | Child whose original parent ended | `getppid()`, `ps` |
| Process hierarchy | Parent-child tree | `pstree -p` |

---

# 65. Questions Students Should Be Able to Answer

After completing the practicals, students should be able to explain:

1. What is the difference between a program and a process?
2. Why does every process need a PID?
3. What information is stored in a PCB?
4. What are code, data, heap, and stack?
5. What happens when a process moves from Ready to Running?
6. Why does a process enter the Waiting state?
7. How can we observe processes in Linux?
8. What does `fork()` do?
9. What are the three important `fork()` return cases?
10. Why can one `printf()` execute twice after `fork()`?
11. What does `exec()` do?
12. Does `exec()` create a new PID?
13. Why are `fork()` and `exec()` commonly used together?
14. Why does a parent use `wait()`?
15. What is the difference between `wait()` and `waitpid()`?
16. How does a process terminate?
17. What is a zombie process?
18. Why does a zombie exist?
19. What is an orphan process?
20. What is the difference between zombie and orphan?
21. How can you identify a zombie using `ps`?
22. How can you observe parent-child relationships?
23. What happens to an orphan on modern Linux systems?
24. Why is `fork()` + `exec()` important for shells?
25. Can you build a small program that creates, executes, waits for, and observes a child process?

---

# 66. Core Takeaway

The most important practical sequence to remember is:

```text
                 PROGRAM
                    |
                    v
                 PROCESS
                    |
                  fork()
                 /      \
                /        \
          PARENT          CHILD
             |              |
             |            exec()
             |              |
             |         New Program
             |              |
             |            exit()
             |              |
             +----------> wait()
                            |
                           reap
```

If students can **write, compile, run, observe, and explain** this sequence, they have a strong practical foundation for process management in Linux.
