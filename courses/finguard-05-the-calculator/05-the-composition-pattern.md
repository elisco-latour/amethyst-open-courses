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
items = [Decimal("100.00"), Decimal("200.00")]
# Subtotal: 300
# Tax (8%): 24 -> 324
# Shipping: 10 -> 334
total = calculate_grand_total(items)
assert total == Decimal("334.00")
# Test the pipeline
transactions: list[dict] = [
    {"id": "TXN-001", "amount": Decimal("500.00")},
    {"id": "TXN-002", "amount": Decimal("1500.00")},
    {"id": "TXN-003", "amount": Decimal("1000.00")},
]

print("=== Batch Processing Pipeline ===")
base = calculate_base_amount(transactions)
print(f"Base Amount: ${base:,.2f}")

with_fraud_fee = apply_fraud_fee(base)
print(f"+ Fraud Fee (0.1%): ${with_fraud_fee:,.2f}")

final = apply_processing_fee(with_fraud_fee)
print(f"+ Processing Fee ($2.50): ${final:,.2f}")

print(f"")
print(f"Pipeline Result: ${process_batch(transactions):,.2f}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

transactions = [
    {"id": "TXN-001", "amount": Decimal("500.00")},
    {"id": "TXN-002", "amount": Decimal("1500.00")},
    {"id": "TXN-003", "amount": Decimal("1000.00")},
]

# Base amount: 500 + 1500 + 1000 = 3000
assert calculate_base_amount(transactions) == Decimal("3000.00"), "Base should be 3000.00"

# Fraud fee: 3000 * 1.001 = 3003.00
assert apply_fraud_fee(Decimal("3000.00")) == Decimal("3003.00"), "Fraud fee should add 0.1%"

# Processing fee: 3003 + 2.50 = 3005.50
assert apply_processing_fee(Decimal("3003.00")) == Decimal("3005.50"), "Processing fee should add $2.50"

# Full pipeline: 3000 → 3003 → 3005.50
assert process_batch(transactions) == Decimal("3005.50"), "Pipeline should return 3005.50"
