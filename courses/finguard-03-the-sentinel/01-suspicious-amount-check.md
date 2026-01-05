---
id: "finguard_03_01"
title: "The Gatekeeper"
type: "coding"
xp: 100
---

# The Gatekeeper

In **The Vault**, you built secure storage. But a vault with an open door is useless. You need a **Sentinel**—a guard that decides who gets in and who stays out.

In programming, this is **Control Flow**. It acts as the brain of your application, making decisions based on data.

## The Business Rule

At FinGuard, we don't just "run code." We enforce **policies**.
One of our strictest policies involves **High Value Transactions**.

> **Policy #101:** "Any transaction exceeding $10,000 must be flagged for manual review."

Software enforces this policy using the `if` statement.

## The `if` Statement

```python
from decimal import Decimal

amount: Decimal = Decimal("15000.00")
policy_limit: Decimal = Decimal("10000.00")

if amount > policy_limit:
    # This block usually sends an alert to a compliance officer
    print("⚠️ ALERT: High Value Transaction Detected")
```

The code specific to the alert **only runs** if the condition (`amount > policy_limit`) is `True`. If it's `False`, the program skips it entirely, like a guard waving a trusted employee through.

## The `else` Clause

What happens if the transaction is normal? We often need a default path.

```python
if amount > policy_limit:
    status = "FLAGGED"
else:
    status = "APPROVED"
```

This is a binary decision: **A or B**. There is no middle ground.

## The Engineering Standard: Indentation

Python is unique because **whitespace matters**. It's not just for style; it's syntax.

> **The Rule:** We use **4 spaces** for indentation. Not 2. Not tabs.

```python
# ✅ Engineering Standard
if amount > 10000:
    print("Flagged")  # This belongs to the policy

# ❌ Syntax Error / Logic Error
if amount > 10000:
print("Flagged")      # Python won't know this belongs to the `if`!
```

## Task

Implement **Policy #101** in the system.

1.  Check if `transaction_amount` is strictly greater than `10,000.00`.
2.  If it is, set `is_flagged` to `True` and `flag_reason` to `"Policy #101: High Value"`.
3.  Otherwise, set `is_flagged` to `False` and `flag_reason` to `"None"`.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Incoming transaction
transaction_amount: Decimal = Decimal("15000.00")
policy_limit: Decimal = Decimal("10000.00")

# Initialize variables (Good practice!)
is_flagged: bool = False
flag_reason: str = "Unknown"

# Apply Policy #101



# Audit Log
print(f"Amount: ${transaction_amount}")
print(f"Flagged: {is_flagged}")
print(f"Reason: {flag_reason}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert transaction_amount == Decimal("15000.00"), "transaction_amount should be Decimal('15000.00')"
assert is_flagged is True, "is_flagged must be a boolean True"
assert flag_reason == "Policy #101: High Value", "flag_reason text must match exact policy text"
