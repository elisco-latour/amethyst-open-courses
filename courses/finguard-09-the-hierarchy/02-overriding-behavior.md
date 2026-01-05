---
id: "finguard_09_02"
title: "Overriding Behavior"
type: "coding"
xp: 100
---

# Overriding Behavior

Children can **override** parent methods to change behavior.

## Method Overriding

```python
from decimal import Decimal

class Account:
    def withdraw(self, amount: Decimal) -> bool:
        if amount > self.balance:
            return False
        self.balance -= amount
        return True


class CheckingAccount(Account):
    def withdraw(self, amount: Decimal) -> bool:
        """Override: allow overdraft up to limit."""
        available = self.balance + self.overdraft_limit
        if amount > available:
            return False
        self.balance -= amount
        return True
```

## Calling Parent's Method

Use `super()` to extend (not replace) parent behavior:

```python
class SavingsAccount(Account):
    def withdraw(self, amount: Decimal) -> bool:
        """Override: check withdrawal limit first, then call parent."""
        if amount > self.monthly_withdrawal_limit:
            return False
        return super().withdraw(amount)  # Delegate to parent
```

## The Analogy: The Recipe Modification

- **Parent**: Basic cake recipe
- **Child Override**: Chocolate cake (completely different steps)
- **Child Extension**: Vanilla cake (basic recipe + vanilla extract)

## Polymorphism: Same Interface, Different Behavior

```python
accounts: list[Account] = [
    CheckingAccount("CHK-001", Decimal("100.00"), Decimal("500.00")),
    SavingsAccount("SAV-001", Decimal("1000.00"), Decimal("0.02")),
]

# Same method call, different behavior
for account in accounts:
    account.withdraw(Decimal("50.00"))  # Each uses its own implementation
```

## The "Pro" Tip

> **Polymorphism means "many forms". The caller doesn't need to know which specific type — just that it's an Account.**

## Task

Create account types with different withdrawal rules:
- `Account`: Basic withdrawal (balance check only)
- `CheckingAccount`: Allow overdraft (balance + overdraft_limit)
- `SavingsAccount`: Limit 3 withdrawals per month

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

class Account:
    def __init__(self, account_id: str, balance: Decimal):
        self.account_id: str = account_id
        self.balance: Decimal = balance
    
    def withdraw(self, amount: Decimal) -> tuple[bool, str]:
        """Withdraw funds. Returns (success, message)."""
        pass  # Replace with your implementation


class CheckingAccount(Account):
    def __init__(self, account_id: str, balance: Decimal, overdraft_limit: Decimal):
        super().__init__(account_id, balance)
        self.overdraft_limit: Decimal = overdraft_limit
    
    def withdraw(self, amount: Decimal) -> tuple[bool, str]:
        """Override: allow overdraft up to limit."""
        pass  # Replace with your implementation


class SavingsAccount(Account):
    def __init__(self, account_id: str, balance: Decimal, interest_rate: Decimal):
        super().__init__(account_id, balance)
        self.interest_rate: Decimal = interest_rate
        self.monthly_withdrawals: int = 0
        self.max_withdrawals: int = 3
    
    def withdraw(self, amount: Decimal) -> tuple[bool, str]:
        """Override: limit withdrawals to 3 per month."""
        pass  # Replace with your implementation


# Test polymorphism
print("=== Withdrawal Behavior Test ===")

accounts: list[Account] = [
    Account("BASE-001", Decimal("100.00")),
    CheckingAccount("CHK-001", Decimal("100.00"), Decimal("500.00")),
    SavingsAccount("SAV-001", Decimal("1000.00"), Decimal("0.02")),
]

for acc in accounts:
    success, msg = acc.withdraw(Decimal("150.00"))
    print(f"{acc.__class__.__name__}: {msg}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Test base Account
acc = Account("A1", Decimal("100.00"))
success, msg = acc.withdraw(Decimal("50.00"))
assert success == True, "Should succeed with sufficient balance"
success, msg = acc.withdraw(Decimal("100.00"))
assert success == False, "Should fail with insufficient balance"

# Test CheckingAccount overdraft
chk = CheckingAccount("C1", Decimal("100.00"), Decimal("500.00"))
success, msg = chk.withdraw(Decimal("150.00"))  # Over balance, under overdraft
assert success == True, "Should allow overdraft"
assert chk.balance == Decimal("-50.00"), "Balance can go negative"

# Test SavingsAccount withdrawal limit
sav = SavingsAccount("S1", Decimal("10000.00"), Decimal("0.02"))
sav.withdraw(Decimal("100.00"))  # 1
sav.withdraw(Decimal("100.00"))  # 2
sav.withdraw(Decimal("100.00"))  # 3
success, msg = sav.withdraw(Decimal("100.00"))  # 4 - should fail
assert success == False, "Should fail after 3 withdrawals"
assert "limit" in msg.lower() or "3" in msg, "Should mention withdrawal limit"
