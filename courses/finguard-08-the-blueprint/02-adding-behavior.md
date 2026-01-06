---
id: "finguard_08_02"
title: "Cohesion"
type: "coding"
xp: 100
---

# Cohesion

In procedural programming, you separate Data (Dicts) from Logic (Functions).
In **Object-Oriented Programming (OOP)**, you combine them. This is **Cohesion**.

A `BankAccount` shouldn't just *hold* a balance. It should know how to *change* it.

## Methods (Active Behavior)

A method is a function inside a class.
It always accepts `self` as the first argument. `self` is "This specific object."

```python
class Account:
    def __init__(self, balance: int):
        self.balance = balance

    # ❌ External Logic (Not Cohesive)
    # def add_money(acc, amt): 
    #    acc.balance += amt

    # ✅ Internal Logic (Cohesive)
    def deposit(self, amount: int):
        self.balance += amount
```

## Why?

You don't want scattered logic like `balance = balance + 100` all over your codebase. If the rule changes (e.g., "Add logging"), you have to find every place you touched the balance.
With methods, you change it **once**.

## Task

Add `deposit` and `withdraw` behaviors to `BankAccount`.
1.  `deposit(amount)`: Adds to balance.
2.  `withdraw(amount)`: Subtracts from balance.
3.  **Bonus**: Validate negatives in `deposit` (raise ValueError).

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

class BankAccount:
    def __init__(self, balance: Decimal):
        self.balance = balance

    def deposit(self, amount: Decimal):
        # Implementation
        pass

    def withdraw(self, amount: Decimal):
        # Implementation
        pass

# Integration
acc = BankAccount(Decimal("100.00"))
acc.deposit(Decimal("50.00"))
acc.withdraw(Decimal("20.00"))
print(f"Final Balance: {acc.balance}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

acc = BankAccount(Decimal("100.00"))

# Test deposit
acc.deposit(Decimal("50.00"))
assert acc.balance == Decimal("150.00"), "Deposit should add to balance"

# Test withdraw
acc.withdraw(Decimal("30.00"))
assert acc.balance == Decimal("120.00"), "Withdraw should subtract from balance"

# Test method chaining
acc.deposit(Decimal("80.00"))
acc.withdraw(Decimal("100.00"))
assert acc.balance == Decimal("100.00"), "Balance should be 100 after operations"
