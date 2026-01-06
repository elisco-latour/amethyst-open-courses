---
id: "finguard_07_02"
title: "Flat-File Ingestion"
type: "coding"
xp: 100
---

# Flat-File Ingestion

In high-tech banking, we still rely on low-tech files. **CSV (Comma-Separated Values)** is the industry standard for bulk data transfer.

Why? Because it's a **Flat File**. No hidden formatting, no specialized software required.

## The Structure

```csv
id,amount,currency
TX001,500.00,USD
TX002,120.50,EUR
```

## Parsing: The Manual vs. The Tool

You *could* `.split(",")` manually. Don't. You will fail on `"Smith, John"`.
Use Python's `csv` module.

## The `DictReader` Pattern

Engineers prefer **named access** over positional access.

```python
import csv

# ❌ Risky: accessing by index [1]
# If columns shift, this breaks.
for row in csv.reader(f):
    print(row[1])

# ✅ Robust: accessing by name "amount"
# Order doesn't matter.
with open("data.csv") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row["amount"])
```

## Task

Ingest a partial CSV string and calculate the total volume.
1.  Use `io.StringIO` to mimic a file (provided).
2.  Use `csv.DictReader` to parse it.
3.  Sum the `volume` column (convert to int).

<!-- SEPARATOR -->

# seed_code
import csv
from io import StringIO

csv_data = """ticker,volume,price
AAPL,100,150.00
GOOGL,50,2800.00
MSFT,75,300.00"""

def calculate_total_volume(csv_text: str) -> int:
    """
    Parses CSV text and returns sum of 'volume' column.
    """
    val_file = StringIO(csv_text)
    # Use csv.DictReader(val_file)
    pass

# Integration
total = calculate_total_volume(csv_data)
print(f"Total Volume: {total}")

<!-- SEPARATOR -->

# validation_code
import csv
from io import StringIO

data = """ticker,volume
A,10
B,20
C,30"""

assert calculate_total_volume(data) == 60
print("Validation passed!")

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
