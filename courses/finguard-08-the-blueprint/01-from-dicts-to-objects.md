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
    assert acc.account_id == "TEST", "Should have account_id attribute"
    assert acc.balance == Decimal("50.00"), "Should have balance attribute"
except AttributeError:
    assert False, "Attributes account_id and balance are missing"
except TypeError:
    assert False, "__init__ signature is incorrect"

# Test multiple instances are independent
acc2 = BankAccount("OTHER", Decimal("999.00"))
assert acc.account_id != acc2.account_id, "Each instance should have its own data"
assert acc.balance != acc2.balance, "Instance attributes should be independent"
