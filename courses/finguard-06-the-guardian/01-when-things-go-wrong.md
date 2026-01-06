---
id: "finguard_06_01"
title: "System Resilience"
type: "coding"
xp: 100
---

# System Resilience

In critical infrastructure (banking, healthcare, aerospace), **failure is an option**, but **catastrophe is not**.

Your system **will** encounter invalid data, network timeouts, and disk errors. A fragile system crashes. A resilient system **handles** the fault and recovers (or fails safely).

## The Crash vs. The Catch

When Python sees an error, it raises an **Exception**. If unhandled, the program crashes.

```python
# ❌ Fragile: One bad value kills the entire process
amount = int("invalid")  # CRASH!
print("Transaction saved")  # Never runs
```

## The Safety Mechanism: `try/except`

We wrap dangerous code in a `try` block. If it fails, the `except` block catches the specific error.

```python
def parse_amount(value_str: str) -> int:
    try:
        # 1. Attempt the risky operation
        return int(value_str)
    except ValueError:
        # 2. Handle the specific failure
        # Log it, return default, or re-raise
        print(f"⚠️ Error: '{value_str}' is not a valid integer")
        return 0  # Fallback value

# The system stays alive
print(parse_amount("100"))      # -> 100
print(parse_amount("invalid"))  # -> 0 (No crash)
```

## Engineering Mindset: EAFP

Python engineers follow **EAFP**: "It's Easier to Ask Forgiveness than Permission."

**LBYL (Look Before You Leap):**
```python
if value.isdigit():
    num = int(value)
```

**EAFP (Pythonic):**
```python
try:
    num = int(value)
except ValueError:
    handle_error()
```
EAFP is often faster and handles edge cases (what if the string is empty?) more robustly.

## Task

Create a resilient `convert_to_decimal` function.
1.  Accept a string input.
2.  Use `try/except` to catch `InvalidOperation` (from `decimal` module).
3.  Return `None` if conversion fails.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal, InvalidOperation
from typing import Optional

def convert_to_decimal(value: str) -> Optional[Decimal]:
    """
    Safely converts a string to Decimal.
    Returns None if conversion fails.
    """
    # Implementation
    pass

# Integration
values = ["100.50", "invalid", "500.00"]
for v in values:
    result = convert_to_decimal(v)
    if result:
        print(f"✅ Processed: {result}")
    else:
        print(f"❌ Failed: {v}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Test valid conversions
assert convert_to_decimal("10.5") == Decimal("10.5"), "Should convert valid decimal"
assert convert_to_decimal("100") == Decimal("100"), "Should convert integer string"

# Test invalid conversions
assert convert_to_decimal("abc") is None, "Invalid string should return None"
assert convert_to_decimal("") is None, "Empty string should return None"
assert convert_to_decimal("12.34.56") is None, "Multiple decimals should return None"
for raw in raw_amounts:
    pass  # Replace with your implementation


print("=== Transaction Parsing Results ===")
print(f"Successfully parsed: {len(parsed_amounts)}")
print(f"Failed to parse: {failed_count}")
print(f"Valid amounts: {parsed_amounts}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert len(parsed_amounts) == 3, "Should have 3 valid amounts"
assert failed_count == 2, "Should have 2 failed parses"
assert Decimal("500.00") in parsed_amounts, "500.00 should be in parsed_amounts"
assert Decimal("1500.50") in parsed_amounts, "1500.50 should be in parsed_amounts"
assert Decimal("2000.00") in parsed_amounts, "2000.00 should be in parsed_amounts"
