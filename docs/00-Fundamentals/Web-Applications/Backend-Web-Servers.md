# Back End Servers & Web Servers

## Back End Server Overview
Back-end server = **hardware + OS** jo web application ke saare processes run karta hai. Ye **Data Access Layer** mein fit hota hai.

### Back-end Server Ke 3 Software Components
1. **Web Server** (Apache, NGINX, IIS)
2. **Database** (MySQL, MS SQL, Oracle)
3. **Development Framework** (PHP, C#, Java)

(Extra: hypervisors, containers, WAFs bhi ho sakte hain)

### Common Stack Combinations

| Stack | Components |
|---|---|
| LAMP | Linux, Apache, MySQL, PHP |
| WAMP | Windows, Apache, MySQL, PHP |
| WINS | Windows, IIS, .NET, SQL Server |
| MAMP | macOS, Apache, MySQL, PHP |
| XAMPP | Cross-platform, Apache, MySQL, PHP/PERL |

### Hardware
Server ki performance/stability hardware pe depend karti hai. Bade applications load ko **multiple back-end servers** mein distribute karte hain (data centers/cloud hosting).

---

## Web Servers

Web server = application jo back-end server pe chalti hai, **HTTP traffic handle** karti hai (client requests → routing → response). Common ports: **80** (HTTP), **443** (HTTPS).

### Common HTTP Response Codes (Web Server Context)

| Code | Meaning |
|---|---|
| `200 OK` | Request successful |
| `301 Moved Permanently` | Resource URL permanently changed |
| `302 Found` | Resource URL temporarily changed |
| `400 Bad Request` | Invalid syntax |
| `401 Unauthorized` | Unauthenticated access attempt |
| `403 Forbidden` | Access rights nahi hain |
| `404 Not Found` | Resource exist nahi karta |
| `405 Method Not Allowed` | Method known hai lekin disabled hai |
| `408 Request Timeout` | Idle connection timeout |
| `500 Internal Server Error` | Server situation handle nahi kar paya |
| `502 Bad Gateway` | Gateway ko invalid response mila |
| `504 Gateway Timeout` | Gateway response time out ho gaya |

### Request/Response Inspection via cURL

```bash
# Sirf headers dekho
curl -I https://academy.hackthebox.com

# Poora page source dekho
curl https://academy.hackthebox.com
```

## Popular Web Servers

### Apache (httpd)
- **~40%** internet websites host karta hai — sabse common
- Linux mein usually pre-installed, Windows/macOS pe bhi installable
- PHP ke saath commonly use hota hai (`mod_php` module chahiye)
- Open-source, well-documented, community-maintained
- Users: Apple, Adobe, Baidu

### NGINX
- **~30%** websites host karta hai, high-traffic sites mein sabse popular (~60% top 100k sites)
- **Async architecture** — kam memory/CPU mein zyada concurrent requests handle karta hai
- Open-source, reliable
- Users: Google, Facebook, Twitter, Netflix, HackTheBox

### IIS (Internet Information Services)
- **~15%** websites host karta hai
- Microsoft-developed, Windows Server pe chalta hai
- Mainly **.NET** apps ke liye, PHP/FTP bhi support karta hai
- **Active Directory integration** — Windows Auth se auto sign-in
- Users: Microsoft, Office365, Skype, Dell

> Other web servers: Apache Tomcat (Java apps), Node.js (JavaScript back-end)

## Pentest Relevance
- 🎯 `Server` header aur error pages se web server type identify karo → known CVEs check karo
- Web server type se stack guess karo (Apache→PHP likely, IIS→.NET likely) → targeted enumeration
- `mod_php`/extension misconfigurations Apache mein common vuln source hote hain
- IIS + Active Directory integration → auth bypass/AD-related attack vectors explore karne layak
- Response codes ka pattern (`403` vs `404` vs `500`) fuzzing/directory enumeration mein useful signal hai

---
*Source: HTB Academy - Introduction to Web Applications*
