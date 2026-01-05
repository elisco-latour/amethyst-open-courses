---
id: "finguard_06_02"
title: "The Multi-Error Shield"
type: "coding"
xp: 100
---

# The Multi-Error Shield

Different errors need different responses. A validation error should show a message; a database error might need a retry.

## Handling Multiple Exception Types

```python
def process_transaction(data: dict) -> str:
    try:
        amount = data["amount"]  # Might raise KeyError
        value = Decimal(amount)  # Might raise InvalidOperation
        result = value / data["exchange_rate"]  # Might raise ZeroDivisionError
        return f"Processed: {result}"
    
    except KeyError as e:
        return f"Missing required field: {e}"
    
    except InvalidOperation:
        return "Invalid amount format"
    
    except ZeroDivisionError:
        return "Exchange rate cannot be zero"
```

## The Order Matters

Python checks exceptions **in order**. Put specific exceptions first, general last.

```python
# ✅ Correct: specific first
try:
    ...
except KeyError:
    handle_missing_key()
except Exception:
    handle_everything_else()

# ❌ Wrong: general first catches everything
try:
    ...
except Exception:  # This catches KeyError too!
    handle_everything()
except KeyError:  # Never reached!
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
