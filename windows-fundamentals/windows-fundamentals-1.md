# Windows Fundamentals 1

## Overview

This room introduced the fundamentals of the Windows operating system, covering the different Windows editions, the desktop environment, the file system, key system folders, user accounts and permissions, and core administration tools.

Topics explored included:
- Windows editions and their use cases
- The Windows desktop (GUI)
- The Windows file system (NTFS)
- The Windows\System32 folder
- User accounts, profiles, and permissions
- User Account Control (UAC)
- Settings and the Control Panel
- Task Manager

---

## Windows Editions

Windows is released in several editions depending on the target audience and use case:

- **Home** — general consumer use
- **Pro** — additional features for professionals and small businesses (e.g. BitLocker, Remote Desktop)
- **Enterprise** — advanced management and security features for large organisations
- **Server** — designed to run on servers, providing services such as Active Directory, DNS, and DHCP

Understanding which edition is in use matters in cyber security, since available features (and therefore attack surface) differ between them.

---

## The Desktop (GUI)

The Windows GUI (Graphical User Interface) is how most users interact with the system.

Key components:
- **Taskbar** — quick access to open apps, the Start Menu, and system tray
- **Start Menu** — search and launch installed applications
- **File Explorer** — browse the file system visually
- **System Tray** — shows background processes, network status, volume, etc.

---

## The File System

Windows primarily uses **NTFS** (New Technology File System) as its default file system.

Key points:
- Drives are labelled with letters, e.g. `C:\` for the primary drive
- Common top-level folders: `Program Files`, `Users`, `Windows`
- File paths use backslashes `\` rather than the forward slashes `/` used in Linux

Example path:
```
C:\Users\Max\Documents
```

---

## The Windows\System32 Folder

`C:\Windows\System32` is one of the most important folders on a Windows system.

- Contains core system files, DLLs, and executables required for Windows to function
- Many built-in tools and utilities live here (e.g. `notepad.exe`, `cmd.exe`)
- Frequently targeted by malware for persistence or masquerading, since files here are trusted by default

---

## User Accounts, Profiles, and Permissions

Windows supports two main local account types:

| Account Type | Capabilities |
|---|---|
| **Administrator** | Full control — install software, manage users/groups, change system settings |
| **Standard User** | Limited to their own files/folders — cannot make system-level changes |

Key concepts:
- Each user gets a profile folder under `C:\Users\<username>` on first login
- Default profile subfolders include Desktop, Documents, Downloads, Music, Pictures
- **Local Users and Groups** (`lusrmgr.msc`) is used to view/manage accounts and group memberships
- Users inherit permissions from any group they belong to
- The built-in **Guest** account provides limited, temporary access to the system

Useful tool:
```
lusrmgr.msc
```
Run via Start Menu → Run, to open Local Users and Groups management.

---

## User Account Control (UAC)

UAC is a security feature that prompts for confirmation before allowing actions that require administrative privileges.

- Prevents unauthorised changes from being made silently (e.g. by malware or scripts)
- Appears as a pop-up asking "Do you want to allow this app to make changes to your device?"
- Can be adjusted in Control Panel, though lowering it reduces security

---

## Settings and the Control Panel

Windows provides two main interfaces for configuration:

- **Settings** — the modern, streamlined configuration app
- **Control Panel** — the legacy configuration interface, still used for some advanced options not yet migrated to Settings

Both allow management of things like user accounts, network settings, and system properties.

---

## Task Manager

Task Manager provides visibility into what's running on the system.

- View running processes, apps, and background services
- Monitor CPU, memory, disk, and network usage in real time
- End unresponsive or suspicious processes
- Useful for basic incident response — spotting unusual processes consuming resources

Open with: `Ctrl + Shift + Esc` or right-click the taskbar.

---

## Why This Matters

A solid understanding of Windows internals — accounts, permissions, the file system, and system tools — underpins almost all practical work in cyber security roles, since Windows remains the most widely deployed OS in enterprise environments. This knowledge is foundational for:
- SOC analysts triaging alerts on Windows endpoints
- Penetration testers assessing privilege escalation paths
- System administrators managing user access and permissions

---

## Reflection

This room laid the groundwork for understanding how Windows is structured and administered — accounts, permissions, and system tools. These fundamentals will be essential background for later modules covering Windows security, Active Directory, and privilege escalation.