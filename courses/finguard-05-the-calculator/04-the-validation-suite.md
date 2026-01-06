---
id: "finguard_05_04"
title: "The Inspector"
type: "coding"
xp: 100
---

# The Inspector

In banking, "I don't know" is not an acceptable answer.
A validation check shouldn't just crash; it should report a detailed **Inspection Result**.

Did the check pass? Variable `True/False`.
If not, why? Variable `Reason`.

## The Tuple Pack

Python allows functions to return multiple values packed into a **Tuple**.

```python
def check_balance(balance: Decimal, amount: Decimal) -> tuple[bool, str]:
    if amount > balance:
        return (False, "Insufficient Funds")
    if amount < 0:
        return (False, "Negative Amount Prohibited")
    
    return (True, "Authorized")
```

## Unpacking The Result

The caller receives this tuple and "unpacks" it immediately.

```python
is_authorized, reason = check_balance(my_balance, my_request)

if not is_authorized:
    print(f"Declined: {reason}")
```

This pattern — `(success_flag, message_payload)` — is a cornerstone of defensive engineering.

## Task

Write a `validate_transfer` function.

**Inputs:** `amount` (Decimal), `is_verified` (bool).

**Logic:**
1.  If `amount` is <= 0: Fail with "Amount must be positive".
2.  If `amount` > 1000 and user is not verified: Fail with "Verification required for amounts over $1000".
3.  Otherwise: Succeed with "Approved".

**Output:** Return `tuple[bool, str]`.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

def validate_transfer(amount: Decimal, is_verified: bool) -> tuple[bool, str]:
    """Inspects transfer for validity."""
    pass

# Test
valid, msg = validate_transfer(Decimal("1500.00"), False)
print(f"Result: {valid} / {msg}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
# Negative/zero amount
assert validate_transfer(Decimal("-10"), True) == (False, "Amount must be positive"), "Negative amount fails"
assert validate_transfer(Decimal("0"), True) == (False, "Amount must be positive"), "Zero amount fails"
# Over threshold, unverified
assert validate_transfer(Decimal("2000"), False) == (False, "Verification required for amounts over $1000"), "Large unverified fails"
# Over threshold, verified
assert validate_transfer(Decimal("2000"), True) == (True, "Approved"), "Large verified passes"
# Under threshold, unverified (should pass)
assert validate_transfer(Decimal("500"), False) == (True, "Approved"), "Small unverified passes"
