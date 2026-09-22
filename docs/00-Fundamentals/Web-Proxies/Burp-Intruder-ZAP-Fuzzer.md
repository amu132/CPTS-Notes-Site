# Web Fuzzing — Burp Intruder & ZAP Fuzzer

## Overview
Burp aur ZAP dono ke paas built-in **web fuzzers** hain jo directories, parameters, sub-domains, values fuzz/brute-force kar sakte hain — CLI tools (ffuf, gobuster, dirbuster, wfuzz) ka GUI alternative.

| Tool | Speed (Free/Community) | Speed (Paid) |
|---|---|---|
| Burp Intruder | 1 request/sec (bahut slow) | Unlimited |
| ZAP Fuzzer | Throttled nahi - full speed | N/A (fully free) |

> Burp free version sirf **short queries** ke liye practical hai. Bade wordlists ke liye ZAP Fuzzer better hai kyunki wo free mein bhi fast hai.

---

## Burp Intruder

### Setup Flow
1. Proxy History mein target request locate karo
2. Right-click - **Send to Intruder** (shortcut: `Ctrl+I`)
3. Intruder tab kholo (shortcut: `Ctrl+Shift+I`)

### Positions
Payload position wahi hota hai jahan wordlist ke words iterate honge. Directory fuzzing ke liye:
`DIRECTORY` ko select karke `§` markers se wrap karo (ya "Add §" button use karo).

> Request ke end mein extra 2 blank lines chhodni zaroori hai, warna server error de sakta hai.

### Payloads (4 configs)

**1. Payload Position & Type:**
- Attack type = **Sniper** (single position) ke liye Payload Set 1 select karo
- Payload Types: Simple List (basic wordlist), Runtime File (bade wordlists ke liye - memory-efficient), Character Substitution (permutations), aur bhi options

**2. Payload Configuration:**
- Wordlist load karo (e.g. `/opt/useful/seclists/Discovery/Web-Content/common.txt`)
- Manually items add bhi kar sakte ho (`Add` button)
- Multiple wordlists combine ho sakti hain

**3. Payload Processing:**
- Rules add kar sakte ho (e.g. extension add karna, ya filter karna)
- Example: Lines jo `.` se start hoti hain skip karo - "Skip if matches regex" - pattern: `^\..*$`

**4. Payload Encoding:**
- URL-encoding on/off toggle kar sakte ho special characters ke liye

### Settings Tab
- Retries on network failure = 0 (recommended)
- **Grep - Match**: specific response pattern flag karo (e.g. sirf `200 OK` wale results highlight karo)
- **Grep - Extract**: lambi responses mein se sirf specific part dikhata hai
- **Exclude HTTP Headers**: agar match value header mein hai toh isse disable karo

### Attack & Results
`Start Attack` click karo - results table mein Status/Length/200 OK column se sort karke matches dhundo.

**Example result:** `/admin/` payload ne **200 OK** diya (baaki sab 404) - matlab wo directory exist karti hai. Manually visit karke confirm karo.

### Other Use Cases
- Password brute-forcing
- PHP parameter fuzzing
- **Password spraying against AD authentication** (OWA, SSL VPN, RDS, Citrix, custom AD-integrated apps)

---

## ZAP Fuzzer

Burp Intruder se **speed throttling nahi hai** — free version mein bhi full speed. Feature-wise thoda kam advanced hai lekin practical use ke liye kaafi hai.

### Setup Flow
1. Target request proxy history mein locate karo
2. Right-click - **Attack - Fuzz** - Fuzzer window khulega

### Locations
Word select karo jahan payload jaana hai, **Add** button click karo - green marker lag jayega us position pe.

### Payloads (8 types available)

| Type | Description |
|---|---|
| File | Custom wordlist file load karo |
| File Fuzzers | ZAP ke **built-in wordlists** (e.g. dirbuster's `directory-list-1.0.txt`) |
| Numberzz | Number sequences generate karta hai custom increments ke saath |

> ZAP ka advantage: built-in wordlists already available hain, apni khud ki file dene ki zarurat nahi. ZAP Marketplace se aur wordlists install bhi ho sakti hain.

### Processors (Payload Processing)
Har payload pe apply hone wali transformations:
- Base64 Encode/Decode
- MD5/SHA-1/256/512 Hash
- Prefix/Postfix String
- URL Encode/Decode
- Custom Script

> **Tip:** URL Encode processor use karo taaki special characters wale payloads se server error na aaye. "Generate Preview" se final payload confirm kar sakte ho apply karne se pehle.

### Options
- **Concurrent Scanning Threads**: e.g. 20 (jitna zyada, utna fast - server capacity aur apni processing power dono consider karo)
- **Depth First**: ek position ke saare payloads try karo pehle, phir agli position pe jao (e.g. ek user ke saare passwords try karo, phir next user)
- **Breadth First**: har payload ko saari positions pe try karo, phir next payload (e.g. ek password saare users pe try karo, phir next password)

### Start & Results
`Start Fuzzer` - results ko **Response code** se sort karo (200 OK dhundo).

**Example result:** `/skills/` payload ne 200 OK diya - directory exists confirmed via response details (Set-Cookie header, HTML content mila).

**Other useful indicators:**
- `Size Resp. Body` - agar size different hai baaki responses se, toh potentially different/unique page mil sakta hai
- `RTT` (Round-Trip Time) - time-based attacks (jaise time-based SQLi) detect karne ke liye useful - response delay dikhata hai

## Pentest Relevance
- Directory/file enumeration - hidden admin panels, backup files, config files dhundhne ka core technique
- Parameter fuzzing - hidden/undocumented parameters discover karna
- AD password spraying - Intruder se directly corporate auth portals target kiye ja sakte hain
- ZAP Fuzzer free mein full-speed hone ki wajah se **budget-conscious testing/bug bounty** ke liye zyada practical hai
- Depth-first vs Breadth-first strategy samajhna important hai - lockout policies wale targets pe Breadth-first zyada safe hota hai (ek user pe multiple attempts se account lock hone ka risk kam)

---
*Source: HTB Academy - Using Web Proxies*
