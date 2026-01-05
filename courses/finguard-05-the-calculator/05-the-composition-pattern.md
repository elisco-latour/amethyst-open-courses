---
id: "finguard_05_05"
title: "The Composition Pattern"
type: "coding"
xp: 100
---

# The Composition Pattern

Real systems are built by combining small functions into larger workflows.

## The Bridge

**Data doesn't just sit there** — it flows through transformations. Functions are the pipes that data flows through. This is the essence of **data pipelines**.

## Function Composition

```python
from decimal import Decimal

def calculate_subtotal(items: list[Decimal]) -> Decimal:
    """Sum all item amounts."""
    return sum(items, Decimal("0.00"))

def apply_discount(amount: Decimal, discount_rate: Decimal) -> Decimal:
    """Apply discount to amount."""
    return amount * (Decimal("1.00") - discount_rate)

def add_tax(amount: Decimal, tax_rate: Decimal) -> Decimal:
    """Add tax to amount."""
    return amount * (Decimal("1.00") + tax_rate)

# Compose them into a workflow
def calculate_total(
    items: list[Decimal],
    discount_rate: Decimal,
    tax_rate: Decimal
) -> Decimal:
    """Calculate final total: subtotal → discount → tax."""
    subtotal = calculate_subtotal(items)
    discounted = apply_discount(subtotal, discount_rate)
    final = add_tax(discounted, tax_rate)
    return final
```

## The Analogy: The Assembly Line (Revisited)

Each station (function) does one thing:
1. Station 1: Attach wheels
2. Station 2: Install engine
3. Station 3: Paint body

The car flows through each station. The final car is the composition of all steps.

## Why Composition?

| Benefit | Explanation |
|---------|-------------|
| **Testable** | Test each function independently |
| **Reusable** | Use `add_tax()` anywhere you need taxes |
| **Readable** | Each function name explains what it does |
| **Maintainable** | Change tax logic in one place |

## The "Pro" Tip

> **Build small functions that do one thing. Compose them into larger workflows. This is the foundation of data engineering.**

## Task

Build a transaction processing pipeline:
1. `calculate_base_amount(transactions)` — sum all transaction amounts
2. `apply_fraud_fee(amount)` — add 0.1% fraud protection fee
3. `apply_processing_fee(amount)` — add flat $2.50 processing fee
4. `process_batch(transactions)` — compose all steps, return final amount

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

def calculate_base_amount(transactions: list[dict]) -> Decimal:
    """Sum all transaction amounts."""
    pass  # Replace with your implementation


def apply_fraud_fee(amount: Decimal) -> Decimal:
    """Add 0.1% fraud protection fee."""
    pass  # Replace with your implementation


def apply_processing_fee(amount: Decimal) -> Decimal:
    """Add flat $2.50 processing fee."""
    pass  # Replace with your implementation


def process_batch(transactions: list[dict]) -> Decimal:
    """Process a batch: sum → fraud fee → processing fee."""
    pass  # Replace with your implementation


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
