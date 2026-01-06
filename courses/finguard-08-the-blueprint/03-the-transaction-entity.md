---
id: "finguard_08_03"
title: "Domain Logic"
type: "coding"
xp: 100
---

# Domain Logic

A generic `dict` lets you set `transaction["status"] = "banana"`.
A Domain Entity prevents this.

**Domain Logic** means your code represents the business rules, not just the data structure.

## Enforcing Invariants

An **Invariant** is a truth that must always be upheld.
Example: "A transaction amount cannot be negative."

We enforce invariants in the **Constructor** and **Methods**.

```python
class Transaction:
    def __init__(self, amount: int):
        # 🛡️ Guard Clause (Invariant)
        if amount <= 0:
            raise ValueError("Amount must be positive")
        
        self.amount = amount
        self.status = "pending"

    def complete(self):
        # 🛡️ State Validation
        if self.status != "pending":
            raise ValueError(f"Cannot complete a {self.status} transaction")
        
        self.status = "completed"
```

Now, it is **impossible** to create an invalid transaction or break the lifecycle. The Class defends itself.

## Task

Create `Transaction` class.
1. `__init__`: Accepts `id` and `amount`. Validator: `amount` must be positive.
2. `process()`: Checks if status is "pending". If so, sets to "processed". Else raises `ValueError`.
3. Initial status is "pending".

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

class Transaction:
    def __init__(self, id: str, amount: Decimal):
        self.id = id
        self.status = "pending"
        # validation logic here
        self.amount = amount

    def process(self):
        # transition logic here
        pass

# Integration
t = Transaction("T1", Decimal("100"))
t.process()
print(t.status)

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Test Negative
try:
    Transaction("T1", Decimal("-10"))
    assert False, "Should raise ValueError for negative"
except ValueError:
    pass

# Test Transition
t = Transaction("T1", Decimal("10"))
assert t.status == "pending"
t.process()
assert t.status == "processed"

# Test Double Process
try:
    t.process()
    assert False, "Should raise error on double process"
except ValueError:
    pass

print("Validation passed!")
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
