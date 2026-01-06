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

# Test negative amount rejection
try:
    Transaction("T1", Decimal("-10"))
    assert False, "Should raise ValueError for negative amount"
except ValueError:
    pass

# Test zero amount rejection
try:
    Transaction("T2", Decimal("0"))
    assert False, "Should raise ValueError for zero amount"
except ValueError:
    pass

# Test valid creation and initial state
t = Transaction("T1", Decimal("10"))
assert t.status == "pending", "Initial status should be 'pending'"

# Test process transition
t.process()
assert t.status == "processed", "Status should be 'processed' after calling process()"

# Test double process prevention
try:
    t.process()
    assert False, "Should raise error when processing already-processed transaction"
except ValueError:
    pass
