---
id: "finguard_02_01"
title: "The Ledger: Lists"
type: "coding"
xp: 100
---

# The Ledger: Lists

In banking, a single transaction is meaningless. We need the **History**.
The history tells a story: Deposit, Withdrawal, Deposit.

## The Chronological List

The most important property of a Ledger is **Order**.
If you swap a "Deposit" and a "Withdrawal", you might cause an overdraft fee. The order matters.

In Python, the **List** preserves order.

```python
# A chronological ledger
ledger: list[str] = ["Deposit $500", "Withdraw $20", "deposit $100"]
```

## The "Append-Only" Rule

In FinGuard, we never *delete* from a list. We only **Append**.
If a transaction was a mistake, we append a *new* transaction to reverse it. This creates an audit trail.

## Task

Build a Ledger.
1. Start with 3 transactions.
2. **Append** a new one to the end.
3. Count the total volume.

<!-- SEPARATOR -->

# seed_code
# The Ledger for Account ACC-1001
# Order matters: [Oldest ... Newest]

transaction_history: list[str] = [
    "TXN-001: Credit $500.00",
    "TXN-002: Debit  $12.50",
    "TXN-003: Debit  $5.00"
]

# A new transaction occurs. Use .append() to add it to the END of the history.
new_txn = "TXN-004: Credit $100.00"
# (Write code here)


# Calculate total transactions
count = len(transaction_history)

print(f"Ledger State: {transaction_history}")
print(f"Total Entries: {count}")

<!-- SEPARATOR -->

# validation_code
assert len(transaction_history) == 4, "History should have 4 items"
assert transaction_history[-1] == new_txn, "The new entry should be last"
assert count == 4, "Count should be 4"
