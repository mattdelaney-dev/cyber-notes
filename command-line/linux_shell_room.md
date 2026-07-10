# Linux Shells

Part of my TryHackMe practice (Cyber Security 101 path) — covering how to interact with a Linux shell, the different shell types available, and the fundamentals of shell scripting (variables, loops, conditionals, comments). Foundational skills for automating repetitive tasks and understanding what a script is actually doing when I run into one during an investigation or CTF.

## Topics Covered
- Interacting with a Shell
- Types of Linux Shells
- Shell Scripting Components
- The Locker Script (applied exercise)
- Practical Exercise (log searching)

---

## Interacting with a Shell

Core navigation and search commands used to move around and inspect a filesystem from the CLI.

- `pwd` — print working directory, shows where you currently are
- `cd` — change directory
- `ls` — list directory contents
- `cat` — display a file's contents
- `grep` — search for a word or pattern inside a file (e.g. `grep THM dictionary.txt` pulls out the matching line)

**Why it matters:** `grep` is the one that scales — `cat`-ing a huge file to eyeball it doesn't work once you're dealing with logs or wordlists, but `grep <pattern> <file>` finds the needle instantly. This is the same instinct as `netstat -ano` → `tasklist /svc` on the Windows side: narrow a big pile of output down to the one line that matters.

---

## Types of Linux Shells

Different shells trade off scripting power, customization, and beginner-friendliness.

- `echo $SHELL` — shows the shell currently in use
- `cat /etc/shells` — lists every shell installed on the system
- `chsh -s /usr/bin/zsh` — permanently changes the default shell

Compared three shells:

| | Bash | Fish | Zsh |
|---|---|---|---|
| Default on most distros | Yes | No | No |
| Scripting | Strong, widely documented | Limited | Strong, extended |
| Tab completion | Basic | Advanced (history-based) | Advanced (plugin-extended) |
| Syntax highlighting | No | Built-in | Via plugins |
| Customization | Basic | Good | Extensive (oh-my-zsh) |

**Why it matters:** Bash being the default almost everywhere is exactly why it's worth knowing cold — on someone else's box or a stripped-down server, Bash (or `sh`) is usually all that's guaranteed to exist. Fish/Zsh are nice daily-driver upgrades but not something to assume during triage.

---

## Shell Scripting Components

The building blocks used to turn a series of manual commands into a repeatable script.

- **Shebang** (`#!/bin/bash`) — first line of a script, tells the OS which interpreter to run it with
- **Variables** — store and reuse values (`read name` takes input into a variable, `$name` recalls it)
- **Loops** — repeat a block of code (`for i in {1..10}; do ... done`)
- **Conditionals** — branch logic based on a test (`if [ "$name" = "Stewart" ]; then ... else ... fi`)
- **Comments** (`# text`) — non-executing notes for readability
- `chmod +x script.sh` — grants execute permission
- `./script.sh` — runs a script in the current directory (the `./` is required so the shell doesn't go hunting through `$PATH` instead)

**Why it matters:** This is the same logic used in every malicious shell script I'd need to read during an investigation — a suspicious `.sh` file dropped on a compromised host is just variables, a loop, and a conditional wearing a disguise. Being able to read shebang → variables → logic quickly means I can tell what a script does without having to run it.

---

## The Locker Script

Applied exercise combining all three components above into one script: a mock authentication check that loops through three prompts (username, company, PIN), stores each answer in a variable, then uses a chained conditional (`&&`) to check all three against known-good values before granting or denying access.

```bash
#!/bin/bash
username=""
companyname=""
pin=""

for i in {1..3}; do
    if [ "$i" -eq 1 ]; then
        echo "Enter your Username:"
        read username
    elif [ "$i" -eq 2 ]; then
        echo "Enter your Company name:"
        read companyname
    else
        echo "Enter your PIN:"
        read pin
    fi
done

if [ "$username" = "John" ] && [ "$companyname" = "Tryhackme" ] && [ "$pin" = "7385" ]; then
    echo "Authentication Successful. You can now access your locker, John."
else
    echo "Authentication Denied!!"
fi
```

**Why it matters:** The loop-driving-a-conditional pattern here (collect N inputs via loop, then gate access on all of them matching) is a template worth recognizing — it shows up constantly in both legitimate auth scripts and in scripts designed to look like one.

---

## Practical Exercise

Given a pre-placed script on `/home/user` designed to search all `.log` files in a target directory (`/var/log`) for a specific flag string, with the search parameters left blank inside the script (empty `" "` fields) for me to fill in before running it.

- `sudo su` — escalate to root, needed here to read across all files in `/var/log` regardless of ownership/permissions
- Filled in the flag string and target directory inside the script, then ran it to search `.log` files and locate the matching entry

**Why it matters:** This tied the scripting task directly to something practical — instead of writing a script from scratch, I had to *read* an existing one, understand what the blank fields were for, and correctly modify it to do what I needed. That's closer to real-world scripting work than writing one greenfield.

---

## Why This Matters

This room moved from "typing individual commands" to "chaining them into logic" — the natural next step after Linux Fundamentals. The Locker Script tied variables, loops, and conditionals together in one place, and the practical exercise forced me to read someone else's script and adapt it rather than just writing my own from a blank file, which is the more realistic skill.

## Reflection

Good structural companion to the Windows Command Line room — that one was about querying system state fast (`systeminfo`, `netstat -ano`, `tasklist /svc`), this one is about understanding *logic*, not just commands. The clearest tie-in going forward: any time I hit a dropped `.sh` script during a Linux-side investigation, the shebang → variables → loop/conditional structure from this room is exactly what I'll be parsing to figure out what it actually does before deciding whether to run it.