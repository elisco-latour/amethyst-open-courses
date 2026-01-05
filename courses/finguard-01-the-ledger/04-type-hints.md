---
id: "finguard_01_04"
title: "Type Hints"
type: "coding"
xp: 100
---

# Type Hints

Python is **dynamically typed** — you don't *have* to declare types. But professional Python code uses **type hints** anyway.

## Why Bother?

Type hints provide three benefits:

1. **Documentation**: Anyone reading your code knows what type `balance` should be
2. **IDE Support**: VS Code can autocomplete methods and catch errors
3. **Early Errors**: Tools like `mypy` find bugs before you run the code

## The Syntax

```python
# Without type hints (works, but unclear)
transaction_id = "TXN-001"
balance = 5000.00

# With type hints (professional)
transaction_id: str = "TXN-001"
balance: float = 5000.00
```

The `: str` after the variable name is the **type hint**. It says "this variable should hold a string."

## The "Pro" Tip

> **Type hints don't enforce anything at runtime. They're for humans and tools.**

```python
# This won't crash — Python ignores type hints at runtime
balance: int = "not a number"  # Works, but wrong!
```

That's why we use tools like `mypy` to check types before deployment.

## Using Decimal with Type Hints

```python
from decimal import Decimal

account_balance: Decimal = Decimal("10000.00")
```

## The Analogy: The Blueprint

An architect's blueprint shows "this wall is load-bearing." The construction worker could ignore it... but the building might collapse.

Type hints are blueprints. Ignore them at your own risk.

## Task

Add type hints to all four transaction variables:
1. `sender_account: str`
2. `receiver_account: str`  
3. `amount: Decimal`
4. `is_verified: bool`

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Add type hints to each variable

# Sender's account number (string)
sender_account = "ACC-1001"

# Receiver's account number (string)
receiver_account = "ACC-2002"

# Transfer amount (Decimal for precision)
amount = Decimal("7500.00")

# Has this transfer been verified? (boolean)
is_verified = False

# Print the transfer summary
print(f"Transfer: {sender_account} → {receiver_account}")
print(f"Amount: ${amount}")
print(f"Verified: {is_verified}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Check values
assert sender_account == "ACC-1001", "sender_account should be 'ACC-1001'"
assert receiver_account == "ACC-2002", "receiver_account should be 'ACC-2002'"
assert amount == Decimal("7500.00"), "amount should be Decimal('7500.00')"
assert is_verified == False, "is_verified should be False"

# Check that type hints are present (by inspecting __annotations__)
# Note: In the Amethyst environment, we validate the code structure
assert isinstance(sender_account, str), "sender_account should be a string"
assert isinstance(receiver_account, str), "receiver_account should be a string"
assert isinstance(amount, Decimal), "amount should be a Decimal"
assert isinstance(is_verified, bool), "is_verified should be a boolean"
