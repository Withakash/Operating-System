# Lec 30: Domain of Protection
## Operating Systems — Student Notes

## 1. Need for OS Protection

### What is OS Protection?

**OS Protection** is a mechanism that controls how users and programs access system resources such as files, memory, CPU, and devices.

It ensures that a user or program cannot access resources without proper permission.

### Why is Protection Needed?

- **Prevent unauthorized access** to files and data.
- **Protect memory** so one process cannot improperly access another process's memory.
- **Control resource usage** such as CPU, files, and devices.
- **Maintain system stability** by preventing programs from interfering with each other.
- **Improve security** by restricting access to sensitive information.

### Real-Life Example

Imagine a college computer system:

- Students can view their marks.
- Teachers can enter or update marks.
- Only authorized administrators can manage student accounts.

These permissions protect the system from unauthorized changes.

---

## 2. Protection vs Security

| Protection | Security |
|---|---|
| Controls how resources are accessed. | Protects the system against threats and attacks. |
| Defines permissions for users and processes. | Includes protection, authentication, monitoring, and other defenses. |
| Example: Only a teacher can edit marks. | Example: Preventing an attacker from stealing teacher credentials. |

**Remember:** Protection is one important part of overall system security.

---

## 3. Domain of Protection

A **protection domain** defines the set of resources a process or user can access and the operations it can perform on them.

A domain specifies:

- **Objects:** Resources such as files, memory, printers, and devices.
- **Access rights:** Operations allowed on those objects, such as read, write, or execute.

### Example

| Object | Allowed Rights |
|---|---|
| Student marks file | Read |
| Assignment file | Read, Write |
| System settings | No Access |

A process operating in this domain can only perform the permitted operations.

### Simple Diagram

```text
        Protection Domain
       +-------------------+
       | Marks File: Read  |
       | Assignment: R/W   |
       | Settings: No Access|
       +-------------------+
                 |
                 v
       Process follows these
          access rights
```

**R = Read, W = Write**

### Access Matrix

An **access matrix** represents which subjects can perform which operations on which objects.

- Rows represent users or processes.
- Columns represent objects.
- Each cell contains the allowed access rights.

Example:

| Subject | Marks File | Assignment | Printer |
|---|---|---|---|
| Student | Read | Read, Write | Use |
| Teacher | Read, Write | Read, Write | Use |
| Guest | No Access | Read | No Access |

This matrix helps the OS decide whether an operation is allowed.

---

## 4. Real-World Security Incidents

Security incidents show what can happen when access controls, software, or security procedures fail.

### Incident 1: WannaCry Ransomware (2017)

**What happened?**

WannaCry spread across many computer systems and encrypted files, demanding ransom payments. It exploited a Windows vulnerability.

**Impact:**
- Organizations lost access to important files.
- Hospitals and businesses experienced disruption.
- Systems required recovery and security updates.

**Lesson:** Keep systems patched, maintain backups, and use layered security.

### Incident 2: Equifax Data Breach (2017)

**What happened?**

Attackers exploited a known vulnerability in a web application used by Equifax, a credit-reporting company.

**Impact:**
- Sensitive personal information of many people was exposed.
- The incident caused major financial and reputational damage.

**Lesson:** Apply security patches promptly, monitor systems, and protect sensitive data.

### Incident 3: Colonial Pipeline Ransomware Attack (2021)

**What happened?**

A ransomware attack affected Colonial Pipeline, a major fuel pipeline operator in the United States.

**Impact:**
- Pipeline operations were temporarily shut down.
- Fuel supply and distribution were disrupted.
- The incident demonstrated how cyberattacks can affect real-world services.

**Lesson:** Use strong authentication, separate critical systems, maintain incident-response plans, and prepare reliable backups.

---

## 5. How OS Protection Helps Prevent Incidents

Operating systems provide several protection mechanisms:

### 1. Authentication
Verifies the identity of a user, for example, through a password or multi-factor authentication.

### 2. Authorization
Determines what an authenticated user is allowed to do.

### 3. File Permissions
Controls who can read, write, or execute a file.

### 4. Memory Protection
Prevents one process from improperly accessing another process's memory.

### 5. Least Privilege
Users and programs receive only the permissions necessary to perform their tasks.

### 6. Auditing and Logging
Records system activities to help detect suspicious actions and investigate incidents.

---

## 6. Quick Revision

- **OS Protection:** Controls access to system resources.
- **Protection Domain:** Defines accessible objects and permitted operations.
- **Access Right:** An allowed operation, such as read or write.
- **Access Matrix:** Shows permissions for subjects and objects.
- **Authentication:** Verifies who the user is.
- **Authorization:** Determines what the user can do.
- **Least Privilege:** Gives only the minimum required permissions.
- **Security Incident:** An event that compromises or threatens a system's confidentiality, integrity, or availability.

## 7. Practice Questions

1. What is OS protection, and why is it needed?
2. Differentiate protection and security.
3. Define a protection domain with an example.
4. What is an access matrix?
5. Differentiate authentication and authorization.
6. What is the principle of least privilege?
7. Explain the WannaCry ransomware incident.
8. What lessons can organizations learn from real-world security incidents?

## 8. University Exam Questions — 5 Marks

1. Explain the need for protection in an operating system.
2. Define a protection domain and explain it with a diagram.
3. Explain the access matrix with a suitable example.
4. Discuss any two real-world security incidents and their lessons.
5. Explain how authentication, authorization, and least privilege help protect an operating system.

---
**End of Lec 30 Notes**
