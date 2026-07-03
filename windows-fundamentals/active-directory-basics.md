# Active Directory Basics

Part of my TryHackMe Windows & Active Directory Fundamentals module — covering the core concepts of Active Directory, how domains are structured, and how to manage users, computers, and policies in a Windows domain environment.

## Topics Covered
- Windows Domains
- Active Directory structure
- Managing Users in AD
- Managing Computers in AD
- Group Policies
- Authentication Methods
- Trees, Forests and Trusts

---

## Windows Domains

A **Windows Domain** centralises the administration of a network's computers and users under a single management point.

- All configuration and identity is managed through a **Domain Controller (DC)**
- Users log in with domain credentials that are authenticated by the DC — not the local machine
- Allows IT to manage hundreds of machines from one place

Key benefit: instead of configuring each machine individually, policies and permissions are pushed from the DC to all machines in the domain.

---

## Active Directory Structure

**Active Directory (AD)** is the directory service running on a Domain Controller that stores information about all objects in the domain.

Common AD objects:
| Object | Description |
|---|---|
| **Users** | People or services that need access to the domain |
| **Computers** | Machines joined to the domain |
| **Groups** | Collections of users/computers for easier permission management |
| **OUs** | Organisational Units — folders used to organise objects and apply policies |

AD is queried and managed via **Active Directory Users and Computers** (`dsa.msc`).

---

## Managing Users in AD

### Organisational Units (OUs)
OUs are used to organise users and computers logically — usually mirroring the company's structure (e.g. Sales, IT, Management).

- By default OUs are **protected against accidental deletion** — to delete one you must first disable that protection via View → Advanced Features → Properties → Object tab
- Deleting an OU also deletes everything inside it

### Delegation
Delegation allows you to grant a specific user control over an OU without making them a Domain Admin.

Common use case: giving IT support the ability to reset passwords for users in their department.

To delegate control:
1. Right-click the OU → **Delegate Control**
2. Select the user to delegate to
3. Choose the specific tasks to delegate (e.g. reset passwords)

Resetting a password via PowerShell (as a delegated user):
```powershell
Set-ADAccountPassword <username> -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose

# Force password change at next logon
Set-ADUser -ChangePasswordAtLogon $true -Identity <username> -Verbose

# Disable forced change (needed before RDP login)
Set-ADUser -ChangePasswordAtLogon $false -Identity <username> -Verbose
```

---

## Managing Computers in AD

By default all domain-joined machines land in the **Computers** container. Best practice is to move them into organised OUs so different policies can be applied to each group.

Common split:
- **Workstations** — desktops and laptops used by regular users (LPT-*, PC-*)
- **Servers** — machines providing services (SRV-*)
- **Domain Controllers** — already in their own OU created by Windows

To move computers: right-click → **Move** → select destination OU. Multiple machines can be selected and moved at once.

---

## Group Policies

**Group Policy Objects (GPOs)** are collections of settings that can be applied to OUs, pushing configuration to all users and computers inside them.

- Managed via **Group Policy Management** (`gpmc.msc`)
- GPOs are linked to OUs and apply to everything inside that OU
- Settings can control: password policies, software installation, desktop lockdown, firewall rules, mapped drives, scripts and more
- GPOs are processed in order: Local → Site → Domain → OU (LSDOU)

---

## Authentication Methods

AD supports two main authentication protocols:

### Kerberos (default)
- Used by modern Windows domains
- Ticket-based: users authenticate once and receive a **Ticket Granting Ticket (TGT)** from the DC
- Services are accessed using **Service Tickets** issued by the **Key Distribution Centre (KDC)** on the DC
- Passwords are never sent over the network

### NTLM (legacy)
- Challenge-response protocol — still used as a fallback
- Less secure than Kerberos and a common target in attacks (Pass-the-Hash, NTLM relay)
- Still present in most environments for compatibility

---

## Trees, Forests and Trusts

### Trees
Multiple domains sharing the same namespace can be joined into a **tree**.
- Example: `thm.local` → `uk.thm.local`, `us.thm.local`
- Parent and child domains automatically have a **two-way trust**

### Forests
Multiple trees with different namespaces joined together form a **forest** — the top-level boundary of an AD environment.

### Trusts
Trusts define whether users in one domain can access resources in another.
- **One-way trust** — users in the trusted domain can access resources in the trusting domain, not vice versa
- **Two-way trust** — users in both domains can access each other's resources
- Trusts do not automatically grant permissions — they only open the possibility of access

---

## Why This Matters

Active Directory is the backbone of almost every enterprise Windows environment. From a security perspective:
- **Misconfigurations in delegation** can allow privilege escalation
- **NTLM** is actively exploited in lateral movement and credential attacks
- **GPOs** are abused by attackers to push malicious scripts or configurations domain-wide
- **Trusts** can be leveraged to move between domains in a forest during an attack

Understanding AD structure is foundational for both blue team defence and red team enumeration.

---

## Reflection

This room gave a solid practical introduction to AD — creating and managing OUs, delegating password reset rights, organising computers, and understanding how Kerberos vs NTLM authentication works. The delegation task (resetting Sophie's password as Phillip via PowerShell) and the GPO concepts in particular are directly relevant to real-world enterprise environments and attack paths covered in later TryHackMe content.