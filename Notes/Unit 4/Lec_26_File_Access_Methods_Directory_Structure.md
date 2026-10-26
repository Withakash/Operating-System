# Lecture 26: File Access Methods & Directory Structure
## Operating Systems — Short Student Notes

**Lecture duration:** 50 minutes

## 1. Directory Structure

A **directory** is used by an operating system to organize files and help users find them.

### Types of Directory Structure

#### A. Single-Level Directory
All files are stored in one directory.

```text
Root
├── file1.txt
├── notes.pdf
└── photo.jpg
```

**Advantages:** Simple to understand and use.  
**Disadvantages:** File-name conflicts can occur; difficult to manage many files or multiple users.

#### B. Two-Level Directory
Each user has a separate directory under a main directory.

```text
Master Directory
├── User A
│   ├── notes.txt
│   └── photo.jpg
└── User B
    ├── notes.txt
    └── project.doc
```

**Advantages:** Different users can have files with the same name; better separation.  
**Disadvantages:** Sharing files between users can be less convenient.

#### C. Tree-Structured Directory
Directories can contain files and subdirectories, creating a hierarchy.

```text
Root
├── home
│   ├── akash
│   │   ├── notes.txt
│   │   └── projects
│   │       └── app.java
└── system
    └── config.txt
```

**Advantages:** Easy organization, supports subfolders, and makes large collections easier to manage.  
**Disadvantages:** Users must understand paths to locate files.

### Quick Comparison

| Type | Structure | Main Point |
|---|---|---|
| Single-level | One directory for all files | Simplest, but name conflicts are possible |
| Two-level | One directory per user | Separates users' files |
| Tree-structured | Directories inside directories | Flexible hierarchical organization |

---

## 2. File

A **file** is a named collection of related information stored on a storage device.

Examples: `notes.txt`, `photo.jpg`, `program.java`, `marks.xlsx`.

### Common File Attributes

- **Name:** Human-readable file name.
- **Type:** Kind of file, often suggested by its extension.
- **Size:** Amount of data stored.
- **Location:** Where the file is stored.
- **Protection/permissions:** Who can read, write, or execute it.
- **Timestamps:** Information such as creation or modification time, depending on the file system.

### Common File Operations

- **Create:** Make a new file.
- **Open:** Prepare a file for access.
- **Read:** Get data from the file.
- **Write:** Store or change data.
- **Rename:** Change the file's name.
- **Close:** Finish using an open file.
- **Delete:** Remove the file's directory entry and release storage as appropriate.

### File Access Methods

A file access method describes how a program reads or writes data in a file.

1. **Sequential Access:** Data is processed in order, from beginning to end.  
   *Example:* Reading a text file line by line.

2. **Direct (Random) Access:** A program can move to a specific location or record without reading everything before it.  
   *Example:* Accessing a particular record in a database file.

3. **Indexed Access:** An index helps locate a record quickly.  
   *Example:* Searching a large file using an index of record keys.

---

## 3. File vs Directory

| File | Directory |
|---|---|
| Stores data or program content. | Organizes and helps locate files and other directories. |
| Example: `report.pdf` | Example: `Documents/` |
| May contain text, images, code, or other data. | Contains entries referring to files and subdirectories. |
| Common operations include read and write. | Common operations include list, create, rename, and remove entries. |

**Easy example:** A file is like a document; a directory is like a folder used to organize documents.

---

## 4. Quick Revision

- A **directory** organizes files.
- **Single-level:** All files in one directory.
- **Two-level:** Each user has a separate directory.
- **Tree-structured:** Directories can contain subdirectories.
- A **file** stores related data under a name.
- **Sequential access:** Read in order.
- **Direct access:** Jump to a required location.
- **Indexed access:** Use an index to find records.
- A **file stores content**; a **directory organizes entries**.

---

## 5. Practice Questions

1. What is a directory?
2. Explain single-level, two-level, and tree-structured directories.
3. Define a file and list any four file attributes.
4. What are the three common file access methods?
5. Differentiate between a file and a directory.
6. Give one advantage and one disadvantage of a single-level directory.

### Important 5-Mark Questions

1. Explain the types of directory structures with diagrams.
2. Explain sequential, direct, and indexed file access methods with examples.
3. Differentiate between a file and a directory.

---

**End of Lecture 26**
