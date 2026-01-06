---
id: "finguard_00_02"
title: "Data at Rest: Where Information Lives"
type: "coding"
xp: 100
---

# Data at Rest

In the previous chapter, you learned that computers store numbers as patterns of switches (binary). But there's a problem: **when you turn off the computer, all those switches reset**.

This is why we need to save data to files. In banking, we call this **persistence** — making sure information survives even when systems restart.

## The Format Question

When you save an account record to a file, you have to choose a format. Think of it like choosing how to write a note:

**Option 1: Plain Text (like a log)**
```
Account 1001 belongs to Alice Smith with balance 5000.00
```
This is easy to read, but hard for a program to understand. Where does the name end? Is "Smith" the last name or part of the balance?

**Option 2: Comma-Separated Values (CSV)**
```
1001,Alice Smith,5000.00
```
Compact and simple. But if Alice's name was "Smith, Alice" (with a comma), we'd have a problem!

**Option 3: Structured Format (JSON)**
```json
{"account_id": 1001, "name": "Alice Smith", "balance": 5000.00}
```
Each piece of data is labeled. A program can easily find the "name" field. But it takes more space.

## The Trade-Off

There's always a trade-off:
- **Human-readable** formats are easier to inspect but take more storage space
- **Compact** formats save space but are harder to debug

In FinGuard, we'll use both — the right format for the right job.

## Task

A new customer has opened an account. Create the three different representations of their record:
1. `plain_text` — A simple sentence describing the account
2. `csv_record` — A comma-separated string with header and data rows
3. `json_record` — A JSON-formatted string with labeled fields

Use these values: Account ID `2001`, Name `Bob Chen`, Balance `12500.00`

<!-- SEPARATOR -->

# seed_code
# Customer Record: Account 2001, Bob Chen, $12,500.00

# Plain text format (human readable sentence)
plain_text = 

# CSV format (header row + data row, separated by newline \n)
csv_record = 

# JSON format (use double quotes for keys and string values)
json_record = 

# Compare the storage cost of each format
print("=== FORMAT COMPARISON ===")
print(f"Plain Text ({len(plain_text)} bytes): {plain_text}")
print(f"CSV ({len(csv_record)} bytes): {csv_record}")
print(f"JSON ({len(json_record)} bytes): {json_record}")

<!-- SEPARATOR -->

# validation_code
assert "2001" in plain_text, "Plain text should contain account ID 2001"
assert "Bob Chen" in plain_text, "Plain text should contain the customer name"
assert "account_id" in csv_record or "2001" in csv_record, "CSV should contain the account data"
assert "\\n" in csv_record or "\n" in csv_record, "CSV should have header and data rows separated by newline"
assert "account_id" in json_record, "JSON should have labeled fields"
assert "2001" in json_record, "JSON should contain account ID"
