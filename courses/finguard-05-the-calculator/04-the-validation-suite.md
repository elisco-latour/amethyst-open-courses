---
id: "finguard_05_04"
title: "The Validation Suite"
type: "coding"
xp: 100
---

# The Validation Suite

Functions can return **multiple values** or **different types** based on the situation.

## Returning Multiple Values (Tuples)

```python
from decimal import Decimal

def validate_transaction(amount: Decimal) -> tuple[bool, str]:
    """Validate a transaction amount.
    
    Returns:
        (is_valid, message) tuple
    """
    if amount <= Decimal("0"):
        return (False, "Amount must be positive")
    if amount > Decimal("1000000"):
        return (False, "Amount exceeds daily limit")
    return (True, "Transaction valid")
```

## Using Multiple Return Values

```python
# Unpack into separate variables
is_valid, message = validate_transaction(Decimal("500.00"))

if is_valid:
    process_transaction()
else:
    show_error(message)
```

## The Analogy: The Security Checkpoint

Airport security doesn't just say "pass" or "fail". They give you:
1. The **result**: Cleared / Not Cleared
2. The **reason**: "All clear" or "Please remove laptop from bag"

## Returning `None` for "No Result"

```python
def find_transaction(txn_id: str) -> dict | None:
    """Find a transaction by ID.
    
    Returns:
        Transaction dict if found, None if not found.
    """
    transactions = {"TXN-001": {"amount": Decimal("500")}}
    
    return transactions.get(txn_id)  # Returns None if not found
```

## The "Pro" Tip

> **Use `tuple[bool, str]` for validation functions. The boolean is for code, the message is for humans.**

```python
# ✅ Clear contract
def validate(x) -> tuple[bool, str]:
    if not valid:
        return (False, "Human-readable error")
    return (True, "Success")
```

## Task

Write a `validate_wire_transfer` function that:
- Takes `amount: Decimal`, `sender_balance: Decimal`, `recipient_country: str`
- Returns `tuple[bool, str]` (is_valid, message)
- Validates:
  1. Amount must be positive
  2. Sender must have sufficient balance
  3. Recipient country must not be in sanctions list: `["NK", "IR", "SY"]`

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

SANCTIONED_COUNTRIES: list[str] = ["NK", "IR", "SY"]

def validate_wire_transfer(
    amount: Decimal,
    sender_balance: Decimal,
    recipient_country: str
) -> tuple[bool, str]:
    """
    Validate a wire transfer.
    
    Returns:
        (is_valid, message) tuple
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
