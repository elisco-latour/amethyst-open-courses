---
id: "finguard_03_01"
title: "The Gatekeeper: If/Else"
type: "coding"
xp: 100
---

# The Gatekeeper

In **The Vault**, you built secure storage. But a vault with an open door is useless. You need a **sentinel** — a guard that decides what gets through and what gets blocked.

In programming, this decision-making is called **control flow**.

## The Business Rule

At FinGuard, we don't just "run code." We enforce **policies**.

> **Policy #101:** "Any transaction exceeding $10,000 must be flagged for manual review."

This isn't optional — it's a regulatory requirement. Software must enforce it automatically.

## The `if` Statement

The `if` statement checks a condition. If it's `True`, the indented code runs. If it's `False`, Python skips it:

```python
amount = 15000

if amount > 10000:
    print("⚠️ ALERT: High Value Transaction")
    # This only prints if amount > 10000
```

## The `else` Clause

What if the transaction is normal? We need a default path:

```python
if amount > 10000:
    status = "FLAGGED"
else:
    status = "APPROVED"
```

This is a binary decision: one path or the other, never both.

## Comparison Operators

Here are the operators you can use in conditions:

| Operator | Meaning | Example |
|----------|---------|---------|
| `>` | Greater than | `amount > 10000` |
| `<` | Less than | `amount < 100` |
| `>=` | Greater than or equal | `age >= 18` |
| `<=` | Less than or equal | `balance <= 0` |
| `==` | Equal to | `status == "ACTIVE"` |
| `!=` | Not equal to | `country != "US"` |

## Indentation Matters

Python uses **indentation** (spaces at the start of a line) to know what code belongs to the `if`:

```python
# ✓ Correct: 4 spaces of indentation
if amount > 10000:
    status = "FLAGGED"
    print("Alert sent")

# ✗ Wrong: no indentation
if amount > 10000:
status = "FLAGGED"  # Error! Python doesn't know this belongs to the if
```

<div class="key-concept">
<h4>🔑 Key Concept: The 4-Space Rule</h4>

In Python, use **4 spaces** for each level of indentation. This is the universal standard. Most editors do this automatically when you press Tab.
</div>

## Task

Implement **Policy #101** for FinGuard:

1. Check if `transaction_amount` is **greater than** `policy_limit`
2. If yes: set `is_flagged` to `True` and `flag_reason` to `"Policy #101: High Value"`
3. If no: set `is_flagged` to `False` and `flag_reason` to `"None"`

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Incoming transaction
transaction_amount: Decimal = Decimal("15000.00")
policy_limit: Decimal = Decimal("10000.00")

# Initialize variables before the if statement
# (This ensures they always have a value, even if our logic has a bug)
is_flagged: bool = False
flag_reason: str = "Unknown"

# Apply Policy #101 here
# Use if/else to check transaction_amount against policy_limit



# Audit Log
print(f"Amount: ${transaction_amount}")
print(f"Flagged: {is_flagged}")
print(f"Reason: {flag_reason}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert transaction_amount == Decimal("15000.00"), "Don't modify transaction_amount"
assert is_flagged == True, "is_flagged should be True (15000 > 10000)"
assert flag_reason == "Policy #101: High Value", "flag_reason must be exactly 'Policy #101: High Value'"
