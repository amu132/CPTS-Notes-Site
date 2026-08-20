# JavaScript

## Overview
JavaScript front end ki functionality/interactivity control karta hai (HTML/CSS sirf look control karte hain). Bina JS ke page mostly static rehta. Back-end bhi possible hai (NodeJS).

## Loading JS
```html
<script type="text/javascript">
..JavaScript code..
</script>
```

Remote file load:
```html
<script src="./script.js"></script>
```

Example usage:
```javascript
document.getElementById("button1").innerHTML = "Changed Text!";
```

## Usage
- Real-time page updates/content changes
- User input accept + process
- Ajax se back end ke saath HTTP requests (bina page reload ke)
- CSS ke saath advanced animations
- Browser ke JS engine se client-side hi execute hota hai — server dependency kam, fast

## Frameworks
Pure JS se pura app banana inefficient hota hai large scale pe — isliye frameworks:
- **Angular**
- **React**
- **Vue**
- **jQuery**

Ye login/registration jaisi common functionality readymade libraries se provide karte hain, aur dynamic HTML rendering enable karte hain (static HTML ki jagah).

## Pentest Relevance
- Client-side execution = attack surface for **XSS**, DOM manipulation
- Framework identify karna recon ka part hai (known CVEs check karne ke liye)

---
*Source: HTB Academy - Introduction to Web Applications*
