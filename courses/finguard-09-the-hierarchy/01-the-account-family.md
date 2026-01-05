---
id: "finguard_09_01"
title: "The Account Family"
type: "coding"
xp: 100
---

# The Account Family

Banks have many account types: Checking, Savings, Business. They share common features but behave differently.

## The Problem: Code Duplication

```python
# ❌ Duplicated code
class CheckingAccount:
    def __init__(self, id: str, balance: Decimal):
        self.id = id
        self.balance = balance
    
    def deposit(self, amount: Decimal):
        self.balance += amount  # Same as Savings!

class SavingsAccount:
    def __init__(self, id: str, balance: Decimal):
        self.id = id
        self.balance = balance
    
    def deposit(self, amount: Decimal):
        self.balance += amount  # Same as Checking!
```

## The Solution: Inheritance

```python
from decimal import Decimal

class BankAccount:
    """Base class for all accounts."""
    
    def __init__(self, account_id: str, balance: Decimal):
        self.account_id: str = account_id
        self.balance: Decimal = balance
    
    def deposit(self, amount: Decimal) -> None:
        self.balance += amount


class CheckingAccount(BankAccount):
    """Checking account with overdraft protection."""
    
    def __init__(self, account_id: str, balance: Decimal, overdraft_limit: Decimal):
        super().__init__(account_id, balance)  # Call parent constructor
        self.overdraft_limit = overdraft_limit


class SavingsAccount(BankAccount):
    """Savings account with interest rate."""
    
    def __init__(self, account_id: str, balance: Decimal, interest_rate: Decimal):
        super().__init__(account_id, balance)
        self.interest_rate = interest_rate
```

## The `super()` Function

`super()` calls the parent class's method:

```python
class Child(Parent):
    def __init__(self, x, y):
        super().__init__(x)  # Call Parent.__init__(x)
        self.y = y           # Add child-specific attribute
```

## The Analogy: The Family Tree

- **BankAccount** = Parent (grandparent to all)
- **CheckingAccount** = Child (inherits from parent)
- **SavingsAccount** = Sibling (also inherits from parent)

Children inherit everything from parents but can have their own traits.

## The "Pro" Tip

> **"Is-A" relationship means inheritance. "A CheckingAccount IS A BankAccount."**

## Task

Create an account hierarchy:
- `Account` base class with `account_id`, `balance`, `deposit()`, `withdraw()`
- `CheckingAccount` adds `overdraft_limit`
- `SavingsAccount` adds `interest_rate`

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

class Account:
    """Base class for all account types."""
    
    def __init__(self, account_id: str, balance: Decimal):
        pass  # Replace with your implementation
    
    def deposit(self, amount: Decimal) -> None:
        pass  # Replace with your implementation
    
    def withdraw(self, amount: Decimal) -> bool:
        """Withdraw if sufficient balance. Returns success."""
        pass  # Replace with your implementation


class CheckingAccount(Account):
    """Checking account with overdraft limit."""
    
    def __init__(self, account_id: str, balance: Decimal, overdraft_limit: Decimal):
        pass  # Replace with your implementation


class SavingsAccount(Account):
    """Savings account with interest rate."""
    
    def __init__(self, account_id: str, balance: Decimal, interest_rate: Decimal):
        pass  # Replace with your implementation


# Test the hierarchy
print("=== Account Hierarchy ===")

checking = CheckingAccount("CHK-001", Decimal("1000.00"), Decimal("500.00"))
print(f"Checking: ${checking.balance}, overdraft: ${checking.overdraft_limit}")

savings = SavingsAccount("SAV-001", Decimal("5000.00"), Decimal("0.02"))
print(f"Savings: ${savings.balance}, rate: {savings.interest_rate}")

# Test inherited methods
checking.deposit(Decimal("200.00"))
print(f"After deposit: ${checking.balance}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Test base class
acc = Account("A1", Decimal("100.00"))
assert acc.account_id == "A1", "Should set account_id"
assert acc.balance == Decimal("100.00"), "Should set balance"

acc.deposit(Decimal("50.00"))
assert acc.balance == Decimal("150.00"), "Deposit should work"

result = acc.withdraw(Decimal("50.00"))
assert result == True, "Withdraw should succeed"
assert acc.balance == Decimal("100.00"), "Withdraw should reduce balance"

# Test CheckingAccount inherits
chk = CheckingAccount("C1", Decimal("500.00"), Decimal("200.00"))
assert chk.account_id == "C1", "Should inherit account_id"
assert chk.balance == Decimal("500.00"), "Should inherit balance"
assert chk.overdraft_limit == Decimal("200.00"), "Should have overdraft_limit"

chk.deposit(Decimal("100.00"))  # Inherited method
assert chk.balance == Decimal("600.00"), "Should inherit deposit()"

# Test SavingsAccount inherits
sav = SavingsAccount("S1", Decimal("1000.00"), Decimal("0.05"))
assert sav.balance == Decimal("1000.00"), "Should inherit balance"
assert sav.interest_rate == Decimal("0.05"), "Should have interest_rate"
