---
id: "finguard_01_02"
title: "Precision Matters: Why Floats Lie"
type: "coding"
xp: 100
---

# Precision Matters

In banking, **rounding error is theft**.

If you calculate interest on a billion dollars and you're off by `0.0000001`, you've "lost" a hundred dollars. Multiply that across millions of transactions, and you've got a serious problem.

## The Flaw of Binary Math

Computers think in binary (0s and 1s). Humans think in decimal (0-9).

Some decimal numbers, like `0.1` (one-tenth), are **impossible** to represent exactly in binary. It's like trying to write ⅓ in decimal — you get `0.33333...` forever.

When you use a regular Python number for decimals (called a `float`), the computer *approximates*:

```python
>>> 0.1 + 0.2
0.30000000000000004
```

That tiny error at the end? In a video game, nobody cares. In FinGuard, it's a bug that could corrupt the entire ledger.

## The Solution: Decimal

Python has a special tool called `Decimal` that does math like a human accountant — no binary approximations.

To use it, we need to **import** it first:

```python
from decimal import Decimal
```

This line tells Python: *"I need the Decimal tool from the decimal toolbox."*

Then we create precise numbers by passing **strings** (not floats):

```python
# WRONG: This converts 15000.75 to a float first, then to Decimal
bad = Decimal(15000.75)

# RIGHT: This keeps full precision
good = Decimal("15000.75")
```

<div class="key-concept">
<h4>🔑 Key Concept: The FinGuard Rule</h4>

**Never use `float` for money. Always use `Decimal` with string input.**

This is non-negotiable in financial systems.
</div>

## Task

A customer's account shows a balance of `$8,750.50`. Your job:

1. The import statement is provided for you
2. Create a variable `account_balance` using `Decimal` with the value `"8750.50"`
3. Create a variable `deposit_amount` using `Decimal` with the value `"1249.50"`
4. Create a variable `new_balance` that adds them together

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Create the account balance using Decimal (value: "8750.50")
account_balance = 

# Create the deposit amount using Decimal (value: "1249.50")
deposit_amount = 

# Calculate the new balance
new_balance = 

# Verify precision
print(f"Starting Balance: ${account_balance}")
print(f"Deposit: ${deposit_amount}")
print(f"New Balance: ${new_balance}")

# Compare with float math (see the danger!)
float_result = 8750.50 + 1249.50
print(f"\nFloat would give: ${float_result}")
print(f"Decimal gives:    ${new_balance}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert account_balance == Decimal("8750.50"), "account_balance should be Decimal('8750.50')"
assert deposit_amount == Decimal("1249.50"), "deposit_amount should be Decimal('1249.50')"
assert new_balance == Decimal("10000.00"), "new_balance should equal account_balance + deposit_amount"
assert isinstance(account_balance, Decimal), "account_balance must be a Decimal, not a float"
assert isinstance(new_balance, Decimal), "new_balance must be a Decimal"
