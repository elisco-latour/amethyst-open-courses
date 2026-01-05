---
id: "finguard_06_03"
title: "The Cleanup Protocol"
type: "coding"
xp: 100
---

# The Cleanup Protocol

Some code must run **regardless** of success or failure — closing files, releasing locks, logging completion.

## The `finally` Block

```python
def process_file(filename: str) -> None:
    file = None
    try:
        file = open(filename, "r")
        data = file.read()
        process(data)
    except FileNotFoundError:
        print("File not found")
    finally:
        # This ALWAYS runs — success or failure
        if file:
            file.close()
        print("Cleanup complete")
```

## When Does `finally` Run?

| Scenario | Does `finally` run? |
|----------|---------------------|
| No exception | ✅ Yes |
| Exception caught | ✅ Yes |
| Exception not caught | ✅ Yes (before crash) |
| `return` in try block | ✅ Yes |

## The `else` Block

`else` runs only if **no exception** was raised:

```python
try:
    result = risky_operation()
except SomeError:
    handle_error()
else:
    # Only runs if no exception
    save_result(result)
finally:
    # Always runs
    cleanup()
```

## The Analogy: The Lab Experiment

Every experiment has three phases:
1. **Try**: Run the experiment
2. **Except**: Handle accidents (spills, fires)
3. **Finally**: Clean up the lab (ALWAYS, whether experiment succeeded or failed)

## The "Pro" Tip

> **Use `finally` for cleanup that must happen no matter what. Don't put cleanup in `except` — it won't run on success.**

```python
# ❌ Wrong: cleanup only on error
try:
    process()
except Exception:
    cleanup()  # Doesn't run on success!

# ✅ Correct: cleanup always
try:
    process()
except Exception:
    handle_error()
finally:
    cleanup()  # Always runs
```

## Task

Simulate a batch transaction processor that:
1. Starts processing (set `processing = True`)
2. May encounter errors (handle them)
3. Always logs completion and resets `processing = False`

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal, InvalidOperation

# Simulated transaction batch
transactions: list[dict] = [
    {"id": "TXN-001", "amount": "500.00"},
    {"id": "TXN-002", "amount": "invalid"},  # Will fail
    {"id": "TXN-003", "amount": "1500.00"},
]

processing: bool = False
processed: list[str] = []
failed: list[str] = []
log_messages: list[str] = []

def process_batch() -> None:
    """Process all transactions with proper cleanup."""
    global processing
    
    pass  # Replace with your implementation


# Run the processor
process_batch()

print("=== Batch Processing Log ===")
for msg in log_messages:
    print(msg)

print(f"\nProcessed: {processed}")
print(f"Failed: {failed}")
print(f"Processing flag: {processing}")

<!-- SEPARATOR -->

# validation_code
assert processing == False, "processing should be False after completion (cleanup)"
assert "TXN-001" in processed, "TXN-001 should be processed"
assert "TXN-003" in processed, "TXN-003 should be processed"
assert "TXN-002" in failed, "TXN-002 should be in failed list"
assert len(log_messages) >= 2, "Should have start and end log messages"
assert any("start" in msg.lower() for msg in log_messages), "Should log start"
assert any("complete" in msg.lower() or "end" in msg.lower() or "finish" in msg.lower() for msg in log_messages), "Should log completion"
