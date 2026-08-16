# HTTP Methods and Status Codes

## Overview
HTTP **methods** batate hain server ko ki request se kya action lena hai (data fetch/send/update/delete). HTTP **status codes** batate hain client ko ki request process hone ke baad result kya raha.

- cURL `-v` mein first line pe method dikhta hai (`GET / HTTP/1.1`)
- Browser DevTools Network tab → **Method** column
- Status code response headers ki first line mein hota hai

## HTTP Request Methods

| Method | Description | Pentest Note |
|---|---|---|
| `GET` | Resource request karta hai, data URL query string mein bhej sakta hai (`?param=value`) | Params tamper karke IDOR/injection test |
| `POST` | Server ko data bhejta hai (forms, files, JSON) — request body mein, headers ke baad | Login forms, uploads — injection points |
| `HEAD` | GET jaisa hi hai lekin sirf headers return hote hain, body nahi | Resource size check karne ke liye download se pehle |
| `PUT` | Server pe naya resource create karta hai | ⚠️ Agar properly secured nahi hai toh malicious file upload possible |
| `DELETE` | Existing resource delete karta hai | ⚠️ Improperly secured toh DoS risk (critical files delete) |
| `OPTIONS` | Server info return karta hai — kaunse methods accepted hain | 🎯 Recon step — pata chalta hai server kya-kya support karta hai |
| `PATCH` | Resource ko partially modify karta hai | REST API endpoints mein common |

> **Note:** Zyadatar web apps mainly `GET`/`POST` use karte hain. REST APIs `PUT`/`DELETE` bhi use karti hain — data update/delete ke liye.

## HTTP Status Codes

**5 classes:**

| Class | Meaning |
|---|---|
| `1xx` | Informational — request processing affect nahi karta |
| `2xx` | Success |
| `3xx` | Redirection |
| `4xx` | Client error — improper/bad request |
| `5xx` | Server error — server side problem |

### Commonly Seen Codes

| Code | Meaning | Pentest Relevance |
|---|---|---|
| `200 OK` | Request successful, resource body return hui | Baseline — endpoint accessible hai |
| `302 Found` | Redirect (e.g. login ke baad dashboard) | Redirect chains follow karo, open redirect check karo |
| `400 Bad Request` | Malformed request | Fuzzing/malformed input ka response indicator |
| `403 Forbidden` | Access nahi hai (ya malicious input detect hua) | 🎯 Access control check, WAF block detection |
| `404 Not Found` | Resource exist nahi karta | Path enumeration ke baseline ke liye |
| `500 Internal Server Error` | Server process nahi kar paya request | 🎯 Unhandled errors → info leakage, stack traces, potential vuln indicator |

> **Note:** Standard codes ke alawa, Cloudflare/AWS jaise providers apne custom status codes bhi implement karte hain — unfamiliar code mile toh provider-specific documentation check karna.

## Pentest Relevance
- `OPTIONS` request → server ke supported methods enumerate karo, agar `PUT`/`DELETE` allowed hai toh further test karo
- Method tampering (e.g. `GET` ko `POST` mein convert karke test karna) → access control bypass possibility
- Status code patterns fuzzing mein useful — `200` vs `403` vs `404` se hidden endpoints/directories differentiate ho sakte hain (Gobuster/ffuf jaise tools isi pe based hain)
- `500` errors → verbose error messages se tech stack/DB info leak ho sakta hai

---
*Source: HTB Academy - HTTP module*
