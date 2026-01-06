---
id: "finguard_06_02"
title: "Granular Defense"
type: "coding"
xp: 100
---

# Granular Defense

A novice engineer catches everything. An expert engineer catches **what they expect**.

In a banking system, different errors require different actions:
- **`ConnectionError`**: Retry the database connection.
- **`ValueError`**: Reject the transaction (invalid amount).
- **`PermissionError`**: Alert security.

## The Anti-Pattern: The Blindfold

```python
# ❌ BAD: Swallows errors you didn't expect
try:
    process_payment()
except Exception:
    print("Something went wrong")
```
If `process_payment()` failed because of a typo in your code (e.g., `NameError`), you'll never know. You just hid a bug.

## The Pattern: Surgical Precision

We use multiple `except` blocks to handle specific failure modes.

```python
def process_transaction(data: dict) -> str:
    try:
        amount = Decimal(data["amount"])
        if amount == 0:
            return 100 / amount  # Causes ZeroDivisionError
            
    except KeyError:
        return "REJECT: Missing 'amount' field"
        
    except InvalidOperation:
        return "REJECT: Invalid number format"
        
    except ZeroDivisionError:
        return "REJECT: Cannot divide by zero"
        
    # Any other error (e.g., TypeError) will still crash 
    # the program, which is GOOD (fails loudly during dev).
```

## Exception Hierarchy

Python exceptions are classes. `Exception` is the parent of most errors.

**Rule:** Catch specific errors **first**, generic ones **last**.

```python
try:
    ...
except ValueError:     # Specific
    handle_value()
except Exception:      # General (Catch-all)
    log_unexpected_crash()
```

## Task

Implement `safe_divide`.
1.  Accept two strings: `numerator_str`, `denominator_str`.
2.  Convert to `Decimal`.
3.  Handle `InvalidOperation` (bad string) -> return "Invalid Inputs".
4.  Handle `ZeroDivisionError` (divide by zero) -> return "Cannot divide by zero".
5.  If successful, return the result string.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal, InvalidOperation

def safe_divide(num_str: str, den_str: str) -> str:
    try:
        # Implementation
        pass
    except ...:
        pass

# Integration
print(safe_divide("100", "4"))      # -> 25
print(safe_divide("100", "0"))      # -> Cannot divide by zero
print(safe_divide("abc", "4"))      # -> Invalid Inputs

<!-- SEPARATOR -->

# validation_code
# Test Normal
assert safe_divide("10", "2") == str(Decimal("5"))

# Test Zero
assert safe_divide("10", "0") == "Cannot divide by zero"

# Test Invalid Format
assert safe_divide("bad", "2") == "Invalid Inputs"
assert safe_divide("10", "bad") == "Invalid Inputs"

print("Validation passed!")
    handle_missing_key()
```

## Catching Multiple Exceptions with One Handler

```python
try:
    process()
except (ValueError, TypeError, KeyError) as e:
    # Same handler for all three
    log_error(f"Data validation failed: {e}")
```

## The Analogy: The Emergency Room Triage

The ER doesn't treat everyone the same:
- Heart attack → Immediate surgery
- Broken arm → Orthopedics
- Cold → Wait in line

Your error handlers are like triage — different problems, different responses.

## The "Pro" Tip

> **Include the exception variable (`as e`) so you can log what went wrong.**

```python
except ValueError as e:
    logger.error(f"ValueError: {e}")  # Helpful for debugging
```

## Task

Write a `safe_get_balance` function that handles:
- `KeyError`: Account not found
- `TypeError`: Balance is not a number
- Return appropriate error messages for each

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Account database (some accounts have issues)
accounts: dict = {
    "ACC-001": {"holder": "Alice", "balance": Decimal("5000.00")},
    "ACC-002": {"holder": "Bob", "balance": "invalid"},  # Bad data!
    # ACC-003 doesn't exist
}

def safe_get_balance(account_id: str) -> tuple[bool, str]:
    """
    Safely retrieve account balance.
    
    Returns:
        (success, result) where result is balance string or error message
    """
    pass  # Replace with your implementation


# Test the function
print("=== Account Balance Lookup ===")
test_accounts = ["ACC-001", "ACC-002", "ACC-003"]

for acc_id in test_accounts:
    success, result = safe_get_balance(acc_id)
    status = "✓" if success else "✗"
    print(f"{status} {acc_id}: {result}")

<!-- SEPARATOR -->

# validation_code
# Valid account
success, result = safe_get_balance("ACC-001")
assert success == True, "ACC-001 should succeed"
assert "5000" in result, "Should return balance for ACC-001"

# Invalid balance type
success, result = safe_get_balance("ACC-002")
assert success == False, "ACC-002 should fail (bad balance type)"

# Missing account
success, result = safe_get_balance("ACC-003")
assert success == False, "ACC-003 should fail (not found)"
assert "not found" in result.lower() or "missing" in result.lower() or "exist" in result.lower(), "Should indicate account not found"
