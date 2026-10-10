# Fuzzing Commands Cheatsheet

Quick reference - ffuf, gobuster, feroxbuster, wenum ke commonly used commands.

## Install

```bash
go install github.com/ffuf/ffuf/v2@latest
go install github.com/OJ/gobuster/v3@latest
curl -sL https://raw.githubusercontent.com/epi052/feroxbuster/main/install-nix.sh | sudo bash -s $HOME/.local/bin
pipx install git+https://github.com/WebFuzzForge/wenum
pipx runpip wenum install setuptools
```

## Directory Fuzzing

```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt -u http://IP:PORT/FUZZ
```

## File Fuzzing (with extensions)

```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://IP:PORT/DIR/FUZZ -e .php,.html,.txt,.bak,.js -v
```

## Recursive Fuzzing (with rate limiting)

```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt -ic -u http://IP:PORT/FUZZ -e .html -recursion -recursion-depth 2 -rate 500
```

## GET Parameter Fuzzing (wenum)

```bash
wenum -w /usr/share/seclists/Discovery/Web-Content/common.txt --hc 404 -u "http://IP:PORT/get.php?x=FUZZ"
```

## POST Parameter Fuzzing (ffuf)

```bash
ffuf -u http://IP:PORT/post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -w /usr/share/seclists/Discovery/Web-Content/common.txt -mc 200 -v
```

## VHost Fuzzing (Gobuster)

```bash
echo "IP inlanefreight.htb" | sudo tee -a /etc/hosts
gobuster vhost -u http://inlanefreight.htb:81 -w /usr/share/seclists/Discovery/Web-Content/common.txt --append-domain
```

## Subdomain Fuzzing (Gobuster)

```bash
gobuster dns -d inlanefreight.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```
> Newer gobuster: use `--do` or `--domain` instead of `-d` (which now sets delay).

## Useful Flags Quick Reference

| Flag | Tool | Purpose |
|---|---|---|
| `-w` | ffuf/wenum | Wordlist path |
| `-u` | ffuf/wenum | Target URL (FUZZ placeholder) |
| `-e` | ffuf | File extensions |
| `-v` | ffuf | Verbose output |
| `-recursion` | ffuf | Recursive directory fuzzing |
| `-recursion-depth N` | ffuf | Limit recursion depth |
| `-rate N` | ffuf | Requests per second limit |
| `-ic` | ffuf | Ignore commented wordlist lines |
| `-mc` / `-fc` | ffuf | Match/filter status code |
| `-fs` / `-ms` | ffuf | Filter/match response size |
| `-fw` / `-mw` | ffuf | Filter/match word count |
| `-fl` / `-ml` | ffuf | Filter/match line count |
| `-mt` | ffuf | Match response time (TTFB) |
| `-X POST` | ffuf | HTTP method |
| `-d "key=FUZZ"` | ffuf | POST body data |
| `--hc` / `--sc` | wenum | Hide/show status code |
| `--hl` / `--sl` | wenum | Hide/show line count |
| `--hw` / `--sw` | wenum | Hide/show word count |
| `--hs` / `--ss` | wenum | Hide/show size |
| `--hr` / `--sr` | wenum | Hide/show regex match |
| `-s` / `-b` | gobuster (dir mode) | Include/exclude status codes |
| `--exclude-length` | gobuster | Exclude specific content lengths |
| `-S, --filter-size` | feroxbuster | Exclude by size |
| `-X, --filter-regex` | feroxbuster | Exclude by regex match |
| `-C, --filter-status` | feroxbuster | Exclude status codes |
| `-s, --status-codes` | feroxbuster | Include only specified codes |
| `--append-domain` | gobuster vhost | Append base domain to wordlist words |

## Common SecLists Wordlists

| Path | Use |
|---|---|
| `Discovery/Web-Content/common.txt` | General-purpose starting point |
| `Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt` | Deeper directory scan |
| `Discovery/Web-Content/raft-large-directories.txt` | Massive directory list |
| `Discovery/Web-Content/big.txt` | Directories + files combined |
| `Discovery/DNS/subdomains-top1million-5000.txt` | Subdomain enumeration |

---
*Source: HTB Academy - Fuzzing Web Applications*
