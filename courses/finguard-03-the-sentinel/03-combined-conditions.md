---
id: "finguard_03_03"
title: "Composite Risk"
type: "coding"
xp: 100
---

# Composite Risk

In the real world, rarely is a single factor enough to stop a transaction.
Is a $15,000 transfer suspicious?
*   If it's to `amazon.com`, probably not.
*   If it's to an unknown account overseas at 3 AM... **yes**.

To model this, we combine conditions using **Logical Operators**.

## The Logic Gates

We use three primary operators to forge complex rules:

1.  **`and` (The Strict Gate):** Both conditions must be true.
    *   *Usage:* "High Value **AND** International".
2.  **`or` (The Permissive Gate):** At least one condition must be true.
    *   *Usage:* "User is an Admin **OR** User is a Manager".
3.  **`not` (The Inverter):** Flipped logic.
    *   *Usage:* "Account is **NOT** verified".

## Engineering Clarity: Grouping

When mixing `and` and `or`, code can become ambiguous.

```python
# ❌ Ambiguous: Does 'and' or 'or' happen first?
if amount > 10000 and is_international or has_fraud_history:

# ✅ Explicit: Parentheses clarify intent
if (amount > 10000 and is_international) or has_fraud_history:
```

This second version says: "Flag it if it's a big international transfer, OR if this person is a known fraudster (regardless of amount)."

## Task

Implement the **Compound Flagging Policy**.

Set `is_flagged` to `True` if:
1.  (Condition A) The `amount` is over 10,000 **AND** `is_international` is True.
    **OR**
2.  (Condition B) The `has_fraud_history` flag is True.

Otherwise set `is_flagged` to `False`.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Transaction data
amount: Decimal = Decimal("5000.00")
is_international: bool = False
has_fraud_history: bool = True

# Initialize
is_flagged: bool = False

# Apply Compound Policy



# Audit Log
print(f"Amount: ${amount:,.2f}")
print(f"International: {is_international}")
print(f"Fraud History: {has_fraud_history}")
print(f"")
print(f"🚨 FLAGGED: {is_flagged}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert is_flagged is True, "Should be flagged because has_fraud_history is True"
assert amount == Decimal("5000.00"), "amount should be Decimal('5000.00')"
assert is_international == False, "is_international should be False"
assert has_fraud_history == True, "has_fraud_history should be True"
assert is_flagged == True, "is_flagged should be True (has fraud history)"
