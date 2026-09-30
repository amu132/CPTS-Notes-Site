# Burp Scanner (Crawler, Passive & Active Scanning)

## Overview
Burp Scanner ek **Pro-only feature** hai - Community (free) version mein available nahi hai. Ye enterprise-grade automated vulnerability scanner hai jo **Crawler** (site structure map karta hai) aur **Scanner** (passive + active vulnerability detection) combine karta hai.

## Target Scope

Scan start karne ke 3 tareeke:
1. Proxy History se specific request pe right-click - Scan/Passive Scan/Active Scan
2. Dashboard - **New Scan** button - custom targets set karo
3. **Scope** mein defined items pe scan chalao

### Scope Setup Flow
1. `Target - Site map` mein saare detected directories/files dikhte hain
2. Right-click kisi item pe - **Add to scope**
3. Pehli baar item add karte waqt Burp puchega ki sirf in-scope items pe hi focus kare (recommended - resources save hote hain)
4. Kuch items **exclude** bhi kar sakte ho scope se (e.g. logout function - scan karne se session end ho sakta hai) - right-click - **Remove from scope**
5. Poori scope details: `Target - Scope` - advanced regex-based include/exclude bhi possible hai

> ⚠️ **Important:** Dangerous functionality (logout, delete actions) scope se exclude karo, warna scan session break kar sakta hai ya unintended damage ho sakta hai.

## Crawler

Web Crawler links follow karta hai, forms access karta hai, requests examine karta hai - **comprehensive site map** banata hai.

**2 scan options:**
- **Crawl** - sirf mapping (links follow karke structure banata hai)
- **Crawl and Audit** - mapping + vulnerability scanning dono

> **Limitation:** Crawl sirf **referenced links** follow karta hai - unreferenced/hidden pages (jo kahin link nahi hain) discover nahi karega. Uske liye Burp Intruder ya Content Discovery use karo.

### Crawl Configuration
- `Scan configuration - New` (custom) ya `Select from library` (presets, e.g. "Crawl strategy - fastest")
- **Application login**: agar authenticated scan chahiye, credentials provide kar sakte ho, ya manual login record karke Burp ko login steps sikha sakte ho
  - Authenticated scan zyada coverage deta hai (wo areas bhi scan hote hain jo login ke baad hi accessible hain)

**Progress track karo:** Dashboard - Tasks pane mein live status dikhta hai (requests sent, errors, locations crawled, time remaining)

Scan complete hone pe `Target - Site map` refresh karo - updated structure dikhega.

## Passive Scanner

**Passive Scan** = **koi naya request nahi bhejta** - sirf already-visited pages ka source analyze karta hai potential vulnerabilities ke liye.

**Kaise start karo:** `Site map`/Proxy History mein target pe right-click - **Do passive scan**

**Output:** Issues list with **Severity** (Info/Low/Medium/High) aur **Confidence** (Certain/Firm/Tentative)

**Common passive findings:**
- Missing HTML tags
- Potential DOM-based XSS
- Cookie without HttpOnly flag
- Frameable response (potential Clickjacking)

> Passive scan sirf **suggest** karta hai vulnerabilities, verify nahi karta (koi test request nahi bheja). Priority: **High severity + Certain/Firm confidence** pe focus karo, lekin sensitive apps ke liye saari severities review karo.

## Active Scanner

Sabse powerful feature - comprehensive scanning:

1. Crawl + web fuzzing (dirbuster/ffuf jaisa) - saare pages identify karta hai
2. Passive Scan run karta hai identified pages pe
3. Passive findings ko **verify** karta hai - actual test requests bhejke
4. JavaScript analysis - additional vulnerabilities ke liye
5. Insertion points/parameters fuzz karta hai - XSS, Command Injection, SQLi, etc. dhundhne ke liye

**Start karne ka tareeka:** Same as Passive - right-click - **Do active scan**, ya `New Scan` - **Crawl and Audit** select karo

### Audit Configuration
- Kaunse vulnerability types scan karne hain (default: sab)
- Presets: e.g. **"Audit checks - critical issues only"** - jab sirf High-severity backend-control vulns chahiye
- Login details bhi add kar sakte ho (Crawl jaisa)

**Duration:** Active scan **bahut zyada time** leta hai (Crawl se kai guna zyada) - configuration pe depend karta hai (example: 3749 requests, 1h 16m remaining)

**Live monitoring:** Dashboard - Tasks - **View details** - **Logger tab** (ya direct Burp Logger tab) - saare requests dikhte hain jo Burp bhej raha hai

### Reviewing Results
`Issue activity` pane - filter by Severity/Confidence (e.g. **High + Certain**)

**Example finding:** OS Command Injection - `ip` parameter mein - **High severity, Firm confidence**. Click karke advisory + sent request/response dekh sakte ho - exploitability samajhne ke liye.

## Reporting

`Target - Site map` - target pe right-click - **Issue - Report issues for this host**

- Export format select karo, jo info include karni hai wo choose karo
- Report mein: severity/confidence breakdown, PoC details, remediation guidance
- Browser mein khol ke view kar sakte ho

> ⚠️ **Professional practice:** Tool-generated report ko **kabhi bhi as-is final deliverable** na bhejo client ko. Ye sirf **supplementary/appendix data** hai - detailed manual analysis ke saath combine karke professional report banao.

## Pentest Relevance
- 🎯 Passive Scan = **safe, non-intrusive** first pass - production-like environments mein bhi relatively safe
- Active Scan = **comprehensive but risky** - dangerous functionality accidentally trigger ho sakti hai agar scope sahi se set na ho
- Automated scanning **manual testing ka replacement nahi** - false positives/negatives dono aa sakte hain, hamesha manually verify karo especially High-severity findings
- Scope management critical hai - galat scope se unintended systems/functionality test ho sakti hai (legal/ethical issue ban sakta hai real engagements mein)
- Authenticated scanning coverage significantly badhata hai - login credentials/session properly configure karna important hai thorough assessment ke liye

---
*Source: HTB Academy - Using Web Proxies*
