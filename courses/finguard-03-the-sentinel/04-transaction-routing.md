---
id: "finguard_03_04"
title: "The Dispatcher"
type: "coding"
xp: 100
---

# The Dispatcher

So far, we've used `if` statements to handle **Scales** (Low, High, Critical) or **Policies** (Allowed/Blocked). 

However, sometimes we just need to route data to the correct department based on a category. This is **Pattern Matching**.

FinGuard processes four distinct payment rails:
*   **SWIFT:** International wires. Slow, expensive.
*   **SEPA:** Euro-zone transfers. Fast, cheap.
*   **ACH:** US Domestic. Slow, very cheap.
*   **INTERNAL:** Instant, free.

## The `match-case` Statement

Instead of a messy chain of `if rail == "SWIFT" ... elif rail == "SEPA" ...`, Python 3.10+ gives us a specialized tool:

```python
payment_rail: str = "SWIFT"

match payment_rail:
    case "SWIFT":
        processor = "IntlTeam"
    case "SEPA":
        processor = "EuroTeam"
    case "ACH":
        processor = "USTeam"
    case _:
        processor = "ManualReview" # The 'catch-all' case
```

This acts like a physical **Switchboard**, plugging the connection into the right socket instantly.

## Why use `match`?

It signals intent. When another engineer sees `match`, they know: *"Ah, we are selecting one option from a fixed menu."* When they see `if/elif`, they think: *"We are evaluating complex logic."*

## Task

Build the **Routing Logic** for the fee calculator.
Depending on the `transaction_type`, set the `processing_rules` dictionary:

*   `"SWIFT"` -> `{"fee": 25.00, "speed": "Slow"}`
*   `"SEPA"` -> `{"fee": 5.00, "speed": "Fast"}`
*   `"ACH"` -> `{"fee": 0.50, "speed": "Medium"}`
*   `"INTERNAL"` -> `{"fee": 0.00, "speed": "Instant"}`
*   `"WIRE"` -> `{"fee": 15.00, "speed": "Fast"}`
*   Anything else (`case _`) -> `{"fee": 0.00, "speed": "Unknown"}`

<!-- SEPARATOR -->

# seed_code
# Incoming transaction type
transaction_type: str = "SWIFT"

# Route the transaction using match-case



# Audit Log
print(f"Type: {transaction_type}")
print(f"Fee: ${processing_rules['fee']:.2f}")
print(f"Speed: {processing_rules['speed']}")

<!-- SEPARATOR -->

# validation_code
assert processing_rules['fee'] == 25.00
assert processing_rules['speed'] == "Slow"
assert transaction_type == "SWIFT"
assert transaction_type == "SWIFT", "transaction_type should be 'SWIFT'"
assert processing_rules["fee"] == 25.00, "SWIFT fee should be 25.00"
assert processing_rules["days"] == 3, "SWIFT processing days should be 3"
assert processing_rules["compliance_check"] == True, "SWIFT should require compliance check"
