Here's a structured notes file similar to your Part 1 README:

---

# Linux Fundamentals Part 2

## Overview

This room continued the Linux fundamentals journey, covering how to access a Linux machine remotely using SSH, how to use flags and switches with commands, and how to interact further with the filesystem.

Topics explored included:
- Connecting to a Linux machine using SSH
- Using flags and switches to extend commands
- Creating, copying, moving, and deleting files and folders
- Determining file types
- Linux permissions
- Common Linux directories

---

## Accessing Your Linux Machine Using SSH

SSH (Secure Shell) is a protocol used to remotely log into a Linux machine securely over a network.

Basic SSH syntax:
```bash
ssh username@ip-address
```

Example:
```bash
ssh tryhackme@10.49.158.81
```

SSH allows you to interact with a remote machine's terminal as if you were sitting in front of it. It is one of the most commonly used tools in cyber security for accessing remote systems.

---

## Introduction to Flags and Switches

Commands on Linux can be extended using flags and switches to change or enhance their behaviour.

- Flags are typically prefixed with `-` (short form) or `--` (long form)
- `-h` or `--human-readable` displays output in a readable format (e.g. KB, MB, GB instead of raw bytes)
- The `--help` flag can be used with most commands to display usage information
- `man <command>` opens the manual page for a command

Examples:
- `ls -a` shows hidden files
- `du -h` shows disk usage in human-readable sizes
- `man ls` opens the manual for the ls command

---

## Filesystem Interaction Continued

Beyond basic navigation, Linux provides commands for creating, copying, moving, and removing files and folders.

| Command | Full Name | Purpose |
|--------|-----------|---------|
| `touch` | touch | Create a file |
| `mkdir` | make directory | Create a folder |
| `cp` | copy | Copy a file or folder |
| `mv` | move | Move or rename a file or folder |
| `rm` | remove | Remove a file or folder |
| `file` | file | Determine the type of a file |

Examples:
- `touch note` creates a blank file called note
- `mkdir myfolder` creates a directory called myfolder
- `cp note note2` copies note into a new file called note2
- `mv note2 note3` renames note2 to note3
- `rm note` deletes the file note
- `rm -R myfolder` deletes a directory and all its contents
- `file unknown1` determines the file type of unknown1

---

## Permissions 101

Linux uses a permissions system to control who can read, write, or execute files.

- Every file has an owner and an associated group
- Permissions are displayed with `ls -l`
- `su <username>` switches to another user
- Permission types: **r** (read), **w** (write), **x** (execute)

---

## Common Directories

Key directories in the Linux filesystem:

- `/etc` — stores system configuration files
- `/var` — stores frequently changing data such as logs
- `/root` — home directory of the root user
- `/tmp` — stores temporary files, cleared on reboot

---

## Why This Matters

SSH and filesystem interaction are core skills used daily in cyber security roles. Being able to connect to remote machines, navigate filesystems, manage files, and understand permissions is essential for:
- Penetration testers accessing target machines
- SOC analysts investigating compromised systems
- System administrators managing servers remotely

---

## Reflection

This room built directly on Part 1 by introducing remote access via SSH and expanding filesystem interaction commands. Understanding how to manage files and permissions on Linux is a critical skill that underpins nearly all practical cyber security work.