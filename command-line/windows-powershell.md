# Windows PowerShell

Part of my TryHackMe practice (Cyber Security 101 path) — moving beyond the basic Windows CLI into PowerShell's object-based cmdlets for system enumeration, filtering/piping data, and real-time investigation (processes, services, network connections, file hashes). Also covered basic remote command execution with `Invoke-Command`.

## Topics Covered
- PowerShell Basics
- Navigating the File System and Working with Files
- Piping, Filtering, and Sorting Data
- System and Network Information
- Real-Time System Analysis
- Remote Command Execution

---

## PowerShell Basics

PowerShell cmdlets follow a consistent `Verb-Noun` naming pattern (`Get-Process`, `Get-Service`, `Get-Item`), which makes the language far more discoverable than traditional cmd.exe commands — you can often guess the right cmdlet just from the pattern.

**Why it matters:** Object-based output (vs. cmd.exe's plain text) is the core reason PowerShell is worth learning on top of the basic command line — every cmdlet returns structured objects with properties, which is what makes filtering and piping so powerful later on.

---

## Navigating the File System and Working with Files

- `Get-ChildItem` (`dir`, `ls`) — list directory contents, `-Force` reveals hidden items
- `Get-Content` — read file contents (equivalent of `cat`/`type`)
- `Get-Item` — retrieve an item, including with `-Stream *` to reveal Alternate Data Streams (ADS) attached to a file

**Why it matters:** `Get-Item -Path <file> -Stream *` is the PowerShell-native way to check for ADS, which ties directly back to the Windows Fundamentals 1 room — same hidden-data concept, just accessed through cmdlets instead of `dir /r` or a GUI tool.

---

## Piping, Filtering, and Sorting Data

- `|` (pipe) — passes objects from one cmdlet into the next
- `Where-Object` — filters objects based on a property condition, e.g. `{$_.DisplayName -like "*motto*"}`
- `Sort-Object`, `Select-Object` — reorder or narrow down which properties are returned

**Why it matters:** This is the single most useful skill in the room. Once you can pipe `Get-Service` or `Get-Process` into `Where-Object`, you're no longer scrolling through a full dump looking for anomalies by eye — you're querying the system directly, which is exactly how you'd triage a live host under time pressure.

---

## System and Network Information

- `Get-ComputerInfo` — detailed OS/hardware info, PowerShell's equivalent of `systeminfo`
- `Get-LocalUser` — lists local accounts, including the `Description` field
- `Get-NetIPConfiguration` / `Get-NetAdapter` — network interface details, equivalent of `ipconfig`

**Why it matters:** `Get-LocalUser` surfacing account `Description` fields was the pivot point in this room's challenge — a seemingly cosmetic field turned out to be the thread that connected a suspicious user account to a tampered service.

---

## Real-Time System Analysis

- `Get-Process` — running processes with CPU/memory usage, PID, etc.
- `Get-Service` — service status, Name, and DisplayName
- `Get-NetTCPConnection` — active TCP connections, including `OwningProcess` (the PID that opened the connection)
- `Get-FileHash` — generate a file's hash (e.g. SHA256) for integrity checks

**Why it matters:** `Get-NetTCPConnection`'s `OwningProcess` property is the PowerShell equivalent of what `netstat -ano` gives you at the cmd.exe level — same PID-to-connection mapping, reinforcing that this is a core technique regardless of which shell you're in. `Get-FileHash` is the natural next step after finding a suspicious file: confirm what it is before you decide what to do with it.

---

## Remote Command Execution

- `Invoke-Command -ComputerName <host> -ScriptBlock {<command>}` — runs a command against a remote machine
- Script blocks (`{ }`) are treated as executable code objects, not strings — no quotes needed for simple unquoted arguments

**Why it matters:** This is the first real step from "querying my own machine" toward "querying machines across a network," which is the actual day-to-day reality of SOC/blue-team work — you're rarely investigating only the box you're sitting at.

---

## Room Challenge — Tampered Service

The scenario: a service's `DisplayName` had been altered to match a "motto" left in a suspicious user's account description. The intended solve path:

1. `Get-LocalUser` → inspect the `Description` field of the suspicious account to recover the motto.
2. `Get-Service | Where-Object {$_.DisplayName -like "*keyword_from_motto*"}` → filter services for a `DisplayName` match.
3. Read the **Name** column (not `DisplayName`) from the result — that's the actual service identifier, since `DisplayName` is just the label that was tampered with.

**Why it matters:** This mirrors real service-tampering detection — attackers or malware sometimes rename a service's display name to blend in or (in this case) leave a calling card, but the underlying `Name` (used by `sc.exe`, the registry, and service control internally) is a separate, more reliable identifier to pivot on.

---

## Reflection

This room builds directly on the Windows Command Line room — `Get-Process`/`Get-Service`/`Get-NetTCPConnection` are the object-based upgrades of `tasklist`/`sc query`/`netstat -ano`, and the underlying investigative logic (map a PID or property back to something suspicious) is identical. The bigger shift here is `Where-Object` filtering, which turns manual scrolling-and-squinting (like the Process Monitor tree analysis in the PaperCut CVE-2023-27350 room) into a direct query — a meaningful step up in efficiency for actual triage work.