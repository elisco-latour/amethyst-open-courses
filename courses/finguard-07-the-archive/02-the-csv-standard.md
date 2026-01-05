---
id: "finguard_07_02"
title: "The CSV Standard"
type: "coding"
xp: 100
---

# The CSV Standard

**CSV (Comma-Separated Values)** is the lingua franca of data exchange. Every bank, every system, every spreadsheet speaks CSV.

## Why CSV?

- **Universal**: Excel, databases, Python — everyone reads CSV
- **Human-readable**: You can open it in a text editor
- **Simple**: Just text with commas between values

## CSV Structure

```csv
transaction_id,date,amount,type
TXN-001,2025-01-15,500.00,DEPOSIT
TXN-002,2025-01-15,200.00,WITHDRAWAL
TXN-003,2025-01-15,1500.00,TRANSFER
```

First line = **headers** (column names)
Following lines = **data rows**

## Python's `csv` Module

```python
import csv

# Reading CSV
with open("transactions.csv", "r") as file:
    reader = csv.DictReader(file)
    for row in reader:
        print(row["transaction_id"], row["amount"])

# Writing CSV
with open("output.csv", "w", newline="") as file:
    fieldnames = ["id", "amount"]
    writer = csv.DictWriter(file, fieldnames=fieldnames)
    
    writer.writeheader()
    writer.writerow({"id": "TXN-001", "amount": "500.00"})
```

## The `newline=""` Mystery

On Windows, CSV files need `newline=""` to prevent double-spacing. Always include it.

## The Analogy: The Spreadsheet

CSV is a text representation of a spreadsheet:
- Each line = a row
- Commas = column separators
- First row = column headers

## The "Pro" Tip

> **Use `csv.DictReader` and `csv.DictWriter` instead of the basic reader/writer. Named columns prevent index errors.**

```python
# ❌ Fragile: depends on column position
row[0]  # What if columns are reordered?

# ✅ Robust: uses column name
row["transaction_id"]  # Always the right column
```

## Task

Parse the CSV transaction data and calculate total deposits and withdrawals.

<!-- SEPARATOR -->

# seed_code
import csv
from decimal import Decimal
from io import StringIO

# Simulated CSV file content
csv_content: str = """transaction_id,date,amount,type
TXN-001,2025-01-15,500.00,DEPOSIT
TXN-002,2025-01-15,200.00,WITHDRAWAL
TXN-003,2025-01-15,1500.00,TRANSFER
TXN-004,2025-01-15,3000.00,DEPOSIT
TXN-005,2025-01-15,750.00,WITHDRAWAL"""

# Parse CSV (using StringIO to simulate file)
total_deposits: Decimal = Decimal("0.00")
total_withdrawals: Decimal = Decimal("0.00")
transaction_count: int = 0

# In real code: with open("transactions.csv", "r") as file:
file = StringIO(csv_content)
reader = csv.DictReader(file)

for row in reader:
    pass  # Replace with your implementation


print("=== CSV Transaction Summary ===")
print(f"Total Transactions: {transaction_count}")
print(f"Total Deposits: ${total_deposits:,.2f}")
print(f"Total Withdrawals: ${total_withdrawals:,.2f}")
print(f"Net Flow: ${total_deposits - total_withdrawals:,.2f}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert transaction_count == 5, "Should process 5 transactions"
assert total_deposits == Decimal("3500.00"), "Deposits: 500 + 3000 = 3500"
assert total_withdrawals == Decimal("950.00"), "Withdrawals: 200 + 750 = 950"
