# HTTP Requests and Responses

## Overview
HTTP communication mein do main parts hote hain — **HTTP Request** (client bhejta hai) aur **HTTP Response** (server return karta hai). Client request mein resource, headers, params sab specify karta hai; server usko process karke response deta hai (status code + response body).

## HTTP Request Structure

Example request:
```
GET /users/login.html HTTP/1.1
Host: inlanefreight.com
User-Agent: Mozilla/5.0
Cookie: PHPSESSID=c4ggt4jull9obt7aupa55o8vbf
```

**Pehli line ke 3 fields (space se separated):**

| Field | Example | Description |
|---|---|---|
| Method | `GET` | HTTP verb — action ka type (GET, POST, etc.) |
| Path | `/users/login.html` | Resource ka path, query string bhi suffix ho sakti hai (`?username=user`) |
| Version | `HTTP/1.1` | HTTP protocol version |

Uske baad **headers** (key-value pairs) — Host, User-Agent, Cookie, etc. Headers ek blank line se terminate hote hain, uske baad agar hai toh **request body**.

> **Note:** HTTP/1.X clear-text mein bheja jata hai (newline-separated fields). HTTP/2.X binary format mein bheja jata hai (dictionary form) — zyada efficient.

## HTTP Response Structure

Example response:
```
HTTP/1.1 200 OK
Date: ...
Server: Apache/2.4.41
Set-Cookie: PHPSESSID=m4u64rqlpfthrvvb12ai9voqgf
Content-Type: text/html; charset=UTF-8
```

**Pehli line ke 2 fields:**
- HTTP version (`HTTP/1.1`)
- Response/status code (`200 OK`)

Uske baad headers, phir (agar present ho) **response body** — HTML, JSON, images, PDFs, ya koi bhi resource type ho sakta hai.

## cURL — Full Request/Response View

`-v` (verbose) flag se poora request + response dono dikhte hain:

```bash
curl inlanefreight.com -v
```

Output breakdown:
- `>` prefix wali lines = **request** (jo humne bheja)
- `<` prefix wali lines = **response** (jo server ne return kiya)

```
> GET / HTTP/1.1
> Host: inlanefreight.com
> User-Agent: curl/7.65.3
> Accept: */*
> Connection: close
>
< HTTP/1.1 401 Unauthorized
< Date: ...
< Server: Apache/X.Y.ZZ (Ubuntu)
< WWW-Authenticate: Basic realm="Restricted Content"
< Content-Length: 464
< Content-Type: text/html; charset=iso-8859-1
```

`401 Unauthorized` yahan indicate karta hai ki humein resource access nahi mila.

**Aur bhi verbose:** `-vvv` flag use karke extra details dekhi ja sakti hain (deeper connection/TLS-level info).

## Browser DevTools (Network Tab)

Web pentest ke liye **built-in DevTools** ek zaroori asset hai — har assessment mein browser available hota hai.

**Open karne ka shortcut:** `Ctrl + Shift + I` ya `F12`

**Network tab** mein page refresh karne pe:
- Har request ki list dikhti hai — status code, method, URL, path
- **Filter URLs** se specific request dhundh sakte ho agar bahut saari requests load ho rahi ho
- Kisi bhi request pe click karke → **Response tab** → **Raw** button se unrendered raw response body dekh sakte ho

## Pentest Relevance
- `curl -v` / `-vvv` — request/response ka poora picture milta hai bina browser ke, scripting mein bhi useful
- DevTools Network tab — hidden API calls, auth tokens, cookies, aur backend endpoints discover karne mein kaam aata hai
- Response headers (`Server`, `WWW-Authenticate`, etc.) fingerprinting aur tech stack identify karne mein help karte hain

---
*Source: HTB Academy - HTTP module*
