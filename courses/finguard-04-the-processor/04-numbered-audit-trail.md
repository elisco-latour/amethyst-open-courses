---
id: "finguard_04_04"
title: "The Indexed Audit"
type: "coding"
xp: 100
---

# The Indexed Audit

When reading a long manifesto of transactions, saying "The transaction" is vague. Saying "Transaction #42" is precise.

In Python, we often need both the **Data** (the transaction) and its **Index** (its position in the list).

## The `enumerate()` Function

Instead of managing a manual counter (`i = 0`), Python extracts the index automatically.

```python
emails = ["ceo@finguard.io", "admin@finguard.io"]

# enumerate() yields a tuple: (index, item)
for index, email in enumerate(emails, start=1):
    print(f"Line {index}: {email}")
```

## Why `enumerate`?

It is **Pythonic**. It reduces visual noise and prevents "Off-By-One" errors common with manual counters.

## Task

Create a numbered Audit Trail string for each transaction.
Format: `[Line/Total] ID: $Amount`

1.  Use `enumerate` on the `transactions` list with `start=1`.
2.  For each item, format a string: `[line_number/total_count] TXN-ID: $Amount`.
    *   Example: `[1/4] TXN-001: $500.00`
3.  Append this string to `audit_trail`.

*Note: The total count is provided as `total_count`.*

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Today's transactions
transactions: list[dict] = [
    {"id": "TXN-001", "amount": Decimal("500.00")},
    {"id": "TXN-002", "amount": Decimal("15000.00")},
    {"id": "TXN-003", "amount": Decimal("2500.00")},
    {"id": "TXN-004", "amount": Decimal("75000.00")},
]

total_count: int = len(transactions)
audit_trail: list[str] = []

# Generate Audit Trail



# Print
print("=== Daily Audit Trail ===")
for line in audit_trail:
    print(line)

<!-- SEPARATOR -->

# validation_code
# Check for correct formatting
assert len(audit_trail) == 4
assert audit_trail[0].startswith("[1/4]"), "Should start with [1/4]"
assert "TXN-001" in audit_trail[0]
