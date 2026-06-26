# Cryptography Concepts

## Overview

This room provided an understanding of cryptography and how it is used to protect data in everyday digital encounters.

Topics explored included:
- Why cryptography matters in cyber security
- Plaintext and ciphertext
- Encryption keys and algorithms
- Symmetric encryption
- Asymmetric encryption
- How symmetric and asymmetric encryption work together

## Key Concepts Learned

Cryptography uses mathematical rules and secret keys to scramble information so that only authorised parties can read it. It directly supports two pillars of the CIA Triad: confidentiality and integrity.

---

## Why Cryptography Matters

Without cryptography, data travelling across the internet passes through dozens of routers and systems where it could be intercepted or modified.

Cryptography solves this by:
- Scrambling data so it cannot be read by unauthorised parties
- Detecting whether data has been tampered with in transit
- Ensuring only the intended recipient can access the information

Example:
A medical clinic sending patient records over the internet needs cryptography to prevent those records from being read or altered along the way.

---

## Plaintext and Ciphertext

Plaintext is readable, unencrypted data. Ciphertext is that same data after it has been encrypted and is no longer human-readable.

The transformation from plaintext to ciphertext and back again is controlled by:
- An algorithm — the set of mathematical rules used to encrypt or decrypt
- A key — the secret value that makes the encryption unique

Example:
The message "Hello" in plaintext becomes unreadable gibberish in ciphertext, and can only be restored with the correct key.

---

## Symmetric Encryption

Symmetric encryption uses the same key to both encrypt and decrypt data.

Advantages:
- Fast and efficient
- Suitable for encrypting large amounts of data

Disadvantage:
- Both parties must share the same key, which creates a challenge: how do you securely share the key in the first place?

Analogy:
Symmetric encryption is like a lockbox where both the sender and receiver have a copy of the same key.

---

## Asymmetric Encryption

Asymmetric encryption uses two mathematically linked keys:
- A public key — shared openly, used to encrypt data
- A private key — kept secret, used to decrypt data

Only the private key can decrypt what the public key encrypts.

Advantages:
- Solves the key-sharing problem of symmetric encryption
- Enables secure communication between strangers

Disadvantage:
- Slower than symmetric encryption, so not ideal for large data

Analogy:
Asymmetric encryption is like a mailbox. Anyone can drop a letter in through the slot (encrypt with the public key), but only the owner with the key can open the box and read the mail (decrypt with the private key).

---

## How They Work Together

In practice, both types of encryption are used together to protect web browsing.

The process works like this:
- Asymmetric encryption is used first to securely exchange a shared key
- Symmetric encryption then takes over to encrypt the actual data using that shared key

This gives the security benefits of asymmetric encryption combined with the speed of symmetric encryption.

Example:
When you see a padlock icon in your browser, this combined process is protecting your connection.

---

## Why This Matters

Cryptography is one of the most practical tools in cyber security.

It is used in:
- HTTPS and secure web browsing
- Email encryption
- File and database protection
- Secure messaging applications
- Digital signatures and authentication

Without cryptography, confidentiality and integrity across the internet would be impossible to guarantee.

---

## Reflection

This room demonstrated how cryptography directly supports the CIA Triad by protecting confidentiality and integrity. Understanding the difference between symmetric and asymmetric encryption, and how they complement each other, provides a strong foundation for understanding how secure communications work in practice.