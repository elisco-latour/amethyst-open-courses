---
id: "finguard_13_01"
title: "Project Setup"
type: "coding"
xp: 100
---

# Project Setup: The FinGuard Batch Processor

## The Mission

You're building the **first working module** of FinGuard: a batch processor that:
1. Reads daily transaction files
2. Applies fraud detection rules
3. Generates compliance reports
4. Saves alerts for review

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                    BATCH FRAUD PROCESSOR                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────┐   ┌─────────────┐   ┌─────────────┐           │
│  │ Transaction │   │   Fraud     │   │   Report    │           │
│  │   Reader    │──►│  Detector   │──►│  Generator  │           │
│  └─────────────┘   └─────────────┘   └─────────────┘           │
│        │                 │                 │                    │
│        ▼                 ▼                 ▼                    │
│  ┌─────────────────────────────────────────────────┐           │
│  │               Transaction Repository             │           │
│  └─────────────────────────────────────────────────┘           │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## The Transaction Entity

This is the core domain object everything works with:

```python
class Transaction:
    txn_id: str           # Unique ID (TXN-000001)
    account_id: str       # Account reference (ACC-XXXX)
    amount: Decimal       # Transaction amount
    txn_type: str         # DEPOSIT, WITHDRAWAL, TRANSFER
    timestamp: datetime   # When it occurred
    status: str          # PENDING, COMPLETED, FLAGGED
```

## What You'll Build

| Module | Purpose | Pattern |
|--------|---------|---------|
| `Transaction` | Domain entity | Entity/Value Object |
| `TransactionRepository` | Data access | Repository |
| `FraudStrategy` | Detection rules | Strategy |
| `AlertObserver` | Notifications | Observer |
| `ReportBuilder` | Report generation | Builder |

## Task

Create the `Transaction` entity with validation logic.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal
from datetime import datetime

class Transaction:
    """Core domain entity for FinGuard.
    
    Represents a financial transaction with validation.
    
    Attributes:
        txn_id: Unique transaction identifier (format: TXN-XXXXXX)
        account_id: Account reference (format: ACC-XXXX)
        amount: Transaction amount (must be positive)
        txn_type: One of DEPOSIT, WITHDRAWAL, TRANSFER
        timestamp: When the transaction occurred
        status: One of PENDING, COMPLETED, FLAGGED
    """
    
    VALID_TYPES: list[str] = ["DEPOSIT", "WITHDRAWAL", "TRANSFER"]
    VALID_STATUSES: list[str] = ["PENDING", "COMPLETED", "FLAGGED"]
    
    def __init__(
        self,
        txn_id: str,
        account_id: str,
        amount: Decimal,
        txn_type: str,
        timestamp: datetime | None = None,
        status: str = "PENDING"
    ):
        """Initialize a transaction with validation.
        
        Raises:
            ValueError: If any validation fails
        """
        # Validate and set txn_id
        pass  # Replace: validate format (TXN-XXXXXX) and set
        
        # Validate and set account_id
        pass  # Replace: validate format (ACC-XXXX) and set
        
        # Validate and set amount
        pass  # Replace: must be positive Decimal
        
        # Validate and set txn_type
        pass  # Replace: must be in VALID_TYPES
        
        # Set timestamp (default to now)
        pass  # Replace: use provided or datetime.now()
        
        # Validate and set status
        pass  # Replace: must be in VALID_STATUSES
    
    def flag(self, reason: str) -> None:
        """Mark transaction as flagged for review."""
        self.status = "FLAGGED"
        self.flag_reason = reason
    
    def complete(self) -> None:
        """Mark transaction as completed."""
        if self.status == "FLAGGED":
            raise ValueError("Cannot complete a flagged transaction")
        self.status = "COMPLETED"
    
    def to_dict(self) -> dict:
        """Convert to dictionary for serialization."""
        data = {
            "txn_id": self.txn_id,
            "account_id": self.account_id,
            "amount": str(self.amount),
            "txn_type": self.txn_type,
            "timestamp": self.timestamp.isoformat(),
            "status": self.status
        }
        if hasattr(self, "flag_reason"):
            data["flag_reason"] = self.flag_reason
        return data
    
    @classmethod
    def from_dict(cls, data: dict) -> "Transaction":
        """Create Transaction from dictionary."""
        return cls(
            txn_id=data["txn_id"],
            account_id=data["account_id"],
            amount=Decimal(data["amount"]),
            txn_type=data["txn_type"],
            timestamp=datetime.fromisoformat(data["timestamp"]),
            status=data.get("status", "PENDING")
        )
    
    def __repr__(self) -> str:
        return f"Transaction({self.txn_id}, {self.txn_type}, ${self.amount})"


# Test the Transaction entity
print("=== Transaction Entity ===")

# Valid transaction
t1 = Transaction(
    txn_id="TXN-000001",
    account_id="ACC-1234",
    amount=Decimal("1500.00"),
    txn_type="DEPOSIT"
)
print(f"Created: {t1}")

# Test serialization round-trip
data = t1.to_dict()
t1_restored = Transaction.from_dict(data)
print(f"Restored: {t1_restored}")

# Test flagging
t1.flag("High amount for new account")
print(f"Flagged: {t1.status} - {t1.flag_reason}")

# Test validation errors
print("\n=== Validation Tests ===")

test_cases = [
    {"txn_id": "INVALID", "account_id": "ACC-1234", "amount": Decimal("100"), "txn_type": "DEPOSIT"},
    {"txn_id": "TXN-000002", "account_id": "INVALID", "amount": Decimal("100"), "txn_type": "DEPOSIT"},
    {"txn_id": "TXN-000003", "account_id": "ACC-1234", "amount": Decimal("-100"), "txn_type": "DEPOSIT"},
    {"txn_id": "TXN-000004", "account_id": "ACC-1234", "amount": Decimal("100"), "txn_type": "INVALID"},
]

for case in test_cases:
    try:
        Transaction(**case)
        print(f"✗ Should have failed: {case}")
    except ValueError as e:
        print(f"✓ Caught: {e}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
from datetime import datetime

# Test valid transaction creation
t = Transaction(
    txn_id="TXN-000001",
    account_id="ACC-1234",
    amount=Decimal("1500.00"),
    txn_type="DEPOSIT"
)
assert t.txn_id == "TXN-000001", "txn_id should be set"
assert t.amount == Decimal("1500.00"), "amount should be set"
assert t.status == "PENDING", "default status should be PENDING"

# Test validation - invalid txn_id
try:
    Transaction("INVALID", "ACC-1234", Decimal("100"), "DEPOSIT")
    assert False, "Should reject invalid txn_id"
except ValueError:
    pass

# Test validation - invalid account_id
try:
    Transaction("TXN-000001", "INVALID", Decimal("100"), "DEPOSIT")
    assert False, "Should reject invalid account_id"
except ValueError:
    pass

# Test validation - negative amount
try:
    Transaction("TXN-000001", "ACC-1234", Decimal("-100"), "DEPOSIT")
    assert False, "Should reject negative amount"
except ValueError:
    pass

# Test validation - invalid type
try:
    Transaction("TXN-000001", "ACC-1234", Decimal("100"), "INVALID")
    assert False, "Should reject invalid type"
except ValueError:
    pass

# Test flag and complete
t.flag("Test reason")
assert t.status == "FLAGGED", "Should be flagged"

try:
    t.complete()
    assert False, "Should not complete flagged transaction"
except ValueError:
    pass
