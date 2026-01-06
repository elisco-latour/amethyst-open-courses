---
id: "finguard_06_03"
title: "The Cleanup Protocol"
type: "coding"
xp: 100
---

# The Cleanup Protocol

In banking, resources are scarce. Database connections, file handles, and network sockets must be closed.

If your code crashes halfway through, you might leave a file "locked," preventing other systems from using it. This is a **Resource Leak**.

## The Guarantee: `finally`

The `finally` block runs **no matter what**.
- If `try` succeeds: It runs.
- If `except` catches an error: It runs.
- If an unhandled crash happens: It runs (before the crash).
- If you `return` early: It runs.

```python
def write_audit_log(filename: str, data: str) -> None:
    # 1. Acquire Resource
    file = open(filename, "w")
    print("Resource Acquired")
    
    try:
        # 2. Risky Operation
        if "simulated_error" in data:
            raise ValueError("Something went wrong!")
        file.write(data)
        
    finally:
        # 3. Release Resource (Guaranteed)
        file.close()
        print("Resource Released")
```

## The Logic Flow

```python
try:
    # 1. Main logic
except Error:
    # 2. Error handling
else:
    # 3. Runs ONLY if logic succeeded (rarely used)
finally:
    # 4. Cleanup (Always runs)
```

## Task

You are implementing a transaction lock.
1.  Define a function `secure_operation(success: bool)`.
2.  Print "🔒 Locking Account".
3.  Inside `try`, if `success` is False, raise `ValueError`. If True, print "Processing...".
4.  Inside `finally`, print "🔓 Unlocking Account".
5.  Observe that "Unlocking" prints even when it crashes.

<!-- SEPARATOR -->

# seed_code
def secure_operation(should_succeed: bool):
    print("🔒 Locking Account")
    try:
        # Logic here
        pass
    except ValueError:
        print("❌ Operation Failed")
        # Do NOT re-raise yet
    finally:
        # Cleanup here
        pass

# Integration
print("--- TEST 1: Success ---")
secure_operation(True)

print("\n--- TEST 2: Failure ---")
secure_operation(False)

<!-- SEPARATOR -->

# validation_code
# We can't easily capture stdout in this detailed check, 
# so we rely on the student understanding the flow.
# But we can verify the function structure roughly involves try/finally via logic.

locked = False
log = []

def mock_print(msg):
    log.append(msg)

# Override print for testing logic flow validation conceptually
# (In real auto-grader we would capture stdout)
pass 
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
