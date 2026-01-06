---
id: "finguard_01_05"
title: "From Data to Information: Formatting"
type: "coding"
xp: 100
---

# From Data to Information

A database stores raw data: `12500`

A customer wants **information**: `$12,500.00`

The difference? **Formatting**. As a data engineer, you control how raw data becomes readable information.

## f-strings: Your Formatting Tool

An **f-string** (formatted string) lets you insert variables directly into text. Just put `f` before the quotes and wrap variables in `{curly braces}`:

```python
amount = 500
print(f"Your balance is {amount} dollars.")
# Output: Your balance is 500 dollars.
```

## Formatting Numbers for Finance

FinGuard has strict rules for displaying money:
- Always show **2 decimal places** (even for round numbers)
- Use **commas** for thousands

We use a **format specifier** inside the braces: `{value:,.2f}`

Let's break that down:
- `,` — Add commas as thousand separators
- `.2f` — Show exactly 2 decimal places (f = fixed-point)

```python
amount = 12500
print(f"${amount:,.2f}")
# Output: $12,500.00

amount = 50
print(f"${amount:,.2f}")  
# Output: $50.00
```

## Multi-line Strings

For longer output like receipts, use triple quotes `"""`:

```python
name = "Alice"
message = f"""Hello {name},

Your order has been confirmed.
Thank you for shopping with us."""
```

This preserves line breaks exactly as you write them.

## Task

Generate a transaction receipt. You're given the transaction data — build the formatted receipt string.

Requirements for the receipt:
- Include the transaction ID
- Show sender and recipient accounts
- Format the amount with commas and 2 decimal places (use `${amount:,.2f}`)
- Include the status

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Transaction data (provided)
transaction_id: str = "TXN-2025-00042"
sender: str = "ACC-1001"
recipient: str = "ACC-2002"
amount: Decimal = Decimal("12500.00")
status: str = "PENDING"

# Build the receipt using an f-string
# Use triple quotes for multi-line output
# Format the amount with :,.2f
receipt: str = f"""=== FINGUARD RECEIPT ===
Transaction: 
From:  -> To: 
Amount: 
Status: 
========================"""

print(receipt)

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Verify the receipt contains all required information
assert "TXN-2025-00042" in receipt, "Receipt must contain the transaction ID"
assert "ACC-1001" in receipt, "Receipt must contain the sender account"
assert "ACC-2002" in receipt, "Receipt must contain the recipient account"
assert "12,500.00" in receipt, "Receipt must format amount with commas and 2 decimal places"
assert "PENDING" in receipt, "Receipt must contain the status"
