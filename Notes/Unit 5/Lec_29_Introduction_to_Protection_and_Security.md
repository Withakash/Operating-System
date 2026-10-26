# Lec 29: Introduction to Protection & Security

**Subject:** Operating Systems\
**Lecture Duration:** 50 minutes\
**Audience:** Undergraduate students

------------------------------------------------------------------------

## Learning Outcomes

By the end of this lecture, you should be able to:

-   Explain the meaning of protection and security in an operating
    system.
-   Differentiate protection from security with examples.
-   Describe the three main security goals: Confidentiality, Integrity,
    and Availability (CIA).
-   Identify basic threats and connect them to the CIA Triad.

## 1. Introduction: Why Do We Need Protection and Security?

An operating system manages important resources such as files, memory,
processes, devices, and user accounts. Not every user or program should
be allowed to access every resource.

For example: - A student should not be able to edit another student's
private files. - A normal application should not be able to change
protected operating-system files without permission. - An attacker
should not be able to steal passwords or make an important service
unavailable.

The OS uses **protection mechanisms** to control access to resources,
while **security measures** help defend the system against threats and
unauthorized activity.

``` mermaid
flowchart TD
    A["Operating System Resources"] --> B["Files and Folders"]
    A --> C["Memory and Processes"]
    A --> D["Devices and Services"]
    B --> E["Protection: Who may access them?"]
    C --> E
    D --> E
    E --> F["Security: How do we defend the system against threats?"]
    style E fill:#dbeafe,stroke:#2563eb,color:#222
    style F fill:#d1fae5,stroke:#047857,color:#222
```

------------------------------------------------------------------------

## 2. Protection vs Security

### 2.1 What Is Protection?

**Protection** is the set of operating-system mechanisms that control
how users and processes access system resources.

It answers:

> **Who is allowed to do what with which resource?**

Examples: - Giving a user read-only access to a file. - Allowing only an
administrator to install system-wide software. - Preventing one process
from reading or overwriting another process's memory. - Using file
permissions such as read (`r`), write (`w`), and execute (`x`) in Linux.

### 2.2 What Is Security?

**Security** is the broader effort to protect a system and its
information from unauthorized access, misuse, modification, disruption,
or destruction.

It answers:

> **How do we protect the system and its data from threats?**

Examples: - Using strong passwords and multi-factor authentication. -
Installing security updates to fix vulnerabilities. - Encrypting
sensitive information. - Detecting malware and suspicious activity. -
Keeping backups to help recover data.

### 2.3 Comparison Table

  -----------------------------------------------------------------------
  Basis                   Protection              Security
  ----------------------- ----------------------- -----------------------
  Main focus              Controlled access to    Defending systems and
                          resources               data against threats

  Main question           Who can access which    How can the system
                          resource, and how?      resist, detect, and
                                                  recover from attacks or
                                                  misuse?

  Scope                   Often focused on        Broader; includes
                          access-control          protection,
                          mechanisms inside a     authentication,
                          system                  encryption, monitoring,
                                                  updates, and recovery

  Example                 A file is readable only Preventing an attacker
                          by its owner            from stealing the
                                                  owner's password

  OS example              File permissions and    Access controls plus
                          process isolation       patching, encryption,
                                                  and threat detection
  -----------------------------------------------------------------------

**Important:** Protection and security are closely related, not
competing concepts. Protection is one important part of an overall
security strategy. Different textbooks may use the terms with slightly
different scope.

### Real-life analogy: College building

Imagine a college building:

-   **Protection:** A staff-only room can be entered only by authorized
    staff using an access card.
-   **Security:** The college also uses guards, CCTV, visitor checks,
    alarms, and emergency procedures to protect people and property.

The access card is one protection mechanism. The full set of safeguards
contributes to security.

------------------------------------------------------------------------

## 3. Access Control: A Simple Protection Example

Suppose a Linux file named `marks.txt` contains exam marks. The owner
wants to read and edit it, while other users should not be able to
access it.

``` text
          User / Process
                |
                v
       Request to open file
                |
                v
      Check identity and permissions
                |
          +-----+-----+
          |           |
      Allowed       Denied
          |           |
          v           v
    Access file    Reject request
```

Linux permission example:

``` bash
ls -l marks.txt
```

Possible output:

``` text
-rw------- 1 akash staff 2048 Oct 26 09:00 marks.txt
```

The first ten characters show the file type and permissions. Here, the
owner has read and write permissions, while group members and others
have no permissions.

**Note:** Actual access also depends on ownership, directory
permissions, ACLs, privileges, and other system rules. A permission
string is a useful starting point, not the entire security model.

------------------------------------------------------------------------

## 4. Security Goals: The CIA Triad

The **CIA Triad** is a fundamental model used to describe three
important goals of information security:

-   **C --- Confidentiality**
-   **I --- Integrity**
-   **A --- Availability**

``` mermaid
flowchart TD
    C["Confidentiality<br/>Keep information private"]
    I["Integrity<br/>Keep information accurate"]
    A["Availability<br/>Keep systems accessible"]
    C --- I
    I --- A
    A --- C
    style C fill:#dbeafe,stroke:#2563eb,color:#222
    style I fill:#d1fae5,stroke:#047857,color:#222
    style A fill:#fef3c7,stroke:#b45309,color:#222
```

Think of an online banking application. Its security depends on keeping
account information private, ensuring balances and transactions are
correct, and making the service available when customers need it.

### 4.1 Confidentiality

**Definition:** Confidentiality means that information is accessible
only to authorized people, users, or processes.

It protects information from unauthorized disclosure.

**Examples** - Only authorized staff can view student marks. - Passwords
and private messages are not exposed to unauthorized users. - A bank
customer cannot view another customer's account details.

**Threats** - Stolen passwords. - Unauthorized file access. - Data
leaks. - Eavesdropping on unprotected communications.

**Common safeguards** - Authentication and access control. - File
permissions. - Encryption. - Careful handling of sensitive data.

**Memory hint:** Confidentiality = **Keep it secret.**

### 4.2 Integrity

**Definition:** Integrity means maintaining the accuracy and
completeness of information and preventing unauthorized or improper
changes.

It protects information from unauthorized modification or destruction.

**Examples** - A student's marks should not be changed by an
unauthorized user. - A bank balance should reflect valid transactions. -
A downloaded software package should not be altered by an attacker
without detection.

**Threats** - Unauthorized modification of files. - Malware changing
data. - Accidental deletion or editing. - Tampering with messages or
transactions.

**Common safeguards** - Access permissions and separation of duties. -
Checksums or cryptographic hashes to detect changes. - Digital
signatures for authenticity and integrity checks. - Audit logs and
backups.

A hash can help detect whether data has changed, but a plain hash alone
does not prevent tampering if an attacker can replace both the file and
its hash.

**Memory hint:** Integrity = **Keep it correct and unchanged unless an
authorized change is made.**

### 4.3 Availability

**Definition:** Availability means that authorized users can access
information, systems, and services when needed.

**Examples** - Students can access the college portal during
registration. - An ATM or banking service is operational when customers
need it. - A server continues to provide its service despite ordinary
faults.

**Threats** - Denial-of-service (DoS) attacks. - Hardware failures. -
Power outages. - Storage exhaustion. - Ransomware or other incidents
that make files unavailable.

**Common safeguards** - Backups and tested recovery plans. - Redundant
systems and failover. - Monitoring and maintenance. - Capacity planning
and protection against denial-of-service attacks.

**Memory hint:** Availability = **Keep it accessible.**

------------------------------------------------------------------------

## 5. CIA Triad: Comparison Table

  -----------------------------------------------------------------------
  Goal              Protects against  Example of a      Possible
                                      violation         safeguard
  ----------------- ----------------- ----------------- -----------------
  Confidentiality   Unauthorized      Someone reads     Permissions and
                    disclosure        private exam      encryption
                                      results           

  Integrity         Unauthorized or   Someone alters    Access control,
                    improper changes  marks without     audit logs,
                                      permission        digital
                                                        signatures

  Availability      Loss of access or The college       Redundancy,
                    service           portal goes       backups, recovery
                                      offline during    planning
                                      registration      
  -----------------------------------------------------------------------

### One scenario, three different failures

Suppose an attacker targets a college result portal:

``` mermaid
flowchart TD
    A["Attack on result portal"] --> B["Reads private student results"]
    A --> C["Changes marks without permission"]
    A --> D["Makes portal unavailable"]
    B --> E["Confidentiality failure"]
    C --> F["Integrity failure"]
    D --> G["Availability failure"]
    style E fill:#dbeafe,stroke:#2563eb,color:#222
    style F fill:#fef3c7,stroke:#b45309,color:#222
    style G fill:#f8caca,stroke:#b42318,color:#222
```

One incident can affect more than one goal. For example, ransomware may
make files unavailable and also expose confidential data if the attacker
steals it.

------------------------------------------------------------------------

## 6. Applying the CIA Triad to Operating Systems

The OS contributes to security by managing users, processes, files,
memory, and devices.

  -----------------------------------------------------------------------
  OS feature                          Security contribution
  ----------------------------------- -----------------------------------
  User accounts and authentication    Helps identify who is requesting
                                      access

  File permissions and ACLs           Helps preserve confidentiality and
                                      integrity

  Process isolation                   Helps prevent one process from
                                      interfering with another

  Memory protection                   Restricts unauthorized access to
                                      memory regions

  System updates                      Fix known vulnerabilities

  Logging and monitoring              Helps detect and investigate
                                      suspicious activity

  Backup and recovery tools           Help restore availability and
                                      recover data
  -----------------------------------------------------------------------

No single feature guarantees security. Effective security combines
suitable controls, correct configuration, updates, monitoring, and
recovery planning.

------------------------------------------------------------------------

## 7. Quick Revision

-   **Protection:** Controls access to system resources.
-   **Security:** Protects systems and information against threats and
    misuse.
-   **Confidentiality:** Prevents unauthorized disclosure.
-   **Integrity:** Prevents unauthorized or improper changes and helps
    maintain accuracy.
-   **Availability:** Ensures authorized users can access services when
    required.
-   **CIA Triad:** Confidentiality + Integrity + Availability.

### Easy way to remember

For a student result portal:

-   **C:** Other students cannot see your private results.
-   **I:** Nobody can change your marks without authorization.
-   **A:** You can access the portal when results are announced.

------------------------------------------------------------------------

## 8. Practice Questions

### Short-answer questions

1.  Define protection in an operating system.
2.  Define security and give two examples of security measures.
3.  Differentiate protection and security.
4.  What is the CIA Triad?
5.  Explain confidentiality with an example.
6.  Explain integrity with an example.
7.  Explain availability with an example.

### Application questions

1.  A user reads another student's private file without permission.
    Which CIA goal is violated?
2.  An unauthorized person changes a student's marks. Which CIA goal is
    violated?
3.  A denial-of-service attack prevents students from opening the
    college portal. Which CIA goal is affected?
4.  Explain how Linux file permissions can support protection.
5.  Give one example where a single incident affects two or more CIA
    goals.

### Self-check answers

1.  Confidentiality.
2.  Integrity.
3.  Availability.
4.  File permissions restrict which users can read, write, or execute a
    file.
5.  Example: ransomware encrypts files, making them unavailable; if data
    was also stolen, confidentiality is affected too.

------------------------------------------------------------------------

## Key Takeaway

**Protection controls access to resources. Security is the broader
effort to defend systems and data. The CIA Triad describes three
essential security goals: keep information private (Confidentiality),
correct (Integrity), and accessible (Availability).**
