---
id: "finguard_06_04"
title: "The Active Alarm"
type: "coding"
xp: 100
---

# The Active Alarm

So far, we've **caught** errors others made. Now, we **raise** our own.

You are the guardian of your function. If someone passes you bad data, do not try to fix it. **Raise an alarm.**

## The `raise` Statement

Use `raise` to stop execution immediately and signal a violation of the function's contract.

```python
from decimal import Decimal

def deposit(balance: Decimal, amount: Decimal) -> Decimal:
    if amount <= 0:
        # Stop immediately! Do not process negative deposits.
        raise ValueError(f"Deposit amount must be positive. Got: {amount}")
    
    return balance + amount
```

## When to Raise?

1.  **Contract Violations:** Arguments are the wrong type or range.
2.  **Invalid State:** The system is in a state where the operation is impossible (e.g., "Account Closed").
3.  **Fatal Errors:** Something happened that the current function cannot handle.

## Re-Raising (The Observer)

Sometimes you want to know an error happened, but you can't fix it. You catch it, log it, and throw it again.

```python
try:
    process_payment()
except ConnectionError as e:
    logger.error(f"Payment failed: {e}")
    raise  # 👈 Throws the exact same error up to the caller
```

## Task

Implement `withdraw(balance: Decimal, amount: Decimal)`.
1.  Check if `amount` is negative. If so, raise `ValueError("Amount must be positive")`.
2.  Check if `amount` > `balance`. If so, raise `ValueError("Insufficient funds")`.
3.  Return the new balance.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

def withdraw(balance: Decimal, amount: Decimal) -> Decimal:
    """
    Withdraws amount from balance.
    Raises ValueError on invalid inputs.
    """
    # 1. Validation Logic
    
    # 2. Calculation
    return balance - amount

# Integration
try:
    withdraw(Decimal("100"), Decimal("200"))
except ValueError as e:
    print(f"✅ Caught Expected Error: {e}")

try:
    withdraw(Decimal("100"), Decimal("-50"))
except ValueError as e:
    print(f"✅ Caught Expected Error: {e}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Test negative amount
try:
    withdraw(Decimal("100"), Decimal("-1"))
    assert False, "Should have raised ValueError for negative amount"
except ValueError as e:
    assert "positive" in str(e).lower(), "Error should mention 'positive'"

# Test overdraft
try:
    withdraw(Decimal("50"), Decimal("100"))
    assert False, "Should have raised ValueError for overdraft"
except ValueError as e:
    assert "insufficient" in str(e).lower() or "funds" in str(e).lower(), "Error should mention insufficient funds"

# Test successful withdrawal
assert withdraw(Decimal("100"), Decimal("40")) == Decimal("60"), "100 - 40 should equal 60"
assert withdraw(Decimal("500"), Decimal("500")) == Decimal("0"), "Exact balance withdrawal should work"
