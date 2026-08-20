# Introduction to Web Applications

## Overview
Web applications interactive apps hain jo browser mein chalti hain — **client-server architecture** follow karti hain:
- **Front-end** (client-side/browser) → UI jo user dekhta hai
- **Back-end** (server-side) → source code, logic, databases

Examples: Gmail (email), Amazon (retail), Google Docs (word processor). Koi bhi developer web app bana ke host kar sakta hai — isliye internet pe millions of web apps hain.

## Web Applications vs Websites

| | Website (Web 1.0) | Web Application (Web 2.0) |
|---|---|---|
| Content | Static — sabke liye same | Dynamic — user interaction ke hisaab se badalta hai |
| Functionality | Minimal/none, sirf info display | Fully functional, real-time features |
| Update | Manual edit developer se | Real-time, user-driven |
| Design | Fixed | Modular, responsive (any screen size), platform-independent |

## Web Applications vs Native OS Applications

**Web apps ke advantages:**
- Platform-independent — kisi bhi OS pe browser mein chal jati hain
- Install nahi karni padti, local storage nahi khaati
- **Version unity** — sab users same version use karte hain, update ek jagah (server) se ho jata hai

**Native apps ke advantages:**
- Speed — native OS libraries directly use karti hain
- Deeper OS/hardware integration, browser tak limited nahi

**Middle ground:** Hybrid/Progressive Web Apps (PWAs) — native capabilities + web app flexibility dono combine karte hain.

## Web App Distribution Types

| Type | Examples |
|---|---|
| Open-source | WordPress, OpenCart, Joomla |
| Closed-source (proprietary) | Wix, Shopify, DotNetNuke |

## Security Risks of Web Applications

Web apps ka **attack surface bahut bada** hota hai:
- Worldwide accessible — koi bhi internet + browser wala access kar sakta hai
- Automated scanning/attack tools freely available (galat haath mein damaging)
- Complex apps = zyada chances of critical vulnerabilities in design
- Server pe often sensitive data + database connections hoti hain — ek successful attack se poora data compromise ho sakta hai

**Testing approach (OWASP WSTG based):**
1. Front-end trinity review — HTML/CSS/JavaScript (Sensitive Data Exposure, XSS check)
2. Core functionality + browser-server interaction — tech stack enumerate karo
3. Dono perspectives se test karo — **unauthenticated** aur **authenticated** (agar login hai)

## Modern Attack Landscape

Aajkal directly exploitable network services (jaise Windows RCE) rare hote hain externally facing hosts pe. Attack surface zyada **web application layer** pe shift ho gaya hai — kyunki apps constantly change hote hain aur ek chhota sa code change catastrophic vulnerability introduce kar sakta hai.

### SQL Injection + Active Directory Chain (Real-world Example)

Common scenario: Web app AD (Active Directory) se authenticate hota hai
1. SQL injection se AD passwords directly nahi milte (AD manage karta hai)
2. Lekin **email addresses/usernames** extract ho sakte hain (often username = email)
3. Extracted usernames se **password spraying attack** VPN/O365/Outlook portal pe try karo
4. Success → corporate network mein foothold ya email access

Ye example dikhata hai kaise ek single web vuln **chain** ho ke bade infrastructure compromise mein badal sakta hai.

## Common Web Flaws — Real-World Impact

| Flaw | Real-world Scenario |
|---|---|
| **SQL Injection** | AD usernames extract karke VPN/email portal pe password spraying |
| **File Inclusion** | Source code read karke hidden pages/functionality expose, phir RCE tak |
| **Unrestricted File Upload** | Profile picture upload jo file-type validate nahi karta → malicious file upload → full server control |
| **IDOR (Insecure Direct Object Reference)** | `/user/701/edit-profile` → `701` ko `702` mein badal ke doosre user ka profile edit karna |
| **Broken Access Control** | Registration request mein `roleid=3` ko `roleid=0`/`1` mein manipulate karke admin account bana lena |

## Pentest Relevance
- Web app pentesting sirf ek "nice to have" skill nahi — modern external assessments ka **primary attack surface** hai
- Chained vulnerabilities (e.g. SQLi → AD emails → password spray → network foothold) real-world impact dikhate hain — single low-severity bug bhi bade breach ka starting point ban sakta hai
- IDOR aur Broken Access Control jaise flaws — **parameter tampering** se test karo (IDs, role fields, hidden params sab manipulate karke dekho)
- Authenticated + unauthenticated dono perspectives se test karna zaroori hai — coverage maximize karne ke liye

---
*Source: HTB Academy - Introduction to Web Applications*
