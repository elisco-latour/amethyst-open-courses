---
id: "finguard_03_03"
title: "Combined Conditions"
type: "coding"
xp: 100
---

# Combined Conditions

Real fraud rules are complex. A transaction might be suspicious only if:
- Amount is over $10,000 **AND** it's international
- Account is new **OR** has previous fraud flags

## Logical Operators

| Operator | Meaning | True when... |
|----------|---------|--------------|
| `and` | Both must be true | `True and True` → `True` |
| `or` | At least one must be true | `True or False` → `True` |
| `not` | Inverts the boolean | `not False` → `True` |

## Examples

```python
amount: float = 15000.00
is_international: bool = True

# AND — both conditions must be true
if amount > 10000 and is_international:
    print("High-value international transfer — extra review")

# OR — at least one must be true
is_new_account: bool = True
has_fraud_history: bool = False

if is_new_account or has_fraud_history:
    print("Elevated scrutiny required")

# NOT — inverting a condition
is_verified: bool = False

if not is_verified:
    print("Transaction not yet verified")
```

## The Analogy: The Security Protocol

To enter the vault, you need:
- Valid badge **AND** correct PIN **AND** fingerprint match

Any single failure = access denied. All three must pass.

## The "Pro" Tip

> **Use parentheses for complex conditions to make intent clear.**

```python
# ❌ Ambiguous — what runs first?
if amount > 10000 and is_international or is_new_account:
    ...

# ✅ Clear — explicit grouping
if (amount > 10000 and is_international) or is_new_account:
    ...
```

## Task

Build a fraud detection rule that flags a transaction if:
- Amount is over $10,000 **AND** the transfer is international

**OR**

- The account has previous fraud history (regardless of amount)

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Transaction data
amount: Decimal = Decimal("5000.00")
is_international: bool = False
has_fraud_history: bool = True

# Apply the combined fraud detection rule



# Print result
print(f"Amount: ${amount:,.2f}")
print(f"International: {is_international}")
print(f"Fraud History: {has_fraud_history}")
print(f"")
print(f"🚨 FLAGGED: {is_flagged}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert amount == Decimal("5000.00"), "amount should be Decimal('5000.00')"
assert is_international == False, "is_international should be False"
assert has_fraud_history == True, "has_fraud_history should be True"
assert is_flagged == True, "is_flagged should be True (has fraud history)"
