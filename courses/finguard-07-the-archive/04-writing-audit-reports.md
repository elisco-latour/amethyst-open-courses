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

if os.path.exists("audit_trail.txt"):
    os.remove("audit_trail.txt")

log_transaction("A", "1")
log_transaction("B", "2")

with open("audit_trail.txt") as f:
    content = f.read()

assert "A - 1\n" in content
assert "B - 2\n" in content
assert content.count("\n") >= 2

# Cleanup
if os.path.exists("audit_trail.txt"):
    os.remove("audit_trail.txt")

print("Validation passed!")
=== FinGuard Daily Report ===
Generated: {datetime.now().isoformat()}
Total Transactions: {len(transactions)}
Total Amount: ${total:,.2f}
=============================
"""
    return report
```

## The "Pro" Tip

> **Use multiline f-strings (`"""`) for report templates. They're readable and maintainable.**

```python
# ❌ Hard to read
report = "=== Report ===\n" + "Date: " + date + "\n" + "Total: " + str(total)

# ✅ Clear template
report = f"""
=== Report ===
Date: {date}
Total: {total}
"""
```

## Task

Generate a daily transaction report and store it in `report_content`.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Daily transaction data
transactions: list[dict] = [
    {"id": "TXN-001", "amount": Decimal("500.00"), "type": "DEPOSIT"},
    {"id": "TXN-002", "amount": Decimal("200.00"), "type": "WITHDRAWAL"},
    {"id": "TXN-003", "amount": Decimal("1500.00"), "type": "TRANSFER"},
    {"id": "TXN-004", "amount": Decimal("3000.00"), "type": "DEPOSIT"},
]

report_date: str = "2025-01-15"
report_content: str = ""

# Generate the report content
# Include:
# - Header with date
# - Total transaction count
# - Total amount
# - Breakdown by type (count per type)
# - List of all transactions



# In real code, you'd write to a file:
# with open(f"report_{report_date}.txt", "w") as file:
#     file.write(report_content)

print(report_content)

<!-- SEPARATOR -->

# validation_code
assert "2025-01-15" in report_content, "Report should include the date"
assert "4" in report_content, "Report should include transaction count (4)"
assert "5200" in report_content or "5,200" in report_content, "Report should include total amount"
assert "DEPOSIT" in report_content, "Report should mention DEPOSIT"
assert "WITHDRAWAL" in report_content, "Report should mention WITHDRAWAL"
assert "TXN-001" in report_content or "TXN-" in report_content, "Report should list transactions"
