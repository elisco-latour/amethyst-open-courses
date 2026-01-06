---
id: "finguard_05_02"
title: "The Calculation Contract"
type: "coding"
xp: 100
---

# The Calculation Contract

When you sign a mortgage, you sign a **Contract**. You agree to terms (Principal, Rate, Term), and the bank agrees to the outcome.

In Python, a function signature is that contract.

## Explicit Signatures

Engineers don't guess types. We declare them.

```python
from decimal import Decimal

def calculate_simple_interest(
    principal: Decimal,
    rate: Decimal,
    years: int
) -> Decimal:
    # Contract: 
    # Give me 2 Decimals and an Int
    # I promise to give you back a Decimal
    return principal * rate * Decimal(str(years))
```

## Keyword Arguments (Explicitness)

When calling functions with many numbers, it's easy to mix them up. Did I pass the rate first or the term?

To prevent million-dollar mistakes, we use **Keyword Arguments**:

```python
# ❌ Ambiguous: What are these numbers?
calculate(1000, 0.05, 5)

# ✅ Explicit: Impossible to misunderstand
calculate(
    principal=Decimal("1000.00"),
    rate=Decimal("0.05"),
    years=5
)
```

## The "Pro" Tip

> **In Financial Code, prefer Keyword Arguments.**
> Ambiguity creates bugs. Explicitness creates trust.

## Task

Implement the `calculate_compound_interest` function.
Formula: $A = P(1 + r)^t$

In code, power is represented by `**`: `(1 + r) ** t`.

1.  Accept `principal`, `rate`, and `years`.
2.  Return the final amount (Principal + Interest).
3.  Use the correct types.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

def calculate_compound_interest(
    principal: Decimal,
    rate: Decimal,
    years: int
) -> Decimal:
    """
    Calculates compound interest.
    Formula: A = P * (1 + r)^t
    """
    # Implementation
    pass

# Integration
final_amt = calculate_compound_interest(
    principal=Decimal("1000.00"), 
    rate=Decimal("0.05"), 
    years=10
)

print(f"Final Amount: ${final_amt:,.2f}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
result = calculate_compound_interest(Decimal("1000"), Decimal("0.05"), 2)
# 1000 * (1.05)^2 = 1000 * 1.1025 = 1102.50
assert abs(result - Decimal("1102.50")) < Decimal("0.01"), "Result should match compound interest formula"
