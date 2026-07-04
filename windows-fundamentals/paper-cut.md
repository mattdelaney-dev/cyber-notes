# PaperCut: CVE-2023-27350

Part of my TryHackMe practice — covering the authentication bypass vulnerability in PaperCut MF/NG print management software that leads to remote code execution.

## Topics Covered
- Understanding PaperCut and CVE-2023-27350
- Exploiting CVE-2023-27350
- Detection and Mitigation
- Process Monitor analysis of a compromised host

---

## Understanding PaperCut and CVE-2023-27350

**PaperCut MF/NG** is a widely used print management platform deployed in schools, enterprises, and government environments to track and control printing across an organisation.

**CVE-2023-27350** is an authentication bypass vulnerability in the PaperCut Application Server that allows an unauthenticated attacker to reach administrative functionality.

- Affects the built-in web-based admin console
- Exploited in the wild by ransomware affiliates (notably Cl0p and LockBit-linked actors) shortly after disclosure in 2023
- CVSS score of 9.8 — critical severity
- Root cause: a flaw in the `SecurityRequestFilter` class that improperly handles certain URL paths, letting an attacker bypass login checks and reach the `SetupCompleted` servlet used during initial setup

Once inside the admin console, an attacker can abuse PaperCut's legitimate **"Scripting"** feature (used to run print scripts) to execute arbitrary OS commands under the privileges of the PaperCut service account — typically `SYSTEM` on Windows.

---

## Exploiting CVE-2023-27350

At a high level, the exploitation chain looks like:

1. Access the unauthenticated setup/admin endpoint to bypass login
2. Create a new admin user or reuse the bypass to reach the print scripting configuration
3. Inject a malicious script (e.g. Java-based) into a print script hook
4. Trigger the script execution, which spawns a native OS process (`cmd.exe` / `powershell.exe`) as a child of the PaperCut process
5. Use the resulting shell for further post-exploitation (credential harvesting, lateral movement, ransomware staging)

The key artifact this leaves behind: **a legitimate-looking PaperCut process spawning a command shell**, which it would never do under normal operation.

---

## Detection and Mitigation

### Process Monitor Analysis

Using **Process Monitor's Process Tree** view on an affected server, the PaperCut process branch looked like this:

```
pc-server.exe (2924)
 ├─ pc-app.exe (3628)
 │    ├─ conhost.exe (3696)
 │    └─ cmd.exe (8436)          <-- suspicious
 ├─ svchhost.exe (2976)          <-- suspicious (typosquatted svchost.exe)
 ├─ pc-print.exe (2940)
 └─ pc-web-print.exe (2956)
```

**Findings:**
- `cmd.exe (PID 8436)` was spawned directly by `pc-app.exe` — a print management process has no legitimate reason to launch a command shell. This matches the known exploitation pattern for CVE-2023-27350, where the PaperCut process tree shows a shell spawned as a direct child.
- `svchhost.exe (PID 2976)` is a likely dropped payload masquerading as the legitimate Windows `svchost.exe` (note the extra "h"). Genuine `svchost.exe` should only ever appear as a child of `services.exe`, not `pc-server.exe`.

**Conclusion of the investigation:** PID **8436** was submitted as the process related to PaperCut requiring further investigation — command-line arguments, child processes, and network connections around that timestamp should be pulled next. `svchhost.exe` (2976) was flagged separately as a likely secondary implant.

### Detection Indicators
- PaperCut processes (`pc-server.exe`, `pc-app.exe`) spawning `cmd.exe` or `powershell.exe`
- Unusual outbound network connections from the PaperCut server shortly after such a process spawn
- New/unexpected admin accounts created in the PaperCut admin console
- Modifications to print scripts not tied to a known change request

### Mitigation
- Patch PaperCut MF/NG to a fixed version (20.1.7+, 21.2.11+, 22.0.9+ or later)
- Restrict access to the PaperCut admin console (`:9191`) to trusted management networks only
- Monitor for anomalous child processes of PaperCut services via EDR/Process Monitor
- Disable or tightly control the print script execution feature where not required

---

## Why This Matters

CVE-2023-27350 is a strong real-world example of:
- How a single authentication bypass can cascade into full remote code execution when combined with legitimate "scripting" functionality
- Why process lineage (parent-child relationships) is one of the fastest ways to spot compromise — a print server spawning a shell is never normal
- The value of naming-convention tricks (typosquatting system process names like `svchost.exe`) as a persistence/evasion technique attackers rely on defenders overlooking

---

## Reflection

This room reinforced how important it is to baseline "normal" process behaviour before hunting for anomalies — spotting `cmd.exe` under `pc-app.exe` and the typosquatted `svchhost.exe` came down to comparing the PaperCut branch against expected Windows process trees elsewhere in the same trace. Good practical tie-in to the AD fundamentals module, since privilege escalation via a compromised service account often leads directly into domain-level attacks.