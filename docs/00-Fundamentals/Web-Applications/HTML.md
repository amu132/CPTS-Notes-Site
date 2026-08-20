# HTML

## Overview
HTML (HyperText Markup Language) front end ka core component hai — page ke basic elements (titles, forms, images etc.) define karta hai. Browser inhe interpret karke end-user ko display karta hai.

## Structure
HTML elements tree form mein hote hain (XML jaisa):
Har element open/close tag se bana hota hai (e.g. `<p>...</p>`). Tags mein `id`/`class` bhi ho sakta hai (CSS targeting ke liye):
```html
<p id="para1">...</p>
```

`<head>` = non-visible stuff (title, meta). `<body>` = visible content. `<style>` = CSS hold karta hai, `<script>` = JS hold karta hai.

## URL Encoding (Percent-Encoding)
Browsers URLs mein sirf ASCII use karte hain. Non-ASCII/unsafe characters ko `%` + 2 hex digits se encode karna padta hai.

| Char | Encoding |
|---|---|
| space | %20 |
| ! | %21 |
| " | %22 |
| # | %23 |
| $ | %24 |
| % | %25 |
| & | %26 |
| ' | %27 |
| ( | %28 |
| ) | %29 |

Space ko `+` ya `%20` se replace kiya jata hai. Burp Suite ke Decoder tool se encoding/decoding easily test kar sakte hain.

## DOM (Document Object Model)
Har HTML element ek DOM node hota hai — `document.head`, `document.h1` etc. se access hota hai (JS ke through).

3 parts:
- **Core DOM** — sab document types ke liye
- **XML DOM** — XML documents ke liye
- **HTML DOM** — HTML documents ke liye

## Pentest Relevance
- DOM structure samajhna important hai — kisi element ka source dekh ke issues spot kar sakte hain (id/tag/class se locate karke)
- **XSS** jaise front-end vulns DOM manipulate karte hain — existing elements change karna ya naye inject karna

---
*Source: HTB Academy - Introduction to Web Applications*
