# Lec 27: Free Space Management & Recovery

**Subject:** Operating Systems\
**Lecture Duration:** 50 minutes\
**Audience:** Undergraduate students

------------------------------------------------------------------------

## Learning Outcomes

By the end of this lecture, you should be able to:

-   Explain why an operating system must track free disk blocks.
-   Describe the bitmap, linked-list, grouping, and counting methods of
    free-space management.
-   Explain the basic idea of file-system recovery.
-   Differentiate logging, checkpointing, and backup-based recovery.

## 1. Why Is Free-Space Management Needed?

A file system stores files and directories in disk blocks. Some blocks
are occupied by file data or file-system metadata, while others are
available for future use.

**Free-space management** is the file system's method of tracking which
disk blocks are available and allocating them when a file grows or a new
file is created.

### Disk-block overview

``` mermaid
flowchart LR
    A["Disk blocks"] --> B["Allocated blocks"]
    A --> C["Free blocks"]
    B --> D["Contain file data or metadata"]
    C --> E["Available for new data"]
    style B fill:#f8caca,stroke:#b42318,color:#222
    style C fill:#c9f7d5,stroke:#16803c,color:#222
```

**Example:** Suppose a disk has 1,000 blocks and 720 are in use. The
file system must identify the remaining 280 free blocks so that it can
allocate them safely.

### What must the file system do?

1.  Find a suitable free block when storage is requested.
2.  Mark the block as allocated.
3.  Return blocks to the free-space structure when files are deleted.
4.  Keep the information consistent, even after a crash or power
    failure.

------------------------------------------------------------------------

## 2. Methods of Free-Space Management

The common methods are:

1.  Bit map (bit vector)
2.  Linked list
3.  Grouping
4.  Counting

### 2.1 Bit Map / Bit Vector

A bitmap uses one bit for each disk block. The bit value represents
whether the block is free or allocated. The exact convention can vary by
file system; in this example, **0 = free** and **1 = allocated**.

``` text
Disk block:  0  1  2  3  4  5  6  7
Bitmap bit:  1  1  0  0  1  0  1  0
             |  |  |  |  |  |  |  |
Status:      A  A  F  F  A  F  A  F

A = Allocated     F = Free
```

**How it works**

-   To allocate a block, the file system searches for a free bit and
    changes it to the allocated value.
-   To free a block, it changes that block's bit back to the free value.
-   Consecutive free bits can help locate contiguous blocks.

**Advantages** - Compact: one bit per block. - Easy to check whether a
particular block is free. - Can help find consecutive free blocks.

**Disadvantages** - A large disk needs a large bitmap. - Searching may
take time if the bitmap is large and not indexed or cached.

**Space calculation example**

If a file system has 1,000,000 blocks, it needs 1,000,000 bits:

\[
`\frac{1,000,000}{8}`{=tex}=125,000`\text{ bytes}`{=tex}`\approx122.1`{=tex}`\text{ KiB}`{=tex}
\]

This is the bitmap size only; actual implementations may add structures
or keep cached copies.

### 2.2 Linked List

In a linked-list approach, free blocks are linked together. The file
system keeps a pointer to the first free block, and each free block
stores a pointer to the next one (or the links are stored in associated
metadata).

``` mermaid
flowchart LR
    H["Free-list head"] --> B7["Block 7"]
    B7 --> B12["Block 12"]
    B12 --> B19["Block 19"]
    B19 --> B24["Block 24"]
    B24 --> N["End"]
    style H fill:#dbeafe,stroke:#2563eb,color:#222
    style N fill:#eee,stroke:#666,color:#222
```

**How it works** - To allocate a block, the file system removes a block
from the free list. - To free a block, it adds the block to the list.

**Advantages** - Simple to implement. - Does not require a separate bit
for every block.

**Disadvantages** - Traversing the list to find many free blocks can be
slow. - Links and disk reads add overhead. - Finding a large run of
contiguous blocks is difficult.

### 2.3 Grouping

Grouping improves on the linked-list idea. One free block contains the
addresses of several other free blocks. The final entry points to
another block that contains the next group of addresses.

``` mermaid
flowchart LR
    A["Group block A<br/>B3, B8, B14, B20"] --> B["Group block B<br/>B27, B31, B40, ..."]
    B --> C["Next group"]
    style A fill:#dbeafe,stroke:#2563eb,color:#222
    style B fill:#dbeafe,stroke:#2563eb,color:#222
```

**Advantages** - Several free-block addresses can be obtained at once. -
Usually more efficient than following one link per free block.

**Disadvantages** - The file system must manage and update the groups
correctly. - If a group block is damaged, information about multiple
free blocks may be affected.

### 2.4 Counting

Free blocks often appear in consecutive runs. Instead of storing every
block address separately, the file system stores the **starting block
address** and the **number of consecutive free blocks**.

    Starting block   Count Meaning
  ---------------- ------- ------------------------
                10       5 Blocks 10--14 are free
                28       3 Blocks 28--30 are free
                50       8 Blocks 50--57 are free

``` text
Stored entry: (start = 10, count = 5)

10  11  12  13  14
 F   F   F   F   F
```

**Advantages** - Efficient when free blocks occur in long consecutive
runs. - Stores fewer entries than listing every free block separately.

**Disadvantages** - Less effective when the disk is highly fragmented
into many short runs. - The file system must update counts when blocks
are allocated or freed.

### Method comparison

  -----------------------------------------------------------------------
  Method            Main idea         Main advantage    Main limitation
  ----------------- ----------------- ----------------- -----------------
  Bitmap            One bit per block Compact; supports Bitmap scanning
                                      searching for     can be costly
                                      runs              

  Linked list       Link free blocks  Simple structure  Slow to search
                                                        and find
                                                        contiguous runs

  Grouping          Store several     Gets many         Group metadata
                    free-block        addresses at once must be
                    addresses                           maintained
                    together                            

  Counting          Store start       Excellent for     Less effective
                    address and run   consecutive free  with heavy
                    length            blocks            fragmentation
  -----------------------------------------------------------------------

**Remember:** These are general techniques. Real file systems may
combine methods or use more advanced allocation structures.

------------------------------------------------------------------------

## 3. What Is File-System Recovery?

File-system recovery is the process of restoring a file system to a
consistent state after an unexpected event such as a power failure,
operating-system crash, or storage error.

A crash can occur while the system is updating file data or metadata.
For example, a file may be created, but the system may stop before all
directory and allocation information is safely written.

### A simple crash scenario

``` mermaid
flowchart TD
    A["User creates a file"] --> B["File data is written"]
    B --> C["Directory / allocation metadata is updated"]
    C --> D["Changes are safely committed"]
    B --> X["Crash before all updates finish"]
    X --> Y["Possible inconsistent file system"]
    D --> Z["Consistent state"]
    style X fill:#f8caca,stroke:#b42318,color:#222
    style Y fill:#ffe7c2,stroke:#b45309,color:#222
    style Z fill:#c9f7d5,stroke:#16803c,color:#222
```

Possible problems include: - A block is marked as allocated but is not
referenced by any file. - A file references a block that is incorrectly
marked as free. - Directory or file metadata contains incomplete
updates. - Recently written data may be lost if it was not safely
stored.

Recovery techniques aim to reduce these problems and restore
consistency. They cannot always recover user data that was never written
or that has been physically damaged.

------------------------------------------------------------------------

## 4. Common File-System Recovery Techniques

### 4.1 Consistency Checking

A checking utility examines file-system metadata and searches for
inconsistencies. It may compare allocation information with the blocks
referenced by files and directories.

``` text
File-system metadata
        |
        v
Check directory and file references
        |
        v
Compare with block-allocation information
        |
        v
Find inconsistencies
        |
        v
Repair when possible
```

For example, a Linux system may use `fsck` for supported file systems.
The correct procedure depends on the file system and whether it is
mounted; repairs should not be run carelessly on a mounted file system.

### 4.2 Journaling / Logging

A journaling file system records intended metadata changes in a log
(journal) before or as those changes are committed to their normal
locations. After a crash, the system can inspect the journal and replay
or discard incomplete transactions as appropriate.

``` mermaid
flowchart TD
    A["Prepare metadata change"] --> B["Record change in journal"]
    B --> C["Commit journal transaction"]
    C --> D["Apply changes to main file system"]
    D --> E["Mark transaction complete"]
    X["Crash / restart"] -.-> R["Inspect journal and recover"]
    B -.-> X
    C -.-> X
    D -.-> X
    style B fill:#dbeafe,stroke:#2563eb,color:#222
    style R fill:#c9f7d5,stroke:#16803c,color:#222
```

**Benefits** - Usually speeds up recovery after a crash. - Helps
preserve metadata consistency. - Avoids checking every block in many
ordinary crash-recovery cases.

**Important:** Journaling does not automatically guarantee that every
recent user-data write survives. Protection depends on the file system's
journaling mode and configuration.

### 4.3 Checkpointing

A checkpoint records a known recovery point or ensures that a set of
logged changes has been safely applied. It can limit how much log
history must be processed during recovery.

**Analogy:** Like saving progress in a game. If the system stops
unexpectedly, it can restart from a recent safe point rather than
rebuilding everything from the beginning.

### 4.4 Backup and Restore

A backup is a separate copy of important data. If data is deleted,
corrupted, or a device fails, it may be restored from the backup.

**Advantages** - Can recover user files after deletion, corruption, or
device failure. - Protects against some events that journaling cannot,
such as physical disk failure.

**Limitations** - A backup may not contain the latest changes. - Backups
need storage, scheduling, and periodic restore tests. - A backup on the
same failed device may be lost too.

**Best practice:** Keep backups separate from the original device and
periodically verify that they can actually be restored.

### Recovery techniques compared

  -----------------------------------------------------------------------
  Technique               Main purpose            Typical use
  ----------------------- ----------------------- -----------------------
  Consistency checking    Detect and repair       After an unclean
                          metadata                shutdown or suspected
                          inconsistencies         corruption

  Journaling              Record changes to       Routine crash recovery
                          support crash recovery  in journaling file
                                                  systems

  Checkpointing           Establish a recovery    Reduce recovery work
                          point / limit log       
                          replay                  

  Backup and restore      Recover copies of data  Deletion, corruption,
                                                  or device failure
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 5. Free-Space Management and Recovery: How They Relate

Free-space management tracks which blocks are available. Recovery helps
ensure that the allocation information and other file-system metadata
remain consistent after a crash.

**Example:** A program deletes a file, and the system crashes while
updating the directory and free-space information. Recovery mechanisms
may use a journal or consistency checking to determine whether the
updates were completed and to repair inconsistent metadata.

``` mermaid
flowchart TD
    A["File created / changed / deleted"] --> B["Update file data and metadata"]
    B --> C["Update free-space information"]
    C --> D["Commit changes safely"]
    D --> E["Consistent file system"]
    B -. "Crash during update" .-> F["Recovery process"]
    C -.-> F
    F --> G["Replay log or check consistency"]
    G --> E
    style E fill:#c9f7d5,stroke:#16803c,color:#222
    style F fill:#ffe7c2,stroke:#b45309,color:#222
```

------------------------------------------------------------------------

## 6. Quick Revision

-   **Free-space management:** Tracks disk blocks that are available for
    allocation.
-   **Bitmap:** One bit represents each block.
-   **Linked list:** Free blocks are linked together.
-   **Grouping:** A free block stores addresses of multiple free blocks.
-   **Counting:** Stores the start address and length of a consecutive
    free run.
-   **Recovery:** Restores file-system consistency after a crash or
    error.
-   **Journaling:** Records changes so they can be recovered after a
    crash.
-   **Checkpointing:** Provides a recovery point and can reduce log
    replay work.
-   **Backup:** Keeps separate copies for restoring lost or damaged
    data.

## 7. Practice Questions

### Short-answer questions

1.  What is free-space management, and why is it necessary?
2.  Explain bitmap-based free-space management with an example.
3.  Differentiate linked-list and grouping methods.
4.  What information is stored by the counting method?
5.  What is file-system recovery?
6.  What is journaling, and how does it help after a crash?
7.  Differentiate journaling and backup.

### Application questions

1.  A file system has 8,000 blocks. How many bytes are needed for a
    bitmap if one bit represents one block? Ignore additional overhead.
2.  A counting entry is `(start = 120, count = 12)`. Which blocks are
    represented?
3.  Why might a bitmap be preferred over a linked list when the file
    system needs to find several consecutive free blocks?
4.  A power failure occurs during a file deletion. Explain how
    free-space information could become inconsistent and name one
    recovery technique that may help.

### Answers for self-check

1.  (8000/8 = 1000) bytes.
2.  Blocks 120 through 131.
3.  A bitmap makes it easier to inspect neighboring blocks and identify
    runs of free blocks.
4.  The directory entry and free-space metadata might not both be
    updated. Journaling or a consistency-checking tool may help restore
    consistency, depending on the file system and the failure.

------------------------------------------------------------------------

## Key Takeaway

**Free-space management answers "Which disk blocks are available?"
Recovery answers "How can the file system return to a consistent state
after an interruption?"** Both are important for reliable storage.
