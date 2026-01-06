---
id: "finguard_03_03"
title: "Composite Risk: Logical Operators"
type: "coding"
xp: 100
---

# Composite Risk

In the real world, a single factor is rarely enough to flag a transaction.

Is a $15,000 transfer suspicious?
- If it's going to Amazon.com... probably not.
- If it's going to an unknown overseas account at 3 AM from a new device... **yes**.

Real fraud detection combines multiple signals. We do this with **logical operators**.

## The Three Operators

| Operator | Meaning | True When... |
|----------|---------|--------------|
| `and` | Both must be true | `A and B` → both A and B are True |
| `or` | At least one true | `A or B` → either A or B (or both) are True |
| `not` | Flip the value | `not A` → A is False |

## Examples in Banking

```python
# AND: Both conditions must be true
if amount > 10000 and is_international:
    flag = "High-value international transfer"

# OR: Either condition is enough
if is_fraud_reported or is_account_frozen:
    block_transaction()

# NOT: Checking for the opposite
if not is_verified:
    require_verification()
```

## Grouping with Parentheses

When mixing `and` and `or`, things get confusing:

```python
# ❌ Ambiguous: What happens first?
if amount > 10000 and is_international or has_fraud_history:
    flag = True

# ✓ Clear: Parentheses show exactly what we mean
if (amount > 10000 and is_international) or has_fraud_history:
    flag = True
```

The second version says: "Flag if it's a big international transfer, OR if the person has fraud history (regardless of amount)."

<div class="key-concept">
<h4>🔑 Key Concept: Always Use Parentheses</h4>

When combining `and` with `or`, always use parentheses to make your intent explicit. Future you (and your teammates) will thank you.
</div>

## Task

Implement the **Compound Flagging Policy**:

A transaction should be flagged (`is_flagged = True`) if:
- **Condition A:** The `amount` is over 10,000 **AND** `is_international` is True
- **OR**
- **Condition B:** `has_fraud_history` is True

Otherwise, `is_flagged` should be `False`.

Write a single `if/else` statement using `and`, `or`, and parentheses.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Transaction data
amount: Decimal = Decimal("5000.00")
is_international: bool = False
has_fraud_history: bool = True

# Initialize
is_flagged: bool = False

# Apply the compound policy using logical operators
# Hint: (condition_a) or (condition_b)



# Audit Log
print(f"Amount: ${amount:,.2f}")
print(f"International: {is_international}")
print(f"Fraud History: {has_fraud_history}")
print(f"")
print(f"🚨 FLAGGED: {is_flagged}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert amount == Decimal("5000.00"), "Don't modify amount"
assert is_international == False, "Don't modify is_international"
assert has_fraud_history == True, "Don't modify has_fraud_history"
assert is_flagged == True, "Should be flagged because has_fraud_history is True (even though amount is under 10k)"
