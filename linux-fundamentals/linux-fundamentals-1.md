# Linux Fundamentals Part 1

## Overview

This room introduced the fundamentals of Linux, covering its background and teaching the first essential commands used in a Linux terminal.

Topics explored included:
- Background and history of Linux
- Interacting with a Linux machine in-browser
- Running basic commands
- Navigating and interacting with the filesystem
- Searching for files
- Shell operators

## Key Concepts Learned

Linux is one of the most widely used operating systems in cyber security, used across servers, embedded devices, and security tools. Understanding how to navigate and interact with Linux via the command line is an essential foundational skill.

---

## A Bit of Background on Linux

Linux is an open-source operating system first released in 1991. It is widely used across:
- Web servers and cloud infrastructure
- Networking equipment
- Security tools and penetration testing distributions (e.g. Kali Linux, Parrot OS)
- Embedded systems and IoT devices

Unlike Windows, Linux is primarily interacted with via a command-line interface (CLI) rather than a graphical user interface (GUI), making command-line proficiency essential for anyone working in cyber security.

Linux comes in many distributions (distros), such as Ubuntu, Debian, CentOS, and Kali, each suited to different use cases.

---

## Running Your First Commands

The terminal is where Linux commands are entered and executed. Some of the first and most commonly used commands include:

- `echo` — outputs text to the terminal
- `whoami` — displays the current logged-in user

Example:
Running `echo Hello World` prints "Hello World" to the terminal. Running `whoami` returns the username of the current user.

These simple commands introduce the basic idea of how the terminal works: type a command, press enter, receive output.

---

## Interacting With the Filesystem

Linux organises files and directories in a hierarchical structure. Several core commands are used to navigate and interact with this structure:

- `ls` — lists the files and directories in the current location
- `cd` — changes the current directory
- `cat` — outputs the contents of a file to the terminal
- `pwd` — prints the full path of the current working directory

Examples:
- `ls` shows what is in the current directory
- `cd Documents` moves into the Documents directory
- `cat file.txt` displays the contents of file.txt
- `pwd` confirms exactly where you are in the filesystem

Understanding how to move around the filesystem and read files is fundamental to working in Linux.

---

## Searching for Files

Linux provides powerful tools for locating files across the filesystem.

- `find` — searches for files and directories based on criteria such as name, type, or size
- `grep` — searches for specific text within files

Examples:
- `find / -name passwords.txt` searches the entire filesystem for a file named passwords.txt
- `grep "hello" file.txt` searches inside file.txt for the word "hello"

These tools are especially useful in cyber security for locating configuration files, credentials, and other points of interest on a system.

---

## Shell Operators

Shell operators extend what can be done with commands by allowing them to be combined or redirected.

Key operators include:

- `&` — runs a command in the background, allowing other commands to be run simultaneously
- `&&` — chains commands together; the second command only runs if the first succeeds
- `>` — redirects the output of a command into a file, overwriting its contents
- `>>` — appends the output of a command to a file without overwriting existing content

Examples:
- `echo hello > file.txt` writes "hello" to file.txt, replacing any existing content
- `echo world >> file.txt` adds "world" to the end of file.txt
- `command1 && command2` runs command2 only if command1 completes successfully

Shell operators are a key part of working efficiently in the Linux terminal.

---

## Why This Matters

Linux is the dominant operating system in cyber security environments. Most servers, security tools, and CTF challenges run on Linux.

Linux skills are essential for roles such as:
- Penetration tester
- SOC analyst
- System administrator
- Malware analyst
- DevSecOps engineer

Being comfortable in a Linux terminal is a baseline expectation across almost every technical cyber security role.

---

## Reflection

This room provided a practical introduction to Linux by covering its background and teaching the core commands needed to navigate the filesystem, search for files, and use shell operators. These fundamentals form the foundation for all future Linux-based work in cyber security.