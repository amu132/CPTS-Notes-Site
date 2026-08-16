# CRUD APIs

## Overview
APIs database ke saath directly interact karne ka tareeka dete hain — URL mein **table** aur **row/entity** specify karo, phir HTTP method se decide hota hai kya operation perform hoga.

```
curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london ...
```
Yahan `city` = table, `london` = specific row/entity.

## CRUD ↔ HTTP Method Mapping

| Operation | HTTP Method | Description |
|---|---|---|
| **C**reate | `POST` | Naya data table mein add karta hai |
| **R**ead | `GET` | Specified entity read karta hai |
| **U**pdate | `PUT` | Poori entity ki data update karta hai |
| **D**elete | `DELETE` | Specified row remove karta hai |

> **Note:** `PATCH` bhi update ke liye use hota hai — lekin **partial** update ke liye (sirf kuch fields), jabki `PUT` **poori entity** replace karta hai. `OPTIONS` method se pata chal sakta hai server konsa accept karta hai.

Ye same principle CRUD APIs, REST APIs, aur most database-backed APIs mein use hota hai. Access control (user privileges) decide karta hai kaun kya kar sakta hai.

## Read (GET)

```bash
# Specific entity
curl http://<SERVER_IP>:<PORT>/api.php/city/london
# → [{"city_name":"London","country_name":"(UK)"}]

# Pretty-print with jq
curl -s http://<SERVER_IP>:<PORT>/api.php/city/london | jq

# Partial match search
curl -s http://<SERVER_IP>:<PORT>/api.php/city/le | jq

# Empty string = saari entries
curl -s http://<SERVER_IP>:<PORT>/api.php/city/ | jq
```

`-s` = silent (progress hide), `| jq` = JSON ko readable format mein pretty-print karta hai.

## Create (POST)

```bash
curl -X POST http://<SERVER_IP>:<PORT>/api.php/city/ \
  -d '{"city_name":"HTB_City", "country_name":"HTB"}' \
  -H 'Content-Type: application/json'
```

Verify karo entry ban gayi:
```bash
curl -s http://<SERVER_IP>:<PORT>/api.php/city/HTB_City | jq
```

## Update (PUT)

URL mein **jo entity edit karni hai uska naam specify karna zaroori hai** (warna API ko pata nahi chalega kya update karna hai):

```bash
curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london \
  -d '{"city_name":"New_HTB_City", "country_name":"HTB"}' \
  -H 'Content-Type: application/json'
```

Is example mein `london` entry replace ho gayi `New_HTB_City` se. Verify:
```bash
curl -s http://<SERVER_IP>:<PORT>/api.php/city/london | jq        # empty — no longer exists
curl -s http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City | jq  # naya entry milega
```

> **Note:** Kuch APIs mein Update operation **upsert** ki tarah kaam karta hai — agar entity exist nahi karti, toh create kar deta hai. Sab APIs aisa nahi karte, test karke confirm karo.

## Delete (DELETE)

```bash
curl -X DELETE http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City
```

Verify — deleted entry read karne pe empty array milega:
```bash
curl -s http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City | jq
# → []
```

## Authentication with CRUD APIs

Real-world APIs mein sab users ko sab operations allow nahi hote. Authenticate karne ke liye:
- **Cookie** pass karo (`-b 'PHPSESSID=...'`)
- Ya **Authorization header** pass karo (e.g. `Bearer <JWT token>`)

Agar koi bhi authentication ke bina modify/delete kar paye, ye ek **major vulnerability** hai (Broken Access Control).

## Pentest Relevance
- 🎯 **IDOR (Insecure Direct Object Reference):** Agar `PUT`/`DELETE` par proper authorization check nahi hai, toh koi bhi user ID/entity name badal ke doosre users ka data modify/delete kar sakta hai
- **Mass assignment:** POST/PUT body mein extra fields daal ke dekho (e.g. `"is_admin":true`) — agar server blindly accept karta hai, privilege escalation ho sakta hai
- **Method tampering:** `GET` allowed hai but `PUT`/`DELETE` block dikh raha hai? `OPTIONS` request se check karo actually kaunse methods server accept karta hai — misconfiguration common hai
- `jq` filtering se bulk data (poori table) enumerate karke sensitive info leak check karo (`/api.php/city/` empty string trick)
- CRUD endpoints automate karke script bana sakte ho poore workflow (create → read → update → delete) test karne ke liye — CTF/bug bounty dono mein useful

---
*Source: HTB Academy - HTTP module (Complete)*
