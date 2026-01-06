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

Simulate a **Batch Transaction Processor** with proper cleanup.
1. Set `processing = True` and log "Batch started".
2. Loop through each transaction:
   - Try to convert the `amount` string to `Decimal`.
   - If successful, add the `id` to `processed`.
   - If `InvalidOperation` occurs, add the `id` to `failed`.
3. In `finally`, set `processing = False` and log "Batch complete".

The `processing` flag must be `False` after completion — even if errors occurred.

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

# Batch Processor Implementation


# Report
print("=== Batch Processing Log ===")
for msg in log_messages:
    print(msg)

print(f"\nProcessed: {processed}")
print(f"Failed: {failed}")
print(f"Processing flag: {processing}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Cleanup must happen
assert processing == False, "processing should be False after completion (cleanup)"

# Successful transactions
assert "TXN-001" in processed, "TXN-001 should be processed"
assert "TXN-003" in processed, "TXN-003 should be processed"

# Failed transaction
assert "TXN-002" in failed, "TXN-002 should be in failed list"

# Logging
assert len(log_messages) >= 2, "Should have start and end log messages"
assert any("start" in msg.lower() for msg in log_messages), "Should log start"
assert any("complete" in msg.lower() or "end" in msg.lower() or "finish" in msg.lower() for msg in log_messages), "Should log completion"
