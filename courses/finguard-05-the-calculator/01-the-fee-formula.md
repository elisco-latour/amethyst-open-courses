---
id: "finguard_05_01"
title: "The Logic Engine"
type: "coding"
xp: 100
---

# The Logic Engine

In FinGuard, we don't scatter math all over the codebase. If the fee calculation changes, we don't want to hunt down 50 different files.

We encapsulate logic in **Functions**. Use them as "Logic Engines" — you pour raw data in the top, and processed results come out the bottom.

## The Problem: "Spaghetti Math"

```python
# ❌ Bad: Logic scattered everywhere
# File A
total = amount * Decimal("1.05") 

# File B
# Wait, did the tax rate prompt change? 
final = price * Decimal("1.06") 
```

## The Solution: Single Source of Truth

```python
from decimal import Decimal

# ✅ Good: Defined once, verified once.
def calculate_tax(amount: Decimal) -> Decimal:
    """Calculates standard 5% tax."""
    tax_rate = Decimal("0.05")
    return amount * tax_rate
```

## Anatomy of a Function

A function is a **Contract**.

1.  **Input (Parameters):** What the function *demands* to work.
2.  **Logic (Body):** The proprietary "secret sauce."
3.  **Output (Return):** The guaranteed deliverable.

```python
def get_fee(amount: Decimal) -> Decimal:
    # ... logic ...
    return fee
```

## Engineering Principle: Pure Functions

In banking, functions should ideally be **Pure**.
*   **Deterministic:** Inputting `$100` must *always* return the *same* fee. No random numbers, no database reads inside the math.
*   **No Side Effects:** Calculating a fee shouldn't print text or email a customer. It should just *calculate*.

## Task

Build a **Tiered Fee Calculator** engine.

*   **Inputs:** `amount` (Decimal).
*   **Logic:**
    *   If amount <= 10,000: Fee is **0.5%**.
    *   If amount > 10,000: Fee is **0.25%**.
*   **Output:** The calculated fee (Decimal).

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# The Engine
def calculate_wire_fee(amount: Decimal) -> Decimal:
    """
    Calculates the wire transfer fee based on volume.
    Tiers:
    - Up to $10k: 0.5%
    - Over $10k: 0.25%
    """
    # Implementation here
    pass

# Integration Test
test_amounts: list[Decimal] = [
    Decimal("5000.00"),
    Decimal("20000.00"),
]

print("=== Fee Engine Test ===")
for amount in test_amounts:
    fee = calculate_wire_fee(amount)
    print(f"Amount: ${amount:,.2f} -> Fee: ${fee:.2f}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert calculate_wire_fee(Decimal("5000.00")) == Decimal("25.00"), "0.5% of 5000 is 25"
assert calculate_wire_fee(Decimal("20000.00")) == Decimal("50.00"), "0.25% of 20000 is 50"

# validation_code
from decimal import Decimal
assert calculate_wire_fee(Decimal("5000.00")) == Decimal("25.00"), "5000 * 0.005 = 25.00"
assert calculate_wire_fee(Decimal("10000.00")) == Decimal("50.00"), "10000 * 0.005 = 50.00"
assert calculate_wire_fee(Decimal("50000.00")) == Decimal("125.00"), "50000 * 0.0025 = 125.00"
