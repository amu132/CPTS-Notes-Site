# Intro to Web Fuzzing & Tooling Setup

## What is Web Fuzzing?
Web fuzzing = automated testing jisme web application ko **unexpected/random data** diya jata hai vulnerabilities uncover karne ke liye.

## Fuzzing vs Brute-forcing

| | Fuzzing | Brute-forcing |
|---|---|---|
| Approach | Wide net - malformed data, invalid chars, nonsensical combos | Targeted - specific value guess karna (password, ID) |
| Goal | App kaise react karti hai unexpected input pe | Correct value find karna trial-and-error se |
| Analogy | Lock pe sab kuch try karna (keys, screwdriver, rubber duck) | Key-ring ki har key try karna |

## Why Fuzz Web Applications?
- **Hidden vulnerabilities** uncover karta hai jo manual testing miss kar sakti hai
- **Automation** - time/resources save
- **Real-world attack simulation** - attacker techniques mimic karta hai
- **Input validation** weaknesses identify karta hai (SQLi/XSS prevention ke liye critical)
- **CI/CD integration** possible - continuous security testing

## Essential Concepts

| Concept | Description | Example |
|---|---|---|
| Wordlist | Potential directory/file/param names ki list | `admin`, `login`, `backup` |
| Payload | Actual data jo bheja jata hai | `' OR 1=1 --` (SQLi) |
| Response Analysis | Status codes/errors analyze karna anomalies ke liye | 500 error with DB message = potential SQLi |
| Fuzzer | Automation tool | ffuf, wfuzz, Burp Intruder |
| False Positive | Galat tarike se vulnerability flag hona | 404 ko vuln samajh lena |
| False Negative | Real vulnerability miss ho jana | Logic flaw jo fuzzer catch nahi kar paya |
| Fuzzing Scope | Target ka specific part jo fuzz kar rahe ho | Sirf login page, ya ek API endpoint |

---

## Tooling Setup

### Prerequisites — Go, Python, pipx

```bash
sudo apt update
sudo apt install -y golang
sudo apt install -y python3 python3-pip
sudo apt install pipx
pipx ensurepath
sudo pipx ensurepath --global

# Verify
go version
python3 --version
```

> `pipx` Python apps ko **isolated virtual environments** mein install karta hai - dependency conflicts avoid karta hai.

### FFUF (Fuzz Faster U Fool)
Go mein likha, **sabse fast** fuzzer - directories, files, params enumerate karne ke liye.

```bash
go install github.com/ffuf/ffuf/v2@latest
```

**Use cases:** Directory/file enumeration, parameter discovery, brute-force attacks

### Gobuster
Dusra popular directory/file fuzzer - speed + simplicity.

```bash
go install github.com/OJ/gobuster/v3@latest
```

**Use cases:** Content discovery, DNS subdomain enumeration, WordPress content detection

### FeroxBuster
Rust mein likha - **recursive** content discovery, "forced browsing" tool (fuzzer se zyada).

```bash
curl -sL https://raw.githubusercontent.com/epi052/feroxbuster/main/install-nix.sh | sudo bash -s $HOME/.local/bin
```

**Use cases:** Recursive scanning, unlinked content discovery, high-performance scans

### wenum (wfuzz fork)
Parameter fuzzing ke liye best - highly customizable.

```bash
pipx install git+https://github.com/WebFuzzForge/wenum
pipx runpip wenum install setuptools
```

> Note: `wfuzz` pre-installed ho sakta hai PwnBox/Kali mein, lekin install issues ki wajah se `wenum` substitute use karo - syntax same hai.

**Use cases:** Directory/file enumeration, parameter discovery, brute-force attacks

## Wordlists - SecLists

Sabse comprehensive wordlist collection: **SecLists** (github.com/danielmiessler/SecLists)

PwnBox pe location: `/usr/share/seclists/` (lowercase)

**Most-used wordlists:**

| Wordlist | Use Case |
|---|---|
| `Discovery/Web-Content/common.txt` | General-purpose, good starting point |
| `Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt` | Deeper directory-focused scan |
| `Discovery/Web-Content/raft-large-directories.txt` | Massive directory collection |
| `Discovery/Web-Content/big.txt` | Directories + files, wide net |

## Pentest Relevance
- Fuzzing = **recon phase ka core technique** - attack surface map karne ke liye zaroori
- Tool choice depends on task: ffuf (speed, general), gobuster (simplicity), feroxbuster (recursive), wenum (parameter-focused)
- SecLists wordlists industry-standard hain - almost har CTF/real engagement mein use hote hain

---
*Source: HTB Academy - Fuzzing Web Applications*
