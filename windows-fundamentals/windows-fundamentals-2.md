# Windows Fundamentals 2

## Overview

This room built on Part 1 by going deeper into system configuration and administration tools — how to tune system settings, adjust UAC, manage the system from a central console, monitor resources, use the command line, and edit the registry.

Topics explored included:
- System Configuration and Advanced System Settings
- Changing UAC settings
- Computer Management
- System Information
- Resource Monitor
- Command Prompt
- Registry Editor

---

## System Configuration and Advanced System Settings

The **System Configuration** utility (`msconfig`) and **Advanced System Settings** give access to deeper system controls than the standard Settings app.

Open with:
```
msconfig
```

Key areas:
- **General** — choose startup type (normal, diagnostic, selective)
- **Boot** — configure boot options, timeout, safe boot
- **Services** — enable/disable services that start with Windows
- **Startup** — manage startup programs (in newer Windows, this links to Task Manager's Startup tab)

**Advanced System Settings** (via `sysdm.cpl` or System Properties) also provides access to:
- Performance settings (visual effects vs performance)
- Environment variables
- System protection / restore points
- Remote settings

---

## Change UAC Settings

User Account Control (UAC) can be tuned to be more or less strict about prompting for administrative confirmation.

- Accessed via Control Panel → User Accounts → Change User Account Control settings
- A slider controls the level, from **Always notify** to **Never notify**
- Lowering UAC reduces the number of prompts but also reduces protection against unauthorised or malicious changes
- Best practice is to leave UAC at a higher setting rather than disabling it for convenience

---

## Computer Management

**Computer Management** is a consolidated console bringing together several administrative tools in one place.

Open with:
```
compmgmt.msc
```

Includes:
- **Task Scheduler** — automate tasks to run at specific times/events
- **Event Viewer** — review system, security, and application logs
- **Shared Folders** — view/manage network shares
- **Local Users and Groups** — manage accounts and permissions
- **Device Manager** — view/manage hardware devices
- **Disk Management** — manage drives and partitions
- **Services** — start/stop/configure background services

---

## System Information

The **System Information** tool (`msinfo32`) provides a detailed overview of the hardware and software configuration of a machine.

Open with:
```
msinfo32
```

Useful for:
- Checking OS version, build number, and installed RAM
- Viewing hardware resources (IRQs, memory, DMA)
- Checking installed drivers and software environment
- Useful starting point when investigating an unfamiliar system

---

## Resource Monitor

**Resource Monitor** (`resmon`) gives a more detailed, real-time breakdown of system resource usage than Task Manager.

Open with:
```
resmon
```

Tabs:
- **CPU** — per-process CPU usage, associated services
- **Memory** — per-process memory usage
- **Disk** — disk read/write activity per process
- **Network** — network activity per process, including active TCP connections

Particularly useful for identifying which specific process is responsible for high resource usage or unexpected network activity.

---

## Command Prompt

The **Command Prompt** (`cmd.exe`) is Windows' traditional command-line interface.

Common commands:
| Command | Purpose |
|---|---|
| `dir` | List files/folders in the current directory |
| `cd` | Change directory |
| `ipconfig` | Display network configuration |
| `systeminfo` | Display detailed system information |
| `tasklist` | List running processes |
| `taskkill /PID <pid>` | Terminate a process by PID |

Command Prompt is heavily used in both system administration and offensive security contexts (e.g. initial enumeration on a compromised host).

---

## Registry Editor

The **Windows Registry** is a hierarchical database storing low-level system and application settings.

Open with:
```
regedit
```

Structure — five root keys ("hives"):
- `HKEY_CLASSES_ROOT`
- `HKEY_CURRENT_USER`
- `HKEY_LOCAL_MACHINE`
- `HKEY_USERS`
- `HKEY_CURRENT_CONFIG`

Notes:
- Incorrect changes to the registry can break Windows or applications — back up before editing
- Frequently targeted by malware for persistence (e.g. adding entries to `Run` keys so a program launches at startup)

---

## Why This Matters

These tools represent the core toolkit for diagnosing, configuring, and monitoring a Windows machine — skills that are directly relevant to:
- SOC analysts investigating unusual processes, startup entries, or registry persistence
- Penetration testers enumerating a Windows host post-compromise
- System administrators maintaining and troubleshooting endpoints

---

## Reflection

This room significantly expanded on Part 1, moving from basic desktop/account concepts into the administrative tools used to configure, monitor, and troubleshoot Windows systems. The Registry Editor and Resource Monitor sections in particular link closely to how attackers achieve persistence and how defenders spot it — useful groundwork heading into more security-focused Windows content.