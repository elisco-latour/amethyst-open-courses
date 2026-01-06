---
id: "finguard_01_03"
title: "The Contract: Data Types"
type: "coding"
xp: 100
---

# The Contract: Data Types

When you fill out a bank form, the "Phone Number" field expects digits. If you write your name there, the teller rejects your form.

In Python, **types** are those rules. They define what kind of data a variable can hold.

## The Four Primitives

Every piece of data in Python belongs to a **type**. Here are the four building blocks:

| Type | Name | What It Holds | Example |
|------|------|---------------|---------|
| `str` | String | Text (letters, symbols) | `"ACC-101"` |
| `int` | Integer | Whole numbers (no decimals) | `500` |
| `float` | Float | Decimal numbers (approximate) | `1.57` |
| `bool` | Boolean | True or False only | `True` |

## Checking Types

You can ask Python what type a value is using `type()`:

```python
>>> type("Hello")
<class 'str'>

>>> type(500)
<class 'int'>

>>> type(3.14)
<class 'float'>

>>> type(True)
<class 'bool'>
```

## Type Safety

Python protects you from mixing incompatible types:

```python
>>> "Alice" + 500
TypeError: can only concatenate str (not "int") to str
```

This error is **good**. It means Python caught a bug before it corrupted your data.

## The Integer Cents Pattern

In the last chapter, we learned to use `Decimal` for money. But there's another common pattern in banking: **store money as integer cents**.

Instead of `$25.99`, store `2599` cents. This avoids decimals entirely:

```python
# Using Decimal (precise)
price = Decimal("25.99")

# Using integer cents (also precise, and faster)
price_cents = 2599  # $25.99 as cents
```

Both approaches are valid. Integer cents are common in payment systems because they're faster to process.

## When Floats Are Okay

We said "never use float for money." But floats *are* acceptable for:
- **Exchange rates** (approximate is fine)
- **Percentages** (like interest rates)
- **Scores** (like fraud probability)

The rule: floats for *rates and ratios*, never for *amounts*.

## Task

A customer is making an international transfer. Create variables for the transaction with the correct types:

1. `transaction_id` — A string: `"TXN-INT-7892"`
2. `amount_cents` — An integer (cents): `350000` (that's $3,500.00)
3. `exchange_rate` — A float: `0.92` (USD to EUR)
4. `is_international` — A boolean: `True`

<!-- SEPARATOR -->

# seed_code
# Define the transaction with correct types

# Transaction identifier (string)
transaction_id = 

# Amount in cents (integer) - $3,500.00 = 350000 cents
amount_cents = 

# USD to EUR exchange rate (float - approximate is okay for rates)
exchange_rate = 

# Is this international? (boolean)
is_international = 

# Verify your types
print(f"ID: {transaction_id} (type: {type(transaction_id).__name__})")
print(f"Amount: {amount_cents} cents (type: {type(amount_cents).__name__})")
print(f"Rate: {exchange_rate} (type: {type(exchange_rate).__name__})")
print(f"International: {is_international} (type: {type(is_international).__name__})")

<!-- SEPARATOR -->

# validation_code
assert transaction_id == "TXN-INT-7892", "transaction_id should be 'TXN-INT-7892'"
assert amount_cents == 350000, "amount_cents should be 350000"
assert exchange_rate == 0.92, "exchange_rate should be 0.92"
assert is_international == True, "is_international should be True"
assert isinstance(transaction_id, str), "transaction_id must be a str"
assert isinstance(amount_cents, int), "amount_cents must be an int (not float!)"
assert isinstance(exchange_rate, float), "exchange_rate must be a float"
assert isinstance(is_international, bool), "is_international must be a bool"
