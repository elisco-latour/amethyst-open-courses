---
id: "finguard_05_03"
title: "Default Configurations"
type: "coding"
xp: 100
---

# Default Configurations

In complex systems, many parameters are often "standard." 
If you send a payment, it's *usually* in **USD**.
It's *usually* a **Standard** transfer (not Expedited).

We don't want to force the developer to type these "defaults" every single time.

## Default Arguments

We can provide pre-filled values in the function definition.

```python
def create_account(owner: str, currency: str = "USD", type: str = "SAVINGS"):
    print(f"Opening {type} account for {owner} in {currency}")
```

Now the API is flexible:

```python
# Minimalist Call (uses defaults)
create_account("Alice") 
# -> Opening SAVINGS account for Alice in USD

# Override Call
create_account("Bob", type="CHECKING")
# -> Opening CHECKING account for Bob in USD
```

## Engineering Standard: Ordering

> **Required First, Optional Last.**
> You cannot put a parameter with a default value *before* one without it. Python will raise a SyntaxError.

## Task

Build a `format_currency` utility.

1.  **Required:** `amount` (Decimal).
2.  **Optional:** `currency` (str) defaulting to `"USD"`.
3.  **Optional:** `marketing_mode` (bool) defaulting to `False`.

Logic:
*   If `marketing_mode` is True, remove the cents (e.g., "$50" instead of "$50.00") and return int-style string.
*   Otherwise, return standard 2-decimal precision (e.g., "$50.00").
*   Prepend the currency symbol (assume a simple map for USD/EUR/GBP).

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

def format_currency(
    amount: Decimal,
    currency: str = "USD",
    marketing_mode: bool = False
) -> str:
    """Formats decimal amounts into currency strings."""
    symbol_map = {"USD": "$", "EUR": "€", "GBP": "£"}
    symbol = symbol_map.get(currency, "$")
    
    # Implementation
    pass

# Test
print(format_currency(Decimal("1200.50"))) # Default
print(format_currency(Decimal("1200.50"), marketing_mode=True)) # Marketing

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert format_currency(Decimal("10.50"), "USD", False) == "$10.50", "Standard format with cents"
assert format_currency(Decimal("10.50"), "USD", True) == "$10", "Marketing mode removes cents"
assert format_currency(Decimal("20.00"), "EUR") == "€20.00", "EUR uses Euro symbol"
assert format_currency(Decimal("99.99"), "GBP", True) == "£99", "GBP marketing mode"
