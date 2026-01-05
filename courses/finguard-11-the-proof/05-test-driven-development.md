---
id: "finguard_11_05"
title: "Test-Driven Development"
type: "coding"
xp: 100
---

# Test-Driven Development (TDD)

Write tests **before** code. Let tests drive your design.

## The TDD Cycle: Red → Green → Refactor

1. **RED**: Write a failing test
2. **GREEN**: Write minimal code to pass
3. **REFACTOR**: Improve code, keep tests passing

```
   ┌─────────────────┐
   │   Write Test    │ ← Start here
   │   (RED)         │
   └────────┬────────┘
            │
            ▼
   ┌─────────────────┐
   │   Write Code    │
   │   (GREEN)       │
   └────────┬────────┘
            │
            ▼
   ┌─────────────────┐
   │   Refactor      │
   │   (CLEAN)       │
   └────────┬────────┘
            │
            └──────────────→ (repeat)
```

## Example: Building a Fee Calculator with TDD

### Step 1: RED - Write failing test

```python
def test_flat_fee_for_small_amounts():
    calc = FeeCalculator()
    fee = calc.calculate(Decimal("50.00"))
    assert fee == Decimal("1.00")  # Flat $1 for amounts under $100
```

Test fails: `FeeCalculator` doesn't exist!

### Step 2: GREEN - Make it pass

```python
class FeeCalculator:
    def calculate(self, amount: Decimal) -> Decimal:
        return Decimal("1.00")  # Simplest thing that works
```

### Step 3: Add another test, refactor

```python
def test_percentage_fee_for_large_amounts():
    calc = FeeCalculator()
    fee = calc.calculate(Decimal("1000.00"))
    assert fee == Decimal("10.00")  # 1% for amounts over $100
```

Now refactor `calculate()` to handle both cases.

## Why TDD?

| Benefit | Explanation |
|---------|-------------|
| **Design guidance** | Tests force you to think about interface first |
| **Confidence** | Every feature has a test from birth |
| **Documentation** | Tests show how code is meant to be used |
| **Small steps** | Forces incremental development |

## The "Pro" Tip

> **TDD is about design, not testing. The tests are a side effect of good design thinking.**

## Task

Use TDD to build a `TransactionBatcher` class.

Requirements (write tests first!):
1. Can add transactions
2. Returns batch when count reaches limit (3)
3. Clears batch after returning
4. Can force-flush incomplete batch

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# TDD Step 1: Write tests first (these should define the interface)

def test_batcher_starts_empty():
    """Batcher should start with no transactions."""
    batcher = TransactionBatcher(batch_size=3)
    assert batcher.count() == 0


def test_batcher_can_add_transaction():
    """Adding a transaction increases count."""
    batcher = TransactionBatcher(batch_size=3)
    batcher.add({"id": "TXN-001", "amount": Decimal("100.00")})
    assert batcher.count() == 1


def test_batcher_returns_none_before_full():
    """Add should return None until batch is full."""
    batcher = TransactionBatcher(batch_size=3)
    result = batcher.add({"id": "TXN-001", "amount": Decimal("100.00")})
    assert result is None


def test_batcher_returns_batch_when_full():
    """Add should return batch when size limit reached."""
    batcher = TransactionBatcher(batch_size=3)
    batcher.add({"id": "TXN-001", "amount": Decimal("100.00")})
    batcher.add({"id": "TXN-002", "amount": Decimal("200.00")})
    result = batcher.add({"id": "TXN-003", "amount": Decimal("300.00")})
    
    assert result is not None
    assert len(result) == 3


def test_batcher_clears_after_returning_batch():
    """After returning batch, batcher should be empty."""
    batcher = TransactionBatcher(batch_size=3)
    batcher.add({"id": "TXN-001", "amount": Decimal("100.00")})
    batcher.add({"id": "TXN-002", "amount": Decimal("200.00")})
    batcher.add({"id": "TXN-003", "amount": Decimal("300.00")})  # Returns batch
    
    assert batcher.count() == 0


def test_batcher_flush_returns_partial_batch():
    """Flush should return current transactions even if not full."""
    batcher = TransactionBatcher(batch_size=3)
    batcher.add({"id": "TXN-001", "amount": Decimal("100.00")})
    batcher.add({"id": "TXN-002", "amount": Decimal("200.00")})
    
    result = batcher.flush()
    
    assert result is not None
    assert len(result) == 2
    assert batcher.count() == 0


# TDD Step 2: Now implement the class to make tests pass
class TransactionBatcher:
    """Batches transactions until batch_size is reached."""
    
    def __init__(self, batch_size: int):
        pass  # Replace with your implementation
    
    def add(self, transaction: dict) -> list[dict] | None:
        """Add transaction. Returns batch if full, else None."""
        pass  # Replace with your implementation
    
    def count(self) -> int:
        """Return current transaction count."""
        pass  # Replace with your implementation
    
    def flush(self) -> list[dict]:
        """Return and clear all current transactions."""
        pass  # Replace with your implementation


# Run TDD tests
print("=== TDD Tests ===")

tdd_tests = [
    test_batcher_starts_empty,
    test_batcher_can_add_transaction,
    test_batcher_returns_none_before_full,
    test_batcher_returns_batch_when_full,
    test_batcher_clears_after_returning_batch,
    test_batcher_flush_returns_partial_batch,
]

passed = 0
for test_fn in tdd_tests:
    try:
        test_fn()
        print(f"✓ {test_fn.__name__}")
        passed += 1
    except Exception as e:
        print(f"✗ {test_fn.__name__}: {e}")

print(f"\nResults: {passed}/{len(tdd_tests)} passed")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# All tests should pass
test_batcher_starts_empty()
test_batcher_can_add_transaction()
test_batcher_returns_none_before_full()
test_batcher_returns_batch_when_full()
test_batcher_clears_after_returning_batch()
test_batcher_flush_returns_partial_batch()

# Additional verification
batcher = TransactionBatcher(batch_size=2)
batcher.add({"id": "T1", "amount": Decimal("100")})
batch = batcher.add({"id": "T2", "amount": Decimal("200")})
assert batch is not None and len(batch) == 2, "Should return batch of 2"
assert batcher.count() == 0, "Should be empty after batch"
