---
id: "finguard_12_01"
title: "Factory Pattern"
type: "coding"
xp: 100
---

# Factory Pattern

Let a factory decide **which object to create** based on input.

## The Problem

```python
# ❌ Client code knows too much about implementations
def create_account(account_type: str) -> ???:
    if account_type == "checking":
        return CheckingAccount(...)
    elif account_type == "savings":
        return SavingsAccount(...)
    elif account_type == "business":
        return BusinessAccount(...)
    # More types = more if statements everywhere!
```

## The Solution: Factory Pattern

```python
# ✅ Factory encapsulates creation logic
class AccountFactory:
    @staticmethod
    def create(account_type: str, account_id: str, balance: Decimal) -> Account:
        if account_type == "checking":
            return CheckingAccount(account_id, balance)
        elif account_type == "savings":
            return SavingsAccount(account_id, balance)
        elif account_type == "business":
            return BusinessAccount(account_id, balance)
        else:
            raise ValueError(f"Unknown account type: {account_type}")

# Client code is simple
account = AccountFactory.create("checking", "ACC-001", Decimal("1000"))
```

## Why Factory?

| Benefit | Explanation |
|---------|-------------|
| **Decoupling** | Client doesn't know concrete classes |
| **Centralized** | One place to change creation logic |
| **Extensible** | Add new types without changing clients |
| **Testable** | Can replace factory with mock |

## The Analogy: The Car Factory

You order a "sedan." The factory decides which exact model, color, and configuration based on current inventory and options. You don't care which assembly line built it.

## The "Pro" Tip

> **Use Factory when object creation is complex or when you need to hide implementation details from clients.**

## Task

Build a `TransactionFactory` that creates different transaction types.

<!-- SEPARATOR -->

# seed_code
from abc import ABC, abstractmethod
from decimal import Decimal
from datetime import datetime

# Base class
class Transaction(ABC):
    def __init__(self, txn_id: str, amount: Decimal, timestamp: datetime):
        self.txn_id = txn_id
        self.amount = amount
        self.timestamp = timestamp
    
    @abstractmethod
    def get_fee(self) -> Decimal:
        """Calculate transaction fee."""
        pass
    
    @abstractmethod
    def get_type(self) -> str:
        pass

# Concrete implementations
class DepositTransaction(Transaction):
    def get_fee(self) -> Decimal:
        return Decimal("0.00")  # No fee for deposits
    
    def get_type(self) -> str:
        return "DEPOSIT"

class WithdrawalTransaction(Transaction):
    def get_fee(self) -> Decimal:
        return Decimal("2.50")  # Flat fee
    
    def get_type(self) -> str:
        return "WITHDRAWAL"

class TransferTransaction(Transaction):
    def __init__(self, txn_id: str, amount: Decimal, timestamp: datetime, target_account: str):
        super().__init__(txn_id, amount, timestamp)
        self.target_account = target_account
    
    def get_fee(self) -> Decimal:
        # 0.5% of amount, minimum $1
        percent_fee = self.amount * Decimal("0.005")
        return max(percent_fee, Decimal("1.00"))
    
    def get_type(self) -> str:
        return "TRANSFER"

class WireTransaction(Transaction):
    def __init__(self, txn_id: str, amount: Decimal, timestamp: datetime, swift_code: str):
        super().__init__(txn_id, amount, timestamp)
        self.swift_code = swift_code
    
    def get_fee(self) -> Decimal:
        return Decimal("25.00")  # Flat international fee
    
    def get_type(self) -> str:
        return "WIRE"


# Factory implementation
class TransactionFactory:
    """Factory for creating transaction objects."""
    
    _next_id: int = 1
    
    @classmethod
    def _generate_id(cls) -> str:
        txn_id = f"TXN-{cls._next_id:06d}"
        cls._next_id += 1
        return txn_id
    
    @staticmethod
    def create(
        txn_type: str,
        amount: Decimal,
        target_account: str | None = None,
        swift_code: str | None = None
    ) -> Transaction:
        """Create a transaction of the specified type.
        
        Args:
            txn_type: One of 'deposit', 'withdrawal', 'transfer', 'wire'
            amount: Transaction amount
            target_account: Required for 'transfer'
            swift_code: Required for 'wire'
        
        Returns:
            Appropriate Transaction subclass instance
        
        Raises:
            ValueError: If txn_type is invalid or required args missing
        """
        pass  # Replace with your implementation


# Test the factory
print("=== Transaction Factory ===")

# Create different transaction types
deposit = TransactionFactory.create("deposit", Decimal("1000.00"))
print(f"Created: {deposit.get_type()} - Fee: ${deposit.get_fee()}")

withdrawal = TransactionFactory.create("withdrawal", Decimal("200.00"))
print(f"Created: {withdrawal.get_type()} - Fee: ${withdrawal.get_fee()}")

transfer = TransactionFactory.create(
    "transfer", 
    Decimal("500.00"),
    target_account="ACC-999"
)
print(f"Created: {transfer.get_type()} - Fee: ${transfer.get_fee()}")

wire = TransactionFactory.create(
    "wire",
    Decimal("10000.00"),
    swift_code="CHASUS33"
)
print(f"Created: {wire.get_type()} - Fee: ${wire.get_fee()}")

# Test error case
try:
    invalid = TransactionFactory.create("invalid", Decimal("100.00"))
except ValueError as e:
    print(f"Caught error: {e}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Test deposit
deposit = TransactionFactory.create("deposit", Decimal("1000.00"))
assert deposit.get_type() == "DEPOSIT", "Should create deposit"
assert deposit.get_fee() == Decimal("0.00"), "Deposit has no fee"

# Test withdrawal
withdrawal = TransactionFactory.create("withdrawal", Decimal("200.00"))
assert withdrawal.get_type() == "WITHDRAWAL", "Should create withdrawal"

# Test transfer
transfer = TransactionFactory.create("transfer", Decimal("500.00"), target_account="ACC-999")
assert transfer.get_type() == "TRANSFER", "Should create transfer"
assert hasattr(transfer, "target_account"), "Transfer should have target_account"

# Test wire
wire = TransactionFactory.create("wire", Decimal("10000.00"), swift_code="CHASUS33")
assert wire.get_type() == "WIRE", "Should create wire"
assert wire.get_fee() == Decimal("25.00"), "Wire has $25 fee"

# Test error
try:
    TransactionFactory.create("invalid", Decimal("100.00"))
    assert False, "Should raise ValueError"
except ValueError:
    pass
