---
id: "finguard_01_05"
title: "Data to Information"
type: "coding"
xp: 100
---

# Data to Information

You've learned to store data in variables. But raw data isn't useful — **information** is.

The difference:
- **Data**: `"ACC-1001"`, `"ACC-2002"`, `Decimal("7500.00")`
- **Information**: `"Transfer of $7,500.00 from ACC-1001 to ACC-2002"`

## f-strings: Python's String Formatting

Python's **f-strings** (formatted string literals) turn data into readable information.

```python
name: str = "Alice"
balance: Decimal = Decimal("5000.00")

# f-string — note the f before the quote
message: str = f"Hello, {name}. Your balance is ${balance}."
# → "Hello, Alice. Your balance is $5000.00."
```

The `f` prefix enables **interpolation** — putting variables inside `{}` brackets.

## Formatting Numbers

f-strings support formatting codes:

```python
amount: float = 1234567.891

f"{amount:,.2f}"     # → "1,234,567.89" (commas, 2 decimal places)
f"{amount:.0f}"      # → "1234568" (no decimals, rounded)
f"{amount:>15,.2f}"  # → "   1,234,567.89" (right-aligned, 15 chars wide)
```

## The Analogy: The Bank Statement

A bank statement doesn't show raw database records. It shows formatted, human-readable information:

```
Date       | Description          | Amount
-----------+----------------------+-----------
2025-01-06 | Wire Transfer        | -$7,500.00
2025-01-05 | Deposit              | +$2,000.00
```

f-strings help you build statements like this.

## Task

Create a formatted transaction summary using f-strings.

Given the transaction data, produce this exact output:
```
=== FINGUARD TRANSACTION RECEIPT ===
Transaction ID: TXN-2025-00042
From: ACC-1001 → To: ACC-2002
Amount: $12,500.00
Status: PENDING
=====================================
```

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Transaction data
transaction_id: str = "TXN-2025-00042"
sender: str = "ACC-1001"
receiver: str = "ACC-2002"
amount: Decimal = Decimal("12500.00")
status: str = "PENDING"

# Build the receipt using f-strings
receipt: str = f"""=== FINGUARD TRANSACTION RECEIPT ===
Transaction ID: {transaction_id}
From: {sender} → To: {receiver}
Amount: ${amount:,.2f}
Status: {status}
====================================="""

print(receipt)

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

assert transaction_id == "TXN-2025-00042", "transaction_id should be 'TXN-2025-00042'"
assert sender == "ACC-1001", "sender should be 'ACC-1001'"
assert receiver == "ACC-2002", "receiver should be 'ACC-2002'"
assert amount == Decimal("12500.00"), "amount should be Decimal('12500.00')"
assert status == "PENDING", "status should be 'PENDING'"

# Verify the receipt contains key elements
assert "TXN-2025-00042" in receipt, "receipt should contain the transaction ID"
assert "ACC-1001" in receipt, "receipt should contain the sender"
assert "ACC-2002" in receipt, "receipt should contain the receiver"
assert "12,500.00" in receipt, "receipt should contain formatted amount with commas"
assert "PENDING" in receipt, "receipt should contain the status"
