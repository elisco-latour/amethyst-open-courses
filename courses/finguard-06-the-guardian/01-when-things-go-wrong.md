---
id: "finguard_06_01"
title: "When Things Go Wrong"
type: "coding"
xp: 100
---

# When Things Go Wrong

In banking, errors can mean **lost money**, **compliance violations**, or **system outages**. Your code must handle failures gracefully.

## The Problem: Unhandled Errors

```python
amount: str = "not a number"
value = int(amount)  # 💥 ValueError: invalid literal for int()
print("This line never runs")
```

When Python encounters an error, it **stops immediately**. In a banking system, this could leave a transaction half-complete.

## The Solution: Try/Except

```python
amount: str = "not a number"

try:
    value = int(amount)
    print(f"Value: {value}")
except ValueError:
    print("Invalid amount — could not convert to integer")

print("Program continues safely")
```

## The Analogy: The Safety Net

A trapeze artist doesn't fall to their death if they miss a catch — there's a **safety net**.

`try/except` is your safety net. The program doesn't crash; it **catches** the error and continues.

## Common Error Types

| Error | When It Happens |
|-------|-----------------|
| `ValueError` | Wrong value type ("abc" → int) |
| `KeyError` | Dict key doesn't exist |
| `TypeError` | Wrong type for operation |
| `ZeroDivisionError` | Division by zero |
| `IndexError` | List index out of range |

## The "Pro" Tip

> **Catch specific exceptions, not all exceptions. `except Exception:` hides bugs.**

```python
# ❌ Too broad — hides bugs
try:
    process()
except Exception:
    pass

# ✅ Specific — you know what went wrong
try:
    process()
except ValueError as e:
    log_error(f"Invalid value: {e}")
except KeyError as e:
    log_error(f"Missing key: {e}")
```

## Task

Write code that safely parses transaction amounts from strings:
- Handle `ValueError` if the string isn't a valid number
- Track how many succeeded and how many failed

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal, InvalidOperation

# Raw transaction data (some are invalid)
raw_amounts: list[str] = [
    "500.00",
    "invalid",
    "1500.50",
    "N/A",
    "2000.00",
]

parsed_amounts: list[Decimal] = []
failed_count: int = 0

# Parse each amount safely
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
