# SAGE University, Indore

## IAC-Core, Department of CSE

# MID SEMESTER TEST -- 1

## PRACTICE QUESTION PAPER -- SET B

**Subject:** Operating System\
**Unit:** Unit 1 -- Operating System Fundamentals\
**Difficulty:** Moderate\
**Maximum Marks:** 20\
**Duration:** 2 Hours

------------------------------------------------------------------------

## SECTION -- I

### OBJECTIVE TYPE QUESTIONS

**5 × 1 = 5 Marks**

### Q.1. Answer all the following questions.

**(a) MCQ:**\
Which type of Operating System is primarily designed to provide
**predictable response within a specified time constraint**?

i)  Batch OS\
ii) Real-Time OS\
iii) Network OS\
iv) Distributed OS

**(b) Fill in the blank:**\
The privileged part of the Operating System that directly manages
hardware resources is called the \_\_\_\_\_\_\_\_\_\_.

**(c) True / False:**\
A process in the **Waiting/Blocked** state is normally waiting for an
event such as I/O completion.

**(d) MCQ:**\
Which of the following is **NOT normally private to each thread** within
the same process?

i)  Stack\
ii) Program Counter\
iii) Registers\
iv) Heap

**(e) Match the following:**

  -----------------------------------------------------------------------
  Column A                            Column B
  ----------------------------------- -----------------------------------
  1\. Internal Fragmentation          A. Scattered free spaces

  2\. External Fragmentation          B. Unused space inside an allocated
                                      block

  3\. Compaction                      C. Combines scattered free spaces
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## SECTION -- II

### SHORT ANSWER QUESTIONS

**Attempt ANY 4**\
**4 × 2.5 = 10 Marks**

### Q.2.

Differentiate between **Multiprogramming and Multitasking Operating
Systems**. Explain how both improve CPU utilization or user experience.

### Q.3.

Explain the difference between **User Space and Kernel Space**. Why
should a normal application not directly access kernel memory?

### Q.4.

A process is currently executing and requests a disk operation.

**(a)** Which state will the process enter while waiting for the disk?\
**(b)** What happens to the CPU during this time?\
**(c)** Which OS component decides which ready process should execute
next?

### Q.5.

Differentiate between a **Process and a Thread** with respect to:

1.  Memory/address space
2.  Creation overhead
3.  Resource sharing
4.  Execution

### Q.6.

Consider the following memory layout:

``` text
+----+------+----+------+----+------+
| P1 | FREE | P2 | FREE | P3 | FREE |
+----+------+----+------+----+------+
```

Explain:

**(a)** What type of fragmentation is represented?\
**(b)** Why may a large process fail to get memory even when enough
total free memory exists?\
**(c)** Which technique can help solve this problem?

------------------------------------------------------------------------

## SECTION -- III

### DESCRIPTIVE / DIAGRAM-BASED QUESTIONS

**Attempt ANY 1**\
**1 × 5 = 5 Marks**

> **IMPORTANT: Diagram is compulsory for every question.**

### Q.7.

**Explain the different types of Operating Systems** covered in Unit 1.

Your answer should include:

-   Batch Operating System
-   Multiprogramming Operating System
-   Multitasking Operating System
-   Time-Sharing Operating System
-   Real-Time Operating System
-   Distributed Operating System
-   Network Operating System
-   Embedded Operating System

For each type, explain its **main purpose and one suitable
application/example**.

**Draw a suitable classification/working diagram of Operating System
types.**

### Q.8.

**Explain the Process State Model in detail.**

Explain the following states:

-   New
-   Ready
-   Running
-   Waiting / Blocked
-   Terminated

Also explain the transitions between these states, including:

-   New → Ready
-   Ready → Running
-   Running → Ready
-   Running → Waiting
-   Waiting → Ready
-   Running → Terminated

**Draw a complete and properly labelled Process State Transition
Diagram.**

### Q.9.

**Explain the different Kernel/OS architecture approaches and their
characteristics.**

Discuss:

-   Monolithic Kernel
-   Layered Architecture
-   Microkernel
-   Modular Architecture
-   Hybrid Architecture

For each, explain the **basic organization, major advantage and
limitation**.

**Draw suitable diagrams for at least THREE kernel/OS architecture
approaches and clearly show the relationship between User Space, Kernel
and Hardware.**

------------------------------------------------------------------------

## Marks Summary

  Section           Questions     Attempt    Marks
  ----------------- ----------- --------- --------
  **Section I**     Q1                All        5
  **Section II**    Q2--Q6          Any 4       10
  **Section III**   Q7--Q9          Any 1        5
  **TOTAL**                                 **20**
