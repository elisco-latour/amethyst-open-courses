---
id: "finguard_02_01"
title: "The Ledger: Lists"
type: "coding"
xp: 100
---

# The Ledger: Lists

A single transaction tells you almost nothing. What matters is the **history** — the sequence of deposits and withdrawals that tells the story of an account.

## Why Order Matters

In banking, the order of transactions is critical. Consider this:

```
Day 1: Balance $100
Day 2: Withdraw $80 (Balance: $20)
Day 3: Deposit $500 (Balance: $520)
```

Now swap Days 2 and 3:

```
Day 1: Balance $100
Day 2: Deposit $500 (Balance: $600)
Day 3: Withdraw $80 (Balance: $520)
```

Same final balance, but in the first scenario, the customer was dangerously close to zero. Order tells the real story.

## The Python List

A **list** is a collection that preserves order. Items stay in the sequence you put them:

```python
transactions = ["Deposit $500", "Withdraw $20", "Deposit $100"]
# Index:              0                1               2
```

You can access items by position (starting from 0):
- `transactions[0]` → `"Deposit $500"` (first item)
- `transactions[-1]` → `"Deposit $100"` (last item)

## The Append-Only Rule

In FinGuard, we **never delete** from a transaction history. If something was a mistake, we add a *new* transaction to reverse it. This creates an **audit trail**.

```python
# WRONG: Deleting history
del transactions[1]  # Never do this!

# RIGHT: Append a reversal
transactions.append("Reversal: Credit $20")
```

<div class="key-concept">
<h4>🔑 Key Concept: Append-Only Ledgers</h4>

Professional financial systems are append-only. Nothing is ever deleted — only corrected with new entries. This ensures every action can be audited.
</div>

## Task

Build a transaction ledger:
1. Create the initial `transaction_history` list with the three transactions shown
2. A new transaction `new_txn` has occurred — use `.append()` to add it to the end
3. Store the total count in `entry_count`

<!-- SEPARATOR -->

# seed_code
# Transaction Ledger for Account ACC-1001
# Format: list of transaction strings, oldest first

# 1. Create the initial history with these 3 transactions:
#    "TXN-001: Credit $500.00"
#    "TXN-002: Debit $12.50"
#    "TXN-003: Debit $5.00"
transaction_history: list[str] = 

# 2. A new transaction has occurred - append it to the history
new_txn: str = "TXN-004: Credit $100.00"
# Add new_txn to the end of transaction_history here


# 3. Count total entries
entry_count: int = 

# Verify
print(f"Ledger: {transaction_history}")
print(f"Total Entries: {entry_count}")
print(f"Latest Entry: {transaction_history[-1]}")

<!-- SEPARATOR -->

# validation_code
assert len(transaction_history) == 4, "History should have 4 items after appending"
assert transaction_history[0] == "TXN-001: Credit $500.00", "First entry should be TXN-001"
assert transaction_history[-1] == new_txn, "The new transaction should be last (use .append())"
assert entry_count == 4, "entry_count should equal len(transaction_history)"
assert isinstance(transaction_history, list), "transaction_history must be a list"
