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
assert validate_transfer(Decimal("-10"), True) == (False, "Amount must be positive")
assert validate_transfer(Decimal("2000"), False) == (False, "Verification required for amounts over $1000")
assert validate_transfer(Decimal("2000"), True) == (True, "Approved")
    """
    pass  # Replace with your implementation


# Test the validation
print("=== Wire Transfer Validation ===")

test_cases = [
    (Decimal("500.00"), Decimal("1000.00"), "US"),
    (Decimal("-100.00"), Decimal("1000.00"), "US"),
    (Decimal("500.00"), Decimal("100.00"), "US"),
    (Decimal("500.00"), Decimal("1000.00"), "NK"),
]

for amount, balance, country in test_cases:
    is_valid, message = validate_wire_transfer(amount, balance, country)
    status = "✓" if is_valid else "✗"
    print(f"{status} ${amount} to {country} (balance: ${balance}): {message}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
# Valid transfer
valid, msg = validate_wire_transfer(Decimal("500.00"), Decimal("1000.00"), "US")
assert valid == True, "Valid transfer should pass"

# Negative amount
valid, msg = validate_wire_transfer(Decimal("-100.00"), Decimal("1000.00"), "US")
assert valid == False, "Negative amount should fail"
assert "positive" in msg.lower() or "amount" in msg.lower(), "Should mention amount issue"

# Insufficient balance
valid, msg = validate_wire_transfer(Decimal("500.00"), Decimal("100.00"), "US")
assert valid == False, "Insufficient balance should fail"
assert "balance" in msg.lower() or "insufficient" in msg.lower(), "Should mention balance issue"

# Sanctioned country
valid, msg = validate_wire_transfer(Decimal("500.00"), Decimal("1000.00"), "NK")
assert valid == False, "Sanctioned country should fail"
assert "sanction" in msg.lower() or "country" in msg.lower() or "blocked" in msg.lower(), "Should mention sanctions"
