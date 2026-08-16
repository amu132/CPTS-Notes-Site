# HTTP Fundamentals

## Overview
HTTP (HyperText Transfer Protocol) application-layer protocol hai jo web resources access karne ke liye use hota hai. Client-server model follow karta hai — client request bhejta hai, server process karke response return karta hai.

- Default port: **80** (HTTP), **443** (HTTPS)
- Client requests a resource → server processes → returns response

## URL Structure

| Component | Example | Notes |
|---|---|---|
| Scheme | `http://`, `https://` | Protocol identifier, ends with `://` |
| User Info | `admin:password@` | Optional credentials, separated by `@` |
| Host | `inlanefreight.com` | Hostname or IP |
| Port | `:80` | Default: 80 (http), 443 (https) if omitted |
| Path | `/dashboard.php` | Resource location (file/folder) |
| Query String | `?login=true` | Params after `?`, multiple joined with `&` |
| Fragment | `#status` | Client-side only, points to section in page |

Mandatory fields: **scheme + host**. Baaki sab optional hain.

## HTTP Request Flow

1. User browser mein URL type karta hai (e.g. `inlanefreight.com`)
2. Browser DNS query bhejta hai domain resolve karne ke liye → IP milta hai
3. Browser GET request bhejta hai resolved IP ke port 80 pe, path `/` ke liye
4. Server request process karta hai, default index file return karta hai (e.g. `index.html`)
5. Response mein status code hota hai (e.g. `200 OK`)
6. Browser HTML render karke user ko dikhata hai

> **Note:** Browser pehle local `/etc/hosts` file check karta hai DNS resolve karne se pehle. Isme manually IP + domain add kar sakte hain custom resolution ke liye.

## cURL — Command Line HTTP Client

cURL scripting/automation ke liye ideal hai — raw response deta hai (HTML render nahi karta jaise browser karta hai).

```bash
# Basic GET request
curl http://info.cern.ch/

# Download and save with original filename
curl -O http://info.cern.ch/index.html

# Download and save with custom filename
curl -o myfile.html http://info.cern.ch/index.html

# Silent mode (no progress/status output)
curl -s -O http://info.cern.ch/index.html

# Help menu
curl -h

# Full help / man page
man curl
```

### Useful Flags Reference

| Flag | Purpose |
|---|---|
| `-d` | POST data bhejne ke liye |
| `-i` | Response headers include karo output mein |
| `-o` | Output file specify karo (custom name) |
| `-O` | Remote file name se save karo |
| `-s` | Silent mode (status hide) |
| `-u` | Auth (user:password) |
| `-A` | Custom User-Agent set karo |
| `-v` | Verbose mode (debugging ke liye useful) |

## Pentest Relevance
- cURL browser se better hai jab focus **request/response** analysis pe ho — fast, scriptable, automatable
- `-v` flag headers/handshake debug karne mein kaam aata hai
- `-A` se User-Agent spoof karke WAF/bot detection bypass test kiya ja sakta hai

---
*Source: HTB Academy - HTTP module*
