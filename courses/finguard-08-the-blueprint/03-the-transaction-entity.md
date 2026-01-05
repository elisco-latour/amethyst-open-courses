---
id: "finguard_08_03"
title: "The Transaction Entity"
type: "coding"
xp: 100
---

# The Transaction Entity

## The Bridge: Data Entities

In **The Signal**, you learned about data having **meaning**. A Transaction isn't just data — it's a business **entity** with rules.

- A transaction amount cannot be negative
- A transaction must have a timestamp
- A transaction status follows a lifecycle: pending → completed/failed

## Business Logic in Classes

```python
from decimal import Decimal
from datetime import datetime

class Transaction:
    def __init__(self, id: str, amount: Decimal):
        if amount <= Decimal("0"):
            raise ValueError("Transaction amount must be positive")
        
        self.id: str = id
        self.amount: Decimal = amount
        self.status: str = "pending"
        self.created_at: datetime = datetime.now()
    
    def complete(self) -> None:
        """Mark transaction as completed."""
        if self.status != "pending":
            raise ValueError(f"Cannot complete {self.status} transaction")
        self.status = "completed"
    
    def fail(self, reason: str) -> None:
        """Mark transaction as failed with reason."""
        if self.status != "pending":
            raise ValueError(f"Cannot fail {self.status} transaction")
        self.status = "failed"
        self.failure_reason = reason
```

## State Transitions

```
        ┌──────────┐
        │ pending  │
        └────┬─────┘
             │
    ┌────────┴────────┐
    ▼                 ▼
┌─────────┐    ┌──────────┐
│completed│    │  failed  │
└─────────┘    └──────────┘
```

## The Analogy: The Document Lifecycle

A legal document has states:
- Draft → Review → Approved/Rejected

You can't go from "Approved" back to "Draft". The class enforces these rules.

## The "Pro" Tip

> **Validate in the constructor. Don't let invalid objects exist.**

```python
# ✅ Fail fast — invalid objects cannot be created
def __init__(self, amount: Decimal):
    if amount <= Decimal("0"):
        raise ValueError("Amount must be positive")
    self.amount = amount
```

## Task

Create a `FraudAlert` entity with:
- Attributes: `alert_id`, `transaction_id`, `severity`, `status`
- Constructor validates severity is one of: `["LOW", "MEDIUM", "HIGH", "CRITICAL"]`
- Methods: `acknowledge()`, `resolve(resolution: str)`, `escalate()`
- Status lifecycle: `open` → `acknowledged` → `resolved`
- `escalate()` changes severity to the next level

<!-- SEPARATOR -->

# seed_code
from datetime import datetime

SEVERITY_LEVELS: list[str] = ["LOW", "MEDIUM", "HIGH", "CRITICAL"]

class FraudAlert:
    """Represents a fraud detection alert."""
    
    def __init__(self, alert_id: str, transaction_id: str, severity: str):
        pass  # Replace with your implementation
    
    def acknowledge(self) -> None:
        """Acknowledge the alert (status: open → acknowledged)."""
        pass  # Replace with your implementation
    
    def resolve(self, resolution: str) -> None:
        """Resolve the alert (status: acknowledged → resolved)."""
        pass  # Replace with your implementation
    
    def escalate(self) -> None:
        """Escalate severity to next level."""
        pass  # Replace with your implementation


# Test the FraudAlert
alert = FraudAlert("ALERT-001", "TXN-999", "MEDIUM")

print("=== Fraud Alert Lifecycle ===")
print(f"Initial: {alert.status} / {alert.severity}")

alert.escalate()
print(f"After escalate: {alert.severity}")

alert.acknowledge()
print(f"After acknowledge: {alert.status}")

alert.resolve("False positive - legitimate transaction")
print(f"After resolve: {alert.status}")
print(f"Resolution: {alert.resolution}")

<!-- SEPARATOR -->

# validation_code
# Test constructor validation
try:
    bad_alert = FraudAlert("A1", "T1", "INVALID")
    assert False, "Should raise ValueError for invalid severity"
except ValueError:
    pass

# Test valid creation
alert = FraudAlert("A1", "T1", "LOW")
assert alert.alert_id == "A1", "Should set alert_id"
assert alert.transaction_id == "T1", "Should set transaction_id"
assert alert.severity == "LOW", "Should set severity"
assert alert.status == "open", "Initial status should be 'open'"

# Test escalation
alert.escalate()
assert alert.severity == "MEDIUM", "LOW should escalate to MEDIUM"
alert.escalate()
assert alert.severity == "HIGH", "MEDIUM should escalate to HIGH"

# Test acknowledge
alert.acknowledge()
assert alert.status == "acknowledged", "Status should be 'acknowledged'"

# Test resolve
alert.resolve("Test resolution")
assert alert.status == "resolved", "Status should be 'resolved'"
assert alert.resolution == "Test resolution", "Should store resolution"
