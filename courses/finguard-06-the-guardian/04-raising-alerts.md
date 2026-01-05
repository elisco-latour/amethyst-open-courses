---
id: "finguard_06_04"
title: "Raising Alerts"
type: "coding"
xp: 100
---

# Raising Alerts

Sometimes **you** need to signal an error. When validation fails, you raise an exception.

## The `raise` Statement

```python
from decimal import Decimal

def transfer(amount: Decimal, balance: Decimal) -> Decimal:
    if amount <= Decimal("0"):
        raise ValueError("Transfer amount must be positive")
    
    if amount > balance:
        raise ValueError("Insufficient funds")
    
    return balance - amount
```

## When to Raise

Raise an exception when:
- A function receives invalid input
- A business rule is violated
- Something unexpected happens that the function can't handle

## Catching Your Own Exceptions

```python
try:
    new_balance = transfer(Decimal("-100"), Decimal("1000"))
except ValueError as e:
    print(f"Transfer failed: {e}")
```

## The Analogy: The Fire Alarm

If you see a fire, you **raise** the alarm. You don't try to put it out yourself if you can't — you alert others (the caller) to handle it.

## Re-Raising After Logging

```python
try:
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
