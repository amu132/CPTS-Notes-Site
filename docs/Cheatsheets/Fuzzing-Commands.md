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
| `-mc 200` | ffuf | Match only specified status code |
| `-X POST` | ffuf | HTTP method |
| `-d "key=FUZZ"` | ffuf | POST body data |
| `--hc 404` | wenum | Hide specified status code |

## Common SecLists Wordlists

| Path | Use |
|---|---|
| `Discovery/Web-Content/common.txt` | General-purpose starting point |
| `Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt` | Deeper directory scan |
| `Discovery/Web-Content/raft-large-directories.txt` | Massive directory list |
| `Discovery/Web-Content/big.txt` | Directories + files combined |

---
*Source: HTB Academy - Fuzzing Web Applications*
