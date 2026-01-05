---
id: "finguard_02_01"
title: "Transaction History"
type: "coding"
xp: 100
---

# Transaction History

In **The Ledger**, you stored single pieces of data. But FinGuard doesn't process one transaction — it processes **millions**.

## The Bridge

Data rarely exists alone. A bank account has dozens of transactions. A customer has multiple accounts. We need ways to **group related data together**.

## The Analogy: The Filing Cabinet Drawer

A filing cabinet drawer holds many folders in order. You can:
- Add a new folder at the end
- Remove a folder
- Find the 5th folder by counting
- Go through each folder one by one

This is exactly what a **List** does in Python.

## Lists: Ordered Collections

A list is an **ordered, mutable** (changeable) collection:

```python
# Creating a list
transactions: list[str] = ["TXN-001", "TXN-002", "TXN-003"]

# Access by index (position) — starts at 0!
first: str = transactions[0]   # "TXN-001"
last: str = transactions[-1]   # "TXN-003" (negative = from end)

# Add to the end
transactions.append("TXN-004")

# Get the length
count: int = len(transactions)  # 4
```

## The "Pro" Tip

> **Use type hints with lists: `list[str]` not just `list`.**

```python
# ✅ Professional — tells us what's inside
transaction_ids: list[str] = ["TXN-001", "TXN-002"]

# ❌ Unclear — what does this list contain?
transaction_ids = ["TXN-001", "TXN-002"]
```

## Task

Create a transaction history for account ACC-1001.

1. Create a list named `transaction_history` with three transaction IDs: `"TXN-001"`, `"TXN-002"`, `"TXN-003"`
2. Add a fourth transaction `"TXN-004"` using `.append()`
3. Store the total count in `total_transactions`

<!-- SEPARATOR -->

# seed_code
# Create the transaction history list


# Add a new transaction


# Count total transactions


# Print the history
print(f"Account ACC-1001 Transaction History:")
print(f"Transactions: {transaction_history}")
print(f"Total: {total_transactions}")

<!-- SEPARATOR -->

# validation_code
assert transaction_history == ["TXN-001", "TXN-002", "TXN-003", "TXN-004"], "transaction_history should have 4 transactions"
assert total_transactions == 4, "total_transactions should be 4"
assert isinstance(transaction_history, list), "transaction_history must be a list"
