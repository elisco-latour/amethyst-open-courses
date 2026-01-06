---
id: "finguard_06_05"
title: "The Guard Pattern"
type: "coding"
xp: 100
---

# The Guard Pattern

Beginners write code that looks like an arrow (`>`). Experts write code that looks like a flat list.

Deeply nested `if` statements are hard to read and test. We solve this with **Guard Clauses**.

## The Anti-Pattern: Arrow Code

```python
def process_loan(application):
    if application is not None:
        if application.is_complete:
            if application.score > 700:
                # Actual logic is buried 3 levels deep
                approve_loan()
            else:
                reject_low_score()
        else:
            reject_incomplete()
    else:
        raise ValueError("No application")
```

## The Solution: Fail Fast

Handle the invalid cases **first** and return/raise immediately. The "Happy Path" stays at the bottom, indentation-free.

```python
def process_loan(application):
    # Guard 1: Existence
    if application is None:
        raise ValueError("No application")

    # Guard 2: Completeness
    if not application.is_complete:
        reject_incomplete()
        return

    # Guard 3: Score
    if application.score <= 700:
        reject_low_score()
        return

    # The Happy Path (No indentation!)
    approve_loan()
```

## Benefits
1.  **Readability:** You can read the checks top-to-bottom.
2.  **Cognitive Load:** You don't have to remember "I am inside 3 `if` blocks".
3.  **Diffs:** Changing logic doesn't require re-indenting the whole function.

## Task

Refactor `verify_transaction` to use Guard Clauses.
1. Return `False` immediately if `transaction` is empty.
2. Return `False` immediately if `amount` is negative.
3. Return `False` immediately if `currency` is not "USD".
4. Return `True` at the end (Happy Path).

<!-- SEPARATOR -->

# seed_code
def verify_transaction(transaction: dict) -> bool:
    """
    Refactor this to use Guard Clauses.
    """
    if transaction:
        if transaction.get("amount", 0) > 0:
            if transaction.get("currency") == "USD":
                return True
            else:
                return False
        else:
            return False
    else:
        return False

# Integration
t1 = {"amount": 100, "currency": "USD"}
t2 = {"amount": -50, "currency": "USD"} 
print(f"Valid: {verify_transaction(t1)}")   # True
print(f"Invalid: {verify_transaction(t2)}") # False

<!-- SEPARATOR -->

# validation_code
# Test cases
assert verify_transaction({"amount": 100, "currency": "USD"}) is True
assert verify_transaction({}) is False
assert verify_transaction({"amount": -10, "currency": "USD"}) is False
assert verify_transaction({"amount": 100, "currency": "EUR"}) is False

# Check for cleanliness (This is a simplified check, real strict check checks AST)
# We trust the student to follow the pattern for XP.
print("Validation passed!")

def validate_transaction(raw_data: dict) -> dict:
    """Validate and normalize raw transaction data.
    
    Raises:
        ValueError: If validation fails
    
    Returns:
        Validated transaction dict
    """
    # Guard clauses
    if not isinstance(raw_data, dict):
        raise ValueError("Transaction must be a dictionary")
    
    if "amount" not in raw_data:
        raise ValueError("Missing required field: amount")
    
    if "account_id" not in raw_data:
        raise ValueError("Missing required field: account_id")
    
    # Parse and validate amount
    try:
        amount = Decimal(str(raw_data["amount"]))
    except Exception:
        raise ValueError("Amount must be a valid number")
    
    if amount <= Decimal("0"):
        raise ValueError("Amount must be positive")
    
    # Return validated, normalized data
    return {
        "amount": amount,
        "account_id": str(raw_data["account_id"]),
        "validated": True
    }
```

## The "Pro" Tip

> **Validate once at the boundary. Don't scatter validation throughout your codebase.**

```python
# ❌ Validation scattered everywhere
def step_1(data):
    if data["amount"] <= 0:  # Validation
        return None
    ...

def step_2(data):
    if data["amount"] <= 0:  # Same validation again!
        return None
    ...

# ✅ Validate at entry point
def process(raw_data):
    validated = validate_transaction(raw_data)  # One place
    step_1(validated)  # Trust the data
    step_2(validated)  # Trust the data
```

## Task

Build a complete validation layer for incoming wire transfers:
1. Create `validate_wire_transfer(raw_data: dict) -> dict`
2. Validate required fields: `sender`, `recipient`, `amount`, `currency`
3. Validate amount is positive
4. Validate currency is in allowed list: `["USD", "EUR", "GBP"]`
5. Return normalized data or raise `ValueError`

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal, InvalidOperation

ALLOWED_CURRENCIES: list[str] = ["USD", "EUR", "GBP"]

def validate_wire_transfer(raw_data: dict) -> dict:
    """
    Validate incoming wire transfer data.
    
    Required fields: sender, recipient, amount, currency
    
    Raises:
        ValueError: If validation fails
    
    Returns:
        Validated and normalized transfer dict
    """
    pass  # Replace with your implementation


# Test the validation layer
test_transfers = [
    # Valid transfer
    {"sender": "ACC-001", "recipient": "ACC-002", "amount": "500.00", "currency": "USD"},
    # Missing field
    {"sender": "ACC-001", "amount": "500.00", "currency": "USD"},
    # Invalid currency
    {"sender": "ACC-001", "recipient": "ACC-002", "amount": "500.00", "currency": "BTC"},
    # Negative amount
    {"sender": "ACC-001", "recipient": "ACC-002", "amount": "-100", "currency": "USD"},
]

print("=== Wire Transfer Validation ===")
for i, transfer in enumerate(test_transfers, 1):
    try:
        validated = validate_wire_transfer(transfer)
        print(f"✓ Transfer {i}: Valid → {validated}")
    except ValueError as e:
        print(f"✗ Transfer {i}: {e}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Valid transfer
valid = validate_wire_transfer({
    "sender": "ACC-001",
    "recipient": "ACC-002",
    "amount": "500.00",
    "currency": "USD"
})
assert valid["sender"] == "ACC-001", "Should preserve sender"
assert valid["recipient"] == "ACC-002", "Should preserve recipient"
assert valid["amount"] == Decimal("500.00"), "Should convert amount to Decimal"
assert valid["currency"] == "USD", "Should preserve currency"

# Missing field
try:
    validate_wire_transfer({"sender": "ACC-001", "amount": "500.00", "currency": "USD"})
    assert False, "Missing recipient should raise ValueError"
except ValueError as e:
    assert "recipient" in str(e).lower() or "required" in str(e).lower() or "missing" in str(e).lower()

# Invalid currency
try:
    validate_wire_transfer({"sender": "A", "recipient": "B", "amount": "100", "currency": "BTC"})
    assert False, "Invalid currency should raise ValueError"
except ValueError as e:
    assert "currency" in str(e).lower()

# Negative amount
try:
    validate_wire_transfer({"sender": "A", "recipient": "B", "amount": "-100", "currency": "USD"})
    assert False, "Negative amount should raise ValueError"
except ValueError as e:
    assert "amount" in str(e).lower() or "positive" in str(e).lower()
