---
id: "finguard_04_01"
title: "Daily Transaction Scan"
type: "coding"
xp: 100
---

# Daily Transaction Scan

FinGuard doesn't process one transaction at a time. Every night, it must scan **all** transactions from the day, apply fraud rules, and generate a report.

## The Bridge

In **The Sentinel**, you learned to evaluate a single transaction. Now you'll learn to process **collections** of transactions automatically.

## The Analogy: The Assembly Line

A car factory doesn't build one car from start to finish, then move to the next. It has an **assembly line** where each station performs one operation on **every car** that passes through.

A `for` loop is your assembly line.

## The `for` Loop

```python
transactions: list[str] = ["TXN-001", "TXN-002", "TXN-003"]

for txn_id in transactions:
    print(f"Processing: {txn_id}")
```

Each iteration:
1. Python takes the **next item** from the list
2. Assigns it to `txn_id`
3. Runs the code block
4. Repeats until the list is exhausted

## Processing Transaction Amounts

```python
from decimal import Decimal

amounts: list[Decimal] = [
    Decimal("500.00"),
    Decimal("15000.00"),
    Decimal("250.00")
]

total: Decimal = Decimal("0.00")

for amount in amounts:
    total = total + amount

print(f"Total: ${total}")  # $15,750.00
```

## The "Pro" Tip

> **Use descriptive loop variable names. `txn` is better than `t` or `x`.**

```python
# ✅ Clear
for transaction in daily_transactions:
    process(transaction)

# ❌ Cryptic
for t in dt:
    p(t)
```

## Task

Process the daily transaction batch:
1. Loop through each transaction
2. Count total transactions
3. Sum total amount
4. Count how many are flagged (amount > 10,000)

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Today's transaction batch
transactions: list[dict] = [
    {"id": "TXN-001", "amount": Decimal("500.00")},
    {"id": "TXN-002", "amount": Decimal("15000.00")},
    {"id": "TXN-003", "amount": Decimal("2500.00")},
    {"id": "TXN-004", "amount": Decimal("75000.00")},
    {"id": "TXN-005", "amount": Decimal("800.00")},
]

# Initialize counters
total_count: int = 0
total_amount: Decimal = Decimal("0.00")
flagged_count: int = 0

# Process each transaction



# Print the daily summary
print(f"=== Daily Transaction Summary ===")
print(f"Total Transactions: {total_count}")
print(f"Total Amount: ${total_amount:,.2f}")
print(f"Flagged (>$10,000): {flagged_count}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert total_count == 5, "total_count should be 5"
assert total_amount == Decimal("93800.00"), "total_amount should be Decimal('93800.00')"
assert flagged_count == 2, "flagged_count should be 2 (TXN-002 and TXN-004)"
