---
id: "finguard_08_01"
title: "Encapsulation"
type: "coding"
xp: 100
---

# Encapsulation

In "The Ledger" (Variables), we used dictionaries.
In "The Archive" (Persistence), we used dictionaries.

Dictionaries are flexible. **Too flexible.**
You can misspell keys. You can store Strings where Numbers should be.
For a banking core, we need **Structure**.

## The Safe Deposit Box (Class)

A **Class** is a contract. It defines exactly what data must exist.
An **Object** is a specific instance of that contract.

```python
# ❌ Dictionary: Zero promises.
# Typo in 'amount', 'currency' missing. Code won't crash until you use it.
txn = {"amont": 100, "status": "pending"} 

# ✅ Class: Guaranteed structure.
# The Constructor (__init__) FORCES you to provide the right data.
class Transaction:
    def __init__(self, id: str, amount: int, currency: str):
        self.id = id
        self.amount = amount
        self.currency = currency

# Error! Missing currency. Code crashes IMMEDIATELY (Fail Fast).
t1 = Transaction("T1", 100) 
```

## The Blueprint vs. The House

- **Class**: The Blueprint (Abstract). "A Transaction has an ID."
- **Object**: The House (Concrete). "Transaction T1 has ID 'T1'."

## Task

Define a `BankAccount` class.
1.  Define `__init__`.
2.  Accept `account_id` (str) and `balance` (Decimal).
3.  Store them as attributes (`self.account_id`, `self.balance`).
4.  Type hint everything.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

class BankAccount:
    """
    A blueprint for a bank account.
    """
    # Define __init__ here
    pass

# Integration
acc = BankAccount("A1", Decimal("100.00"))
print(f"ID: {acc.account_id}, Balance: {acc.balance}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

try:
    acc = BankAccount("TEST", Decimal("50.00"))
    assert acc.account_id == "TEST"
    assert acc.balance == Decimal("50.00")
except AttributeError:
    assert False, "Attributes account_id and balance are missing"
except TypeError:
    assert False, "__init__ signature is incorrect"

print("Validation passed!")

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
