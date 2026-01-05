---
id: "finguard_05_01"
title: "The Fee Formula"
type: "coding"
xp: 100
---

# The Fee Formula

Every bank charges transaction fees. Instead of copying the fee calculation everywhere, you write it **once** and reuse it.

## The Problem Without Functions

```python
# ❌ Copy-pasted everywhere
fee_1 = amount_1 * Decimal("0.01")
fee_2 = amount_2 * Decimal("0.01")
fee_3 = amount_3 * Decimal("0.01")

# What if the rate changes? You have to update EVERY line!
```

## The Function Solution

```python
from decimal import Decimal

def calculate_fee(amount: Decimal) -> Decimal:
    """Calculate transaction fee (1% of amount)."""
    fee_rate: Decimal = Decimal("0.01")
    return amount * fee_rate

# Use it anywhere
fee_1 = calculate_fee(Decimal("500.00"))
fee_2 = calculate_fee(Decimal("1000.00"))
```

## Anatomy of a Function

```python
def calculate_fee(amount: Decimal) -> Decimal:
    """Calculate transaction fee (1% of amount)."""
    fee_rate: Decimal = Decimal("0.01")
    return amount * fee_rate
```

| Part | Meaning |
|------|---------|
| `def` | "Define a function" |
| `calculate_fee` | The function name (verb describes action) |
| `amount: Decimal` | Parameter with type hint |
| `-> Decimal` | Return type (what comes out) |
| `"""docstring"""` | Documentation for the function |
| `return` | Send the result back |

## The Analogy: The Vending Machine

A vending machine is a function:
- **Input**: Money + Button selection
- **Process**: Internal mechanism (you don't see it)
- **Output**: Your snack

You don't care **how** it works. You just use it.

## The "Pro" Tip

> **Functions should do ONE thing and do it well. If you can't describe what it does in one sentence, it's doing too much.**

## Task

Write a `calculate_wire_fee` function that:
- Takes an `amount: Decimal` parameter
- Charges 0.5% fee for amounts up to $10,000
- Charges 0.25% fee for amounts over $10,000
- Returns the fee as `Decimal`

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Define the wire transfer fee function
def calculate_wire_fee(amount: Decimal) -> Decimal:
    """Calculate wire transfer fee based on amount tier."""
    pass  # Replace with your implementation


# Test the function
test_amounts: list[Decimal] = [
    Decimal("5000.00"),
    Decimal("10000.00"),
    Decimal("50000.00"),
]

print("=== Wire Transfer Fee Calculator ===")
for amount in test_amounts:
    fee = calculate_wire_fee(amount)
    print(f"Amount: ${amount:,.2f} → Fee: ${fee:.2f}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert calculate_wire_fee(Decimal("5000.00")) == Decimal("25.00"), "5000 * 0.005 = 25.00"
assert calculate_wire_fee(Decimal("10000.00")) == Decimal("50.00"), "10000 * 0.005 = 50.00"
assert calculate_wire_fee(Decimal("50000.00")) == Decimal("125.00"), "50000 * 0.0025 = 125.00"
