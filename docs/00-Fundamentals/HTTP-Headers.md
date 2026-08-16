# HTTP Headers

## Overview
HTTP headers client aur server ke beech extra information pass karte hain — request/response ka context, metadata, aur behavior control karte hain. Headers ka format: `Header-Name: value`, kabhi kabhi multiple values comma/semicolon se separated.

**5 categories:**
1. General Headers
2. Entity Headers
3. Request Headers
4. Response Headers
5. Security Headers

## 1. General Headers
Request aur response dono mein use hote hain — message ko describe karte hain (content ko nahi).

| Header | Example | Description |
|---|---|---|
| `Date` | `Date: Wed, 16 Feb 2022 10:38:44 GMT` | Message origin ka date/time (UTC preferred) |
| `Connection` | `Connection: close` | Connection ka state control — `close` (terminate) ya `keep-alive` (open rakho more data ke liye) |

## 2. Entity Headers
Content (entity) describe karte hain — mostly responses aur POST/PUT requests mein milte hain.

| Header | Example | Description |
|---|---|---|
| `Content-Type` | `Content-Type: text/html` | Resource ka type — browser auto-add karta hai, server response mein return karta hai |
| `Media-Type` | `Media-Type: application/pdf` | Content-Type jaisa hi, data type describe karta hai |
| `Boundary` | `boundary="b4e4fbd93540"` | Multi-part content separate karne ka marker (form-data mein use) |
| `Content-Length` | `Content-Length: 385` | Entity ka size (bytes) — server ko body read karne ke liye chahiye |
| `Content-Encoding` | `Content-Encoding: gzip` | Data transformation type (e.g. compression) |

## 3. Request Headers
Client se bhejte hain, content se related nahi.

| Header | Example | Description | Pentest Note |
|---|---|---|---|
| `Host` | `Host: www.inlanefreight.com` | Target hostname/IP | 🎯 Enumeration ke liye important — same server pe multiple hosted sites reveal kar sakta hai (vHost discovery) |
| `User-Agent` | `User-Agent: curl/7.77.0` | Client ka browser/OS info | Fingerprinting/spoofing ke liye |
| `Referer` | `Referer: http://www.inlanefreight.com/` | Request kahan se aayi | ⚠️ Client-controlled — spoofable, blindly trust mat karo |
| `Accept` | `Accept: */*` | Client kaunse media types accept karta hai | — |
| `Cookie` | `Cookie: PHPSESSID=b4e4fbd93540` | Session/identifier data (name=value pairs, `;` separated) | Session hijacking/fixation attacks ka target |
| `Authorization` | `Authorization: BASIC cGFzc3dvcmQK` | Auth token (client-side stored, server per-request verify karta hai) | Credential/token leakage check karo |

## 4. Response Headers
Server se aate hain, response context dete hain.

| Header | Example | Description | Pentest Note |
|---|---|---|---|
| `Server` | `Server: Apache/2.2.14 (Win32)` | Web server info | 🎯 Version fingerprinting, known CVEs check karne ke liye useful |
| `Set-Cookie` | `Set-Cookie: PHPSESSID=b4e4fbd93540` | Server client ko cookie set karta hai | Cookie flags (`HttpOnly`, `Secure`, `SameSite`) missing hona vulnerability ho sakta hai |
| `WWW-Authenticate` | `WWW-Authenticate: BASIC realm="localhost"` | Kaunsa auth type chahiye resource access ke liye | — |

## 5. Security Headers
Browser-level security policies enforce karte hain.

| Header | Example | Purpose |
|---|---|---|
| `Content-Security-Policy` | `script-src 'self'` | Trusted domains se hi resources load hone dega — **XSS prevention** |
| `Strict-Transport-Security` | `max-age=31536000` | Browser ko force karta hai sirf HTTPS use karne ke liye — **HSTS**, downgrade attacks se protection |
| `Referrer-Policy` | `origin` | Referer header mein kitni info share ho, control karta hai |

> **Pentest tip:** Missing security headers (CSP, HSTS, X-Frame-Options, etc.) ek common low-severity finding hain — bug bounty reports mein bhi frequently report kiye jaate hain.

## cURL Flags for Headers

| Flag | Behavior |
|---|---|
| `-I` | Sirf response headers dikhata hai (HEAD request bhejta hai) |
| `-i` | Specified request bhejta hai + headers aur body dono print karta hai |
| `-H` | Custom request header set karo |
| `-A` | User-Agent set karo (shortcut, `-H "User-Agent: ..."` ke equivalent) |

```bash
# Sirf response headers dekho
curl -I https://www.inlanefreight.com

# Custom User-Agent set karo
curl https://www.inlanefreight.com -A 'Mozilla/5.0'

# Verify karo ki User-Agent change hua (verbose ya headers-only mode se)
curl https://www.inlanefreight.com -A 'Mozilla/5.0' -v
```

## Browser DevTools

Network tab → kisi request pe click karo → **Headers** tab mein request + response headers dono milte hain (auto-categorized). **Raw** button se raw format dekh sakte ho. **Cookies** tab alag se cookies dikhata hai.

## Pentest Relevance
- `Host` header manipulation → vHost enumeration, host header injection attacks
- `Server` / `X-Powered-By` headers → tech stack fingerprint, version-specific CVE lookup
- Missing/misconfigured security headers → common bug bounty finding (low/info severity)
- `Set-Cookie` flags audit (missing `HttpOnly`/`Secure`) → session hijacking risk
- `Authorization` / `Cookie` headers → primary targets for session/auth-related attacks

---
*Source: HTB Academy - HTTP module*
