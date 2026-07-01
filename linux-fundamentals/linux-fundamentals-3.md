# Linux Fundamentals Part 3

## Overview

This room rounded out the Linux fundamentals series, moving from basic navigation and file management into the tools and processes used for day-to-day Linux administration and troubleshooting.

Topics explored included:
- Using terminal-based text editors
- General/useful command-line utilities
- Managing running processes
- Automating tasks with cron
- Installing and managing software packages
- Reading and understanding system logs

---

## Terminal Text Editors

Since many Linux systems (especially servers) don't have a GUI, editing files is done directly in the terminal.

Common editors:
- **nano** — beginner-friendly, shows keyboard shortcuts on screen
- **vi / vim** — more powerful, but has a steeper learning curve (insert mode vs command mode)

Basic nano usage:
```bash
nano filename.txt
```
- `Ctrl + O` — save (write out)
- `Ctrl + X` — exit

Basic vi usage:
```bash
vi filename.txt
```
- `i` — enter insert mode (to type text)
- `Esc` — return to command mode
- `:wq` — write (save) and quit
- `:q!` — quit without saving

---

## General/Useful Utilities

A set of commands that come up constantly in day-to-day Linux use:

| Command | Purpose |
|---|---|
| `grep` | Search for text patterns within files |
| `find` | Search for files/directories by name, type, size, etc. |
| `sed` | Stream editor — find and replace text |
| `awk` | Pattern scanning and text processing |
| `tar` | Archive (bundle) files together |
| `curl` / `wget` | Download files or interact with URLs from the terminal |

Examples:
```bash
grep "error" logfile.txt          # find lines containing "error"
find / -name "*.conf"             # find all .conf files
tar -czvf archive.tar.gz folder/  # compress a folder into an archive
curl https://example.com          # fetch a URL's content
```

---

## Processes 101

Every running program on Linux is a **process**, identified by a unique **PID** (Process ID).

Key commands:
- `ps` — list currently running processes
- `ps aux` — detailed list of all processes on the system
- `top` — live, updating view of running processes and resource usage
- `kill <PID>` — terminate a process by its PID
- `bg` / `fg` — send a process to the background / bring it back to the foreground

Example:
```bash
ps aux | grep firefox
kill 1523
```

Understanding processes is essential for spotting suspicious or resource-heavy activity on a system.

---

## Maintaining Your System: Automation

**Cron** is used to schedule tasks to run automatically at set times/intervals.

- Managed via a user's **crontab** (cron table)
- View/edit with:
```bash
crontab -e
```
- Cron syntax: `minute hour day month weekday command`

Example — run a script every day at midnight:
```
0 0 * * * /home/user/backup.sh
```

Automation like this is also a common technique used for **persistence** by attackers, so it's worth recognising in incident response contexts too.

---

## Maintaining Your System: Package Management

Linux distributions use package managers to install, update, and remove software.

Debian/Ubuntu-based systems use `apt`:
```bash
apt update              # refresh the list of available packages
apt upgrade              # upgrade installed packages
apt install <package>    # install a new package
apt remove <package>     # remove a package
```

Lower-level package handling can be done with `dpkg` (installing `.deb` files directly).

---

## Maintaining Your System: Logs

Logs record what's happening on a system and are critical for troubleshooting and security monitoring.

- Most logs live under `/var/log`
- Common logs: `auth.log` (authentication attempts), `syslog` (general system activity)
- View logs with standard tools:
```bash
cat /var/log/syslog
tail -f /var/log/auth.log   # follow a log in real time
```
- On systems using `systemd`, `journalctl` provides a unified way to view logs

---

## Why This Matters

These skills are the day-to-day toolkit for anyone working on Linux systems in a security context:
- SOC analysts rely on log analysis to detect and investigate incidents
- Penetration testers use utilities like `grep`, `find`, and `curl` constantly during enumeration
- Understanding processes and cron jobs helps identify persistence mechanisms used by attackers
- Package management knowledge is essential for both hardening systems and identifying outdated/vulnerable software

---

## Reflection

This room tied together the practical, everyday side of Linux administration — editing files, managing processes, automating tasks, installing software, and reading logs. Combined with Parts 1 and 2, this gives a solid working foundation in Linux that will support later, more security-focused modules.