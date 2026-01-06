---
id: "finguard_01_01"
title: "The Variable: Labeling Reality"
type: "coding"
xp: 100
---

# The Variable: Labeling Reality

In **The Signal**, you learned that data is just patterns of 1s and 0s. But you can't write software by typing `0b10111011100` everywhere.

You need to give those bits a **name**.

## The Safety Deposit Box

Imagine the FinGuard vault. It has millions of boxes. If you put $5,000 in a box but don't label it, that money is effectively lost — no one can find it.

In Python, a **variable** is that label:

- **The Box:** A specific location in your computer's memory
- **The Content:** The data stored there (e.g., `1500`)
- **The Label:** The variable name (e.g., `deposit_amount`)

```python
deposit_amount = 1500
```

This line does three things:
1. Creates a "box" in memory
2. Puts the value `1500` inside
3. Labels that box `deposit_amount`

## The First Rule: Reveal Your Intent

Amateurs name variables `x`, `y`, or `temp`.

Engineers name variables to reveal **intent**.

Consider these two lines — they do the same thing, but one communicates:

```python
# What does 't' mean? A temperature? A timestamp? A total?
t = "TXN-2025-00001"

# Crystal clear: this is a transaction identifier
transaction_id = "TXN-2025-00001"
```

> **"Code is read 10 times more often than it is written."**
> 
> When you name a variable `t`, your teammates have to guess. When you name it `transaction_id`, you've told the truth.

## Python Naming Convention

In Python, we use **snake_case** for variable names:
- All lowercase letters
- Words separated by underscores

```python
# Good snake_case names
account_balance = 5000
customer_name = "Alice Chen"
is_verified = True

# Bad (not snake_case)
AccountBalance = 5000   # This style is for class names
customerName = "Alice"  # This is JavaScript style
```

## Task

A new wire transfer has come in. Create a variable to store its **Transaction ID**.

Requirements:
- Variable name: `transaction_id`
- Value: `"TXN-2025-00001"` (a string)
- Follow snake_case convention

<!-- SEPARATOR -->

# seed_code
# Your first ledger entry
# Create the transaction_id variable below


# Verify the label works
print(f"Transaction Recorded: {transaction_id}")

<!-- SEPARATOR -->

# validation_code
assert transaction_id == "TXN-2025-00001", "transaction_id should be 'TXN-2025-00001'"
assert isinstance(transaction_id, str), "transaction_id should be a string"
