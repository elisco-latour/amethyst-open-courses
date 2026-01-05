---
id: "finguard_10_01"
title: "Single Responsibility"
type: "coding"
xp: 100
---

# Single Responsibility Principle (SRP)

**A class should have only one reason to change.**

## The Problem: The God Class

```python
# ❌ Does EVERYTHING
class TransactionManager:
    def validate_transaction(self, txn): ...
    def save_to_database(self, txn): ...
    def send_email_notification(self, txn): ...
    def generate_pdf_report(self, txn): ...
    def calculate_taxes(self, txn): ...
```

If email templates change, you modify `TransactionManager`.
If database schema changes, you modify `TransactionManager`.
If tax rules change, you modify `TransactionManager`.

One class, many reasons to change = **fragile code**.

## The Solution: Separate Responsibilities

```python
# ✅ Each class has ONE job
class TransactionValidator:
    def validate(self, txn: dict) -> bool: ...

class TransactionRepository:
    def save(self, txn: dict) -> None: ...

class NotificationService:
    def send_email(self, recipient: str, message: str) -> None: ...

class ReportGenerator:
    def generate_pdf(self, data: dict) -> bytes: ...
```

## The Analogy: The Restaurant

A restaurant doesn't have one person who:
- Takes orders
- Cooks food
- Serves tables
- Handles payments

Instead: **Waiter** takes orders, **Chef** cooks, **Cashier** handles payments.

## How to Identify Violations

Ask: "What reasons could this class change?"

If you can list more than one type of change, split the class.

## The "Pro" Tip

> **If a class name contains "And" or "Manager", it probably does too much.**

```python
# 🚩 Warning signs
class UserManagerAndValidator: ...
class TransactionProcessorAndReporter: ...
class DataHandlerAndFormatter: ...
```

## Task

Refactor a "God Class" into separate single-responsibility classes.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal
from datetime import datetime

# ❌ BAD: This class does too much
class TransactionProcessor:
    """Handles everything about transactions."""
    
    def __init__(self):
        self.transactions: list[dict] = []
        self.notifications: list[str] = []
    
    def process(self, amount: Decimal, account_id: str) -> dict:
        # Validation
        if amount <= Decimal("0"):
            raise ValueError("Amount must be positive")
        
        # Create transaction
        txn = {
            "id": f"TXN-{len(self.transactions)+1:03d}",
            "amount": amount,
            "account_id": account_id,
            "timestamp": datetime.now()
        }
        
        # Save
        self.transactions.append(txn)
        
        # Send notification
        self.notifications.append(f"Transaction {txn['id']} processed")
        
        return txn


# ✅ GOOD: Refactor into separate classes
class TransactionValidator:
    """Validates transaction data."""
    
    def validate(self, amount: Decimal) -> tuple[bool, str]:
        pass  # Replace with your implementation


class TransactionFactory:
    """Creates transaction records."""
    
    def __init__(self):
        self.counter: int = 0
    
    def create(self, amount: Decimal, account_id: str) -> dict:
        pass  # Replace with your implementation


class TransactionStore:
    """Stores transactions."""
    
    def __init__(self):
        self.transactions: list[dict] = []
    
    def save(self, txn: dict) -> None:
        pass  # Replace with your implementation
    
    def get_all(self) -> list[dict]:
        pass  # Replace with your implementation


class NotificationService:
    """Handles notifications."""
    
    def __init__(self):
        self.notifications: list[str] = []
    
    def notify(self, message: str) -> None:
        pass  # Replace with your implementation


# Test the refactored design
print("=== SRP Refactored Design ===")

validator = TransactionValidator()
factory = TransactionFactory()
store = TransactionStore()
notifier = NotificationService()

# Process a transaction using the new design
amount = Decimal("500.00")
account_id = "ACC-001"

valid, msg = validator.validate(amount)
if valid:
    txn = factory.create(amount, account_id)
    store.save(txn)
    notifier.notify(f"Transaction {txn['id']} processed: ${amount}")
    print(f"✓ Created: {txn['id']}")
else:
    print(f"✗ Validation failed: {msg}")

print(f"Stored transactions: {len(store.get_all())}")
print(f"Notifications sent: {len(notifier.notifications)}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Test validator
validator = TransactionValidator()
valid, msg = validator.validate(Decimal("100.00"))
assert valid == True, "Valid amount should pass"
valid, msg = validator.validate(Decimal("-50.00"))
assert valid == False, "Negative amount should fail"

# Test factory
factory = TransactionFactory()
txn1 = factory.create(Decimal("100.00"), "ACC-001")
assert "id" in txn1, "Should create transaction with id"
assert txn1["amount"] == Decimal("100.00"), "Should set amount"
txn2 = factory.create(Decimal("200.00"), "ACC-002")
assert txn1["id"] != txn2["id"], "Should generate unique IDs"

# Test store
store = TransactionStore()
store.save(txn1)
assert len(store.get_all()) == 1, "Should store transaction"
store.save(txn2)
assert len(store.get_all()) == 2, "Should store multiple"

# Test notifier
notifier = NotificationService()
notifier.notify("Test message")
assert len(notifier.notifications) == 1, "Should store notification"
