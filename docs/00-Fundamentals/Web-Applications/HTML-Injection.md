# HTML Injection

## Overview
Front end security ka ek major aspect hai user input **validate/sanitize** karna. Zyada tar validation back end pe hoti hai, par kai baar input back end tak jata hi nahi — front end pe hi process/render ho jata hai. Isliye validation **dono** front end aur back end pe zaroori hai.

## What is HTML Injection
**HTML Injection** tab hota hai jab unfiltered user input page pe display ho jata hai. Do tarike se ho sakta hai:
- Previously submitted data retrieve karke (e.g. DB se comment)
- Directly JavaScript se unfiltered input render karke (front end pe hi)

Agar user apna input jaisa chahe control kar sakta hai, wo raw **HTML code** submit kar sakta hai jo browser page ka part bana ke render kar dega.

## Attack Impact
- **Fake login forms** — attacker external/malicious login form inject kar sakta hai, jo credentials ko malicious server pe bhej de (harvesting ke liye)
- **Web page defacing** — page ka appearance change karna, malicious ads insert karna, ya pura page replace karna → company ko reputational damage

## Example

Vulnerable page — user input directly `innerHTML` mein daal diya ja raha hai, koi sanitization nahi:

```html
<!DOCTYPE html>
<html>
<body>
    <button onclick="inputFunction()">Click to enter your name</button>
    <p id="output"></p>

    <script>
        function inputFunction() {
            var input = prompt("Please enter your name", "");
            if (input != null) {
                document.getElementById("output").innerHTML = "Your name is " + input;
            }
        }
    </script>
</body>
</html>
```

**Test payload** — name field mein ye HTML input karke background image change ho jata hai:

```html
<style> body { background-image: url('https://academy.hackthebox.com/images/logo.svg'); } </style>
```

Input karte hi background instantly change ho jata hai — proof ki raw HTML render ho raha hai bina filtering ke.

**Note:** Ye sirf front-end pe ho raha hai (DB mein store nahi), isliye page refresh karte hi sab reset ho jayega — persistent nahi hai is example mein.

## Pentest Relevance
- HTML Injection test karne ka simplest tareeka: kisi bhi input field mein basic HTML/CSS payload try karo (jaise upar wala `<style>` tag) aur dekho render hota hai ya nahi
- Agar HTML Injection possible hai, usually **XSS (Cross-Site Scripting)** bhi possible hota hai — dono ka root cause same hai: missing input sanitization
- Isse ek foundation banta hai future modules ke liye (XSS module mein deep dive hoga)

---
*Source: HTB Academy - Introduction to Web Applications*
