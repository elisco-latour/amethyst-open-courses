---
id: "finguard_01_02"
title: "The Logic: Why Floats Lie"
type: "coding"
xp: 100
---

# The Logic of Precision

In banking, **Rounding Error is Theft.**

If you calculate interest on a billion dollars and you are off by `0.0000001`, you have "lost" a hundred dollars.

## The Flaw of Binary Math

Computers are binary (0s and 1s). Humans use decimals (0-9).
Some numbers, like `0.1` (1/10), are easy in Decimal but **impossible** in Binary directly (like writing 1/3 in decimal: 0.33333...).

When you use a normal Python number (`float`), the computer approximates.

```python
# The computer tries its best...
>>> 0.1 + 0.2
0.30000000000000004
```

In video games, this is fine. In FinGuard, this is a bug.

## The Solution: `Decimal`

We use the `Decimal` library. It forces Python to do math like a human accountant, not like a binary machine.

> **Rule:** Never use `float` for money. Use `Decimal`.

## Task

Prove the difference.
We will try to safeguard a balance of $15,000.75.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# The Amateur Way: Using Floats
# Note: Python interprets 15000.75 as a float automatically.
bad_balance = 15000.75

# The Engineering Way: Using Decimal
# Note: We pass a STRING "15000.75" so Python doesn't convert it to a float first.
account_balance = Decimal("15000.75")

print(f"Float Balance: {bad_balance}")
print(f"Decimal Balance: {account_balance}")

# If we were to add 0.1 and 0.2...
print(f"Float Math:   0.1 + 0.2 = {0.1 + 0.2}")
print(f"Decimal Math: 0.1 + 0.2 = {Decimal('0.1') + Decimal('0.2')}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert account_balance == Decimal("15000.75"), "account_balance should be Decimal('15000.75')"
assert isinstance(account_balance, Decimal), "account_balance must be a Decimal"
