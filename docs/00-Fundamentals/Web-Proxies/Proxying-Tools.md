# Proxying Tools (Proxychains, Metasploit, etc.)

## Overview
Sirf browser traffic hi nahi - command-line tools aur thick-client applications ka traffic bhi web proxy (Burp/ZAP) se route kiya ja sakta hai. Isse un tools ke actual HTTP requests inspect/modify kar sakte hain.

Basic idea: Tool ka proxy setting `http://127.0.0.1:8080` (Burp/ZAP ka default listener) pe point kar do - jaise browser mein karte hain.

> Note: Proxying tools ko slow kar deti hai (extra hop). Isliye sirf tab use karo jab requests investigate karni ho, normal usage ke liye nahi.

## Proxychains (Linux)

`proxychains` ek utility hai jo kisi bhi CLI tool ka traffic specified proxy se route kar deta hai.

### Setup

Config file edit karo: `/etc/proxychains.conf`

Last line comment out karo aur ye add karo: 
`-q` flag use karo (quiet mode - connection info terminal pe print nahi hoga):

```bash
proxychains -q curl http://SERVER_IP:PORT
```

Request normally execute hoga, aur simultaneously Burp/ZAP mein bhi request capture ho jayega - verify karo proxy history mein.

## Metasploit

Metasploit modules ka HTTP traffic bhi proxy karke debug kiya ja sakta hai - `set PROXIES` option se.

```bash
msfconsole

msf6 > use auxiliary/scanner/http/robots_txt
msf6 auxiliary(scanner/http/robots_txt) > set PROXIES HTTP:127.0.0.1:8080
msf6 auxiliary(scanner/http/robots_txt) > set RHOST SERVER_IP
msf6 auxiliary(scanner/http/robots_txt) > set RPORT PORT
msf6 auxiliary(scanner/http/robots_txt) > run
```

Module run hone ke baad, proxy tool mein history check karo - module ka actual HTTP request dikhega.

> Ye method kisi bhi Metasploit scanner/exploit ke saath kaam karta hai.

## General Principle

1. Tool ki proxy setting dhundo (har tool ka apna tareeka hota hai)
2. Proxy value: `http://127.0.0.1:8080`
3. Tool run karo
4. Burp/ZAP proxy history mein request capture ho jayega - inspect/repeat/modify kar sakte ho

## Pentest Relevance
- CLI tools/scanners debug karna - exact request/params/headers dekh sakte ho
- Custom scripts ka traffic verify karna - apna automation script sahi format mein request bhej raha hai ya nahi
- Thick-client apps ka traffic bhi isi tareeke se intercept ho sakta hai
- Metasploit module ka traffic proxy karna - module exactly kya test kar raha hai samajhne mein madad
- Proxychains se naye/unfamiliar CLI tools ka behavior samajhna easy ho jata hai

---
*Source: HTB Academy - Using Web Proxies*
