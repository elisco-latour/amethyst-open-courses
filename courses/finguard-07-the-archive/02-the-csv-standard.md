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

# Test with different data
data = """ticker,volume
A,10
B,20
C,30"""

assert calculate_total_volume(data) == 60, "Should sum volume column: 10+20+30=60"

# Test with original data
assert calculate_total_volume(csv_data) == 225, "Should sum: 100+50+75=225"
