# TryHackMe — Networking Concepts

**Path:** Cyber Security 101 → Networking (1 of 4)
**Room:** [Networking Concepts](https://tryhackme.com/room/networkingconcepts)
**Status:** ✅ Completed
**Series:** Networking Concepts → Networking Essentials → Networking Core Protocols → Networking Secure Protocols

---

## Overview

First of a four-room networking series. This one's the conceptual foundation — the OSI and TCP/IP models, IP addressing and subnetting, TCP vs UDP, encapsulation, and a hands-on Telnet exercise. Everything after this room builds on these fundamentals.

## Task Breakdown

### OSI Model
Seven-layer conceptual framework for how network communication should work:

| Layer | Function |
|---|---|
| 7 — Application | Direct user-facing protocols (HTTP, FTP) |
| 6 — Presentation | Formatting, encryption, compression |
| 5 — Session | Manages sessions between applications |
| 4 — Transport | Reliable/unreliable delivery (TCP/UDP) |
| 3 — Network | Routing across networks (IP) |
| 2 — Data Link | Error-free transfer within the same network segment |
| 1 — Physical | Raw hardware transmission (cables, switches) |

Theoretical model, but the mental scaffolding is genuinely useful — most real troubleshooting ("is this a cable issue or a routing issue?") maps onto figuring out which layer broke.

### TCP/IP Model
The practical, four-layer version actually used to describe internet protocols — Link, Internet, Transport, Application. The TCP/IP "Application" layer collapses OSI's top three layers (Session, Presentation, Application) into one.

### IP Addresses & Subnets
- IPv4 = 32-bit address, written as 4 octets (e.g. `192.168.0.1`)
- Subnet mask (e.g. `255.255.255.0`, aka `/24`) splits an IP into network portion + host portion
- `/24` means the leftmost 24 bits are fixed across the subnet — leaves the last octet (0-255) for host addresses, with the first (`.0`) and last (`.255`) reserved as network/broadcast addresses
- Checking your own config: `ipconfig` (Windows) or `ifconfig` / `ip a s` (Linux) — genuinely useful now that the Kali VM is set up, since this is a command you'll run constantly

### Private IP Ranges (RFC 1918) — worth memorising cold
- `10.0.0.0 – 10.255.255.255`
- `172.16.0.0 – 172.31.255.255`
- `192.168.0.0 – 192.168.255.255`

Anything outside these three ranges on a network you're looking at is a public-facing address — a distinction that matters constantly once you're reading pcaps or investigating alerts.

### Routing
A router operates at Layer 3 — reads the destination IP, forwards the packet toward the network that gets it closer to its destination. Same logic as a postal system routing mail toward the right regional hub before final delivery.

### TCP vs UDP
- **TCP** — reliable, ordered, connection-based. Establishes a session via the **three-way handshake**: SYN → SYN-ACK → ACK.
- **UDP** — fast, connectionless, no delivery guarantee. Used where speed matters more than reliability (video streaming, DNS lookups).
- Port numbers range 1–65535 (two-byte field, port 0 reserved).

### Encapsulation
Data gets wrapped in headers at each layer on the way down, unwrapped on the way up at the receiving end:

`Application data` → `+ TCP/UDP header = Segment/Datagram` → `+ IP header = Packet` → `+ Ethernet/WiFi header+trailer = Frame`

### Telnet
Legacy plaintext remote-access tool, mostly replaced by SSH but still useful for manually poking at a TCP service to see what's actually happening on the wire. The room has you connect to an echo service (port 7), a daytime service (port 13), and manually issue a raw HTTP request (`GET / HTTP/1.1`) against a web server on port 80 — good exercise for seeing HTTP as it actually is on the wire, not through a browser abstracting it away.

---

## Key Takeaways
- OSI is the theory, TCP/IP is the practice — know both, but TCP/IP is what you'll actually reference day-to-day
- Private IP ranges need to be second nature — comes up constantly once reading logs/pcaps for real
- The three-way handshake (SYN/SYN-ACK/ACK) is foundational — TCP-based attacks and anomalies (SYN floods, scanning) all reference this baseline behaviour
- Manually talking to a raw TCP port via Telnet strips away the abstraction of a browser/app — worth doing once so HTTP stops being a black box

## Cross-References
- **Windows Command Line / PowerShell / Linux Shells rooms** — this room is where the "why" behind a lot of those command outputs (`ipconfig`, `ifconfig`) actually gets explained
- **Home lab (Kali VM)** — `ip a s` and manual Telnet connections are genuinely worth running for real in the VM rather than just reading about them, now that the setup's done
- **Next in series:** Networking Essentials — builds directly on this room's model before moving into core and secure protocols