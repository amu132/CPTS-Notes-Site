# Intro to Web Proxies

## Why Web Proxies?
Modern web/mobile apps continuously connect to back-end servers — data send/receive/process karte rehte hain. Isliye **back-end server testing** web app pentest ka bulk part banata hai. Requests capture aur manipulate karne ke liye **Web Proxies** use karte hain.

## What Are Web Proxies?
Web proxy ek tool hai jo **browser/mobile app** aur **back-end server** ke beech baith kar saara traffic capture karta hai — essentially ek **MITM (Man-in-the-Middle)** tool.

**Wireshark se difference:**
- Wireshark → **poora local network traffic** analyze karta hai
- Web Proxy → sirf **web ports** (HTTP/80, HTTPS/443) pe focus karta hai — web pentest ke liye zyada targeted aur convenient

**Core capability:**
- Har HTTP request/response dekh sakte ho
- Kisi bhi request ko **intercept** karke data modify kar sakte ho, phir dekho backend kaise react karta hai

## Uses of Web Proxies (Beyond Capture/Replay)

| Use Case | Description |
|---|---|
| Vulnerability scanning | Automated scan for common web vulns |
| Web fuzzing | Inputs ko systematically test karna |
| Web crawling | Site structure discover karna |
| Web application mapping | Endpoints/functionality map karna |
| Request analysis | Traffic patterns/behavior samajhna |
| Configuration testing | Server/app misconfigurations dhundhna |
| Code reviews | (indirectly) request/response se logic samajhna |

> **Note:** Is module mein specific attacks (SQLi, XSS, etc.) discuss nahi honge — wo alag HTB modules mein cover hote hain. Ye module sirf **tool usage** pe focus karega.

## Two Main Tools: Burp Suite vs ZAP

### Burp Suite
- Sabse common web proxy pentest ke liye — best-in-class UI
- Built-in Chromium browser included
- **Free (Community) version** most testers ke liye sufficient hai

**Paid-only features (Burp Pro/Enterprise):**
- Active Web App Scanner
- Fast Burp Intruder (rate-limited in free version)
- Custom Burp Extensions load karne ki ability

> **Tip:** Educational/business email ho toh Burp Pro trial free mil sakta hai.

### OWASP ZAP (Zed Attack Proxy)
- **Free aur open-source** — OWASP project, community-maintained
- **Koi paid-only features nahi** — sab kuch free mein available
- Growing community — Burp ke paid features gradually free mein aa rahe hain ZAP mein
- No throttling/limitations jo paid subscription se hi lift hote hain (unlike Burp free tier)

## Burp vs ZAP — Kab Kya Use Karo

| Factor | Burp Suite | ZAP |
|---|---|---|
| Cost | Free (limited) / Paid (full) | Fully free |
| UI/Maturity | Industry-standard, polished | Growing, improving |
| Best for | Corporate/advanced pentests jahan Pro features justify karte hain | Open, unrestricted testing, budget-conscious use |
| Extensions | Pro version mein full support | Community-driven, growing |

**Practical approach:** Dono tools seekhna valuable hai — situation ke hisaab se switch kar sakte ho. Bug bounty/learning phase mein ZAP kaafi hoga; professional/corporate pentest mein Burp Pro ki value justify ho sakti hai.

## Pentest Relevance
- Web proxy setup **first step** hota hai kisi bhi serious web pentest ka — bina iske manual request analysis bahut slow hoti hai
- Intercept + modify capability se **parameter tampering, auth bypass testing, IDOR checks** sab fast ho jaate hain
- Fuzzing aur automated scanning built-in hone se recon phase significantly speed up hota hai
- Free ZAP se start karna beginners/bug bounty hunters ke liye practical hai — koi cost barrier nahi

---
*Source: HTB Academy - Using Web Proxies*
