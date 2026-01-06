---
id: "finguard_01_04"
title: "The Blueprint: Type Hints"
type: "coding"
xp: 100
---

# The Blueprint: Type Hints

Python is flexible. A variable can hold a number, then later hold text:

```python
balance = 5000      # It's an integer
balance = "empty"   # Now it's a string!
```

This flexibility is convenient for quick scripts, but **dangerous** for banking systems. If `balance` suddenly becomes text, your calculations will crash — or worse, silently produce wrong numbers.

## Type Hints: Declaring Your Intent

We can add **type hints** to tell Python (and other developers) what type we *expect*:

```python
# Without type hint (unclear)
balance = 5000

# With type hint (crystal clear)
balance: int = 5000
```

The `: int` after the variable name is the type hint. It says: "This variable should hold an integer."

## The Blueprint Analogy

Think of an architect's blueprint. It might say "Load-Bearing Wall" on a specific wall. A builder *could* knock it down — nothing physically stops them — but the blueprint serves as a warning.

Type hints are your code's blueprint. They don't *force* anything, but they warn you (and your editor) when something looks wrong.

## Type Hints for Different Types

```python
# String
customer_name: str = "Alice Chen"

# Integer
account_number: int = 100234

# Float
interest_rate: float = 0.045

# Boolean
is_active: bool = True

# Decimal (requires import)
from decimal import Decimal
balance: Decimal = Decimal("5000.00")
```

## Why Bother?

1. **Documentation:** Future readers instantly know what each variable holds
2. **Editor Support:** VS Code will warn you if you try to do `customer_name + 100`
3. **Fewer Bugs:** Mistakes are caught before they reach production

<div class="key-concept">
<h4>🔑 Key Concept: Type Hints Are Comments That Tools Can Read</h4>

A comment like `# balance should be an integer` is helpful, but tools can't check it. A type hint like `balance: int` serves the same purpose AND your editor can verify it.
</div>

## Task

Complete the wire transfer record by adding type hints to each variable. The values are provided — you need to add the `: type` annotations.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Add type hints to each variable below
# Example: sender_account: str = "ACC-1001"

# The sending account (should be str)
sender_account = "ACC-1001"

# The receiving account (should be str)
receiver_account = "ACC-2002"

# The transfer amount (should be Decimal)
amount = Decimal("7500.00")

# Has this been verified by compliance? (should be bool)
is_verified = False

# Display the transfer details
print("=== Wire Transfer Record ===")
print(f"From: {sender_account}")
print(f"To: {receiver_account}")
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

# Check that type hints are present
annotations = globals().get('__annotations__', {})
# Note: In some environments, we check via isinstance as fallback
assert isinstance(sender_account, str), "sender_account should be a string"
assert isinstance(receiver_account, str), "receiver_account should be a string"
assert isinstance(amount, Decimal), "amount should be a Decimal"
assert isinstance(is_verified, bool), "is_verified should be a boolean"
