# Recursive Fuzzing

## Why Recursive Fuzzing?
Complex apps mein **multiple nested directories** ho sakti hain. Manually har level fuzz karna slow hai - recursive fuzzing automate kar deta hai.

## How It Works (3 Steps)

1. **Initial Fuzzing:** Web root (`/`) se shuru, wordlist ke basis pe requests bhejta hai
2. **Directory Discovery & Expansion:** Valid directory mile (e.g. `/admin`) toh usi directory ke andar naya fuzzing round shuru ho jata hai (`/admin/FUZZ`)
3. **Iterative Depth:** Ye process repeat hota hai har naye discovered directory ke liye, jab tak depth limit ya koi naya directory na mile

**Analogy:** Tree structure - root = trunk, har discovered directory = branch, recursive fuzzing har branch explore karta hai.

## Benefits
- **Efficiency:** Manual nested exploration se bahut fast
- **Thoroughness:** Har branch systematically explore hoti hai
- **Reduced effort:** Manually har naya directory input nahi karna padta
- **Scalability:** Large apps ke liye practical

## ffuf Recursive Command

```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt -ic -v -u http://IP:PORT/FUZZ -e .html -recursion
```

| Flag | Purpose |
|---|---|
| `-recursion` | Discovered directories ko automatically recursively fuzz karo |
| `-ic` | Ignore commented lines (`#` se start hone wali) wordlist mein |

**Example output flow:**
level1 discovered (301) -> /level1/FUZZ queued
-> index.html found, level2 discovered, level3 discovered
-> /level1/level2/FUZZ and /level1/level3/FUZZ queued
-> index.html found in both (level3's index.html bigger = interesting)  
## Be Responsible — Rate Limiting

Recursive fuzzing **resource-intensive** hoti hai - server overwhelm ho sakta hai ya security mechanisms trigger ho sakte hain.

| Flag | Purpose |
|---|---|
| `-recursion-depth N` | Max depth limit set karo (e.g. `2` = sirf 2 levels deep) |
| `-rate` | Requests/sec limit karo |
| `-timeout` | Individual request timeout set karo |

**Responsible example:**
```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt -ic -u http://IP:PORT/FUZZ -e .html -recursion -recursion-depth 2 -rate 500
```

## Pentest Relevance
- Recursive fuzzing CTF aur real engagements dono mein **nested hidden content** find karne ka standard tareeka hai
- Rate limiting **zaroori hai production-like/client environments mein** - WAF trigger ya DoS avoid karne ke liye
- Depth limit set karna practical hai - bina limit ke scan kabhi khatam nahi ho sakta bade apps mein

---
*Source: HTB Academy - Fuzzing Web Applications*
