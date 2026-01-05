---
id: "finguard_04_05"
title: "Alert Threshold"
type: "coding"
xp: 100
---

# Alert Threshold

Sometimes you need to **stop processing** when a condition is met, or **skip** certain items.

## The Analogy: The Production Line Halt

If a critical defect is found on an assembly line, you **stop everything** immediately (don't keep building broken cars).

If a minor issue is found, you **skip that item** and continue with the rest.

## `break` — Stop the Loop Entirely

```python
transactions: list[int] = [100, 200, 50000, 300]

for amount in transactions:
    if amount > 10000:
        print(f"⚠️ HALT! Critical amount: ${amount}")
        break  # Exit the loop immediately
    print(f"Processing: ${amount}")

# Output:
# Processing: $100
# Processing: $200
# ⚠️ HALT! Critical amount: $50000
# (300 is never processed)
```

## `continue` — Skip This Item, Continue to Next

```python
transactions: list[dict] = [
    {"id": "TXN-001", "status": "completed"},
    {"id": "TXN-002", "status": "cancelled"},
    {"id": "TXN-003", "status": "completed"},
]

for txn in transactions:
    if txn["status"] == "cancelled":
        continue  # Skip this one, move to next
    print(f"Processing: {txn['id']}")

# Output:
# Processing: TXN-001
# Processing: TXN-003
# (TXN-002 is skipped)
```

## The "Pro" Tip

> **Use `break` when you've found what you're looking for. Use `continue` to filter out items you don't want to process.**

## Task

Process a batch of transactions with these rules:
1. **Skip** any transaction with status `"cancelled"`
2. **Stop immediately** if you encounter an amount over $100,000 (critical fraud alert)
3. Track how many transactions were **processed** and if a **critical alert** was triggered

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Transaction batch
transactions: list[dict] = [
    {"id": "TXN-001", "amount": Decimal("500.00"), "status": "completed"},
    {"id": "TXN-002", "amount": Decimal("1500.00"), "status": "cancelled"},
    {"id": "TXN-003", "amount": Decimal("8000.00"), "status": "completed"},
    {"id": "TXN-004", "amount": Decimal("150000.00"), "status": "completed"},
    {"id": "TXN-005", "amount": Decimal("200.00"), "status": "completed"},
]

processed_count: int = 0
critical_alert: bool = False
critical_transaction: str = ""

# Process transactions with break/continue logic



# Print results
print(f"=== Batch Processing Results ===")
print(f"Processed: {processed_count} transactions")
print(f"Critical Alert: {critical_alert}")
if critical_alert:
    print(f"Alert Transaction: {critical_transaction}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert processed_count == 2, "processed_count should be 2 (TXN-001 and TXN-003)"
assert critical_alert == True, "critical_alert should be True (TXN-004 exceeded $100,000)"
assert critical_transaction == "TXN-004", "critical_transaction should be 'TXN-004'"
