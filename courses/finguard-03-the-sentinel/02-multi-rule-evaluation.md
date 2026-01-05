---
id: "finguard_03_02"
title: "Multi-Rule Evaluation"
type: "coding"
xp: 100
---

# Multi-Rule Evaluation

Fraud detection isn't black and white. A transaction might be:
- **Normal** (under $1,000)
- **Elevated** (between $1,000 and $10,000) → Requires manager review
- **High-risk** (over $10,000) → Requires compliance review

## The `elif` Chain

`elif` (else if) lets you check **multiple conditions in order**:

```python
amount: float = 5000.00

if amount > 10000:
    risk_level = "HIGH"
elif amount > 1000:
    risk_level = "ELEVATED"
else:
    risk_level = "NORMAL"
```

Python checks each condition **from top to bottom** and stops at the first `True`.

## The Analogy: The Triage System

In a hospital emergency room:
1. **Critical** → Immediate attention (if heart attack)
2. **Urgent** → See within 1 hour (elif broken bone)
3. **Standard** → See when available (else minor issue)

FinGuard triages transactions the same way.

## Order Matters!

```python
# ❌ Wrong order — everything over 1000 triggers first!
if amount > 1000:
    risk_level = "ELEVATED"
elif amount > 10000:  # Never reached!
    risk_level = "HIGH"

# ✅ Correct — check the stricter condition first
if amount > 10000:
    risk_level = "HIGH"
elif amount > 1000:
    risk_level = "ELEVATED"
```

## The "Pro" Tip

> **Check the most specific/restrictive condition first.**

## Task

Build a transaction risk classifier:
- Over $50,000 → `"CRITICAL"` (requires board approval)
- Over $10,000 → `"HIGH"` (requires compliance review)
- Over $1,000 → `"ELEVATED"` (requires manager approval)
- Otherwise → `"NORMAL"` (auto-approved)

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Test transaction
transaction_amount: Decimal = Decimal("25000.00")

# Classify the risk level



# Print the classification
print(f"Transaction: ${transaction_amount:,.2f}")
print(f"Risk Level: {risk_level}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert transaction_amount == Decimal("25000.00"), "transaction_amount should be Decimal('25000.00')"
assert risk_level == "HIGH", "risk_level should be 'HIGH' for $25,000"
