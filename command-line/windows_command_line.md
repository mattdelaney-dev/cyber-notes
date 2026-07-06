# Windows Command Line

Part of my TryHackMe practice (Cyber Security 101 path) — covering the core Windows CLI commands used for system information gathering, network troubleshooting, file/disk management, and process management. Foundational skills for both general sysadmin work and blue-team triage on Windows hosts.

## Topics Covered
- Basic System Information
- Network Troubleshooting
- File and Disk Management
- Task and Process Management

---

## Basic System Information

Commands used to quickly profile a host — OS version, hardware, hostname, and environment details.

- `systeminfo` — full OS build, patch level, installed hotfixes, memory, and boot time in one dump
- `hostname` — quick identification of the machine on a network
- `whoami` and `whoami /priv` — current user context and assigned privileges (useful for checking if a session is running with elevated rights)
- `ver` — Windows version string

**Why it matters:** `systeminfo` and `whoami /priv` are two of the first commands worth running on any host during triage — patch level tells you what the machine may be vulnerable to, and privilege level tells you what an attacker (or you) can actually do on the box.

---

## Network Troubleshooting

Commands for diagnosing connectivity and inspecting network configuration.

- `ipconfig` / `ipconfig /all` — interface configuration, IP, gateway, DNS
- `ping` — basic reachability test
- `tracert` — path/hop analysis to a destination
- `netstat -ano` — active connections and listening ports, mapped to owning PIDs
- `nslookup` — DNS resolution checks

**Why it matters:** `netstat -ano` is a key pivot point between "network" and "process" — an unexpected listening port or outbound connection can be traced straight back to the PID responsible, which is exactly the kind of lead that turns into a process-tree investigation.

---

## File and Disk Management

Commands for navigating the filesystem and managing storage.

- `dir` — directory listing (with switches like `/a` to show hidden/system files)
- `cd` — navigate directories
- `copy`, `move`, `del` — basic file operations
- `chkdsk` — check disk for filesystem errors
- `diskpart` — partition management

**Why it matters:** `dir /a` is worth remembering specifically — hidden or system-attribute files are a common place for attackers to stash payloads, so knowing how to reveal them from the command line (without relying on GUI "show hidden files" settings) is a small but genuinely useful habit.

---

## Task and Process Management

Commands for viewing and controlling running processes.

- `tasklist` — list running processes, with `/svc` to show hosted services per PID
- `taskkill /PID <pid> /F` — force-terminate a process by PID
- `Get-Process` (PowerShell equivalent) for more detailed process objects

**Why it matters:** This is the most directly security-relevant task in the room. `tasklist /svc` is the command-line equivalent of what I was doing manually in Process Monitor during the PaperCut CVE-2023-27350 room — mapping PIDs to their parent process/service to spot something that shouldn't be there. `taskkill` is the natural next step once a malicious PID is identified.

---

## Why This Matters

This room is a good baseline-builder rather than an exploitation room — the value is in having these commands fast and automatic, so that during an actual investigation (like the PaperCut process tree analysis) the mechanics of querying the system don't slow down the analysis itself. `whoami /priv`, `netstat -ano`, and `tasklist /svc` in particular are commands I'd expect to run in the first five minutes of triaging any Windows host.

## Reflection

Straightforward room, but useful to have written down as a quick-reference rather than relying on memory. The clearest tie-in to prior TryHackMe work is `netstat -ano` → `tasklist /svc` as a manual, command-line version of the PID-to-process mapping I did in Process Monitor for CVE-2023-27350 — same underlying skill (trace a suspicious PID back to what spawned it), just a different tool.