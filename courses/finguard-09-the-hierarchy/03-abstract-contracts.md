---
id: "finguard_09_03"
title: "Abstract Contracts"
type: "coding"
xp: 100
---

# Abstract Contracts

Sometimes you want to **require** subclasses to implement certain methods.

## The Problem: Incomplete Implementations

```python
class PaymentProcessor:
    def process(self, amount: Decimal) -> bool:
        pass  # Subclass should implement, but might forget

class CreditCardProcessor(PaymentProcessor):
    pass  # Forgot to implement process()!

# This silently fails
processor = CreditCardProcessor()
processor.process(Decimal("100"))  # Returns None, no error
```

## The Solution: Abstract Base Classes

```python
from abc import ABC, abstractmethod
from decimal import Decimal

class PaymentProcessor(ABC):  # Inherit from ABC
    
    @abstractmethod
    def process(self, amount: Decimal) -> bool:
        """Process a payment. Must be implemented by subclasses."""
        pass
    
    @abstractmethod
    def refund(self, transaction_id: str) -> bool:
        """Refund a payment. Must be implemented by subclasses."""
        pass


class CreditCardProcessor(PaymentProcessor):
    def process(self, amount: Decimal) -> bool:
        # Now we MUST implement this
        return True
    
    def refund(self, transaction_id: str) -> bool:
        return True
```

## What Happens If You Forget?

```python
class IncompleteProcessor(PaymentProcessor):
    def process(self, amount: Decimal) -> bool:
        return True
    # Forgot refund()!

processor = IncompleteProcessor()  # TypeError: Can't instantiate!
```

## The Analogy: The Job Description

An abstract class is like a job description:
- "Must have ability to process payments" (`@abstractmethod process`)
- "Must have ability to issue refunds" (`@abstractmethod refund`)

If you can't do all required tasks, you can't get the job (instantiate).

## The "Pro" Tip

> **Use abstract classes to define contracts. They ensure all implementations are complete.**

## Task

Create an abstract `TransactionHandler` with required methods, then implement `DepositHandler` and `WithdrawalHandler`.

<!-- SEPARATOR -->

# seed_code
from abc import ABC, abstractmethod
from decimal import Decimal

class TransactionHandler(ABC):
    """Abstract base class for transaction handlers."""
    
    @abstractmethod
    def validate(self, amount: Decimal, account_balance: Decimal) -> tuple[bool, str]:
        """Validate the transaction. Returns (is_valid, message)."""
        pass
    
    @abstractmethod
    def execute(self, amount: Decimal) -> dict:
        """Execute the transaction. Returns transaction record."""
        pass
    
    @abstractmethod
    def get_transaction_type(self) -> str:
        """Return the transaction type name."""
        pass


class DepositHandler(TransactionHandler):
    """Handler for deposit transactions."""
    
    pass  # Replace with your implementation


class WithdrawalHandler(TransactionHandler):
    """Handler for withdrawal transactions."""
    
    pass  # Replace with your implementation


# Test the handlers
print("=== Transaction Handlers ===")

handlers: list[TransactionHandler] = [
    DepositHandler(),
    WithdrawalHandler(),
]

for handler in handlers:
    print(f"\n{handler.get_transaction_type()} Handler:")
    
    valid, msg = handler.validate(Decimal("500.00"), Decimal("1000.00"))
    print(f"  Validate $500 (balance $1000): {msg}")
    
    if valid:
        result = handler.execute(Decimal("500.00"))
        print(f"  Execute result: {result}")

# Test withdrawal with insufficient funds
print("\nWithdrawal insufficient funds test:")
wh = WithdrawalHandler()
valid, msg = wh.validate(Decimal("2000.00"), Decimal("500.00"))
print(f"  Validate $2000 (balance $500): {msg}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Test DepositHandler
dh = DepositHandler()
assert dh.get_transaction_type() == "DEPOSIT" or "deposit" in dh.get_transaction_type().lower()

valid, msg = dh.validate(Decimal("100.00"), Decimal("0.00"))
assert valid == True, "Deposits should always be valid"

result = dh.execute(Decimal("100.00"))
assert "amount" in result or "type" in result, "Execute should return dict with transaction info"

# Test WithdrawalHandler
wh = WithdrawalHandler()
assert wh.get_transaction_type() == "WITHDRAWAL" or "withdrawal" in wh.get_transaction_type().lower()

valid, msg = wh.validate(Decimal("100.00"), Decimal("500.00"))
assert valid == True, "Should allow withdrawal with sufficient funds"

valid, msg = wh.validate(Decimal("1000.00"), Decimal("500.00"))
assert valid == False, "Should reject withdrawal with insufficient funds"

# Test that abstract class can't be instantiated
try:
    th = TransactionHandler()
    assert False, "Should not be able to instantiate abstract class"
except TypeError:
    pass
