---
id: "finguard_01_03"
title: "The Type System"
type: "coding"
xp: 100
---

# The Type System

Every piece of data in Python has a **type**. The type tells Python (and you) what operations are allowed.

## The Four Primitive Types

| Type | Description | FinGuard Example |
|------|-------------|------------------|
| `str` | Text (characters) | `"TXN-001"`, `"Alice Smith"` |
| `int` | Whole numbers | `42`, `1000000`, `-5` |
| `float` | Decimal numbers (approximate) | `3.14`, `99.99` |
| `bool` | True or False | `True`, `False` |

## Why Types Matter

Types prevent nonsense:

```python
# This makes sense
"TXN-" + "001"  # → "TXN-001" (concatenation)

# This is nonsense
"TXN-" + 5  # → TypeError! Can't add string and int
```

Python catches these mistakes **before your code runs in production**.

## The Analogy: Form Fields

A bank form has different field types:
- **Name**: Text only (no numbers)
- **Amount**: Numbers only (no letters)
- **Approved**: Checkbox (yes/no)

If someone writes "fifty dollars" in the Amount field, the form is rejected. Types are those rules.

## Type Checking with `type()`

You can ask Python what type a value is:

```python
type("hello")  # → <class 'str'>
type(42)       # → <class 'int'>
type(3.14)     # → <class 'float'>
type(True)     # → <class 'bool'>
```

## Task

Create four variables representing parts of a transaction:
1. `transaction_id` — a string: `"TXN-5001"`
2. `amount_cents` — an integer: `250000` (representing $2,500.00 in cents)
3. `exchange_rate` — a float: `1.0856` (EUR to USD)
4. `is_international` — a boolean: `True`

<!-- SEPARATOR -->

# seed_code
# Create the four transaction fields

# 1. Transaction ID (string)
transaction_id = "TXN-5001"

# 2. Amount in cents (integer) — $2,500.00 = 250000 cents


# 3. Exchange rate EUR→USD (float)


# 4. Is this an international transfer? (boolean)


# Verify the types
print(f"transaction_id: {transaction_id} ({type(transaction_id).__name__})")
print(f"amount_cents: {amount_cents} ({type(amount_cents).__name__})")
print(f"exchange_rate: {exchange_rate} ({type(exchange_rate).__name__})")
print(f"is_international: {is_international} ({type(is_international).__name__})")

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
