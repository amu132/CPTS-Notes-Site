# Directory and File Fuzzing

## Why Hidden Assets Matter
Web apps mein aksar **unlinked** directories/files hote hain jo UI se accessible nahi hain lekin exist karte hain:
- Backup files, config files, logs (sensitive data)
- Outdated/vulnerable script versions
- Dev/staging environments, admin panels
- Undocumented endpoints

Ye discover karna attack surface samajhne ke liye critical hai.

## ffuf Basics

**Core workflow:**
1. Wordlist provide karo
2. URL mein `FUZZ` keyword placeholder use karo
3. ffuf har wordlist entry ko `FUZZ` ki jagah substitute karke request bhejta hai
4. Response analyze karta hai (status code, length) aur filter karta hai

## Directory Fuzzing

```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt -u http://IP:PORT/FUZZ
```

| Flag | Purpose |
|---|---|
| `-w` | Wordlist path |
| `-u` | Target URL (FUZZ = placeholder) |

**Result example:** `w2ksvrus` directory mila (Status 301 = redirect, directory exists hone ka sign)

## File Fuzzing

Directory ke andar specific files dhundo - common extensions ke saath:

| Extension | Meaning |
|---|---|
| `.php` | Server-side PHP code |
| `.html` | Web page structure |
| `.txt` | Plain text (logs, notes) |
| `.bak` | Backup files - **high value target** |
| `.js` | JavaScript |

```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://IP:PORT/w2ksvrus/FUZZ -e .php,.html,.txt,.bak,.js -v
```

| Flag | Purpose |
|---|---|
| `-e` | Extensions list (comma-separated) |
| `-v` | Verbose output (full URL + payload dikhata hai) |

> **High-value find example:** `config.php.bak` discover hona = database credentials/API keys leak ho sakte hain.

## Pentest Relevance
- `.bak`, `.old`, `.swp` jaisi backup file extensions hamesha fuzz karo - developers accidentally chhod dete hain
- 301 status = directory exists (redirect to trailing slash) - important signal
- Directory fuzz karne ke baad, usi directory ke andar file fuzzing zaroor karo - nested hidden assets mil sakte hain

---
*Source: HTB Academy - Fuzzing Web Applications*
