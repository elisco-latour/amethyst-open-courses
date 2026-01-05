---
id: "finguard_11_01"
title: "Why Test?"
type: "coding"
xp: 100
---

# Why Test?

## The Bridge: Data Quality

In data engineering, **bad data** is worse than no data. A test catches bugs **before** they corrupt your data lake.

## The Cost of Bugs

| Stage | Cost to Fix |
|-------|-------------|
| Development | $1 |
| Testing | $10 |
| Production | $100 |
| After customer impact | $1,000+ |

Tests shift bug discovery **left** (earlier = cheaper).

## Your First Test

```python
# calculator.py
def add(a: int, b: int) -> int:
    return a + b

# test_calculator.py
def test_add():
    assert add(2, 3) == 5
    assert add(-1, 1) == 0
    assert add(0, 0) == 0
```

## The `assert` Statement

`assert` checks if a condition is true:

```python
assert 2 + 2 == 4       # Passes silently
assert 2 + 2 == 5       # Raises AssertionError!
```

## Running Tests with pytest

```bash
# Install pytest
pip install pytest

# Run all tests
pytest

# Run specific file
pytest test_calculator.py

# Verbose output
pytest -v
```

## The Analogy: The Pilot Checklist

Pilots don't "feel" if the plane is safe. They run through a **checklist** before every flight.

Tests are your checklist. Run them before every deploy.

## Test Naming Convention

```python
def test_<what>_<condition>_<expected>():
    ...

# Examples
def test_add_positive_numbers_returns_sum():
    assert add(2, 3) == 5

def test_add_negative_number_subtracts():
    assert add(5, -3) == 2
```

## The "Pro" Tip

> **Test names should read like specifications: `test_withdrawal_insufficient_funds_returns_false`**

## Task

Write tests for a transaction fee calculator.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# The function to test
def calculate_fee(amount: Decimal, fee_rate: Decimal = Decimal("0.01")) -> Decimal:
    """Calculate transaction fee.
    
    Args:
        amount: Transaction amount (must be positive)
        fee_rate: Fee percentage (default 1%)
    
    Returns:
        Fee amount rounded to 2 decimal places
    
    Raises:
        ValueError: If amount is not positive
    """
    if amount <= Decimal("0"):
        raise ValueError("Amount must be positive")
    
    fee = amount * fee_rate
    return fee.quantize(Decimal("0.01"))


# Write your tests
def test_calculate_fee_basic():
    """Test basic fee calculation."""
    pass  # Replace with your implementation


def test_calculate_fee_custom_rate():
    """Test fee with custom rate."""
    pass  # Replace with your implementation


def test_calculate_fee_rounding():
    """Test fee rounds to 2 decimal places."""
    pass  # Replace with your implementation


def test_calculate_fee_zero_amount_raises():
    """Test that zero amount raises ValueError."""
    pass  # Replace with your implementation


def test_calculate_fee_negative_amount_raises():
    """Test that negative amount raises ValueError."""
    pass  # Replace with your implementation


# Run tests manually (in real pytest, this happens automatically)
print("=== Running Tests ===")

test_functions = [
    test_calculate_fee_basic,
    test_calculate_fee_custom_rate,
    test_calculate_fee_rounding,
    test_calculate_fee_zero_amount_raises,
    test_calculate_fee_negative_amount_raises,
]

passed = 0
failed = 0

for test_fn in test_functions:
    try:
        test_fn()
        print(f"✓ {test_fn.__name__}")
        passed += 1
    except Exception as e:
        print(f"✗ {test_fn.__name__}: {e}")
        failed += 1

print(f"\nResults: {passed} passed, {failed} failed")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Verify tests are implemented (not just pass)
import inspect

# Test basic calculation test
test_calculate_fee_basic()  # Should not raise

# Test custom rate test
test_calculate_fee_custom_rate()  # Should not raise

# Test rounding test
test_calculate_fee_rounding()  # Should not raise

# Test error cases are properly tested
test_calculate_fee_zero_amount_raises()
test_calculate_fee_negative_amount_raises()

# Verify tests actually have assertions
source_basic = inspect.getsource(test_calculate_fee_basic)
assert "assert" in source_basic, "test_calculate_fee_basic should contain assertions"

source_raises = inspect.getsource(test_calculate_fee_zero_amount_raises)
assert "ValueError" in source_raises or "except" in source_raises or "try" in source_raises, "Error test should check for ValueError"
