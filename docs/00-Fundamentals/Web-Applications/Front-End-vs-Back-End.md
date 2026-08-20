# Front End vs Back End

## Overview
Web application do parts mein split hoti hai — **Front End** (client-side) aur **Back End** (server-side). Dono alag-alag kaam karte hain aur alag jagah execute hote hain, par saath milke poora application banate hain.

## Front End

Front end = user directly jo dekhta/interact karta hai browser mein. Isme:
- **HTML** — page ka structure/content (title, text)
- **CSS** — design/styling/animation
- **JavaScript** — functionality/interactivity

Browser hi is code ko real-time interpret karta hai (client-side execution).

**Important:** Front end responsive hona chahiye — har screen size, har browser, har device (mobile included) pe kaam karna chahiye. Agar front end optimize nahi hai toh app slow/unresponsive lagega — user samjhega server ya internet slow hai, jabki issue actually client-side (browser) mein hai.

Front end se related other tasks: Visual/Web Design, UI Design, UX Design.

## Back End

Back end = server pe execute hone wala core logic. User isse directly interact nahi karta, par isके bina website sirf static pages ka collection hoti.

Char main back end components:

| Component | Description |
|---|---|
| **Back end Servers** | Hardware + OS jo baaki components host karta hai (Linux, Windows, Containers) |
| **Web Servers** | HTTP requests/connections handle karte hain — Apache, NGINX, IIS |
| **Databases** | Data store/retrieve karte hain — Relational: MySQL, MSSQL, Oracle, PostgreSQL; Non-relational: NoSQL, MongoDB |
| **Development Frameworks** | Core app develop karne ke liye — Laravel (PHP), ASP.NET (C#), Spring (Java), Django (Python), Express (Node.js) |

Docker jaise services se har component ko isolated container mein rakha ja sakta hai (DB alag, main app alag) — isse ek container compromise hone pe doosre safe rehte hain. Fully separate servers bhi possible hain par zyada resource-intensive.

Back end ke main jobs: core logic + services develop, DB maintain, libraries implement, APIs (front-back communication ke liye) banana, cloud/remote server integration.

## Securing Front/Back End

Back end source code humein by default nahi milta — isliye normally sirf **Blackbox Pentesting** possible hai. Lekin:
- Kai apps open-source hoti hain → source code available
- Kuch vulns (jaise **Local File Inclusion**) se back end source code extract ho sakta hai → phir code review possible, sensitive data (passwords) aur deeper vulns mil sakte hain

Front end ka full access ho toh code review karke **Whitebox Pentesting** possible hai (structure/functions analyze karke).

Example attack chain: Agar search function query properly sanitize nahi karta → **SQL Injection** (unauthorized DB access) ya **Command Injection** (OS commands execute) possible.

## Top 20 Developer Mistakes (Pentest-relevant)

1. Invalid data DB mein allow karna
2. System ko whole mein treat karna (individual parts ignore)
3. Personally-developed security methods use karna
4. Security ko last step treat karna
5. Plain text password storage
6. Weak passwords
7. Unencrypted data storage
8. Client-side pe excessive dependency
9. Being too optimistic
10. URL path se variables allow karna
11. Third-party code blindly trust karna
12. Hard-coded backdoor accounts
13. Unverified SQL injections
14. Remote file inclusions
15. Insecure data handling
16. Data encrypt na karna properly
17. Weak/no secure cryptographic system
18. Layer 8 (human factor) ignore karna
19. User actions review na karna
20. WAF misconfigurations

## OWASP Top 10 (in isse hi derive hote hain)

| # | Vulnerability |
|---|---|
| 1 | Broken Access Control |
| 2 | Cryptographic Failures |
| 3 | Injection |
| 4 | Insecure Design |
| 5 | Security Misconfiguration |
| 6 | Vulnerable and Outdated Components |
| 7 | Identification and Authentication Failures |
| 8 | Software and Data Integrity Failures |
| 9 | Security Logging and Monitoring Failures |
| 10 | Server-Side Request Forgery (SSRF) |

## Pentest Relevance
- Front/Back split samajhna zaroori hai kyunki attack surface, testing approach (Whitebox vs Blackbox), aur tools dono side ke liye alag hote hain
- Yeh 20 mistakes hi OWASP Top 10 ka foundation hain — inko pehchanna future modules ke liye base hai

---

*Source: HTB Academy - Introduction to Web Applications*
