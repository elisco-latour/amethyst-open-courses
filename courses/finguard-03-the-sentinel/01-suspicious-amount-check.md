---
id: "finguard_03_01"
title: "Suspicious Amount Check"
type: "coding"
xp: 100
---

# Suspicious Amount Check

FinGuard's core mission is **fraud detection**. At its heart, fraud detection is about **asking questions** about data.

## The Bridge

In **The Vault**, you learned to store transaction data. Now you'll learn to **evaluate** that data against rules. Is this transaction suspicious? Should we flag it?

## The Analogy: The Security Checkpoint

At an airport, security asks questions:
- "Is this bag overweight?" → If yes, extra fee
- "Is this item prohibited?" → If yes, confiscate

FinGuard asks similar questions:
- "Is this amount over $10,000?" → If yes, flag for review
- "Is this an international transfer?" → If yes, apply extra checks

## The `if` Statement

```python
amount: float = 15000.00

if amount > 10000:
    print("Large transaction — flagging for review")
```

The code inside the `if` block **only runs if the condition is true**.

## The `else` Clause

```python
amount: float = 5000.00

if amount > 10000:
    print("⚠️ Large transaction — flagging for review")
else:
    print("✓ Normal transaction — approved")
```

`else` handles the case when the condition is **false**.

## The "Pro" Tip

> **Always indent with 4 spaces. Never mix tabs and spaces.**

```python
# ✅ Correct — 4 spaces
if amount > 10000:
    print("Flagged")

# ❌ Wrong — inconsistent indentation causes errors
if amount > 10000:
  print("Flagged")  # 2 spaces — will crash!
```

## Task

Build the first fraud detection rule:
- If `transaction_amount` is greater than 10,000, set `is_flagged` to `True` and `flag_reason` to `"Large transaction"`
- Otherwise, set `is_flagged` to `False` and `flag_reason` to `"None"`

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Incoming transaction
transaction_amount: Decimal = Decimal("15000.00")

# Apply fraud detection rule



# Print the result
print(f"Amount: ${transaction_amount}")
print(f"Flagged: {is_flagged}")
print(f"Reason: {flag_reason}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert transaction_amount == Decimal("15000.00"), "transaction_amount should be Decimal('15000.00')"
assert is_flagged == True, "is_flagged should be True for amounts > 10000"
assert flag_reason == "Large transaction", "flag_reason should be 'Large transaction'"
