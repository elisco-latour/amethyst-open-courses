---
id: "finguard_05_03"
title: "The Currency Converter"
type: "coding"
xp: 100
---

# The Currency Converter

FinGuard handles international transactions. Functions can have **default values** for common cases.

## Default Parameter Values

```python
from decimal import Decimal

def convert_currency(
    amount: Decimal,
    from_currency: str = "USD",
    to_currency: str = "EUR"
) -> Decimal:
    """Convert amount between currencies."""
    # Simplified rates
    rates: dict[str, Decimal] = {
        "USD_EUR": Decimal("0.92"),
        "USD_GBP": Decimal("0.79"),
        "EUR_USD": Decimal("1.09"),
    }
    
    rate_key = f"{from_currency}_{to_currency}"
    rate = rates.get(rate_key, Decimal("1.00"))
    
    return amount * rate
```

## Using Default Values

```python
# Use all defaults (USD → EUR)
result = convert_currency(Decimal("100.00"))

# Override only what you need
result = convert_currency(Decimal("100.00"), to_currency="GBP")

# Override everything
result = convert_currency(
    Decimal("100.00"),
    from_currency="EUR",
    to_currency="USD"
)
```

## The Analogy: The Coffee Order

At a coffee shop:
- "Coffee" → You get the default (medium, black)
- "Large coffee" → Override size, keep default flavor
- "Large vanilla latte" → Override everything

## The "Pro" Tip

> **Put required parameters first, optional parameters with defaults last.**

```python
# ✅ Correct: required first, optional last
def transfer(amount: Decimal, recipient: str, memo: str = "") -> bool:
    ...

# ❌ Wrong: default before required — SyntaxError!
def transfer(memo: str = "", amount: Decimal, recipient: str) -> bool:
    ...
```

## Task

Write a `format_transaction_amount` function that:
- Takes `amount: Decimal` (required)
- Takes `currency: str` with default `"USD"`
- Takes `show_symbol: bool` with default `True`
- Returns formatted string like `"$1,234.56"` or `"1,234.56 USD"`

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

def format_transaction_amount(
    amount: Decimal,
    currency: str = "USD",
    show_symbol: bool = True
) -> str:
    """Format transaction amount with currency."""
    symbols: dict[str, str] = {
        "USD": "$",
        "EUR": "€",
        "GBP": "£",
    }
    
    pass  # Replace with your implementation


# Test the function
print("=== Transaction Amount Formatter ===")

# Default: USD with symbol
print(format_transaction_amount(Decimal("1234.56")))

# EUR with symbol
print(format_transaction_amount(Decimal("1234.56"), currency="EUR"))

# USD without symbol
print(format_transaction_amount(Decimal("1234.56"), show_symbol=False))

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert format_transaction_amount(Decimal("1234.56")) == "$1,234.56", "Default USD with symbol"
assert format_transaction_amount(Decimal("1234.56"), currency="EUR") == "€1,234.56", "EUR with symbol"
assert format_transaction_amount(Decimal("1234.56"), show_symbol=False) == "1,234.56 USD", "USD without symbol"
assert format_transaction_amount(Decimal("1234.56"), currency="GBP", show_symbol=False) == "1,234.56 GBP", "GBP without symbol"
