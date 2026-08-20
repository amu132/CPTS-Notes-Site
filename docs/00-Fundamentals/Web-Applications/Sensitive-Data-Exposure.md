# Sensitive Data Exposure

## Overview
Front end components client-side execute hote hain — attack hone pe directly back end ko threat nahi, lekin end-user (especially admin) ko expose kar sakte hain → unauthorized access, sensitive data leak, service disruption.

**Sensitive Data Exposure** = clear-text mein sensitive data available hona, usually page source mein (HTML source — back end code se alag, jo server pe hi rehta hai).

## Viewing Page Source
- Right-click → **View Page Source**
- Ya `Ctrl+U` (right-click disabled ho tab bhi kaam karta hai)
- Ya Burp Suite jaisa proxy use karo

Yahan HTML, JS, external links sab dikhte hain.

## What to Look For
- Login credentials / hashes chhupe comments mein
- Sensitive data external JS files mein
- Exposed links/directories
- Exposed user info

## Example
```html
<form action="action_page.php" method="post">
    <div class="container">
        <label for="uname"><b>Username</b></label>
        <input type="text" required>
        <label for="psw"><b>Password</b></label>
        <input type="password" required>

        <!-- TODO: remove test credentials test:test -->

        <button type="submit">Login</button>
    </div>
</form>
```
Developer comment mein test credentials reh gaye — agar remove nahi hue toh still valid ho sakte hain.

## Prevention (dev side, pentest checklist ke liye bhi useful)
- Source code mein sirf necessary code ho — extra comments/debug code nahi
- Data classification — client-side pe kya expose ho sakta hai decide karna
- Code review before deploy
- JS packing/obfuscation — automated tools se scanning mushkil banata hai

## Pentest Relevance
- Assessment ka **first step** hona chahiye: page source review karke low-hanging fruit dhundo (creds, hidden links, debug params)
- Automated tools available hain source scan karne ke liye directories/paths find karne ke liye
- Leverage karke back end tak access mil sakta hai

---
*Source: HTB Academy - Introduction to Web Applications*
