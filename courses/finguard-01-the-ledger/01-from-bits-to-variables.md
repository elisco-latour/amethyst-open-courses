---
id: "finguard_01_01"
title: "From Bits to Variables"
type: "coding"
xp: 100
---

# From Bits to Variables

In **The Signal**, you learned that data is information encoded as bits. Now it's time to work with that data in Python.

## The Bridge

Remember: a computer sees `1500` as `0b10111011100` (binary). But you don't want to think in binary. You want to think in **meaning**.

A **variable** is a named container that holds data. Instead of remembering memory address `0x7fff5fbff8b0`, you just say `transaction_id`.

## The Analogy: The Labeled Box

In a bank vault, every safety deposit box has a **number** on it. When a customer asks for box 1247, the clerk knows exactly where to look.

Variables work the same way:
- The **name** is the label (`transaction_id`)
- The **value** is what's inside (`"TXN-00001"`)
- Python remembers where in memory that value lives

## The "Pro" Tip

> **Always use `snake_case` for variable names.**

```python
# ✅ Professional Python
transaction_id = "TXN-001"
account_balance = 5000.00

# ❌ Amateur Python (don't do this)
transactionId = "TXN-001"    # This is JavaScript style
TransactionID = "TXN-001"    # This is C# style
TRANSACTION_ID = "TXN-001"   # This is for constants only
```

## Task

Create your first ledger entry. Define a variable named `transaction_id` with the value `"TXN-2025-00001"`.

This ID uniquely identifies a wire transfer in FinGuard.

<!-- SEPARATOR -->

# seed_code
# Your first ledger entry
# Create a variable to store the transaction ID


# Print it to verify
print(f"Transaction recorded: {transaction_id}")

<!-- SEPARATOR -->

# validation_code
assert transaction_id == "TXN-2025-00001", "transaction_id should be 'TXN-2025-00001'"
assert isinstance(transaction_id, str), "transaction_id should be a string"
