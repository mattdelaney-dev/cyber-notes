# TryHackMe — Networking Core Protocols

**Path:** Cyber Security 101 → Networking (3 of 4)
**Room:** [Networking Core Protocols](https://tryhackme.com/room/networkingcoreprotocols)
**Status:** ✅ Completed
**Series:** Networking Concepts → Networking Essentials → **Networking Core Protocols (this one)** → Networking Secure Protocols
**Prerequisite:** OSI/TCP-IP models, Ethernet/IP/TCP basics — same baseline as the last two rooms

---

## Overview

Where the last two rooms covered addressing and the glue-protocols (DHCP/ARP/NAT), this one covers the actual **application-layer protocols people use every day**, unencrypted and in their raw text form: DNS, WHOIS, HTTP, FTP, SMTP, POP3, IMAP. The theme running through the whole room: manually talking to each of these over `telnet` to see exactly what a browser/email client is doing under the hood — no abstraction, just the raw commands.

## Task Breakdown

### DNS — Domain Name System
Maps domain names to IP addresses. Runs at Layer 7 (Application), typically over **UDP port 53**.

| Record | Purpose |
|---|---|
| A | Domain → IPv4 address |
| AAAA | Domain → IPv6 address |
| CNAME | Domain → another domain (alias) |
| MX | Domain → mail server |

`nslookup domain.com` from the command line queries this directly. A single lookup for both IPv4 and IPv6 generates 4 packets — A query, A response, AAAA query, AAAA response — visible cleanly in `tshark`.

### WHOIS — Domain Registration Lookup
`whois domain.com` returns who registered a domain, when, and when it expires — registrant name, contact info, registrar, creation/expiry dates. Genuinely useful for early-stage recon on an investigation, though privacy-protection services (like "Domains By Proxy") mean a lot of real registrant info gets masked in practice.

### HTTP(S) — The Web
- **HTTP** — TCP port 80. **HTTPS** — TCP port 443 (adds encryption).
- Core methods: **GET** (retrieve), **POST** (submit new data), **PUT** (create/update), **DELETE** (remove).
- Same trick as the Networking Concepts room's Telnet exercise: manually sending `GET /path HTTP/1.1` + `Host: anything` over a raw TCP connection gets you a page without a browser — useful for troubleshooting since you're "talking HTTP" directly rather than through an abstraction layer.

### FTP — File Transfer Protocol
TCP port 21 for control; data transfer happens over a **separate connection**. Core commands: `USER`, `PASS`, `RETR` (download), `STOR` (upload). Anonymous FTP (`USER anonymous`, blank password) is still common enough to be worth checking on any target. Client commands don't always match what's sent on the wire — e.g. typing `ls` locally sends `LIST` to the server.

### SMTP — Sending Email
TCP port 25. Sequence: `HELO`/`EHLO` (identify) → `MAIL FROM` → `RCPT TO` → `DATA` (message body, ends with a lone `.` on its own line) → `QUIT`. Manually sending an email via `telnet MACHINE_IP 25` and typing these commands by hand is the clearest way to actually understand what an email client automates for you.

### POP3 — Receiving Email
TCP port 110. Downloads mail **and typically removes it from the server** once retrieved. Commands: `USER`, `PASS`, `LIST` (show messages + sizes), `RETR <n>` (fetch one), `DELE <n>` (mark for deletion), `QUIT`.

Important security point this room drives home: POP3 over plain `telnet` sends credentials and message content **completely unencrypted** — anyone capturing the traffic can read the password and the email itself in plaintext.

### IMAP — Synchronising Email
TCP port 143. Unlike POP3, mail **stays on the server**, syncing across multiple devices instead of downloading-and-removing. Commands: `LOGIN`, `SELECT <mailbox>`, `FETCH <n>` (retrieve a message), `MOVE`, `LOGOUT`.

### Port Number Summary (worth memorising)

| Protocol | Port | Transport |
|---|---|---|
| DNS | 53 | UDP |
| HTTP | 80 | TCP |
| HTTPS | 443 | TCP |
| FTP | 21 | TCP |
| SMTP | 25 | TCP |
| POP3 | 110 | TCP |
| IMAP | 143 | TCP |

---

## Key Takeaways
- Every one of these protocols (except DNS) is plain, human-readable text over `telnet` — genuinely worth doing once, since it strips away the GUI and shows exactly what's happening on the wire
- POP3/IMAP/FTP/SMTP send credentials in **plaintext** by default — this is exactly the gap the next room (Secure Protocols) exists to fix
- Port numbers for this whole set are foundational enough that they need to be instant recall, not something you look up mid-investigation
- WHOIS is a genuinely useful early recon step, with the caveat that privacy-proxy registrations limit what it actually reveals

## Cross-References
- **Networking Concepts room** — reuses that room's exact Telnet-to-port-80 technique, extends it to SMTP/POP3/IMAP/FTP
- **Networking Essentials room** — DNS here is the Application-layer counterpart to that room's DHCP (both are "how does this just work automatically" protocols)
- **Linux Shells / Command Line rooms** — `nslookup`, `whois`, `ftp`, `telnet` are exactly the CLI fluency those rooms were building toward
- **Next in series:** Networking Secure Protocols — the encrypted replacements for everything plaintext in this room (HTTPS, SFTP, SMTPS, IMAPS, etc.)