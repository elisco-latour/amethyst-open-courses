---
id: "finguard_03_04"
title: "Transaction Routing"
type: "coding"
xp: 100
---

# Transaction Routing

FinGuard handles multiple transaction types:
- **SWIFT** — International wire transfers
- **SEPA** — European transfers
- **ACH** — US domestic transfers
- **INTERNAL** — Same-bank transfers

Each type has different processing rules. Python 3.10+ introduced `match-case` for clean multi-way branching.

## The `match-case` Statement

```python
transaction_type: str = "SWIFT"

match transaction_type:
    case "SWIFT":
        processing_fee = 25.00
        processing_time = "2-3 business days"
    case "SEPA":
        processing_fee = 5.00
        processing_time = "1 business day"
    case "ACH":
        processing_fee = 0.50
        processing_time = "1-2 business days"
    case _:  # Default case (underscore matches anything)
        processing_fee = 0.00
        processing_time = "Instant"
```

## The Analogy: The Mail Sorting Room

Post office workers sort mail by destination:
- **International** → Customs queue
- **Express** → Priority handling
- **Standard** → Regular processing

`match-case` is the sorting machine.

## Why `match` Instead of `if-elif`?

```python
# ❌ Verbose and repetitive
if transaction_type == "SWIFT":
    fee = 25.00
elif transaction_type == "SEPA":
    fee = 5.00
elif transaction_type == "ACH":
    fee = 0.50
else:
    fee = 0.00

# ✅ Cleaner with match-case
match transaction_type:
    case "SWIFT": fee = 25.00
    case "SEPA": fee = 5.00
    case "ACH": fee = 0.50
    case _: fee = 0.00
```

## The "Pro" Tip

> **Use `match-case` when comparing one value against multiple possible matches. Use `if-elif` when conditions are complex expressions.**

## Task

Build a transaction router that sets the `processing_rules` dictionary based on `transaction_type`:

- `"SWIFT"` → `{"fee": 25.00, "days": 3, "compliance_check": True}`
- `"SEPA"` → `{"fee": 5.00, "days": 1, "compliance_check": False}`
- `"ACH"` → `{"fee": 0.50, "days": 2, "compliance_check": False}`
- `"INTERNAL"` → `{"fee": 0.00, "days": 0, "compliance_check": False}`
- Any other → `{"fee": 10.00, "days": 5, "compliance_check": True}` (unknown type, be cautious)

<!-- SEPARATOR -->

# seed_code
# Incoming transaction type
transaction_type: str = "SWIFT"

# Route the transaction



# Print the routing decision
print(f"Transaction Type: {transaction_type}")
print(f"Processing Fee: ${processing_rules['fee']:.2f}")
print(f"Processing Days: {processing_rules['days']}")
print(f"Compliance Check Required: {processing_rules['compliance_check']}")

<!-- SEPARATOR -->

# validation_code
assert transaction_type == "SWIFT", "transaction_type should be 'SWIFT'"
assert processing_rules["fee"] == 25.00, "SWIFT fee should be 25.00"
assert processing_rules["days"] == 3, "SWIFT processing days should be 3"
assert processing_rules["compliance_check"] == True, "SWIFT should require compliance check"
