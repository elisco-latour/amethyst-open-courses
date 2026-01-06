---
id: "finguard_03_04"
title: "The Dispatcher: Pattern Matching"
type: "coding"
xp: 100
---

# The Dispatcher

So far, we've used `if` statements to handle **scales** (Low → High → Critical) and **policies** (Allowed/Blocked).

But sometimes we just need to route data to the correct handler based on a category. This is **pattern matching**.

## Payment Rails at FinGuard

FinGuard processes transactions through different payment systems:

| Rail | Description | Speed | Typical Fee |
|------|-------------|-------|-------------|
| **SWIFT** | International wires | Slow (1-3 days) | $25+ |
| **SEPA** | Euro-zone transfers | Fast (same day) | €5 |
| **ACH** | US domestic | Slow (2-3 days) | $0.50 |
| **INTERNAL** | Same-bank transfer | Instant | Free |

Each rail has different processing rules. We need to route incoming transactions to the right handler.

## The `match-case` Statement

Instead of writing a long `if/elif/elif/elif/else` chain:

```python
# Messy approach
if rail == "SWIFT":
    handler = "international"
elif rail == "SEPA":
    handler = "euro"
elif rail == "ACH":
    handler = "domestic"
else:
    handler = "unknown"
```

Python 3.10+ offers `match-case`:

```python
# Clean approach
match rail:
    case "SWIFT":
        handler = "international"
    case "SEPA":
        handler = "euro"
    case "ACH":
        handler = "domestic"
    case _:
        handler = "unknown"  # The _ is a catch-all (like "else")
```

## When to Use `match-case`

Use `match-case` when:
- You're selecting from a **fixed set of options** (like a menu)
- Each option maps to different behavior
- You want to signal "this is a routing decision" to other developers

Use `if/elif` when:
- You're evaluating **numeric ranges** or **complex conditions**
- Conditions overlap or have precedence

<div class="key-concept">
<h4>🔑 Key Concept: The Catch-All Pattern</h4>

The `case _:` pattern matches **anything** that didn't match previous cases. Always include it to handle unexpected values safely.
</div>

## Task

Build the routing logic for FinGuard's fee calculator using `match-case`.

Based on `transaction_type`, set `processing_rules` to a dictionary:

| Type | `fee` | `speed` |
|------|-------|---------|
| `"SWIFT"` | 25.00 | `"Slow"` |
| `"SEPA"` | 5.00 | `"Fast"` |
| `"ACH"` | 0.50 | `"Medium"` |
| `"INTERNAL"` | 0.00 | `"Instant"` |
| Anything else | 0.00 | `"Unknown"` |

<!-- SEPARATOR -->

# seed_code
# Incoming transaction type
transaction_type: str = "SWIFT"

# Initialize
processing_rules: dict = {}

# Route using match-case
match transaction_type:
    # Add your cases here
    pass

# Audit Log
print(f"Type: {transaction_type}")
print(f"Fee: ${processing_rules['fee']:.2f}")
print(f"Speed: {processing_rules['speed']}")

<!-- SEPARATOR -->

# validation_code
assert transaction_type == "SWIFT", "Don't modify transaction_type"
assert isinstance(processing_rules, dict), "processing_rules should be a dictionary"
assert processing_rules["fee"] == 25.00, "SWIFT fee should be 25.00"
assert processing_rules["speed"] == "Slow", "SWIFT speed should be 'Slow'"
