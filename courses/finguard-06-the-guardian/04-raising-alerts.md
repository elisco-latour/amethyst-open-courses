---
id: "finguard_06_04"
title: "The Active Alarm"
type: "coding"
xp: 100
---

# The Active Alarm

So far, we've **caught** errors others made. Now, we **raise** our own.

You are the guardian of your function. If someone passes you bad data, do not try to fix it. **Raise an alarm.**

## The `raise` Statement

Use `raise` to stop execution immediately and signal a violation of the function's contract.

```python
from decimal import Decimal

def deposit(balance: Decimal, amount: Decimal) -> Decimal:
    if amount <= 0:
        # Stop immediately! Do not process negative deposits.
        raise ValueError(f"Deposit amount must be positive. Got: {amount}")
    
    return balance + amount
```

## When to Raise?

1.  **Contract Violations:** Arguments are the wrong type or range.
2.  **Invalid State:** The system is in a state where the operation is impossible (e.g., "Account Closed").
3.  **Fatal Errors:** Something happened that the current function cannot handle.

## Re-Raising (The Observer)

Sometimes you want to know an error happened, but you can't fix it. You catch it, log it, and throw it again.

```python
try:
    process_payment()
except ConnectionError as e:
    logger.error(f"Payment failed: {e}")
    raise  # 👈 Throws the exact same error up to the caller
```

## Task

Implement `withdraw(balance: Decimal, amount: Decimal)`.
1.  Check if `amount` is negative. If so, raise `ValueError("Amount must be positive")`.
2.  Check if `amount` > `balance`. If so, raise `ValueError("Insufficient funds")`.
3.  Return the new balance.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

def withdraw(balance: Decimal, amount: Decimal) -> Decimal:
    """
    Withdraws amount from balance.
    Raises ValueError on invalid inputs.
    """
    # 1. Validation Logic
    
    # 2. Calculation
    return balance - amount

# Integration
try:
    withdraw(Decimal("100"), Decimal("200"))
except ValueError as e:
    print(f"✅ Caught Expected Error: {e}")

try:
    withdraw(Decimal("100"), Decimal("-50"))
except ValueError as e:
    print(f"✅ Caught Expected Error: {e}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Test Negative
try:
    withdraw(Decimal("100"), Decimal("-1"))
    assert False, "Should have raised ValueError for negative amount"
except ValueError:
    pass

# Test Overdraft
try:
    withdraw(Decimal("50"), Decimal("100"))
    assert False, "Should have raised ValueError for overdraft"
except ValueError:
    pass

# Test Success
assert withdraw(Decimal("100"), Decimal("40")) == Decimal("60")
print("Validation passed!")
    process()
except ValueError as e:
    logger.error(f"Validation failed: {e}")
    raise  # Re-raise the same exception
```

## The "Pro" Tip

> **Fail fast and fail loud. If input is invalid, raise immediately. Don't let bad data propagate through your system.**

```python
def process_payment(amount: Decimal) -> None:
    # ✅ Validate immediately
    if amount <= Decimal("0"):
        raise ValueError("Amount must be positive")
    
    # If we get here, amount is valid
    execute_payment(amount)
```

## Task

Write a `validate_iban` function that raises `ValueError` with descriptive messages for:
- Empty IBAN
- IBAN shorter than 15 characters
- IBAN longer than 34 characters
- IBAN not starting with two letters

Return `True` if valid.

<!-- SEPARATOR -->

# seed_code
def validate_iban(iban: str) -> bool:
    """
    Validate an IBAN (International Bank Account Number).
    
    Raises:
        ValueError: If IBAN is invalid
    
    Returns:
        True if valid
    """
    pass  # Replace with your implementation


# Test the validator
test_ibans = [
    "DE89370400440532013000",  # Valid German IBAN
    "",                         # Empty
    "DE1234567890",            # Too short
    "12DE370400440532013000",  # Doesn't start with letters
    "A" * 50,                   # Too long
]

print("=== IBAN Validation ===")
for iban in test_ibans:
    display_iban = iban if iban else "(empty)"
    try:
        validate_iban(iban)
        print(f"✓ {display_iban}: Valid")
    except ValueError as e:
        print(f"✗ {display_iban}: {e}")

<!-- SEPARATOR -->

# validation_code
# Valid IBAN should return True
assert validate_iban("DE89370400440532013000") == True, "Valid IBAN should return True"

# Empty IBAN
try:
    validate_iban("")
    assert False, "Empty IBAN should raise ValueError"
except ValueError as e:
    assert "empty" in str(e).lower(), "Should mention empty"

# Too short
try:
    validate_iban("DE1234567890")
    assert False, "Short IBAN should raise ValueError"
except ValueError as e:
    assert "short" in str(e).lower() or "15" in str(e) or "length" in str(e).lower(), "Should mention length"

# Doesn't start with letters
try:
    validate_iban("12DE370400440532013000")
    assert False, "IBAN not starting with letters should raise ValueError"
except ValueError as e:
    assert "letter" in str(e).lower() or "start" in str(e).lower(), "Should mention letters"
