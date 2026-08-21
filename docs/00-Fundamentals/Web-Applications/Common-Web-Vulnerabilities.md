# Common Web Vulnerabilities

## Overview
Internal/custom web apps assess karte time (ya jab public exploit na mile), manually vulnerabilities identify karni padti hain. Ye OWASP Top 10 ka part hain.

## Broken Authentication / Access Control
- **Broken Authentication** — attacker authentication bypass kar sakta hai (bina valid creds ke login, ya normal user → admin escalation)
- **Broken Access Control** — attacker unauthorized pages/features access kar sakta hai (e.g. normal user → admin panel)

**Example:** College Management System 1.2 — email field mein `' or 0=0 #` daal ke (kisi bhi password se) auth bypass ho jata hai.

## Malicious File Upload
Agar file upload feature properly validate nahi karta, malicious script (e.g. PHP) upload karke remote server pe command execution ho sakta hai.

Developers checks lagate bhi hain kabhi kabhi, par wo bypass ho sakte hain.

**Example:** WordPress Plugin "Responsive Thumbnail Slider 1.0" — double extension (`shell.php.jpg`) se arbitrary file upload bypass. Metasploit module bhi available hai.

## Command Injection
Web app OS commands execute karta hai kabhi kabhi (e.g. plugin install karne ke liye download command). Agar input properly sanitize nahi hua, attacker apna command inject kar sakta hai original command ke saath.

**Example:** WordPress Plugin "Plainview Activity Monitor 20161228" — `ip` value mein `| COMMAND...` add karke command injection.

## SQL Injection (SQLi)
Command Injection jaisa hi, par SQL query context mein. User input directly query mein concatenate hone se hota hai:
```php
$query = "select * from users where name like '%$searchInput%'";
```

**Example:** College Management System 1.2 — SQLi se auth query ko always-true bana ke login bypass, ya data retrieve/server control tak possible.

## Pentest Relevance
- Ye sab OWASP Top 10 ka core hain — baar baar milenge real assessments mein
- Har vuln ka basic pattern samajhna future deep-dive modules ka foundation hai

---
*Source: HTB Academy - Introduction to Web Applications*
