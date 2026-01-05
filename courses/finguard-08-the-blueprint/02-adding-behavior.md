---
id: "finguard_08_02"
title: "Adding Behavior"
type: "coding"
xp: 100
---

# Adding Behavior

Classes don't just hold data — they have **behavior** (methods). A `BankAccount` knows how to deposit and withdraw.

## Methods

Methods are functions inside a class:

```python
from decimal import Decimal

class BankAccount:
    def __init__(self, account_id: str, balance: Decimal):
        self.account_id: str = account_id
        self.balance: Decimal = balance
    
    def deposit(self, amount: Decimal) -> None:
        """Add funds to the account."""
        self.balance = self.balance + amount
    
    def get_balance(self) -> Decimal:
        """Return current balance."""
        return self.balance
```

## Using Methods

```python
account = BankAccount("ACC-001", Decimal("1000.00"))

account.deposit(Decimal("500.00"))

print(account.get_balance())  # Decimal("1500.00")
```

## The `self` Parameter

Every method's first parameter is `self` — the instance the method is called on:

```python
account.deposit(Decimal("500.00"))
# Python translates this to:
# BankAccount.deposit(account, Decimal("500.00"))
```

## The Analogy: The ATM

An ATM isn't just a box with money (data). It has **operations**:
- `deposit()` — Put money in
- `withdraw()` — Take money out
- `check_balance()` — See how much you have

The ATM (class) combines data (cash) and behavior (operations).

## The "Pro" Tip

> **Methods that don't modify the object should return values. Methods that modify should return `None` or `self`.**

```python
# ✅ Query method — returns value, doesn't modify
def get_balance(self) -> Decimal:
    return self.balance

# ✅ Command method — modifies object, returns None
def deposit(self, amount: Decimal) -> None:
    self.balance += amount
```

## Task

Create a `BankAccount` class with:
- Attributes: `account_id`, `holder_name`, `balance`
- Methods: `deposit(amount)`, `withdraw(amount)`, `get_statement()`
- `withdraw` should check for sufficient funds

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

class BankAccount:
    """Represents a bank account with deposit/withdraw operations."""
    
    def __init__(self, account_id: str, holder_name: str, balance: Decimal):
        pass  # Replace with your implementation
    
    def deposit(self, amount: Decimal) -> None:
        """Add funds to the account."""
        pass  # Replace with your implementation
    
    def withdraw(self, amount: Decimal) -> bool:
        """Withdraw funds if sufficient balance. Returns True if successful."""
        pass  # Replace with your implementation
    
    def get_statement(self) -> str:
        """Return formatted account statement."""
        pass  # Replace with your implementation


# Test the account
account = BankAccount("ACC-001", "Alice Smith", Decimal("1000.00"))

print("=== Account Operations ===")
print(account.get_statement())

account.deposit(Decimal("500.00"))
print(f"After deposit: ${account.balance}")

success = account.withdraw(Decimal("200.00"))
print(f"Withdraw $200: {'Success' if success else 'Failed'}")
print(f"After withdrawal: ${account.balance}")

success = account.withdraw(Decimal("5000.00"))
print(f"Withdraw $5000: {'Success' if success else 'Failed'}")
print(f"After failed withdrawal: ${account.balance}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Test initialization
acc = BankAccount("ACC-TEST", "Test User", Decimal("1000.00"))
assert acc.account_id == "ACC-TEST", "Should set account_id"
assert acc.holder_name == "Test User", "Should set holder_name"
assert acc.balance == Decimal("1000.00"), "Should set balance"

# Test deposit
acc.deposit(Decimal("500.00"))
assert acc.balance == Decimal("1500.00"), "Deposit should add to balance"

# Test successful withdrawal
result = acc.withdraw(Decimal("200.00"))
assert result == True, "Should return True for successful withdrawal"
assert acc.balance == Decimal("1300.00"), "Withdrawal should subtract from balance"

# Test failed withdrawal (insufficient funds)
result = acc.withdraw(Decimal("5000.00"))
assert result == False, "Should return False for insufficient funds"
assert acc.balance == Decimal("1300.00"), "Balance unchanged after failed withdrawal"

# Test statement
stmt = acc.get_statement()
assert "ACC-TEST" in stmt, "Statement should include account ID"
assert "Test User" in stmt or "1300" in stmt, "Statement should include holder or balance"
