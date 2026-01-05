---
id: "finguard_05_02"
title: "The Interest Engine"
type: "coding"
xp: 100
---

# The Interest Engine

Banks calculate interest daily, monthly, yearly. Each calculation needs **parameters** — the amount, rate, and time period.

## Multiple Parameters

```python
from decimal import Decimal

def calculate_simple_interest(
    principal: Decimal,
    rate: Decimal,
    years: int
) -> Decimal:
    """Calculate simple interest: I = P × R × T"""
    return principal * rate * Decimal(str(years))
```

## The Analogy: The Recipe

A recipe takes **ingredients** (parameters) and produces a **dish** (return value):

```
Recipe: Chocolate Cake
Ingredients:
  - flour: 2 cups
  - sugar: 1 cup
  - cocoa: 1/2 cup
Produces: 1 cake
```

## Parameter Order Matters

```python
def calculate_interest(principal: Decimal, rate: Decimal, years: int) -> Decimal:
    ...

# ❌ Wrong order — gets wrong result
interest = calculate_interest(Decimal("0.05"), Decimal("1000.00"), 5)

# ✅ Correct order
interest = calculate_interest(Decimal("1000.00"), Decimal("0.05"), 5)
```

## Named Arguments (Keyword Arguments)

```python
# ✅ Crystal clear — no confusion about order
interest = calculate_interest(
    principal=Decimal("1000.00"),
    rate=Decimal("0.05"),
    years=5
)
```

## The "Pro" Tip

> **When a function has more than 2 parameters, use named arguments for clarity.**

```python
# ❌ What do these numbers mean?
result = process_payment(Decimal("100"), "USD", True, 3)

# ✅ Self-documenting
result = process_payment(
    amount=Decimal("100"),
    currency="USD",
    expedited=True,
    retry_count=3
)
```

## Task

Write a `calculate_compound_interest` function that:
- Takes `principal`, `annual_rate`, and `years` as parameters
- Uses the compound interest formula: A = P × (1 + r)^t
- Returns the **final amount** (principal + interest)

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

def calculate_compound_interest(
    principal: Decimal,
    annual_rate: Decimal,
    years: int
) -> Decimal:
    """
    Calculate compound interest.
    Formula: A = P × (1 + r)^t
    Returns the final amount.
    """
    pass  # Replace with your implementation


# Test with a $10,000 deposit at 5% for 3 years
result = calculate_compound_interest(
    principal=Decimal("10000.00"),
    annual_rate=Decimal("0.05"),
    years=3
)

print(f"=== Compound Interest Calculator ===")
print(f"Principal: $10,000.00")
print(f"Annual Rate: 5%")
print(f"Years: 3")
print(f"Final Amount: ${result:,.2f}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
# 10000 * (1.05)^3 = 10000 * 1.157625 = 11576.25
result = calculate_compound_interest(
    principal=Decimal("10000.00"),
    annual_rate=Decimal("0.05"),
    years=3
)
# Allow small rounding difference
expected = Decimal("11576.25")
assert abs(result - expected) < Decimal("0.01"), f"Expected ~{expected}, got {result}"
