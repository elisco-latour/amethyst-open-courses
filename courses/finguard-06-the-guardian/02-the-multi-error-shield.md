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
from decimal import Decimal

# Test normal division
assert safe_divide("10", "2") == str(Decimal("5")), "10/2 should equal 5"

# Test divide by zero
assert safe_divide("10", "0") == "Cannot divide by zero", "Should handle zero division"

# Test invalid inputs
assert safe_divide("bad", "2") == "Invalid Inputs", "Should handle invalid numerator"
assert safe_divide("10", "bad") == "Invalid Inputs", "Should handle invalid denominator"
