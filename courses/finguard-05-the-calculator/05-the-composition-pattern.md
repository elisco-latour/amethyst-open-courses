---
id: "finguard_05_05"
title: "The Pipeline"
type: "coding"
xp: 100
---

# The Pipeline

We've accumulated several Logic Engines: Calculation, Fee Assessment, Tax.
In a real application, data flows through all of these in a specific order. This is a **Data Pipeline**.

## Composition: Building Complexity from Simplicity

Instead of writing one giant `process_transaction` function with 500 lines, we chain small functions together.

```python
def process_order(price: Decimal) -> Decimal:
    """The Pipeline"""
    # Step 1: Base Price
    price_with_discount = apply_discount(price)
    
    # Step 2: Taxes
    price_with_tax = add_tax(price_with_discount)
    
    # Step 3: Shipping
    final_price = add_shipping(price_with_tax)
    
    return final_price
```

## Why Compose?

1.  **Testability:** If the Tax calculation is wrong, I fix the `add_tax` engine. I don't touch the discount or shipping code.
2.  **Readability:** The "Pipeline Function" reads like a story (or a receipt).

## Task

Build a **Payment Pipeline**.

You have 3 specialized mini-engines (implement them simply):
1.  `calculate_subtotal(items)`: Sums the list of Decimals.
2.  `apply_tax(amount)`: Adds 8% tax.
3.  `apply_shipping(amount)`: Adds flat $10.00 shipping.

Then, build the **Master Function**:
4.  `calculate_grand_total(items)`: Pipes data through Subtotal -> Tax -> Shipping and returns the result.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Mini-Engines
def calculate_subtotal(items: list[Decimal]) -> Decimal:
    pass

def apply_tax(amount: Decimal) -> Decimal:
    # Tax Rate: 0.08
    pass

def apply_shipping(amount: Decimal) -> Decimal:
    # Flat Rate: 10.00
    pass

# The Pipeline
def calculate_grand_total(items: list[Decimal]) -> Decimal:
    """Orchestrates the calculation flow."""
    pass

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Test individual engines
items = [Decimal("100.00"), Decimal("200.00")]

# Subtotal: 100 + 200 = 300
assert calculate_subtotal(items) == Decimal("300.00"), "Subtotal should sum all items"

# Tax (8%): 300 * 0.08 = 24 -> 324
assert apply_tax(Decimal("300.00")) == Decimal("324.00"), "Tax should add 8%"

# Shipping: 324 + 10 = 334
assert apply_shipping(Decimal("324.00")) == Decimal("334.00"), "Shipping should add $10"

# Full pipeline: 300 -> 324 -> 334
total = calculate_grand_total(items)
assert total == Decimal("334.00"), "Pipeline should chain: subtotal -> tax -> shipping"
