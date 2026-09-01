# Operating System – Lecture 3

## Boot Process | BIOS vs UEFI | Bootloader (GRUB) | Kernel Loading | systemd, Login Screen & Startup Services

> **Subject:** Operating System
> **Lecture:** 3
> **Duration:** 50 Minutes
> **Level:** Beginner → Intermediate
> **Approach:** Concept + Step-by-Step Boot Process + Linux Practical

---

# 1. Learning Objectives

After this lecture, students should be able to:

* Explain what happens after pressing the power button.
* Understand the complete boot process.
* Differentiate BIOS and UEFI.
* Explain the role of POST.
* Understand what a bootloader does.
* Understand GRUB in Linux.
* Explain how the Linux kernel is loaded.
* Understand the role of `initramfs`.
* Understand what `systemd` does.
* Understand how the login screen appears.
* Identify startup services in Linux.
* Observe boot information practically.

---

# 2. The Big Question

Ask students:

> **"When I press the power button, how does my computer go from completely OFF to a usable desktop?"**

For example:

```text
Press Power
     ↓
Hardware starts
     ↓
Firmware starts
     ↓
Hardware checked
     ↓
Boot device selected
     ↓
Bootloader starts
     ↓
Kernel loads
     ↓
systemd starts
     ↓
Services start
     ↓
Login Screen
     ↓
Desktop
```

This entire process is called the:

# Boot Process

---

# 3. What is Booting?

## Definition

**Booting** is the process of starting a computer and loading the operating system into memory so that the system becomes ready for use.

The word comes from **bootstrapping**.

The idea is:

> A small piece of software starts the process of loading a much larger operating system.

---

# 4. Power Off vs Running

When the computer is OFF:

```text
CPU        → Not executing the OS
RAM        → Normal working contents lost after power off
OS         → Not running
Applications → Not running
```

After pressing power:

```text
Power
  ↓
CPU starts execution
  ↓
Firmware executes
  ↓
Boot process begins
```

---

# 5. Complete Boot Process

A simplified Linux boot sequence:

```text
+-----------------------+
| 1. Power On          |
+-----------------------+
          ↓
+-----------------------+
| 2. POST / Firmware   |
|    BIOS or UEFI      |
+-----------------------+
          ↓
+-----------------------+
| 3. Find Boot Device  |
+-----------------------+
          ↓
+-----------------------+
| 4. Bootloader        |
|    GRUB              |
+-----------------------+
          ↓
+-----------------------+
| 5. Linux Kernel      |
+-----------------------+
          ↓
+-----------------------+
| 6. initramfs         |
+-----------------------+
          ↓
+-----------------------+
| 7. systemd / PID 1   |
+-----------------------+
          ↓
+-----------------------+
| 8. Services          |
+-----------------------+
          ↓
+-----------------------+
| 9. Login Manager     |
+-----------------------+
          ↓
+-----------------------+
| 10. Desktop / Shell  |
+-----------------------+
```

---

# 6. Step 1 – Pressing the Power Button

When you press the power button:

```text
Power Button
     ↓
Power Supply
     ↓
Electrical power becomes available
     ↓
Motherboard / CPU starts initialization
```

The exact electrical startup sequence depends on the hardware platform, but conceptually the processor begins executing firmware code.

---

# 7. Where Does the CPU Get Its First Instructions?

At the moment the system starts, the operating system is not yet running.

The CPU needs something to execute.

That initial code comes from firmware stored in non-volatile memory on the motherboard.

Historically this was usually:

```text
BIOS
```

Modern systems generally use:

```text
UEFI
```

So:

```text
Power On
   ↓
Firmware
   ↓
BIOS / UEFI
```

---

# 8. POST – Power-On Self-Test

One of the early firmware responsibilities is checking whether essential hardware is available and functioning well enough to continue startup.

This is commonly referred to as:

# POST

**Power-On Self-Test**

---

## What May Be Checked?

Depending on the system, firmware may initialize/check:

* CPU
* RAM
* Keyboard
* Display
* Storage devices
* USB devices
* Other hardware components

---

# 9. Example of a Hardware Problem

Suppose RAM is not properly installed.

The system may:

```text
Power On
   ↓
Firmware starts
   ↓
Memory initialization/check fails
   ↓
Boot process stops or reports an error
```

Depending on the motherboard, you may see:

* Beep codes
* LED indicators
* Error messages
* Diagnostic codes

---

# 10. BIOS

## Full Form

**Basic Input/Output System**

BIOS is the traditional firmware interface used by older PC systems.

Its responsibilities include:

* Hardware initialization
* Basic hardware checks
* Selecting a boot device
* Starting the bootloader

---

# 11. BIOS Boot Flow

Conceptually:

```text
Power ON
   ↓
BIOS
   ↓
POST
   ↓
Find Boot Device
   ↓
Read Boot Code
   ↓
Bootloader
   ↓
Operating System
```

---

# 12. Limitations of Traditional BIOS

Traditional BIOS implementations were designed around older PC assumptions.

Limitations include:

* Legacy boot mechanisms
* MBR-based booting
* Limited partitioning features compared with modern UEFI/GPT systems
* Limited firmware environment compared with UEFI

Modern computers generally use UEFI instead.

---

# 13. UEFI

## Full Form

**Unified Extensible Firmware Interface**

UEFI is the modern firmware environment used on most current PCs.

It performs the same broad startup role but provides a more capable firmware environment.

---

# 14. BIOS vs UEFI

| Feature              | BIOS                         | UEFI                              |
| -------------------- | ---------------------------- | --------------------------------- |
| Era                  | Older                        | Modern                            |
| Boot Method          | Legacy boot                  | Modern firmware boot              |
| Typical Partitioning | MBR                          | Commonly GPT                      |
| Interface            | Usually text-based           | Can provide graphical interface   |
| Firmware Features    | Limited                      | More extensible                   |
| Secure Boot          | Not part of traditional BIOS | Supported by UEFI                 |
| Boot Manager         | Legacy approach              | Built-in firmware boot management |

> **Important:** UEFI is not simply "BIOS with a different name." It is a different firmware interface standard.

---

# 15. BIOS vs UEFI – Simple Analogy

Imagine two versions of a security gate.

### Old Gate – BIOS

```text
Basic checking
↓
Find the entrance
↓
Start the system
```

### Modern Gate – UEFI

```text
Hardware initialization
↓
Boot management
↓
Configuration
↓
Security features
↓
Start the OS
```

The goal is similar, but UEFI provides a more modern and capable environment.

---

# 16. MBR vs GPT

This is related to BIOS/UEFI and often confuses students.

## MBR

**Master Boot Record**

Traditional partitioning/boot structure.

## GPT

**GUID Partition Table**

Modern partitioning scheme commonly used with UEFI systems.

Simplified relationship:

```text
Legacy BIOS → commonly MBR
UEFI        → commonly GPT
```

This is a useful rule of thumb, not an absolute law.

---

# 17. What Happens After Firmware?

Once firmware determines which device should be used for booting, it starts the next stage.

That next stage is the:

# Bootloader

---

# 18. What is a Bootloader?

A **bootloader** is a small program responsible for loading or starting the operating system kernel.

Its job is essentially:

```text
Find OS
  ↓
Load OS kernel
  ↓
Pass required information
  ↓
Start kernel
```

---

# 19. Why Do We Need a Bootloader?

The firmware's main job is hardware initialization and starting the boot process.

The bootloader provides a dedicated mechanism for selecting and launching an operating system.

For example, one computer may have:

```text
Windows
Linux
```

The bootloader can provide a menu that lets the user choose which operating system to start.

---

# 20. GRUB

Linux systems commonly use:

# GRUB

**GRand Unified Bootloader**

GRUB is a widely used bootloader in Linux environments.

---

# 21. What Does GRUB Do?

GRUB can:

* Display a boot menu
* Select an OS/kernel
* Load the kernel
* Load an initial RAM filesystem
* Pass kernel parameters
* Support multiple operating systems/configurations

---

# 22. GRUB Boot Flow

```text
UEFI
  ↓
GRUB
  ↓
Select Linux Kernel
  ↓
Load Kernel
  ↓
Load initramfs
  ↓
Start Kernel
```

---

# 23. GRUB Menu Example

When the system starts, you may see something conceptually similar to:

```text
GNU GRUB

Ubuntu
Advanced options for Ubuntu
Windows Boot Manager
```

Selecting:

```text
Ubuntu
```

causes GRUB to load the selected Linux boot configuration.

---

# 24. Kernel

Now we reach the most important software component:

# Kernel

The kernel is the core of the operating system.

It manages:

* CPU
* Memory
* Processes
* Devices
* Filesystems
* Networking
* Security

---

# 25. Why Must the Kernel Be Loaded?

Applications cannot run without the underlying operating system environment.

The kernel establishes the core runtime environment.

Conceptually:

```text
Bootloader
    ↓
Linux Kernel
    ↓
Hardware + System Resources
    ↓
Processes
    ↓
Services
    ↓
Users
```

---

# 26. Kernel Loading

The bootloader loads the kernel into RAM.

Conceptually:

```text
Storage

+-------------------+
| Linux Kernel      |
+-------------------+
| initramfs         |
+-------------------+

        ↓

RAM

+-------------------+
| Linux Kernel      |
+-------------------+
| initramfs         |
+-------------------+
```

The exact memory layout is architecture-dependent, but this is the correct high-level picture.

---

# 27. What is the Linux Kernel Image?

On many Linux systems, the kernel image is stored under:

```text
/boot
```

Run:

```bash
ls /boot
```

You may see files such as:

```text
vmlinuz-...
initrd.img-...
config-...
System.map-...
```

The exact filenames depend on the installed kernel version and distribution.

---

# 28. What is `vmlinuz`?

A file with a name similar to:

```text
vmlinuz-6.x.x-...
```

is a Linux kernel image.

The kernel is compressed in many Linux distributions to reduce storage/boot requirements and is decompressed as needed during boot.

---

# 29. What is initramfs?

This is an important concept.

`initramfs` means:

**Initial RAM Filesystem**

It is a temporary filesystem loaded into RAM during early boot.

---

# 30. Why Do We Need initramfs?

The kernel may need access to things such as:

* Storage drivers
* Filesystem drivers
* RAID/LVM support
* Encryption-related components
* Other modules needed to access the real root filesystem

But the real root filesystem is still on the storage device.

So we have a dependency problem:

```text
Need root filesystem
       ↓
Need storage/filesystem drivers
       ↓
Need early userspace
```

`initramfs` helps solve this early-boot problem.

---

# 31. Simplified initramfs Flow

```text
Kernel starts
    ↓
initramfs available
    ↓
Necessary early drivers/tools loaded
    ↓
Root filesystem located
    ↓
Real root filesystem mounted
    ↓
Continue normal userspace startup
```

---

# 32. Kernel Initialization

After the kernel begins execution, it performs many tasks, including:

* Initializing core subsystems
* Detecting/initializing hardware
* Setting up memory management
* Setting up process management
* Initializing device support
* Preparing the system to start userspace

Eventually, the kernel starts the first userspace process.

On modern Linux systems this is normally:

# `systemd`

---

# 33. What is systemd?

`systemd` is a system and service manager used by many modern Linux distributions.

It is typically started as:

# PID 1

PID means:

**Process ID**

---

# 34. Why is PID 1 Important?

The first userspace process has a special role.

Conceptually:

```text
Kernel
  ↓
PID 1
  ↓
Other processes
```

On a system using systemd:

```text
Kernel
   ↓
systemd (PID 1)
   ↓
Services
   ↓
Login Manager
   ↓
User Session
```

PID 1 has important responsibilities for system initialization and process management.

---

# 35. Verify PID 1

Run:

```bash
ps -p 1 -o pid,comm,args
```

On a typical systemd-based Linux distribution, you may see something similar to:

```text
PID  COMMAND  COMMAND
1    systemd  /sbin/init
```

The exact command path can differ.

---

# 36. What Does systemd Do?

`systemd` can:

* Start system services
* Stop services
* Restart services
* Manage dependencies
* Manage targets
* Track services
* Handle parts of system initialization
* Coordinate startup

---

# 37. Startup Services

After the kernel starts and `systemd` takes over, various services are started.

Examples:

```text
Network Manager
SSH Server
Bluetooth
Audio
Logging
Display Manager
Database
```

Not every computer has all of these.

---

# 38. What is a Service?

A service is a background program or system component that performs a specific function.

Examples:

```text
SSH
Network service
Web server
Database server
Bluetooth service
```

---

# 39. Check Running Services

On a systemd-based Linux distribution:

```bash
systemctl list-units --type=service --state=running
```

This shows currently active service units.

---

# 40. Check a Specific Service

For example:

```bash
systemctl status ssh
```

Depending on the distribution, the SSH service may be named differently, such as:

```text
ssh.service
```

or another equivalent service unit.

---

# 41. Startup Timeline

The complete sequence can now be viewed as:

```text
Power Button
     ↓
Firmware
     ↓
POST / Hardware Initialization
     ↓
Boot Device Selection
     ↓
GRUB
     ↓
Linux Kernel
     ↓
initramfs
     ↓
systemd (PID 1)
     ↓
System Services
     ↓
Display/Login Manager
     ↓
User Login
     ↓
Desktop
```

---

# 42. Login Screen

Once essential system services are running, a graphical login manager may present the login screen.

Examples of display/login managers include:

* GDM
* SDDM
* LightDM

The exact component depends on the Linux distribution and desktop environment.

---

# 43. What Happens During Login?

Suppose the user enters:

```text
Username: akash
Password: ********
```

The login system authenticates the user.

Then it starts a user session.

Conceptually:

```text
Login
  ↓
Authentication
  ↓
Create User Session
  ↓
Start Desktop Environment
  ↓
Desktop Appears
```

---

# 44. Desktop Environment

A Linux graphical session may include a desktop environment such as:

* GNOME
* KDE Plasma
* Xfce

A desktop environment provides components such as:

* Windows
* Panels
* Menus
* File manager
* Settings
* Desktop interface

---

# 45. Complete Linux Boot Diagram

```text
                         POWER ON
                            |
                            v
                   +----------------+
                   | BIOS / UEFI    |
                   | Firmware       |
                   +----------------+
                            |
                            v
                   +----------------+
                   | POST / Device  |
                   | Initialization |
                   +----------------+
                            |
                            v
                   +----------------+
                   | Boot Device    |
                   | Selection      |
                   +----------------+
                            |
                            v
                   +----------------+
                   | GRUB           |
                   | Bootloader     |
                   +----------------+
                            |
                            v
                   +----------------+
                   | Linux Kernel   |
                   +----------------+
                            |
                            v
                   +----------------+
                   | initramfs      |
                   +----------------+
                            |
                            v
                   +----------------+
                   | systemd        |
                   | PID 1          |
                   +----------------+
                            |
              +-------------+-------------+
              |             |             |
              v             v             v
           Network        Logging       Other
           Services       Services      Services
              |             |             |
              +-------------+-------------+
                            |
                            v
                   +----------------+
                   | Login Manager  |
                   +----------------+
                            |
                            v
                   +----------------+
                   | User Session   |
                   +----------------+
                            |
                            v
                   +----------------+
                   | Desktop / CLI  |
                   +----------------+
```

---

# 46. Boot Process as a Chain of Responsibility

Remember:

```text
Firmware
   ↓
Bootloader
   ↓
Kernel
   ↓
systemd
   ↓
Services
   ↓
Login
   ↓
User
```

Each stage prepares the system for the next.

---

# 47. Firmware vs Bootloader vs Kernel

Students commonly confuse these three.

| Component  | Main Responsibility                  |
| ---------- | ------------------------------------ |
| BIOS/UEFI  | Initialize hardware and begin boot   |
| Bootloader | Locate/select and load the OS kernel |
| Kernel     | Run and manage the operating system  |

---

# 48. Kernel vs systemd

Another common confusion.

### Kernel

Handles:

* CPU
* Memory
* Processes
* Devices
* Core system operations

### systemd

Runs in userspace and manages:

* Services
* Startup
* Dependencies
* System initialization
* User-space process management roles

Simplified:

```text
Kernel
  ↓
systemd
  ↓
Services
```

---

# 49. Practical 1 – Check Kernel Version

Run:

```bash
uname -r
```

Example:

```text
6.x.x-...
```

Question:

> Which stage of boot does this belong to?

Answer:

**Kernel stage.**

---

# 50. Practical 2 – Check the Kernel Image

Run:

```bash
ls -lh /boot
```

Look for something similar to:

```text
vmlinuz-...
```

This connects the theory of kernel loading to an actual file on disk.

---

# 51. Practical 3 – Check initramfs

Run:

```bash
ls -lh /boot | grep init
```

You may see files similar to:

```text
initrd.img-...
```

These are initramfs images.

---

# 52. Practical 4 – Identify PID 1

Run:

```bash
ps -p 1 -o pid,comm,args
```

Typical systemd system:

```text
1 systemd /sbin/init
```

Question:

> Who is PID 1?

Answer:

Usually `systemd` on modern systemd-based Linux distributions.

---

# 53. Practical 5 – Check systemd

Run:

```bash
systemctl --version
```

This shows whether systemd is available and its version.

---

# 54. Practical 6 – View Running Services

Run:

```bash
systemctl list-units --type=service --state=running
```

Observe:

* Service name
* Load status
* Active status
* Description

---

# 55. Practical 7 – Check System Boot Time

Run:

```bash
systemd-analyze
```

You may get something similar to:

```text
Startup finished in ...
```

This gives a high-level summary of boot timing on a systemd-based system.

---

# 56. Practical 8 – Analyze Boot Time

Run:

```bash
systemd-analyze blame
```

This shows startup units and how much time each took during boot.

Example structure:

```text
5.2s service-a.service
2.1s service-b.service
1.3s service-c.service
```

The exact numbers and services depend on the system.

---

# 57. Practical 9 – View Boot Messages

Run:

```bash
journalctl -b
```

This displays logs from the current boot.

You can think of it as:

```text
Boot
 ↓
Kernel/System startup
 ↓
Services
 ↓
Logs
```

For kernel messages specifically, another useful command is:

```bash
journalctl -k -b
```

---

# 58. Practical 10 – View GRUB Configuration

On many Linux systems:

```bash
cat /etc/default/grub
```

This shows configuration settings used to generate GRUB configuration.

Do not modify the file during a beginner practical unless instructed.

---

# 59. Practical 11 – Check Firmware Boot Mode

On many UEFI-based Linux systems, you can check:

```bash
test -d /sys/firmware/efi && echo "UEFI" || echo "Legacy BIOS"
```

Possible output:

```text
UEFI
```

or:

```text
Legacy BIOS
```

This is a useful practical demonstration of BIOS vs UEFI.

---

# 60. Live Classroom Demonstration

Perform these commands one by one:

```bash
uname -r
```

Then:

```bash
ls -lh /boot
```

Then:

```bash
ps -p 1 -o pid,comm,args
```

Then:

```bash
systemd-analyze
```

Then:

```bash
systemctl list-units --type=service --state=running
```

Finally:

```bash
journalctl -k -b
```

Now connect the commands to the boot process:

```text
/boot
  ↓
Kernel

PID 1
  ↓
systemd

systemctl
  ↓
Services

journalctl
  ↓
Boot / system logs
```

---

# 61. Interesting Question for Students

Ask:

> **"What would happen if the kernel file was missing?"**

Conceptually:

```text
Firmware
   ↓
GRUB
   ↓
Kernel not found
   ↓
Boot cannot continue normally
```

The system would not be able to start the normal operating system environment.

---

# 62. Another Question

Ask:

> **"What if systemd fails after the kernel loads?"**

The kernel may still be running, but normal userspace initialization may fail or become incomplete.

You may see:

* Emergency mode
* Rescue shell
* Boot failure
* Missing services
* No normal login environment

This demonstrates that:

```text
Kernel ≠ Entire User Experience
```

---

# 63. BIOS vs UEFI – Exam Answer

### BIOS

Traditional firmware used to initialize hardware and begin the boot process.

### UEFI

Modern firmware interface that initializes hardware, manages boot entries, supports modern partitioning conventions such as GPT, and can provide security features such as Secure Boot.

---

# 64. What is Secure Boot?

Secure Boot is a UEFI security feature.

Its purpose is to help prevent unauthorized or untrusted boot components from being executed during startup.

Simplified:

```text
UEFI
 ↓
Verify boot component
 ↓
Trusted?
 ├── Yes → Continue
 └── No  → Block / warn
```

The exact trust mechanism depends on configured keys and platform policy.

---

# 65. Bootloader vs Operating System

A bootloader is not the operating system.

```text
GRUB
 ↓
Loads/starts
 ↓
Linux Kernel
 ↓
Operating System environment
```

GRUB's primary job is to get the OS started.

---

# 66. Why Does the Bootloader Need a Kernel Configuration?

GRUB needs to know:

* Which kernel to load
* Where the kernel is located
* Which initramfs to load
* Which kernel parameters to pass

A simplified kernel command line may contain parameters such as:

```text
root=...
quiet
splash
```

These parameters influence kernel startup behavior.

---

# 67. What is `init`?

Historically, Unix/Linux systems used an `init` process as the first userspace process.

Modern distributions commonly use:

```text
systemd
```

as the initialization system.

That's why you may see:

```text
/sbin/init
```

even though the actual program is:

```text
systemd
```

because `/sbin/init` may point to or launch the systemd implementation.

---

# 68. Boot Process in One Story

Imagine you are starting a laptop.

### Step 1

You press:

```text
POWER
```

### Step 2

Firmware starts.

```text
UEFI
```

### Step 3

Hardware is initialized and checked.

```text
POST
```

### Step 4

Firmware finds the configured boot entry.

```text
Boot Device
```

### Step 5

GRUB starts.

```text
GRUB
```

### Step 6

GRUB selects and loads:

```text
Linux Kernel
+
initramfs
```

### Step 7

Kernel initializes the system.

### Step 8

Kernel starts:

```text
systemd
```

### Step 9

systemd starts required services.

```text
Network
Logging
Display Manager
Other Services
```

### Step 10

Login screen appears.

### Step 11

You authenticate.

### Step 12

Your desktop/session starts.

---

# 69. Final Boot Diagram

```text
YOU
 |
 | Press Power
 v
+------------------+
| Firmware         |
| BIOS / UEFI      |
+------------------+
        |
        | Hardware Initialization
        v
+------------------+
| POST             |
+------------------+
        |
        | Find boot target
        v
+------------------+
| Bootloader       |
| GRUB             |
+------------------+
        |
        | Load kernel
        v
+------------------+
| Linux Kernel     |
+------------------+
        |
        | Early userspace
        v
+------------------+
| initramfs        |
+------------------+
        |
        | Start userspace
        v
+------------------+
| systemd          |
| PID 1            |
+------------------+
        |
        +------> Network
        |
        +------> Logging
        |
        +------> Other Services
        |
        +------> Display Manager
                       |
                       v
                 Login Screen
                       |
                       v
                  User Session
                       |
                       v
                    Desktop
```

---

# 70. Key Takeaways

* Pressing the power button starts a sequence called the **boot process**.
* Firmware runs before the operating system.
* Modern PCs typically use **UEFI**; older systems commonly used **BIOS**.
* Firmware performs hardware initialization and starts the boot process.
* **POST** checks/initializes important hardware.
* A **bootloader** starts the operating system.
* **GRUB** is a common Linux bootloader.
* GRUB loads the Linux kernel and initial boot files such as `initramfs`.
* The **Linux kernel** initializes the core operating system environment.
* `initramfs` provides early userspace support needed to locate and mount the real root filesystem.
* On systemd-based Linux distributions, **systemd** usually becomes **PID 1**.
* systemd starts and manages many system services.
* A login manager provides the login screen.
* After authentication, the user's graphical or command-line session starts.

---

# 71. Viva Questions

### Q1. What is booting?

Booting is the process of starting the computer and loading the operating system.

### Q2. What runs first after power-on?

Firmware, such as BIOS or UEFI, begins execution.

### Q3. What is POST?

Power-On Self-Test, an early hardware initialization/check phase performed by firmware.

### Q4. What is the difference between BIOS and UEFI?

BIOS is the traditional firmware interface, while UEFI is the modern firmware standard with a richer boot environment and features such as Secure Boot.

### Q5. What is a bootloader?

Software that starts the operating system by locating/selecting and loading the kernel.

### Q6. What is GRUB?

GRUB is a widely used Linux bootloader.

### Q7. What is the kernel?

The core part of the operating system that manages CPU, memory, processes, devices, filesystems, and other system resources.

### Q8. What is initramfs?

A temporary initial RAM filesystem used during early Linux boot to provide drivers/tools needed before the real root filesystem is available.

### Q9. What is systemd?

A system and service manager used by many modern Linux distributions.

### Q10. What is PID 1?

The first userspace process. On systemd-based systems, it is typically systemd.

### Q11. What happens after systemd starts?

It initializes userspace and starts configured services, eventually leading to a login environment.

### Q12. Can GRUB and the kernel be considered the same thing?

No. GRUB is a bootloader; the kernel is the core operating system component.

---

# 72. Practical Assignment

Run the following commands:

```bash
uname -r
```

```bash
ls -lh /boot
```

```bash
ps -p 1 -o pid,comm,args
```

```bash
systemd-analyze
```

```bash
systemctl list-units --type=service --state=running
```

```bash
journalctl -k -b
```

Then answer:

```text
1. What kernel version are you using?
2. Is the system using UEFI or Legacy BIOS?
3. What is PID 1?
4. Is systemd running?
5. Which services start during boot?
6. How long did systemd report for startup?
7. Can you find the kernel image inside /boot?
8. Can you find an initramfs image inside /boot?
```

---

# 73. One-Line Memory Trick

Remember the boot sequence as:

```text
POWER
  ↓
FIRMWARE
  ↓
POST
  ↓
BOOTLOADER
  ↓
KERNEL
  ↓
INITRAMFS
  ↓
SYSTEMD
  ↓
SERVICES
  ↓
LOGIN
  ↓
DESKTOP
```

> **Core idea:**
> The computer does not suddenly "start Windows/Linux." It goes through a chain of stages where each stage initializes something and hands control to the next stage until the full operating system environment is ready.
