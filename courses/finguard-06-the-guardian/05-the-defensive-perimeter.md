---
id: "finguard_06_05"
title: "The Defensive Perimeter"
type: "coding"
xp: 100
---

# The Defensive Perimeter

Professional code validates **at the boundary** — when data enters your system. Once inside, you trust it.

## The Bridge: Data Quality

In **The Signal**, you learned that data has a lifecycle. **Validation** happens at the boundary between untrusted external data and your trusted internal system.

```
External Data → [Validation Boundary] → Trusted Internal Data
```

## Guard Clauses

Instead of deep nesting, use **guard clauses** — early returns for invalid cases:

```python
# ❌ Deep nesting
def process(data):
    if data is not None:
        if data["amount"] > 0:
            if data["status"] == "active":
                # Finally, the real logic
                return do_something(data)
    return None

# ✅ Guard clauses
def process(data):
    if data is None:
        return None
    if data["amount"] <= 0:
        return None
    if data["status"] != "active":
        return None
    
    # Happy path — data is valid
    return do_something(data)
```

## The Validation Layer Pattern

```python
from decimal import Decimal

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
