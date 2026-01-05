---
id: "finguard_08_01"
title: "From Dicts to Objects"
type: "coding"
xp: 100
---

# From Dicts to Objects

You've been using dictionaries to represent transactions. But dicts have problems:
- No structure enforcement
- No autocomplete in editors
- Easy to misspell keys
- No behavior attached

## The Problem with Dicts

```python
# ❌ Nothing prevents this
transaction = {
    "amont": 500,  # Typo: "amont" instead of "amount"
    "stauts": "pending"  # Another typo
}

# Code breaks at runtime, not edit time
print(transaction["amount"])  # KeyError!
```

## The Solution: Classes

A **class** is a blueprint. An **object** is an instance built from that blueprint.

```python
from decimal import Decimal

class Transaction:
    """Represents a financial transaction."""
    
    def __init__(self, id: str, amount: Decimal, status: str):
        self.id: str = id
        self.amount: Decimal = amount
        self.status: str = status

# Create an instance (object)
txn = Transaction("TXN-001", Decimal("500.00"), "pending")

# Access attributes with dot notation
print(txn.amount)  # Decimal("500.00")
print(txn.status)  # "pending"
```

## The Analogy: The Cookie Cutter

- **Class** = Cookie cutter (defines the shape)
- **Object** = Individual cookie (created from the cutter)

One cutter, many cookies. One class, many objects.

## Anatomy of a Class

```python
class Transaction:                          # Class name (PascalCase)
    """Docstring describing the class."""   # Documentation
    
    def __init__(self, id: str):            # Constructor
        self.id: str = id                   # Instance attribute
```

| Part | Purpose |
|------|---------|
| `class` | Keyword to define a class |
| `Transaction` | Class name (PascalCase convention) |
| `__init__` | Constructor — runs when you create an instance |
| `self` | Reference to the current instance |

## The "Pro" Tip

> **Class names are `PascalCase`. Method and attribute names are `snake_case`.**

```python
# ✅ Correct conventions
class BankAccount:
    def __init__(self, account_number: str):
        self.account_number = account_number
```

## Task

Create a `BankTransaction` class with attributes: `id`, `amount`, `transaction_type`, and `status`.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Define the BankTransaction class



# Create instances
transactions: list[BankTransaction] = [
    BankTransaction("TXN-001", Decimal("500.00"), "DEPOSIT", "completed"),
    BankTransaction("TXN-002", Decimal("200.00"), "WITHDRAWAL", "pending"),
    BankTransaction("TXN-003", Decimal("1500.00"), "TRANSFER", "completed"),
]

# Display the transactions
print("=== Bank Transactions ===")
for txn in transactions:
    print(f"{txn.id}: {txn.transaction_type} ${txn.amount} [{txn.status}]")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Test class exists and has correct attributes
txn = BankTransaction("TXN-TEST", Decimal("100.00"), "DEPOSIT", "pending")
assert txn.id == "TXN-TEST", "Should have id attribute"
assert txn.amount == Decimal("100.00"), "Should have amount attribute"
assert txn.transaction_type == "DEPOSIT", "Should have transaction_type attribute"
assert txn.status == "pending", "Should have status attribute"

# Test multiple instances are independent
txn2 = BankTransaction("TXN-OTHER", Decimal("999.00"), "WITHDRAWAL", "completed")
assert txn.id != txn2.id, "Each instance should have its own id"
