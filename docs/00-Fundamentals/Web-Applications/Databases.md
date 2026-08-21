# Databases

## Overview
Web apps back end databases use karte hain data store karne ke liye — assets (images/files), content (posts/updates), user data (username/password). Isse dynamic, per-user content possible hota hai.

Database choose karte time considerations: **speed**, **size** (large data handle karna), **scalability**, **cost**.

## Relational (SQL)
Data **tables, rows, columns** mein store hota hai. Tables ke beech **keys** se relationship banti hai.

Example: `users` table (`id`, `username`, `first_name`...) aur `posts` table (`id`, `user_id`, `date`, `content`...) — `id` ↔ `user_id` link karke ek query mein user + unke posts retrieve ho jate hain.

Tables ke beech relationship ko **Schema** kehte hain. Ek table ke multiple keys ho sakte hain (multiple tables se link karne ke liye — e.g. posts → comments).

Fast + reliable for large, well-structured datasets.

| DB | Notes |
|---|---|
| **MySQL** | Sabse common, free/open-source |
| **MSSQL** | Microsoft, Windows Server/IIS ke saath common |
| **Oracle** | Big business, reliable, costly |
| **PostgreSQL** | Free, open-source, easily extensible |

Others: SQLite, MariaDB, Amazon Aurora, Azure SQL

## Non-Relational (NoSQL)
Tables/rows/columns/keys/schema nahi hote — flexible storage models use karte hain. Undefined/unstructured data ke liye best, aur highly scalable.

**4 storage models:**
- **Key-Value** — JSON/XML mein key:value pairs (dictionary jaisa)
- **Document-Based** — complex JSON objects, metadata ke saath
- **Wide-Column**
- **Graph**

Key-Value example:
```json
{
  "100001": {"date": "01-01-2021", "content": "Welcome..."},
  "100002": {"date": "02-01-2021", "content": "First post..."}
}
```

| DB | Notes |
|---|---|
| **MongoDB** | Sabse common NoSQL, Document-Based, free/open-source |
| **ElasticSearch** | Huge datasets analyze/search ke liye optimized |
| **Apache Cassandra** | Highly scalable, faulty values gracefully handle karta hai |

Others: Redis, Neo4j, CouchDB, Amazon DynamoDB

## Use in Web Applications (PHP + MySQL example)
```php
// Connect
$conn = new mysqli("localhost", "user", "pass");

// Create DB
$sql = "CREATE DATABASE database1";
$conn->query($sql);

// Connect to specific DB + query
$conn = new mysqli("localhost", "user", "pass", "database1");
$query = "select * from table_1";
$result = $conn->query($query);
```

User input directly query mein use hone ka common (aur risky) example:
```php
$searchInput = $_POST['findUser'];
$query = "select * from users where name like '%$searchInput%'";
$result = $conn->query($query);

while($row = $result->fetch_assoc()){
    echo $row["name"]."<br>";
}
```

## Pentest Relevance
- Ye exact pattern (user input directly query mein concatenate) → **SQL Injection** ka root cause hai
- DB type identify karna (SQL vs NoSQL) exploitation technique decide karta hai

---
*Source: HTB Academy - Introduction to Web Applications*
