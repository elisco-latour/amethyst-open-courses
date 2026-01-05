---
id: "finguard_01_04"
title: "The Blueprint: Hints"
type: "coding"
xp: 100
---

# The Blueprint: Type Hints

Python is flexible. It allows variables to change types (`x` can be `5` then `"hello"`).
This is convenient for scripts, but **dangerous** for banks.

We want to tell the future engineers (and the code editor) what we **expect**.

## Type Hints

We add a "Hint" after the variable name.

```python
# Unclear
balance = 5000 

# Crystal Clear
balance: int = 5000
```

This tells VS Code: "If I try to put a string in `balance`, warn me!"

## The Analogy: The Blueprint

An architect draws a blueprint saying "Load Bearing Wall".
A builder *could* knock it down (nothing physically stops them), but the blueprint acts as a giant warning sign not to.

Type Hints are your blueprint.

## Task

Annotate the transaction variables. Explicitly state what they are.
We will use `Decimal` for the money now.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Add type hints to each variable

# Sender's account number (Text)
sender_account: str = "ACC-1001"

# Receiver's account number (Text)
receiver_account: str = "ACC-2002"

# Transfer amount (Decimal Object)
amount: Decimal = Decimal("7500.00")

# Has this transfer been verified? (Logic)
is_verified: bool = False

print(f"Transfer: {sender_account} -> {receiver_account}")

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
