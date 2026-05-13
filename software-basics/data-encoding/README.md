# Data Encoding

## Overview

This room introduced how computers encode and store characters using character encoding standards such as ASCII and Unicode.

Topics explored included:
- Character encoding
- ASCII
- Unicode
- UTF encoding standards
- Binary representation of text

## Key Concepts Learned

### Character Encoding

Character encoding allows computers to represent letters, numbers, and symbols using binary values.

Each character is assigned a unique numeric value that computers can store and process.

---

## ASCII

ASCII (American Standard Code for Information Interchange) is one of the earliest character encoding standards.

ASCII uses 7 bits to represent characters, allowing for 128 unique values.

Examples include:
- Letters
- Numbers
- Symbols
- Control characters

Example:

```txt
A = 65
a = 97
```

ASCII is simple and widely supported but limited primarily to English characters.

---

## Unicode

Unicode was created to support characters from many languages and writing systems worldwide.

Unicode allows computers to consistently represent:
- International languages
- Symbols
- Emojis
- Special characters

Unicode solves many limitations of ASCII.

---

## UTF Encoding

UTF (Unicode Transformation Format) is used to encode Unicode characters.

Common UTF standards include:
- UTF-8
- UTF-16
- UTF-32

### UTF-8

UTF-8 is the most commonly used encoding standard on the web because it:
- Supports all Unicode characters
- Is efficient for English text
- Maintains compatibility with ASCII

---

## Binary Representation

Computers ultimately store all text as binary values using combinations of:
- 0
- 1

Character encoding standards define how these binary values map to readable characters.

---

## Why This Matters

Understanding character encoding is important in:
- Programming
- Databases
- Networking
- Web development
- Cyber security

Incorrect encoding can lead to:
- Corrupted text
- Security vulnerabilities
- Data handling issues

---

## Reflection

This room provided foundational knowledge about how computers represent and encode text using standards such as ASCII and Unicode. Understanding encoding systems is important for programming, networking, databases, and cyber security.