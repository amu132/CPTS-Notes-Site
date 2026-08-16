# GET Requests, HTTP Basic Auth &amp; GET Parameters

## GET Overview
Browser koi bhi URL visit karte waqt default **GET** request bhejta hai. Page load hone ke baad, page apne aap aur bhi requests (different methods se) bhej sakta hai backend se — ye sab Network tab mein observe kiya ja sakta hai.

> **Exercise habit:** Kisi bhi website pe DevTools ka Network tab khula rakh ke navigate karo — samajh aata hai backend ke saath app kaise interact karti hai. Bug bounty/web assessment ke liye ye ek core recon technique hai.

## HTTP Basic Auth

Normal login forms (POST + credentials validate) se alag — **HTTP Basic Auth** webserver level pe directly kisi page/directory ko protect karta hai, application logic involve kiye bina.

**Unauthenticated request response:**
```bash
curl -i http://<SERVER_IP>:<PORT>/
```
```
HTTP/1.1 401 Authorization Required
WWW-Authenticate: Basic realm="Access denied"
```

`WWW-Authenticate: Basic` header confirm karta hai ki Basic Auth use ho raha hai.

### Credentials Provide Karne Ke Tareeke

**1. `-u` flag se:**
```bash
curl -u admin:admin http://<SERVER_IP>:<PORT>/
```

**2. URL mein directly (`user:pass@host` format):**
```bash
curl http://admin:admin@<SERVER_IP>:<PORT>/
```
(Browser mein bhi same URL format se access ho jata hai)

### Authorization Header (Manual)

`-v` flag lagane pe dikhta hai ki request mein automatically ye header set hota hai:
```
Authorization: Basic YWRtaW46YWRtaW4=
```
Ye `admin:admin` ka **Base64 encoded** value hai (encryption nahi — sirf encoding, easily decodable).

> **Note:** Modern auth methods (jaise JWT) mein `Authorization: Bearer <token>` hota hai — ek lambi encrypted/signed token string.

**Manually header set karke bhi access mil jata hai** (credentials directly diye bina, agar header value pata ho):
```bash
curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' http://<SERVER_IP>:<PORT>/
```

`-H` flag multiple baar use kiya ja sakta hai multiple headers set karne ke liye.

> **Pentest angle:** Base64 sirf encoding hai, encryption nahi — agar `Authorization` header kahin leak ho jaye (logs, cache, ya intercepted traffic mein), toh trivially decode karke credentials mil jaate hain.

## GET Parameters

Authenticated hone ke baad agar koi search/filter function hai (e.g. City Search), toh wo backend ko GET request bhejta hai jisme parameter URL mein hota hai:

```
GET /search.php?search=le
```

**DevTools se identify karna:**
- Network tab (shortcut: `Ctrl+Shift+E`) → trash icon se purani requests clear karo → search term enter karo → naya request dikhega
- Request pe click karo → URL + params + method sab dikh jayega

**Direct request replicate karna (cURL):**
```bash
curl 'http://<SERVER_IP>:<PORT>/search.php?search=le' -H 'Authorization: Basic YWRtaW46YWRtaW4='
```

### DevTools Shortcuts — Copy Request

| Option | Kya karta hai |
|---|---|
| Right-click request → **Copy as cURL** | Poora cURL command copy karta hai (sab headers ke saath) — terminal mein paste karke run karo |
| Right-click request → **Copy as Fetch** | JavaScript `fetch()` command copy karta hai — Console tab (`Ctrl+Shift+K`) mein paste karke run karo |

> **Tip:** Copied cURL command mein bahut saare unnecessary headers honge — sirf zaroori auth headers (jaise `Authorization`) rakh ke baaki hata sakte ho, request cleaner ban jayegi.

## Pentest Relevance
- Basic Auth pages → credentials brute-force target ban sakte hain (`-u` flag automate karne mein easy)
- `Authorization: Basic <base64>` → traffic capture mein mile toh turant decode karke check karo
- **Copy as cURL / Fetch** → burp suite na ho toh bhi requests replicate/manipulate karne ka fast tareeka — manual param tampering, fuzzing scripts banane mein useful
- GET parameters → primary injection testing surface (SQLi, XSS, IDOR sab yahin se shuru hote hain)

---
*Source: HTB Academy - HTTP module*
