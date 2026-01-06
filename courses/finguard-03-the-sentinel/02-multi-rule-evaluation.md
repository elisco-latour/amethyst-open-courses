---
id: "finguard_03_02"
title: "The Hierarchy: elif Chains"
type: "coding"
xp: 100
---

# The Hierarchy of Rules

In banking, not all alerts are equal. A $1,001 transaction is a minor blip. A $1,000,000 transaction is a Code Red.

Simple `if/else` gives us two paths. But what if we need **four** paths?

## The `elif` Chain

`elif` stands for "else if". It lets us check multiple conditions in sequence:

```python
amount = 25000

if amount > 50000:
    risk = "CRITICAL"
elif amount > 10000:
    risk = "HIGH"
elif amount > 1000:
    risk = "ELEVATED"
else:
    risk = "NORMAL"
```

Python checks each condition **in order**:
1. Is amount > 50000? No → move on
2. Is amount > 10000? Yes → set risk to "HIGH", **stop checking**

Once a condition matches, Python skips all remaining `elif` and `else` blocks.

## The Precedence Principle

Think of a hospital triage nurse. They check for the **most severe** condition first:

1. Is the patient dying? → Emergency room
2. Is the patient bleeding? → Urgent care
3. Is the patient in pain? → Standard care
4. Otherwise → Waiting room

The order matters! Watch what happens if we check in the wrong order:

```python
# ✗ WRONG ORDER
if amount > 1000:
    risk = "ELEVATED"    # $50,001 would match here and stop!
elif amount > 10000:
    risk = "HIGH"        # Never reached for amounts > 1000
elif amount > 50000:
    risk = "CRITICAL"    # Never reached!
```

<div class="key-concept">
<h4>🔑 Key Concept: Check Severe First</h4>

When using `elif` chains, always order your conditions from **most specific/severe** to **least specific/safe**. Check for Critical before High before Normal.
</div>

## Task

Implement the **Transaction Severity Ladder**:

| Amount | Risk Level |
|--------|------------|
| Over $50,000 | `"CRITICAL"` |
| Over $10,000 | `"HIGH"` |
| Over $1,000 | `"ELEVATED"` |
| Otherwise | `"NORMAL"` |

The test transaction is $25,000 — what risk level should it get?

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Test transaction
transaction_amount: Decimal = Decimal("25000.00")

# Classify the risk level using if/elif/else
# Remember: check the most severe condition first!
risk_level: str = ""



# Audit Trail
print(f"Transaction: ${transaction_amount:,.2f}")
print(f"Risk Level: {risk_level}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert transaction_amount == Decimal("25000.00"), "Don't modify transaction_amount"
assert risk_level == "HIGH", "risk_level should be 'HIGH' for $25,000 (over 10k but not over 50k)"
