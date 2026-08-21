# Public Vulnerabilities

## Overview
Sabse critical back end vulns wo hain jo **externally** attack ho sakte hain (local access ki zarurat nahi) — external pentesting ka focus. Ye usually dev mistakes se aate hain.

## Public CVE
Open-source/proprietary web apps duniya bhar mein test hote hain → vulnerabilities discover → patch → publicly share with a **CVE** (Common Vulnerabilities and Exposures) record + score.

**Workflow:**
1. Target web app ka **version identify karo** (source code, `version.php` jaisi files, ya open-source repo se compare)
2. Google pe us version ke public exploits search karo
3. Exploit databases use karo: **Exploit-DB**, **Rapid7 DB**, **Vulnerability Lab**

Priority: CVSS score 8-10 ya **RCE** lead karne wale exploits. Web app ke external components (plugins etc.) ke liye bhi separately search karo.

## CVSS (Common Vulnerability Scoring System)
Open industry standard for vuln severity — resource prioritization ke liye.

Metrics: **Base**, **Temporal**, **Environmental**. NVD sirf Base score deta hai (0-10) kyunki Temporal/Environmental change hote rehte hain / org-specific hote hain.

| Severity | CVSS v2 | CVSS v3 |
|---|---|---|
| None | — | 0.0 |
| Low | 0.0-3.9 | 0.1-3.9 |
| Medium | 4.0-6.9 | 4.0-6.9 |
| High | 7.0-10.0 | 7.0-8.9 |
| Critical | — | 9.0-10.0 |

NVD ke CVSS v2/v3 calculators available hain fine-tuning ke liye (Temporal/Environmental apply karke).

## Back-end Server Vulnerabilities
Web servers publicly accessible hone ki wajah se sabse critical target hain (TCP pe exposed).

**Example:** Shell-Shock — Apache web servers (2014 aur pehle) affect karta tha, HTTP requests se remote control.

Back-end server/DB vulnerabilities usually **local access ke baad** exploit hoti hain (external vuln se ya internal pentest mein) — privilege escalation ya lateral movement ke liye. Directly externally exploitable nahi, par patch zaroor karna chahiye.

## Pentest Relevance
- Version identification = first step, hamesha
- CVSS score se prioritize karo kis vuln pe pehle focus karna hai
- External-facing (web server) vs internal (DB, OS level) vulns ka distinction samajhna important hai for scoping

---
*Source: HTB Academy - Introduction to Web Applications*
