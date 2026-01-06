---
id: "finguard_07_04"
title: "The Audit Trail"
type: "coding"
xp: 100
---

# The Audit Trail

In finance, you never delete. You only append.
If a mistake is made, you add a correction transaction. The history must be immutable.

Your system logs are the **Audit Trail**.

## The Append Mode (`"a"`)

Opening a file with `"w"` destroys it. Opening with `"a"` adds to the end.

```python
# Danger: "w" wipes previous history
with open("audit.log", "w") as f:
    f.write("New Entry")  # Goodbye old logs!

# Safe: "a" preserves history
def log_event(message: str):
    with open("audit.log", "a") as f:
        # Always include newline \n
        f.write(f"{message}\n")
```

## Formatting Reports

Reports are for humans. They need structure.

```python
def generate_receipt(txn_id: str, amount: str) -> str:
    # Multi-line strings (f-strings)
    return f"""
    --------------------------
    RECEIPT: {txn_id}
    AMOUNT : ${amount}
    STATUS : CONFIRMED
    --------------------------
    """
```

## Task

Create a `log_transaction` function.
1. Accepts `txn_id` and `status`.
2. Appends a line to `audit_trail.txt`.
3. Format: `[ID] - STATUS`.
4. Ensure it has a newline at the end.

<!-- SEPARATOR -->

# seed_code
def log_transaction(txn_id: str, status: str):
    filename = "audit_trail.txt"
    # Append to file
    pass

# Integration
log_transaction("TX1", "INIT")
log_transaction("TX1", "DONE")

with open("audit_trail.txt") as f:
    print(f.read())

<!-- SEPARATOR -->

# validation_code
import os

# Cleanup before test
if os.path.exists("audit_trail.txt"):
    os.remove("audit_trail.txt")

log_transaction("A", "1")
log_transaction("B", "2")

with open("audit_trail.txt") as f:
    content = f.read()

assert "A - 1\n" in content, "First log entry should be formatted correctly"
assert "B - 2\n" in content, "Second log entry should be formatted correctly"
assert content.count("\n") >= 2, "Each entry should have a newline"

# Test append behavior (not overwrite)
log_transaction("C", "3")
with open("audit_trail.txt") as f:
    content = f.read()
assert "A - 1" in content, "Append should preserve old entries"
assert "C - 3" in content, "New entry should be added"

# Cleanup
os.remove("audit_trail.txt")
