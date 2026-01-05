---
id: "finguard_03_02"
title: "The Hierarchy of Rules"
type: "coding"
xp: 100
---

# The Hierarchy of Rules

In banking, not all alerts are equal. A $1,001 transaction is a "minor blip." A $1,000,000 transaction is a "Code Red."

Simple `if/else` isn't enough. We need a **ladder of escalation**:
1.  **Critical?** Panic immediately.
2.  **High?** Alert a manager.
3.  **Standard?** Log it properly.
4.  **Low?** Just process it.

## The `elif` Chain

`elif` stands for "else if". It allows us to check conditions strictly **in order**.

```python
risk_score: int = 85

if risk_score > 90:
    action = "FREEZE_ACCOUNT"  # Stopped here if True
elif risk_score > 50:
    action = "MANUAL_REVIEW"   # Checked only if score <= 90
else:
    action = "AUTO_APPROVE"    # Checked only if score <= 50
```

## The Principle of Precedence

Imagine a "Triage Nurse" at a hospital or a "Compliance Officer" at FinGuard. They look for the **worst-case scenario first**.

Logic Order Matters:
*   **Correct:** Check `> 90` first.
*   **Incorrect:** Checking `> 50` first would catch the 95s too, preventing them from ever hitting the "Freeze" logic!

> **Engineering Rule:** Always order your checks from **Most Specific (or Severe)** to **Least Specific (or Safe)**.

## Task

Implement the **Transaction Severity Ladder** for FinGuard.

Classify the `transaction_amount` into a `risk_level` string:

1.  Over **$50,000**: Set risk to `"CRITICAL"`
2.  Over **$10,000**: Set risk to `"HIGH"`
3.  Over **$1,000**: Set risk to `"ELEVATED"`
4.  Otherwise: Set risk to `"NORMAL"`

*Hint: Remember the precedence!*

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Test transaction
transaction_amount: Decimal = Decimal("25000.00")

# Initialize
risk_level: str = "UNKNOWN"

# Classify the risk level (Severity Ladder)



# Audit Trail
print(f"Transaction: ${transaction_amount:,.2f}")
print(f"Risk Level: {risk_level}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert transaction_amount == Decimal("25000.00"), "transaction_amount should be Decimal('25000.00')"
assert risk_level == "HIGH", "risk_level should be 'HIGH' for $25,000 (It is > 10,000 but not > 50,000)"
