# Parameter and Value Fuzzing

## Overview
Directory/file fuzzing ke baad agla step - **parameters** manipulate karke dekhna app kaise react karti hai. Parameters = variables jo browser-server ke beech data carry karte hain.

## GET Parameters
URL mein `?` ke baad visible, multiple params `&` se separated:
- Publicly visible (postcard jaisa)
- State-changing nahi actions ke liye use hota hai (search, filter)

## POST Parameters
Request **body** mein hidden, URL mein nahi dikhte:
```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=your_username&password=your_password
```
- Sensitive data ke liye preferred (login, personal info)
- Encoding types: `application/x-www-form-urlencoded` (key-value) ya `multipart/form-data` (files ke saath)

## Why Fuzz Parameters?
- Product ID badal ke pricing errors/unauthorized access expose ho sakta hai
- Hidden parameter se admin features unlock ho sakte hain
- Malicious payload inject karke XSS/SQLi expose ho sakta hai

## GET Parameter Fuzzing — wenum

**Manual recon pehle (curl se):**
```bash
curl http://IP:PORT/get.php
# -> Invalid parameter value, x: (missing)

curl http://IP:PORT/get.php?x=1
# -> Invalid parameter value, x: 1 (param recognized, value invalid)
```

**Automate karo wenum se:**
```bash
wenum -w /usr/share/seclists/Discovery/Web-Content/common.txt --hc 404 -u "http://IP:PORT/get.php?x=FUZZ"
```

| Flag | Purpose |
|---|---|
| `-w` | Wordlist |
| `--hc 404` | Hide 404 responses (noise reduce karo) |
| `FUZZ` | URL mein param value ki jagah placeholder |

**Result:** Jo value 200 OK deti hai (baaki sab invalid message), wahi correct value hai - verify karo `curl`se.

## POST Parameter Fuzzing — ffuf

**Manual recon pehle:**
```bash
curl -d "" http://IP:PORT/post.php
# -> Invalid parameter value, y: (missing)
```

**Automate karo ffuf se:**
```bash
ffuf -u http://IP:PORT/post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -w /usr/share/seclists/Discovery/Web-Content/common.txt -mc 200 -v
```

| Flag | Purpose |
|---|---|
| `-X POST` | HTTP method specify karo |
| `-H` | Custom header (Content-Type) |
| `-d` | POST body data, `FUZZ` placeholder ke saath |
| `-mc 200` | Sirf 200 status wale results match/show karo |

**Result:** Correct value jo 200 OK return karti hai - verify karo `curl -d "y=VALUE"` se.

## Pentest Relevance
- Hamesha **manual curl recon pehle** karo - samajh aata hai app kaisa respond karti hai, phir fuzzing se automate karo
- `--hc`/`-mc` filters (hide/match codes) **noise reduce karne** ke liye critical hain - bina inke results overwhelming ho jaate hain
- Real-world scenarios mein flags/clear indicators nahi honge - response size/timing/content mein subtle differences dhundhne padenge
- GET aur POST dono param fuzzing zaroori hain - app kaise data accept karti hai uske hisaab se approach alag hoga

---
*Source: HTB Academy - Fuzzing Web Applications*
