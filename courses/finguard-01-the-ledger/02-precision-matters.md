---
id: "finguard_01_02"
title: "Precision Matters"
type: "coding"
xp: 100
---

# Precision Matters

When dealing with money, **precision isn't optional — it's mandatory**.

## The Problem with Floats

Try this in any programming language:

```python
>>> 0.1 + 0.2
0.30000000000000004  # NOT 0.3!
```

This is because computers store decimals in **binary**, and some decimal numbers (like 0.1) can't be represented exactly.

For a video game? No one cares about 0.00000000000000004.

For a **bank transfer of $50 million**? That error multiplied across thousands of transactions adds up to **real money disappearing**.

## The Solution: The Decimal Type

Python's `Decimal` type uses **decimal arithmetic** (base 10) instead of binary (base 2). It's slower, but **exact**.

```python
from decimal import Decimal

# ✅ Exact
Decimal("0.1") + Decimal("0.2") == Decimal("0.3")  # True

# ❌ Inexact
0.1 + 0.2 == 0.3  # False!
```

## The "Pro" Tip

> **Always use `Decimal` for money. Always pass strings, not floats.**

```python
# ✅ Correct
balance = Decimal("1000.50")

# ❌ Wrong — the float is already imprecise!
balance = Decimal(1000.50)  # Inherits float's errors
```

## The Analogy: The Accountant's Calculator

A bank accountant doesn't use a regular calculator. They use one that shows **exact cents**, not approximations. `Decimal` is that calculator.

## Task

Create a variable `account_balance` using `Decimal` with a value of `"15000.75"`.

This is the starting balance for account ACC-1001.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Create the account balance using Decimal
# Remember: pass a STRING, not a float


# Print it to verify
print(f"Account ACC-1001 balance: ${account_balance}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert account_balance == Decimal("15000.75"), "account_balance should be Decimal('15000.75')"
assert isinstance(account_balance, Decimal), "account_balance must be a Decimal, not a float"
