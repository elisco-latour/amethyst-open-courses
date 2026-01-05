---
id: "finguard_00_02"
title: "Data at Rest"
type: "coding"
xp: 100
---

# Data at Rest

Data doesn't float in the air. It has to **live somewhere**. When data is saved and waiting to be used, we call it **Data at Rest**.

## The Analogy: The Filing System

A bank doesn't keep all documents in one giant pile. They have:

| Storage Type | Bank Equivalent | Computer Equivalent |
|-------------|-----------------|---------------------|
| **Filing Cabinet** | Paper folders organized alphabetically | **Files** on disk (CSV, JSON, text) |
| **Vault Database** | Ledger books with cross-references | **Databases** (PostgreSQL, SQLite) |
| **Off-site Archive** | Warehouse for old records | **Object Storage** (S3, Azure Blob) |

Each has tradeoffs:

### Files (Simple, but limited)
- ✅ Easy to create and read
- ✅ Human-readable (you can open in Notepad)
- ❌ No protection against corruption
- ❌ Hard to search across millions of records

### Databases (Powerful, but complex)
- ✅ Can search billions of records in milliseconds
- ✅ Guarantees data won't be corrupted (ACID)
- ❌ Requires setup and maintenance
- ❌ Needs a query language (SQL)

### Object Storage (Scalable, but slow)
- ✅ Can store petabytes cheaply
- ✅ Never runs out of space
- ❌ Slower to access than local files
- ❌ Costs money per request

## The Constraint: Durability vs Speed

Here's the fundamental tradeoff:

> **Fast storage loses data. Safe storage is slow.**

- **RAM (Memory)**: Lightning fast, but erased when power goes off
- **SSD (Disk)**: Slower, but survives restarts
- **Database**: Even slower, but guarantees your data is never lost

FinGuard must NEVER lose a transaction. A $50,000 wire transfer can't just "disappear."

## Task

This code shows three different ways to represent the same account data.

Each format has different tradeoffs. Run it to see the differences.

<!-- SEPARATOR -->

# seed_code
# The same account data in three different formats

# FORMAT 1: Plain text (human-readable, but unstructured)
text_format = "Account: 1001, Name: Alice Smith, Balance: 5000.00"

# FORMAT 2: CSV (structured, good for spreadsheets)
csv_format = "account_id,name,balance\n1001,Alice Smith,5000.00"

# FORMAT 3: JSON (structured, good for APIs and configs)
json_format = '{"account_id": 1001, "name": "Alice Smith", "balance": 5000.00}'

print("=== TEXT FORMAT ===")
print(text_format)
print(f"Size: {len(text_format)} bytes\n")

print("=== CSV FORMAT ===")
print(csv_format)
print(f"Size: {len(csv_format)} bytes\n")

print("=== JSON FORMAT ===")
print(json_format)
print(f"Size: {len(json_format)} bytes")

<!-- SEPARATOR -->

# validation_code
assert "1001" in text_format, "text_format should contain account 1001"
assert "account_id" in csv_format, "csv_format should have a header row"
assert "account_id" in json_format, "json_format should have account_id key"
