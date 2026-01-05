---
id: "finguard_07_04"
title: "Writing Audit Reports"
type: "coding"
xp: 100
---

# Writing Audit Reports

FinGuard generates audit reports that compliance teams can read. You'll write structured text files.

## Writing Text Files

```python
# Write entire content at once
with open("report.txt", "w") as file:
    file.write("=== Daily Report ===\n")
    file.write("Date: 2025-01-15\n")
    file.write("Transactions: 150\n")

# Write line by line
with open("report.txt", "w") as file:
    lines = ["Line 1", "Line 2", "Line 3"]
    for line in lines:
        file.write(line + "\n")

# Write all lines at once
with open("report.txt", "w") as file:
    lines = ["Line 1\n", "Line 2\n", "Line 3\n"]
    file.writelines(lines)
```

## Append Mode

```python
# Append to existing file (don't overwrite)
with open("audit.log", "a") as file:
    file.write("2025-01-15 10:30:00 - New entry\n")
```

## Building Reports with f-strings

```python
from decimal import Decimal
from datetime import datetime

def generate_report(transactions: list[dict]) -> str:
    total = sum(t["amount"] for t in transactions)
    
    report = f"""
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
