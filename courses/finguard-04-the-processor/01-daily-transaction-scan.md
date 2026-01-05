---
id: "finguard_04_01"
title: "The Nightly Batch"
type: "coding"
xp: 100
---

# The Nightly Batch

In FinTech, "Real-Time" is expensive. Most heavy lifting happens at night when the markets are closed. This is called **Batch Processing**.

We don't process one transaction manually. We write scripts that wake up at 02:00 AM, read a list of millions of records, and process them one by one.

## The Iteration Pattern

The primary tool for batch processing is the **Loop**. It allows you to define logic *once* and apply it to *many* items.

## The `for` Loop

The `for` loop is designed to iterate over **Collections** (like Lists).

```python
# The Day's Manifest
transaction_ids: list[str] = ["TXN-A1", "TXN-B2", "TXN-C3"]

# The Batch Processor
for txn_id in transaction_ids:
    print(f"Verifying: {txn_id}...")
    # Logic applied to every single item
```

## Accumulation

A common pattern is **Accumulation**: Running totals.

```python
from decimal import Decimal

amounts: list[Decimal] = [Decimal("10.00"), Decimal("20.00"), Decimal("30.00")]
daily_revenue: Decimal = Decimal("0.00")  # The Accumulator

for amount in amounts:
    daily_revenue = daily_revenue + amount

print(f"Revenue: ${daily_revenue}")
```

## Engineering Standard: Naming

> **The Loop Variable Name Matters.** 
> It should be the *singular* version of the list name.

*   `for transaction in transactions:` (Perfect)
*   `for user in user_list:` (Good)
*   `for x in data:` (Unacceptable — Mystery Code)

## Task

You are writing the **End-of-Day Reconciliation** script.
Given a list of transaction dictionaries:

1.  Loop through `transactions`.
2.  Add every `amount` to `total_amount`.
3.  If a transaction's amount is greater than `10,000.00`, increment `flagged_count`.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# The Day's Ledger
transactions: list[dict] = [
    {"id": "TXN-001", "amount": Decimal("500.00")},
    {"id": "TXN-002", "amount": Decimal("15000.00")},
    {"id": "TXN-003", "amount": Decimal("2500.00")},
    {"id": "TXN-004", "amount": Decimal("75000.00")},
    {"id": "TXN-005", "amount": Decimal("800.00")},
]

# Accumulators
total_count: int = 0
total_amount: Decimal = Decimal("0.00")
flagged_count: int = 0

# Run the Batch



# Reconciliation Report
print(f"Total Volume: ${total_amount}")
print(f"Flagged Transactions: {flagged_count}")
print(f"Batch Size: {len(transactions)}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert total_amount == Decimal("93800.00"), "Total amount should be sum of all transactions"
assert flagged_count == 2, "Should detect 2 transactions > 10,000"
