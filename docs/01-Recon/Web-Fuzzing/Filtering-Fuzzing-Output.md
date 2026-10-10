# Filtering Fuzzing Output

## Why Filtering Matters
Fuzzing tools bahut zyada data generate karte hain - bina filtering ke useful results **noise mein dab jaate hain** (especially 404s). Har tool apne filtering flags provide karta hai.

## Gobuster Filters

> Note: `-s` aur `-b` sirf **dir mode** mein available hain.

| Flag | Purpose | Example |
|---|---|---|
| `-s` | Include specific status codes | `-s 301,302,307` (redirects dhundo) |
| `-b` | Exclude specific status codes | `-b 404` |
| `--exclude-length` | Specific content lengths exclude karo | `--exclude-length 0,404` |

```bash
gobuster dir -u http://example.com/ -w wordlist.txt -s 200,301 --exclude-length 0
```

## FFUF Filters

| Flag | Purpose | Example Use |
|---|---|---|
| `-mc` (match code) | Sirf specified status codes include | `-mc 200` |
| `-fc` (filter code) | Specified status codes exclude | `-fc 404` |
| `-fs` (filter size) | Specific size exclude | `-fs 0` (empty responses hide) |
| `-ms` (match size) | Specific size match | `-ms 3456` (exact size dhundo) |
| `-fw` (filter words) | Specific word count exclude | `-fw 219` |
| `-mw` (match words) | Specific word count match | `-mw 5-10` |
| `-fl` (filter lines) | Specific line count exclude | `-fl 10` |
| `-ml` (match lines) | Specific line count match | `-ml 20` |
| `-mt` (match time) | TTFB (response time) condition | `-mt >500` (slow responses - time-based attacks ke liye useful) |

> **Default behavior:** ffuf by default sirf `200-299,301,302,307,401,403,405,500` match karta hai - isliye explicit `-mc`/`-fc` ke bina bhi 404s mostly hide rehte hain.

**Combined examples:**
```bash
# Status 200, specific word count, size > 500 bytes
ffuf -u http://example.com/FUZZ -w wordlist.txt -mc 200 -fw 427 -ms >500

# Common error codes exclude
ffuf -u http://example.com/FUZZ -w wordlist.txt -fc 404,401,302

# Backup files, size range
ffuf -u http://example.com/FUZZ.bak -w wordlist.txt -fs 0-10239 -ms 10240-102400

# Slow endpoints dhundo (time-based attack indicator)
ffuf -u http://example.com/FUZZ -w wordlist.txt -mt >500
```

## wenum Filters

| Flag | Purpose |
|---|---|
| `--hc` (hide code) | Status codes exclude |
| `--sc` (show code) | Status codes include |
| `--hl` / `--sl` | Line count hide/show |
| `--hw` / `--sw` | Word count hide/show |
| `--hs` / `--ss` | Size (bytes) hide/show |
| `--hr` / `--sr` | Regex-based hide/show (response body match) |
| `--filter` / `--hard-filter` | General regex show/hide, hard-filter plugins ko bhi process karne se rokta hai |

```bash
# Success + redirects dikhao
wenum -w wordlist.txt --sc 200,301,302 -u https://example.com/FUZZ

# Common errors hide karo
wenum -w wordlist.txt --hc 404,400,500 -u https://example.com/FUZZ

# Specific keyword wali responses dhundo
wenum -w wordlist.txt --sr "admin\|password" -u https://example.com/FUZZ
```

## Feroxbuster Filters

| Flag | Purpose |
|---|---|
| `--dont-scan` | Specific URLs/patterns scan hi mat karo |
| `-S, --filter-size` | Size se exclude karo |
| `-X, --filter-regex` | Regex match exclude karo (body/headers) |
| `-W, --filter-words` | Word count exclude |
| `-N, --filter-lines` | Line count exclude |
| `-C, --filter-status` | Status codes exclude (denylist) |
| `--filter-similar-to` | Reference page jaisi similar responses exclude |
| `-s, --status-codes` | Sirf specified codes include (allowlist, default: all) |

```bash
feroxbuster --url http://example.com -w wordlist.txt -s 200 -S 10240 -X "error"
```

## Demonstration — Filtering Ka Impact

**Default (filtered) ffuf POST fuzzing:**
```bash
ffuf -u http://IP:PORT/post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -w common.txt -v
```
Default matcher: `200-299,301,302,307,401,403,405,500` - 404s automatically hidden.

**Bina filter ke (`-mc all`):**
```bash
ffuf -u http://IP:PORT/post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -w common.txt -v -mc all
```
Result: **saare 404s bhi dikhne lagte hain** - output overwhelming ho jata hai, actual finding dhundhna mushkil.

## Pentest Relevance
- 🎯 **Filtering = efficiency** - bina iske large wordlists ke results unusable ho jaate hain
- `-mt`/time-based filters **blind SQLi aur time-based attacks** detect karne mein directly useful hain
- Har tool ka apna filtering syntax hai - ek tool se dusre switch karte waqt flags yaad rakhna zaroori
- Default filtering pe blindly trust mat karo - kabhi kabhi interesting response **excluded default range** mein hoti hai (e.g. `500` errors kabhi important info leak karte hain)

---
*Source: HTB Academy - Fuzzing Web Applications*
