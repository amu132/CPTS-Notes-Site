# Intercepting Web Requests

## Overview
Proxy setup ke baad, hum HTTP requests **intercept** kar sakte hain — request ko destination tak pahunchne se pehle rok ke, dekh ke, modify karke aage bhej sakte hain.

## Intercepting Requests — Burp Suite

1. **Proxy tab** → **Intercept** sub-tab
2. Default: **Intercept is ON** hota hai
3. Toggle karne ke liye "Intercept is on/off" button click karo
4. Pre-configured browser (Burp's built-in Chromium) se target site visit karo
5. Burp mein wapas aao → intercepted request dikhega
6. **Forward** button → request ko aage bhejo

> **Note:** Kabhi kabhi target request se pehle koi aur (unrelated Firefox traffic) intercept ho jata hai — tab tak **Forward** karte raho jab tak target IP wali request na aaye.

## Intercepting Requests — ZAP

- Default: **Intercept OFF** (top bar pe green button = requests pass ho rahi hain)
- Toggle: button click karo, ya shortcut **`Ctrl+B`**
- Pre-configured browser se target visit karo
- Top-right pane mein intercepted request dikhega
- **Step button** (red break button ke paas) → request forward karo

### ZAP HUD (Heads Up Display)
ZAP ka powerful feature — browser ke andar hi ZAP ke main features control kar sakte ho.
- Enable: top menu bar ke end mein HUD button
- Intercept toggle: left pane ka **2nd button** (top se)
- Request intercept hone pe dialog aata hai with 3 options:

| Button | Action |
|---|---|
| **Step** | Request send karo, response examine karo, agla request bhi break karo |
| **Continue** | Ye request forward karo, baaki saari requests bhi bina break kiye jaane do |
| **Drop** | Request cancel/discard karo |

**Step vs Continue kab use karo:**
- **Step** → jab page ki har functionality step-by-step examine karni ho
- **Continue** → jab sirf ek specific request mein interest ho, baaki forward kar do

> **Tip:** Pehli baar ZAP browser use karne pe HUD tutorial milega — le lena, basics samajh aa jaayenge.

## Manipulating Intercepted Requests

Intercept hone ke baad request **hang** rehti hai jab tak forward na karo — is beech mein:
- Request examine karo (headers, params, body)
- Values manipulate karo
- Phir forward karo → dekho backend kaise react karta hai

### Common Testing Use Cases
- SQL injection
- Command injection
- Upload bypass
- Authentication bypass
- XSS
- XXE
- Error handling
- Deserialization

## Practical Example — Command Injection via Interception

**Scenario:** Ek "Ping" tool hai jo IP address input leta hai. Front-end JS sirf numbers allow karta hai.

**Normal intercepted request:**
```http
POST /ping HTTP/1.1
Host: 94.237.62.138:32306
Content-Length: 4
Content-Type: application/x-www-form-urlencoded
...

ip=1
```

**Attack idea:** Front-end restriction sirf **client-side** hai — agar back-end bhi validate nahi karta, toh intercept karke bypass kar sakte hain.

**Payload:** `ip` parameter ki value `1` se badal ke `;ls;` kar do:
**Result:** Response mein normal ping output ki jagah `ls` command ka output aa gaya (file listing) — matlab **command injection successful**.

> **Key concept:** "Breaking" the application yahan matlab request/response flow manipulate karna hai (testing purpose), damage karna nahi. Front-end validation bypass karna easy hai jab back-end pe proper validation na ho.

## Pentest Relevance
- 🎯 **Front-end validation ≠ security** — hamesha back-end validation check karo, front-end restrictions ko trust mat karo
- Intercept + manipulate = manual testing ka **core technique** har web vuln class ke liye (SQLi se lekar auth bypass tak)
- ZAP HUD se browser ke andar hi testing workflow fast ho jata hai, tool switch karne ki zarurat nahi
- Command injection jaisa impactful bug bhi sirf ek intercepted parameter change karne se mil sakta hai — **hamesha every input field test karo**, chahe UI restriction kitni bhi strict lage

---
*Source: HTB Academy - Using Web Proxies*
