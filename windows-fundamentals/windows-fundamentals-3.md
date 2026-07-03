# Windows Fundamentals 3

Part of my TryHackMe Windows & Active Directory Fundamentals module — covering the built-in Microsoft security tools used to protect and monitor a Windows machine.

## Topics Covered
- Windows Update
- Windows Security overview
- Virus & threat protection
- Firewall & network protection
- App & browser control
- Device security
- BitLocker
- Volume Shadow Copy Service (VSS)

---

## Windows Update

Windows Update keeps the OS and Microsoft software patched against known vulnerabilities.

- Accessed via **Settings → Windows Update**
- Updates are delivered on **Patch Tuesday** (second Tuesday of each month) — critical patches can arrive out-of-cycle
- Delaying or disabling updates is a common risk factor in compromised systems — unpatched machines are a primary attack vector

---

## Windows Security

**Windows Security** is the central dashboard for the built-in security features.

Open with:
```
windowsdefender:
```

Areas covered:
- Virus & threat protection
- Firewall & network protection
- App & browser control
- Device security
- Account protection
- Family options

Each section has its own status indicator so you can see at a glance what's enabled and what needs attention.

---

## Virus & Threat Protection

Handled by **Microsoft Defender Antivirus** — the built-in AV solution.

Key features:
- **Real-time protection** — scans files as they're accessed
- **Cloud-delivered protection** — uses Microsoft's cloud to identify new threats faster
- **Controlled folder access** — blocks unauthorised apps from modifying protected folders (ransomware mitigation)
- **Scan options** — quick, full, custom, and Windows Defender Offline scan

Threat history shows quarantined items and recent detections — useful starting point when investigating a potentially compromised host.

---

## Firewall & Network Protection

Windows Defender Firewall controls inbound and outbound traffic per network profile.

Three profiles:
| Profile | When it applies |
|---|---|
| **Domain** | Machine is joined to a domain |
| **Private** | Trusted home/work network |
| **Public** | Untrusted networks (cafés, airports) |

- Each profile can be independently enabled/disabled and configured with specific rules
- Advanced settings (`wf.msc`) allow granular inbound/outbound rules by port, protocol, and application
- Firewall logs can be enabled to record dropped packets — useful for spotting unexpected traffic

---

## App & Browser Control

Managed by **Microsoft Defender SmartScreen** — protects against malicious apps, files, and websites.

- **Check apps and files** — warns or blocks unrecognised executables downloaded from the internet
- **SmartScreen for Edge** — blocks known phishing sites and malicious downloads in the browser
- **Exploit protection** — system-level and per-app mitigations against memory exploitation techniques (DEP, ASLR, CFG etc.)

Exploit protection settings can be exported as an XML policy — useful in enterprise environments for consistent deployment.

---

## Device Security

Hardware-level security features built into the CPU and firmware.

Key concepts:
- **Core isolation / Memory integrity** — uses virtualisation to protect kernel processes from tampering; prevents injection of malicious code into high-security processes
- **Security processor (TPM)** — a dedicated chip that stores cryptographic keys; required for BitLocker and Windows Hello
- **Secure Boot** — ensures only trusted bootloaders and OS components load at startup, blocking bootkits

---

## BitLocker

**BitLocker** encrypts entire drives so data is unreadable without the correct credentials or recovery key.

- Applied to OS drives, fixed data drives, and removable drives (BitLocker To Go)
- Requires a **TPM chip** for the most seamless experience (keys stored in TPM, unlocks automatically on boot)
- Without TPM, a USB startup key or PIN is required at boot
- Recovery key should be backed up to a Microsoft account, Active Directory, or printed — losing it means losing access to the data

Relevant in security contexts because full-disk encryption prevents data recovery from a stolen or seized drive.

---

## Volume Shadow Copy Service (VSS)

**VSS** coordinates the creation of consistent snapshots (shadow copies) of data while the system is running.

- Used by Windows Backup, System Restore, and third-party backup tools
- Shadow copies allow recovery of previous file versions without a full backup restore
- Managed via **System Protection** settings or `vssadmin` in the command line

```
vssadmin list shadows        # list existing shadow copies
vssadmin delete shadows /all # delete all shadow copies
```

Security relevance: ransomware commonly runs `vssadmin delete shadows` as one of its first actions to prevent victims from recovering files from shadow copies without paying — making VSS awareness important for both defenders and incident responders.

---

## Why This Matters

These tools form the defensive layer built into every modern Windows machine. For security work specifically:
- **Defender & Firewall** — understanding the defaults helps identify when an attacker has disabled or bypassed them
- **BitLocker** — relevant to data protection and forensic investigations
- **VSS** — a key recovery mechanism that attackers actively target during ransomware attacks
- **SmartScreen & Exploit protection** — knowing these exist (and how they're bypassed) is useful context for both red and blue team work

---

## Reflection

Part 3 shifted the focus from system administration tools to the security layer built into Windows. The VSS and ransomware connection was the most practically useful takeaway — it explains a behaviour seen in almost every real ransomware incident. Solid foundation before moving into more offensive-focused Windows content.