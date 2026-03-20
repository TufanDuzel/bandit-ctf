# Bandit OverTheWire CTF Write-up (Levels 0-33)

This repository contains my personal notes, logic, and command history for solving the **Bandit** wargame from [OverTheWire](https://overthewire.org/wargames/bandit/).

## 📌 Project Overview
The goal of this project is to document my learning journey in Linux fundamentals, privilege escalation, and basic exploitation techniques. Each level focuses on specific security concepts and command-line tools.

## 🧠 What I Learned
Throughout these levels, I gained hands-on experience with:
- [cite_start]**File System Navigation:** Handling hidden files, spaces in filenames, and identifying file types[cite: 9, 11, 14].
- [cite_start]**Data Filtering:** Using `grep`, `sort`, `uniq`, and `strings` to find specific patterns[cite: 26, 29, 33].
- [cite_start]**Cryptography & Encoding:** Decoding Base64, ROT13, and managing RSA private keys[cite: 35, 37, 51].
- [cite_start]**Network Security:** Port scanning with `nmap`, using `nc` (Netcat) for data transfer, and SSL/TLS connections[cite: 46, 49, 53].
- [cite_start]**Privilege Escalation:** Exploiting SUID binaries and analyzing cron jobs[cite: 64, 72].
- [cite_start]**Git Forensics:** Recovering deleted data from commit history, tags, and different branches[cite: 106, 111, 115].
- [cite_start]**Shell Escaping:** Breaking out of restricted shells using `vi` or positional parameters[cite: 91, 124].

## 📂 Structure
The notes follow a consistent format for each level:
- **Goal:** What needs to be achieved.
- **Logic:** The thought process and security concepts behind the solution.
- **Commands:** The exact bash commands used to solve the level.

*Note: For security best practices and to maintain the challenge for others, all actual passwords have been replaced with `[Password Found]`.*

## 🛠️ Tools Used
- [cite_start]SSH [cite: 3]
- [cite_start]Bash Scripting [cite: 85]
- [cite_start]Netcat & Nmap [cite: 46, 52]
- [cite_start]OpenSSL / Ncat --ssl [cite: 51]
- [cite_start]Git [cite: 105]

---
*Feel free to explore the notes and use them as a reference for your own learning!*