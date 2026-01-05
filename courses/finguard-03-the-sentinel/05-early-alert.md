---
id: "finguard_03_05"
title: "Defensive Programming"
type: "coding"
xp: 100
---

# Defensive Programming

Beginners write code that hopes everything goes right (the "Happy Path").
Engineers write code that expects things to go wrong (the "Defensive Path").

We want to **Fail Fast**. If a transaction is invalid, reject it *immediately*. Don't let it wander deep into the system.

## The Pattern: Guard Clauses

Instead of nesting `if` statements like a pyramid, we use **Guard Clauses**. These are gatekeepers at the very top of your logic.

### The Nested Mess (Bad)
```python
if account_active:
    if not is_fraud:
        if balance > amount:
            process() # Hard to find this!
```

### The Guarded Cleanliness (Good)
```python
if not account_active:
    status = "REJECTED: Account Inactive"
elif is_fraud:
    status = "REJECTED: Fraud Detected"
elif balance < amount:
    status = "REJECTED: Insufficient Funds"
else:
    status = "APPROVED" # The Happy Path is usually last
```

This structure is a checklist. We check for disqualifications one by one. If you survive the gauntlet, you are approved.

## Task

Write a Defensive Validation sequence for a transaction in this specific order:

1.  If `account_is_blocked` is `True` -> `"BLOCKED"`
2.  If `amount` <= 0 -> `"INVALID_AMOUNT"`
3.  If `amount` > `balance` -> `"INSUFFICIENT_FUNDS"`
4.  If `is_verified` is `False` -> `"PENDING_VERIFICATION"`
5.  If none of the above -> `"APPROVED"`

Store the result in `transaction_status`.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Context
amount: Decimal = Decimal("500.00")
balance: Decimal = Decimal("1000.00")
account_is_blocked: bool = False
is_verified: bool = True

# Defensive Validation (Guard Clauses)



# Audit
print(f"Status: {transaction_status}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert transaction_status == "APPROVED"
print(f"Status: {transaction_status}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert amount == Decimal("500.00"), "amount should be Decimal('500.00')"
assert available_balance == Decimal("1000.00"), "available_balance should be Decimal('1000.00')"
assert transaction_status == "APPROVED", "transaction_status should be 'APPROVED' for valid transaction"
