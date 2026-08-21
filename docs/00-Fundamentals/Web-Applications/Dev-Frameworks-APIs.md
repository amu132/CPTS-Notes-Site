# Development Frameworks & APIs

## Web Frameworks
Modern web apps scratch se banana complex hai — isliye **frameworks** use hote hain (common functionality jaise user registration ready-made milti hai).

| Framework | Language | Used by |
|---|---|---|
| **Laravel** | PHP | Startups/small companies |
| **Express** | Node.js | PayPal, Yahoo, Uber, IBM, MySpace |
| **Django** | Python | Google, YouTube, Instagram, Mozilla, Pinterest |
| **Rails** | Ruby | GitHub, Hulu, Twitch, Airbnb |

Popular sites usually multiple frameworks/servers combine karte hain, sirf ek nahi.

## APIs (Application Programming Interface)
Front end ↔ back end communication ke liye APIs use hoti hain. Front end request bhejta hai (specific task + input), back end process karke response deta hai, front end render karta hai.

### Query Parameters
Basic method — GET/POST se values pass karna:
- GET: `/search.php?item=apples`
- POST: data body mein (`item=apples`)

Ek hi page multiple types ka input handle kar sakta hai isse.

### Web APIs
API = interface jo batata hai app dusri apps se kaise interact kare. Web APIs usually HTTP pe accessed, web servers handle/translate karte hain.

Example: weather API city name/id leke JSON mein current weather return karta hai. Twitter API tweets JSON/XML mein retrieve karta hai.

### SOAP
XML-based standard — request aur response dono XML mein.
```xml
<?xml version="1.0"?>
<soap:Envelope xmlns:soap="http://www.example.com/soap/soap/"
soap:encodingStyle="http://www.w3.org/soap/soap-encoding">
<soap:Header></soap:Header>
<soap:Body><soap:Fault></soap:Fault></soap:Body>
</soap:Envelope>
```
Structured/complex/binary data ke liye useful, stateful objects share karne ke liye bhi. Lekin beginners ke liye complex, choti queries ke liye bhi lambe requests chahiye.

### REST (Representational State Transfer)
Data URL path se share hota hai (`/search/users/1`), response usually **JSON**.

Query Parameters se alag — REST ek hi type ka input directly URL path mein expect karta hai (naam/type specify kiye bina). Search/sort/filter jaise use cases ke liye useful — APIs ko smaller/modular pieces mein break karta hai.

HTTP methods REST mein:
- **GET** — retrieve data
- **POST** — create data (non-idempotent)
- **PUT** — create/replace data (idempotent)
- **DELETE** — remove data

Example JSON response (`GET /category/posts/`):
```json
{
  "100001": {"date": "01-01-2021", "content": "Welcome..."},
  "100002": {"date": "02-01-2021", "content": "First post..."}
}
```

## Pentest Relevance
- Framework/CMS identify karna → known CVEs check karne ke liye recon step
- REST vs SOAP endpoints alag test approach maangte hain
- API endpoints (especially undocumented/hidden) recon mein high priority — often less protected than main app

---
*Source: HTB Academy - Introduction to Web Applications*
