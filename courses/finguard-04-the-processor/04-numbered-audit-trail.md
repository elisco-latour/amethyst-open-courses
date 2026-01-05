---
id: "finguard_04_04"
title: "Numbered Audit Trail"
type: "coding"
xp: 100
---

# Numbered Audit Trail

Audit reports need transaction numbers: "Transaction 1 of 100", "Transaction 2 of 100", etc. You need both the **item** and its **position**.

## The `enumerate()` Function

`enumerate()` gives you both the index and the value:

```python
transactions: list[str] = ["TXN-001", "TXN-002", "TXN-003"]

for index, txn_id in enumerate(transactions):
    print(f"{index}: {txn_id}")
# 0: TXN-001
# 1: TXN-002
# 2: TXN-003
```

## Starting at 1

```python
# Start counting from 1 (more human-friendly)
for number, txn_id in enumerate(transactions, start=1):
    print(f"{number}: {txn_id}")
# 1: TXN-001
# 2: TXN-002
# 3: TXN-003
```

## The Analogy: The Audit Checklist

An auditor doesn't just check items — they number each check:
- ☑ Item 1 of 5: Verified transaction amount
- ☑ Item 2 of 5: Verified sender account
- ...

`enumerate()` provides that numbering automatically.

## Why Not Use `range(len(...))`?

```python
# ❌ Verbose and error-prone
for i in range(len(transactions)):
    txn = transactions[i]
    print(f"{i}: {txn}")

# ✅ Pythonic
for i, txn in enumerate(transactions):
    print(f"{i}: {txn}")
```

## The "Pro" Tip

> **Use `enumerate(items, start=1)` when you need human-readable numbering.**

## Task

Generate an audit trail for the day's transactions:
```
=== Daily Audit Trail ===
[1/4] TXN-001: $500.00 - NORMAL
[2/4] TXN-002: $15000.00 - FLAGGED
[3/4] TXN-003: $2500.00 - NORMAL
[4/4] TXN-004: $75000.00 - FLAGGED
```

Store each line in the `audit_trail` list.

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

total: int = len(transactions)
audit_trail: list[str] = []

# Generate numbered audit trail



# Print the audit trail
print("=== Daily Audit Trail ===")
for line in audit_trail:
    print(line)

<!-- SEPARATOR -->

# validation_code
assert len(audit_trail) == 4, "audit_trail should have 4 entries"
assert "[1/4]" in audit_trail[0], "First entry should start with [1/4]"
assert "[4/4]" in audit_trail[3], "Last entry should start with [4/4]"
assert "FLAGGED" in audit_trail[1], "TXN-002 should be FLAGGED"
assert "FLAGGED" in audit_trail[3], "TXN-004 should be FLAGGED"
assert "NORMAL" in audit_trail[0], "TXN-001 should be NORMAL"
