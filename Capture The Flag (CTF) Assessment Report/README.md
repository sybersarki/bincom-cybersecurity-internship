# 05 – Capture The Flag: Pickle Rick (TryHackMe)

## Overview

Completed the "Pickle Rick" CTF room on TryHackMe — a beginner-friendly web exploitation challenge themed around the TV show Rick and Morty. The goal was to find 3 hidden flags by exploiting a web application running on a Linux server.

**Room link:** https://tryhackme.com/room/picklerick  
**Status:** ✅ Completed — all 3 flags captured

---

## Objective

> Exploit a vulnerable web application to gain access to a Linux server and retrieve 3 secret ingredients (flags) hidden across the file system.

---

## Tools Used

- Browser DevTools (source code inspection)
- Web application enumeration
- Linux command-line enumeration
- Command injection via web panel

---

## Methodology

### Reconnaissance

Started with manual web enumeration of the target site. Examined the page source code and found a username embedded in an HTML comment — a common developer mistake.

**Finding:** Username discovered in page source → used for login

### Gaining Access

Located the login page and used the discovered credentials to authenticate. Gained access to a command execution panel that allowed running Linux commands on the server.

### Flag 1 – First Ingredient

Used the command panel to list the web server directory and read the first flag file directly.

- **Method:** Directory listing + file read via command panel
- **Screenshot:** `05-flag1-first-ingredient.png`

### Flag 2 – Second Ingredient

Navigated to the `/home/` directory to explore user home folders. Found the second flag in the `rick` user's home directory.

- **Method:** File system enumeration
- **Screenshot:** `06-flag2-second-ingredient.png`

### Flag 3 – Third Ingredient

Escalated to check for elevated privileges. Used `sudo -l` to discover the current user had unrestricted sudo access. Read the final flag from the `/root/` directory.

- **Method:** Privilege check → `sudo` access → root directory access
- **Screenshot:** `07-flag3-third-ingredient.png`

---

## Flags Captured

| Flag | Location | Method |
|------|---------|--------|
| First ingredient | Web root directory | Command panel file read |
| Second ingredient | `/home/rick/` | File system enumeration |
| Third ingredient | `/root/` | Sudo privilege escalation |

---

## Key Takeaway

Three common real-world mistakes led to full server compromise:
1. Credentials left in HTML source code (developer oversight)
2. A command execution panel exposed on the web (dangerous functionality)
3. A web application user with unrestricted `sudo` access (privilege misconfiguration)

Any one of these alone is a serious issue. Combined, they made the server completely trivial to compromise.

---

## Files

```
05-ctf-pickle-rick/
├── ASSIGNMENT 6_ Capture The Flag (CTF) Assessment Report.docx   ← Full writeup
├── flags/
│   └── pickle-rick-flags.docx                                     ← All 3 flags documented
└── screenshots/
    ├── 01-pickle-rick-homepage.png
    ├── 02-username-in-source.png
    ├── 03-login-page.png
    ├── 04-command-panel.png
    ├── 05-flag1-first-ingredient.png
    ├── 06-flag2-second-ingredient.png
    ├── 07-flag3-third-ingredient.png
    └── 08-room-completed.png
```
