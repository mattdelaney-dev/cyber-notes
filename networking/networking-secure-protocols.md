# TryHackMe — Networking Secure Protocols

**Path:** Cyber Security 101 → Networking (4 of 4 — final room in the series)
**Room:** [Networking Secure Protocols](https://tryhackme.com/room/networksecureprotocols)
**Status:** ✅ Completed
**Series:** Networking Concepts → Networking Essentials → Networking Core Protocols → **Networking Secure Protocols (this one)**

---

## Overview

Direct sequel to Networking Core Protocols — that room covered DNS, HTTP, FTP, SMTP, POP3, IMAP all in **plaintext**. This room covers the fix: how TLS, SSH, and VPNs wrap those same protocols in encryption. The three properties being protected, worth having crisp definitions for:

- **Confidentiality** — nobody can read the data in transit (password, credit card, message content)
- **Integrity** — nobody can silently alter the data in transit (e.g. changing a payment amount)
- **Authenticity** — you're actually talking to the real server, not an impersonator

## Task Breakdown

### TLS — Transport Layer Security
Cryptographic protocol operating at the **Transport Layer**, evolved from SSL (TLS 1.3 is current, released 2018). Adding TLS to an existing protocol is what turns HTTP → HTTPS, POP3 → POP3S, SMTP → SMTPS, IMAP → IMAPS — same underlying protocol, wrapped in encryption.

**Certificates:** a server needs a signed TLS certificate to prove its identity, issued by a Certificate Authority (CA) after submitting a Certificate Signing Request (CSR).
- **Trusted certificates** (issued by CAs like Let's Encrypt) — automatically recognized by browsers
- **Self-signed certificates** — not validated by any third party, shouldn't be trusted to confirm authenticity, generally fine only for internal/testing use

### HTTPS — HTTP over TLS
Plain HTTP connection sequence: TCP handshake → HTTP request. HTTPS adds a step in the middle: **TCP handshake → TLS negotiation → HTTP request**. In Wireshark, once TLS is in place, the payload just shows as generic "Application Data" — no way to tell it's HTTP specifically without the decryption key. With the key supplied, the traffic decrypts cleanly back into readable HTTP, GET requests and all — proving the lower/higher layers (TCP, IP, HTTP itself) don't need to change at all; TLS just wraps around them.

### SMTPS, POP3S, IMAPS
Same TLS-wrapping logic applied to the email protocols from the last room:

| Protocol | Plaintext Port | Secure Port |
|---|---|---|
| FTP → FTPS | 21 | 990 |
| Telnet → SSH | 23 | 22 |
| SMTP → SMTPS | 25 | 465 / 587 |
| POP3 → POP3S | 110 | 995 |
| IMAP → IMAPS | 143 | 993 |

### SSH — Secure Shell
Direct replacement for Telnet's remote-login role, but encrypted end-to-end. Runs on **port 22** (vs Telnet's port 23).

Key features:
- **Secure authentication** — password, public-key, or two-factor
- **Confidentiality** — end-to-end encryption, warns on unexpected new server keys (protects against MITM attacks)
- **Integrity** — cryptography protects against tampering, not just eavesdropping
- **Tunneling** — SSH can carry other protocols through it, effectively creating a VPN-like connection
- **X11 Forwarding** — run a remote system's GUI application locally over the SSH connection (`ssh host -X`)

Connect with `ssh username@hostname`, or just `ssh hostname` if your local username matches.

### SFTP vs FTPS — two different ways to secure file transfer
Easy to conflate these since the names look similar, but they're built on completely different foundations:
- **SFTP** (SSH File Transfer Protocol) — part of the SSH suite, port 22
- **FTPS** (File Transfer Protocol Secure) — uses TLS instead, port 990

### VPN — Virtual Private Network
Creates an encrypted tunnel between a client and a VPN server, making remote access feel like being directly on the target network.

- **Encryption** — all traffic between client and server is encrypted
- **Anonymity** — the destination sees the VPN server's IP, not yours; your ISP only sees encrypted traffic (limits their ability to monitor or censor)
- **Not always full-tunnel** — some VPN configs only route traffic to the private network, not all internet traffic; some leak your real IP despite claiming full routing — worth a DNS leak test if it actually matters for the use case
- Legal status varies by country — worth checking local law, especially while travelling

---

## Key Takeaways
- TLS doesn't replace or modify the protocol underneath it — HTTP is still HTTP, just wrapped. This is why "add an S" is enough to describe the whole upgrade (HTTPS, SMTPS, etc.)
- Self-signed certificates are a real authenticity gap — don't trust them the way you'd trust a CA-issued cert
- SSH isn't just "encrypted Telnet" — tunneling and X11 forwarding make it genuinely more capable, not just safer
- SFTP and FTPS solve the same problem via entirely different mechanisms (SSH-based vs TLS-based) — worth not conflating them
- A VPN protects transit confidentiality, but doesn't inherently guarantee full traffic routing or leak-proof anonymity — those need verifying independently, not assumed

## Cross-References
- **Networking Core Protocols room** — this room is the direct "here's the fix" to everything left plaintext and vulnerable there (HTTP, SMTP, POP3, IMAP, and implicitly Telnet from the earlier room too)
- **Networking Concepts room** — the original Telnet-to-port-80 exercise is exactly the traffic this room's HTTPS section shows becoming unreadable "Application Data" once TLS is added
- **Home lab (Kali VM)** — `ssh` and `sftp` are exactly the tools worth using for real once accessing anything remotely, instead of defaulting to plaintext alternatives
- **This closes out the full 4-room Networking series** — Concepts → Essentials → Core Protocols → Secure Protocols gives a complete picture from addressing fundamentals through to how real-world traffic actually gets protected