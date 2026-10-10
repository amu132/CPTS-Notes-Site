# Virtual Host and Subdomain Fuzzing

## Vhosts vs Subdomains

| Feature | Virtual Hosts | Subdomains |
|---|---|---|
| Identification | `Host` header (HTTP request) | DNS records, specific IP pe resolve hote hain |
| Purpose | Multiple websites ek server pe host karna | Website ke different sections/services organize karna |
| Security Risk | Misconfigured vhosts internal apps/data expose kar sakte hain | Subdomain takeover (mismanaged DNS records) |

**Virtual Hosting:** Ek IP/server pe multiple domains serve hote hain - server `Host` header dekh ke decide karta hai konsa content dena hai. Resource-efficient, cost-effective.

**Subdomains:** Primary domain ka hierarchical extension (e.g. `blog.example.com`, `shop.example.com`) - DNS se specific IP pe resolve hote hain.

## Gobuster — VHost Fuzzing

**Setup — hosts file mein vhost add karo:**
```bash
echo "IP inlanefreight.htb" | sudo tee -a /etc/hosts
```

**Command:**
```bash
gobuster vhost -u http://inlanefreight.htb:81 -w /usr/share/seclists/Discovery/Web-Content/common.txt --append-domain
```

| Flag | Purpose |
|---|---|
| `vhost` | Gobuster ka vhost fuzzing mode |
| `-u` | Base target URL |
| `-w` | Wordlist (vhost names) |
| `--append-domain` | Har wordlist word ke saath base domain append karo (e.g. `admin` + `inlanefreight.htb` = `admin.inlanefreight.htb`) |

**Kaam kaise karta hai:** Har word ke saath domain append karke `Host` header mein set karta hai, response status/size analyze karta hai.

**Result example:**Found: admin.inlanefreight.htb:81 Status: 200 [Size: 100]  `200 OK` = valid vhost mila jo publicly advertised nahi tha.

## Gobuster — Subdomain (DNS) Fuzzing

```bash
gobuster dns -d inlanefreight.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

| Flag | Purpose |
|---|---|
| `dns` | Gobuster ka DNS/subdomain fuzzing mode |
| `-d` | Target domain |
| `-w` | Subdomain wordlist |

**Kaam kaise karta hai:** Wordlist se subdomain names generate karta hai, target domain ke saath append karta hai, DNS query se resolve karne ki koshish karta hai. Resolve ho gaya = valid subdomain.

**Result example:**Found: www.inlanefreight.com
Found: blog.inlanefreight.com
> ⚠️ **Version Note:** Naye Gobuster release mein `-d` ab **delay between requests** set karta hai, domain nahi! Domain specify karne ke liye `--do` ya `--domain` use karo.

## Pentest Relevance
- Vhost fuzzing se **hidden admin panels/internal apps** discover ho sakte hain jo same server pe host hain lekin publicly linked nahi
- Subdomain enumeration **attack surface mapping** ka core step hai - forgotten/staging subdomains often weak security rakhte hain
- `Host` header manipulation samajhna zaroori hai - agar server misconfigured hai, ye internal-only content expose kar sakta hai

---
*Source: HTB Academy - Fuzzing Web Applications*
