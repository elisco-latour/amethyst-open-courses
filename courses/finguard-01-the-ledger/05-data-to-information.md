---
id: "finguard_01_05"
title: "The Receipt: Formatting"
type: "coding"
xp: 100
---

# The Receipt: Formatting

A database stores raw data (`12500`).
Customers want **Information** (`$12,500.00`).

As a Data Engineer, you control how data is presented.

## f-strings

The `f-string` is your tool for crafting reports. It allows you to inject variables into text.

```python
amount = 500
print(f"I have {amount} dollars.") 
```

## Number Formatting Code

FinGuard has strict rules for money display.
*   Must show 2 decimal places.
*   Must use commas for thousands.

We use the format specifier `{:,.2f}` inside the f-string.

*   `,`: Add commas.
*   `.2f`: Show 2 decimal places fixed.

```python
amount = 12500
print(f"${amount:,.2f}") # -> $12,500.00
```

## Task

Generate the Customer Receipt. Match the format exactly.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Transaction data
transaction_id: str = "TXN-2025-00042"
sender: str = "ACC-1001"
recipient: str = "ACC-2002" # Renamed from receiver for clarity
amount: Decimal = Decimal("12500.00")
status: str = "PENDING"

# Build the receipt using f-strings
receipt: str = f"""=== FINGUARD TRANSACTION RECEIPT ===
Transaction ID: {transaction_id}
From: {sender} -> To: {recipient}
Amount: ${amount:,.2f}
Status: {status}
====================================="""

print(receipt)

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

assert transaction_id == "TXN-2025-00042", "transaction_id should be 'TXN-2025-00042'"
assert "ACC-1001" in receipt, "Receipt must contain sender"
assert "12,500.00" in receipt, "Receipt must format amount with commas"
assert status == "PENDING", "status should be 'PENDING'"

# Verify the receipt contains key elements
assert "TXN-2025-00042" in receipt, "receipt should contain the transaction ID"
assert "ACC-1001" in receipt, "receipt should contain the sender"
assert "ACC-2002" in receipt, "receipt should contain the receiver"
assert "12,500.00" in receipt, "receipt should contain formatted amount with commas"
assert "PENDING" in receipt, "receipt should contain the status"
