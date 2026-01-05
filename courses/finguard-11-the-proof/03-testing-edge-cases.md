---
id: "finguard_11_03"
title: "Testing Edge Cases"
type: "coding"
xp: 100
---

# Testing Edge Cases

The bugs hide at the edges: zero values, empty lists, maximum limits.

## Common Edge Cases

| Category | Edge Cases |
|----------|------------|
| Numbers | 0, -1, max value, min value, decimals |
| Strings | Empty `""`, whitespace `" "`, very long |
| Lists | Empty `[]`, single item, duplicates |
| Dates | Leap years, month boundaries, timezones |

## Boundary Testing

Test right at the limits:

```python
def test_withdrawal_exactly_at_balance():
    """Edge: Withdraw exactly the balance amount."""
    account = BankAccount("A1", Decimal("100.00"))
    result = account.withdraw(Decimal("100.00"))
    assert result == True
    assert account.balance == Decimal("0.00")

def test_withdrawal_one_cent_over_balance():
    """Edge: Withdraw one cent more than balance."""
    account = BankAccount("A1", Decimal("100.00"))
    result = account.withdraw(Decimal("100.01"))
    assert result == False
```

## Parameterized Tests

Test many inputs with one test function:

```python
import pytest

@pytest.mark.parametrize("amount,expected", [
    (Decimal("100.00"), Decimal("1.00")),    # Normal
    (Decimal("0.01"), Decimal("0.00")),       # Tiny
    (Decimal("99999.99"), Decimal("999.99")), # Large
])
def test_fee_calculation(amount, expected):
    fee = calculate_fee(amount)
    assert fee == expected
```

## The Analogy: The Crash Test

Car manufacturers don't just test normal driving. They test:
- Head-on collision (max force)
- Side impact
- Rollover
- At exactly 5 mph (minimum crash)

## The "Pro" Tip

> **Think "what's the weirdest input someone could give?" Then test it.**

## Task

Write edge case tests for a balance transfer function.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

def transfer_balance(
    source_balance: Decimal,
    target_balance: Decimal,
    amount: Decimal
) -> tuple[Decimal, Decimal, bool]:
    """Transfer amount from source to target.
    
    Returns:
        (new_source_balance, new_target_balance, success)
    """
    if amount <= Decimal("0"):
        return (source_balance, target_balance, False)
    
    if amount > source_balance:
        return (source_balance, target_balance, False)
    
    new_source = source_balance - amount
    new_target = target_balance + amount
    return (new_source, new_target, True)


# Edge case tests
def test_transfer_zero_amount():
    """Edge: Transfer zero should fail."""
    pass  # Replace with your implementation


def test_transfer_negative_amount():
    """Edge: Transfer negative should fail."""
    pass  # Replace with your implementation


def test_transfer_exact_balance():
    """Edge: Transfer exactly the source balance."""
    pass  # Replace with your implementation


def test_transfer_one_cent_over():
    """Edge: Transfer one cent more than source balance."""
    pass  # Replace with your implementation


def test_transfer_to_zero_balance():
    """Edge: Transfer to an account with zero balance."""
    pass  # Replace with your implementation


def test_transfer_from_zero_balance():
    """Edge: Transfer from account with zero balance."""
    pass  # Replace with your implementation


def test_transfer_very_small_amount():
    """Edge: Transfer smallest possible amount ($0.01)."""
    pass  # Replace with your implementation


def test_transfer_very_large_amount():
    """Edge: Transfer a very large amount."""
    pass  # Replace with your implementation


# Run edge case tests
print("=== Edge Case Tests ===")

edge_tests = [
    test_transfer_zero_amount,
    test_transfer_negative_amount,
    test_transfer_exact_balance,
    test_transfer_one_cent_over,
    test_transfer_to_zero_balance,
    test_transfer_from_zero_balance,
    test_transfer_very_small_amount,
    test_transfer_very_large_amount,
]

passed = 0
failed = 0

for test_fn in edge_tests:
    try:
        test_fn()
        print(f"✓ {test_fn.__name__}")
        passed += 1
    except AssertionError as e:
        print(f"✗ {test_fn.__name__}: {e}")
        failed += 1

print(f"\nResults: {passed} passed, {failed} failed")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
import inspect

# Run all edge case tests
test_transfer_zero_amount()
test_transfer_negative_amount()
test_transfer_exact_balance()
test_transfer_one_cent_over()
test_transfer_to_zero_balance()
test_transfer_from_zero_balance()
test_transfer_very_small_amount()
test_transfer_very_large_amount()

# Verify tests have proper assertions
source = inspect.getsource(test_transfer_exact_balance)
assert "assert" in source and "True" in source, "Should assert success for exact balance"

source = inspect.getsource(test_transfer_one_cent_over)
assert "assert" in source and "False" in source, "Should assert failure for over balance"
