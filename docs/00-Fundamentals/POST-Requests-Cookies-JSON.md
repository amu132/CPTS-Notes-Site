# POST Requests, Cookies &amp; JSON Data

## POST vs GET
GET params URL mein hote hain, **POST** params **request body** mein hote hain. POST tab use hota hai jab files transfer karni ho ya params URL se hata ke body mein bhejni ho.

**POST ke 3 main benefits:**

| Benefit | Explanation |
|---|---|
| Lack of Logging | Large files (uploads) URL mein log nahi hote — server logs clean rehte hain |
| Less Encoding | Body binary data accept kar sakta hai, sirf param-separator characters encode karne padte hain |
| More Data | URL length limit hoti hai (~2000 chars, browser/server/CDN pe depend) — body mein ye limit nahi |

## Login Forms — POST Ka Example

**Flow:** Login form → credentials submit → POST request backend ko

**DevTools se identify karna:**
1. Network tab clear karo (trash icon)
2. Server IP se filter lagao (external noise hatane ke liye)
3. Login karo → POST request dikhega
4. Click on request → **Request tab** → **Raw** → body dikhega:
```
username=admin&password=admin
```

### cURL Se POST Request Banana

```bash
curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/
```

- `-X POST` → method specify karta hai
- `-d` → POST data (body) specify karta hai

Response mein agar login form ki jagah authenticated content (e.g. search function) dikhe, toh login successful hai.

> **Tip:** Kai login forms redirect karte hain (`/dashboard.php`). Redirect follow karne ke liye `-L` flag use karo.

## Authenticated Cookies

Successful login ke baad server `Set-Cookie` header bhejta hai — session persist karne ke liye.

**Cookie dekhne ke liye:**
```bash
curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/ -i
```
```
Set-Cookie: PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1; path=/
```

### Cookie Se Authenticate Karna (Bina Login Ke Repeat Kiye)

**Option 1 — `-b` flag:**
```bash
curl -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/
```

**Option 2 — Header se:**
```bash
curl -H 'Cookie: PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/
```

Dono equivalent hain — valid cookie mile toh dobara login nahi karna padta.

### Browser Mein Cookie Manually Set Karna

1. Logout karo (login page pe wapas aa jaoge)
2. DevTools → **Storage tab** (shortcut: `Shift+F9`) → **Cookies** → target site select karo
3. Purani cookie delete karo (right-click → Delete All), `+` se nayi add karo
4. Name = `PHPSESSID`, Value = authenticated session ID
5. Page refresh → bina login kiye authenticated ho jaoge

> ⚠️ **Security implication:** Ek valid cookie hi authentication ke liye kaafi hai kai apps mein — ye **session hijacking / XSS via cookie theft** attacks ka core concept hai.

## JSON Data (Modern APIs)

Kai backend endpoints JSON format mein POST data accept karte hain, form-encoded ki jagah.

**DevTools se identify karna:** Network tab → request click → payload dikhega:
```json
{"search":"london"}
```

**Confirm karo ki JSON hai:** Request headers check karo (right-click → **Copy Request Headers**):
```
Content-Type: application/json
```

### cURL Se JSON POST Request

```bash
curl -X POST -d '{"search":"london"}' \
  -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' \
  -H 'Content-Type: application/json' \
  http://<SERVER_IP>:<PORT>/search.php
```

Response:
```json
["London (UK)"]
```

> **Important:** `Content-Type: application/json` header zaroori hai — bina iske server request ko sahi se parse nahi karega, cookie bhi zaroori hai agar endpoint authenticated hai.

## DevTools Copy Shortcuts (Recap)

| Option | Use |
|---|---|
| **Copy as cURL** | Terminal mein direct run karne ke liye poora command |
| **Copy as Fetch** | Console tab (`Ctrl+Shift+K`) mein JS se request replicate karne ke liye |
| **Copy Request Headers** | Sirf headers dekhne/verify karne ke liye (e.g. Content-Type confirm karna) |

## Pentest Relevance
- Login forms directly cURL se automate ho sakte hain — **credential brute-forcing scripts** banane mein base
- Authenticated cookie mil jaye toh poori application front-end ke bina, directly API/endpoints test kiye ja sakte hain — **fast assessment workflow**
- JSON endpoints → injection testing (SQLi, NoSQLi, command injection) ke liye directly params manipulate karo bina UI ke
- Cookie ka structure/predictability check karo — agar session IDs predictable hain, session fixation/hijacking possible
- Missing/weak `Content-Type` validation → potential for request smuggling ya parser confusion attacks

---
*Source: HTB Academy - HTTP module*
