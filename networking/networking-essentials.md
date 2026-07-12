# TryHackMe — Networking Essentials

**Path:** Cyber Security 101 → Networking (2 of 4)
**Room:** [Networking Essentials](https://tryhackme.com/room/networkingessentials)
**Status:** ✅ Completed
**Series:** Networking Concepts → **Networking Essentials (this one)** → Networking Core Protocols → Networking Secure Protocols
**Prerequisite:** Networking Concepts (OSI/TCP-IP models, IP/subnets, TCP/UDP) — this room assumes that's already solid

---

## Overview

Where Networking Concepts covered the theory (models, addressing, TCP/UDP), this room covers the protocols that actually glue a working network together day-to-day: DHCP, ARP, ICMP (ping/traceroute), routing protocols, and NAT. These are the "why does my laptop just work when I connect to a new WiFi" protocols.

## Task Breakdown

### DHCP — Dynamic Host Configuration Protocol
Automates network config (IP address, subnet mask, gateway, DNS) so devices don't need manual setup every time they connect to a network. Runs as a 4-step exchange, remembered by the acronym **DORA**:

1. **D**iscover — client broadcasts to `255.255.255.255` looking for any DHCP server (client itself has no IP yet, so it sources from `0.0.0.0`)
2. **O**ffer — server responds with an available IP + settings
3. **R**equest — client formally asks to use that offered IP
4. **A**cknowledge — server confirms, assignment is final

Visible clearly in a packet capture via `tshark -r file.pcap -n` — same tool logic as `tcpdump`, just a different (often more readable) output format.

### ARP — Address Resolution Protocol
Bridges Layer 3 (IP address) to Layer 2 (MAC address) — needed because a device on the same local network can't build an Ethernet frame without knowing the destination's MAC address, even if it already knows the IP.

- **ARP Request** — broadcast to `ff:ff:ff:ff:ff:ff` ("who has this IP? tell me")
- **ARP Reply** — the device holding that IP responds directly with its MAC address

Notable detail: ARP isn't wrapped in IP or UDP at all — it's encapsulated directly inside the Ethernet frame, one layer lower than most other traffic you'll analyze.

### ICMP — Ping & Traceroute
ICMP exists for network diagnostics, not general data transfer:

- **Ping** — sends an ICMP Echo Request (Type 8), waits for an Echo Reply (Type 0). No reply doesn't always mean the host is down — a firewall blocking ICMP along the way produces the same silence.
- **Traceroute** — exploits the IP header's **TTL (Time-to-Live)** field. Each router decrements TTL by 1; when it hits zero, that router drops the packet and sends back an ICMP Time Exceeded (Type 11). Traceroute deliberately sends packets with increasing TTL values (1, 2, 3...) so each router along the path reveals itself one at a time.

### Routing Protocols
How routers decide the best path for a packet:

| Protocol | Notes |
|---|---|
| OSPF | Open standard, calculates shortest path via network topology |
| EIGRP | Cisco proprietary, similar goal, vendor-specific |
| BGP | The actual backbone protocol of the internet — how ISPs exchange routing info with each other |
| RIP | Simple, hop-count based, mostly small/legacy networks |

### NAT — Network Address Translation
Lets many devices on a private network share one public IP. The router keeps a translation table mapping internal (private IP + port) to external (public IP + port) for every active connection, rewriting packets on the way out and back in. This is the mechanism that lets an entire household or office sit behind a single ISP-assigned IP.

---

## Key Takeaways
- **DORA** (Discover, Offer, Request, Acknowledge) — the DHCP handshake, worth having memorised
- ARP operates below IP entirely — a genuinely different layer than most protocols you'll be analyzing
- TTL isn't just a routing detail — it's the entire mechanism traceroute exploits to map a path
- NAT is why "how many devices can share one public IP" is really a question about available port numbers, not IP addresses
- This room is the connective tissue — DHCP gets you an address, ARP finds your neighbour, NAT lets you share a public IP, ICMP tells you if any of it is actually working

## Cross-References
- **Networking Concepts (room 1)** — this room assumes you're already solid on OSI/TCP-IP layers and TCP/UDP; ARP and NAT specifically only make sense once IP addressing already clicked
- **Linux Shells / Windows Command Line / PowerShell rooms** — `ping` and `traceroute` are exactly the kind of command-line tools those rooms were building fluency toward
- **Home lab (Kali VM)** — worth actually running `ping`, `traceroute`, and capturing an ARP exchange with `tcpdump`/`tshark` for real once the VM's set up, rather than just reading about it
- **Next in series:** Networking Core Protocols