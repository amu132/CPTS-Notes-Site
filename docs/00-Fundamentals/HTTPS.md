# HTTPS (HTTP Secure)

## Overview
HTTP ka sabse bada drawback — saara data **clear-text** mein transfer hota hai. Isse koi bhi beech mein baitha attacker (MiTM) data read/capture kar sakta hai.

HTTPS is problem ko solve karta hai — communication **encrypted** hoti hai, isliye intercept hone pe bhi data readable nahi hota. Isi wajah se HTTPS ab standard ban gaya hai, aur browsers HTTP ko slowly deprecate kar rahe hain.

## HTTP vs HTTPS Traffic

**HTTP (clear-text) — Wireshark mein:**
- POST `/login.php` request capture karo toh `username=admin&password=password` seedha plain text mein dikhega
- Public WiFi jaisi shared network pe koi bhi credentials sniff kar sakta hai

**HTTPS (encrypted) — Wireshark mein:**
- Sirf `TLSv1.2 Application Data` dikhega — ek encrypted stream, readable kuch nahi

**Identification:**
- URL mein `https://` prefix
- Address bar mein lock icon 🔒

> **Note:** HTTPS encrypted hone ke bawajood, agar DNS query clear-text server pe ja rahi hai, toh visited URL leak ho sakta hai. Isliye encrypted DNS (`8.8.8.8`, `1.1.1.1`) ya VPN recommend kiya jata hai.

## HTTPS Flow (High Level)

1. User `http://` type karta hai (ya sirf domain)
2. Browser port 80 pe request bhejta hai
3. Server `301 Moved Permanently` response deta hai → redirect to `https://` (port 443)
4. **TLS Handshake** start hota hai:
   - Client → Server: `Client Hello`
   - Server → Client: `Server Hello` + SSL certificate
   - Client certificate verify karta hai, apna bhejta hai
   - Encrypted handshake confirm hota hai (encryption working check)
5. Handshake complete → normal HTTP communication continue, ab encrypted

> **Attack angle:** SSL stripping / **HTTP downgrade attack** — attacker MITM proxy set karke HTTPS ko forcefully HTTP pe downgrade kar sakta hai, traffic clear-text mein expose ho jata hai. Modern browsers/servers isse protect karte hain (HSTS etc.), lekin misconfigured targets pe still relevant hai.

## cURL with HTTPS

cURL automatically HTTPS handshake handle karta hai. Lekin agar SSL certificate invalid/expired/self-signed ho, toh cURL by default request block kar deta hai (MITM se protect karne ke liye):

```bash
curl https://inlanefreight.com
# curl: (60) SSL certificate problem: Invalid certificate chain
```

**Fix — certificate check skip karo** (sirf lab/practice targets ke liye, production pe kabhi mat karo):

```bash
curl -k https://www.inlanefreight.com
```

`-k` flag SSL verification bypass kar deta hai.

## Pentest Relevance
- Self-hosted labs/practice apps mein aksar invalid/self-signed certs hote hain — `-k` flag zaroori hoga
- Traffic capture karte waqt (Wireshark/Burp) HTTP vs HTTPS ka difference samajhna important hai — agar target HTTP pe hai toh credentials directly capture ho sakte hain
- HTTP → HTTPS downgrade attack scenario recon/testing mein consider karne layak hai agar target properly HSTS enforce nahi karta

---
*Source: HTB Academy - HTTP module*
