# Automatic Modification (Match & Replace)

## Overview
Kabhi kabhi hume **har outgoing request** ya **har incoming response** pe ek fixed change apply karna hota hai. Manually har request intercept karke edit karna slow hai — isliye **rule-based automatic modification** use karte hain (Burp: Match & Replace, ZAP: Replacer).

## Automatic Request Modification

### Burp — Match and Replace

**Location:** `Proxy -> Proxy Settings -> HTTP match and replace rules -> Add`

**Example: User-Agent replace karna** (filters bypass karne ke liye useful):

| Setting | Value |
|---|---|
| Type | Request header |
| Match | `^User-Agent.*$` (regex) |
| Replace | `User-Agent: HackTheBox Agent 1.0` |
| Regex match | `True` |

Rule add hone ke baad **automatically har request** mein User-Agent replace ho jayega — verify karne ke liye kisi bhi site pe visit karo aur intercepted request check karo.

### ZAP — Replacer

**Location:** Shortcut `Ctrl+R`, ya Options menu -> **Replacer**

**Same rule ZAP mein:**

| Setting | Value |
|---|---|
| Description | HTB User-Agent |
| Match Type | Request Header (adds if not present) |
| Match String | `User-Agent` |
| Replacement String | `HackTheBox Agent 1.0` |
| Enable | `True` |

> **Note:** ZAP mein bhi **Request Header String** option regex ke saath available hai (Burp jaisa).

**Initiators (ZAP-specific):** Ye control karta hai rule kahan apply ho. Default = `Apply to all HTTP(S) messages`.

**Verify:** `Ctrl+B` se interception ON karo, koi bhi page visit karo — User-Agent header automatically replaced dikhega.

## Automatic Response Modification

**Problem:** Manual intercept se kiya gaya change **temporary** hota hai — page refresh hone pe wapas original ho jata hai.

**Solution:** Response body/header pe bhi match-and-replace rule laga do — permanent-feeling change ban jata hai.

### Example — Input Field Restriction Bypass (Persistent)

**Burp — Rule 1 (input type change):**

| Setting | Value |
|---|---|
| Type | Response body |
| Match | `type="number"` |
| Replace | `type="text"` |
| Regex match | `False` |

**Burp — Rule 2 (max length change):**

| Setting | Value |
|---|---|
| Type | Response body |
| Match | `maxlength="3"` |
| Replace | `maxlength="100"` |
| Regex match | `False` |

Page refresh (`Ctrl+Shift+R`) karne ke baad — input field ab kisi bhi text/length ke liye open ho jayegi, bina baar-baar intercept kiye.

### ZAP — Same Rules (Replacer)

Rule 1: Match Type = Response Body String, Match Regex = False, Match String = `type="number"`, Replacement String = `type="text"`, Enable = True

Rule 2: Match Type = Response Body String, Match Regex = False, Match String = `maxlength="3"`, Replacement String = `maxlength="100"`, Enable = True

## Pentest Relevance
- Filter/WAF bypass: User-Agent, custom headers automatically spoof karna
- Client-side restriction bypass: maxlength, type=number, JS validation permanently disable
- Persistent payload injection: repeated manual typing avoid ho jati hai
- Testing ke baad rules disable/remove karna mat bhoolna, warna normal browsing mein bhi apply hoti rahengi

---
*Source: HTB Academy - Using Web Proxies*
