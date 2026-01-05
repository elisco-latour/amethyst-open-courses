---
id: "finguard_01_03"
title: "The Contract: Types"
type: "coding"
xp: 100
---

# The Contract: Types

When you fill out a bank form, the "Phone Number" Box expects numbers. If you write your name there, the teller rejects it.

In Python, **Types** are those rules. They are the **Contract** between different parts of the code.

## The Four Primitives

These are the atoms of our universe:

| Type | Name | Purpose | Example |
|------|------|---------|---------|
| `str` | String | Text, IDs, Names | `"ACC-101"` |
| `int` | Integer | Counting (Whole numbers) | `500` |
| `float` | Float | Measuring (Approximate decimals) | `1.57` |
| `bool` | Boolean | Logic (Yes/No) | `True` |

## Safety Checks

Python protects you. It won't let you add a Name to a Number.

```python
"Alice" + 500  # ERROR: Can only concatenate str (not "int") to str
```

This error is good. It means the system caught a bug before it corrupted the database.

## Task

Define the primitives for a standard transfer.
Observe how we use `int` for cents to avoid decimals entirely! (Another pro trick).

<!-- SEPARATOR -->

# seed_code
# Define the contract for a transfer

# 1. Who is sending? (Text)
transaction_id = "TXN-5001"

# 2. How much? (Integer Cents - $2,500.00)
amount_cents = 250000

# 3. What is the rate using? (Float - approximate is okay for rates, not balances)
exchange_rate = 1.0856

# 4. Is it international? (Logic)
is_international = True

# Inspect the types
print(f"ID Type: {type(transaction_id)}")
print(f"Amount Type: {type(amount_cents)}")

<!-- SEPARATOR -->

# validation_code
assert transaction_id == "TXN-5001", "transaction_id should be 'TXN-5001'"
assert amount_cents == 250000, "amount_cents should be 250000"
assert exchange_rate == 1.0856, "exchange_rate should be 1.0856"
assert is_international == True, "is_international should be True"
assert isinstance(transaction_id, str), "transaction_id must be a str"
assert isinstance(amount_cents, int), "amount_cents must be an int"
assert isinstance(exchange_rate, float), "exchange_rate must be a float"
assert isinstance(is_international, bool), "is_international must be a bool"
