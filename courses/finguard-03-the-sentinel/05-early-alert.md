---
id: "finguard_03_05"
title: "Defensive Code: Guard Clauses"
type: "coding"
xp: 100
---

# Defensive Programming

Beginners write code that hopes everything goes right — the "Happy Path."

Engineers write code that **expects things to go wrong** — the "Defensive Path."

## The Nesting Problem

Look at this typical beginner code:

```python
if account_active:
    if not is_fraud:
        if balance > amount:
            process_transaction()  # Buried deep!
```

Every condition adds another level of nesting. After a few more checks, you're lost in a pyramid of indentation. Finding the actual business logic is like finding a needle in a haystack.

## The Guard Clause Solution

Guard clauses flip the logic. Instead of nesting "if everything is okay," we check for problems first and exit early:

```python
# Check each problem, one by one
if not account_active:
    status = "REJECTED: Account Inactive"
elif is_fraud:
    status = "REJECTED: Fraud Detected"  
elif balance < amount:
    status = "REJECTED: Insufficient Funds"
else:
    status = "APPROVED"  # Happy path is clean and obvious
```

This structure is like a security checkpoint:
1. Check ID → if invalid, reject
2. Check watchlist → if flagged, reject
3. Check balance → if insufficient, reject
4. All clear? → Approved!

<div class="key-concept">
<h4>🔑 Key Concept: Fail Fast</h4>

Guard clauses embody the "Fail Fast" principle: if something is wrong, detect it immediately and stop. Don't let bad data wander deep into your system before failing.
</div>

## Why Guard Clauses Matter

1. **Flat is better than nested** — Easier to read and maintain
2. **One reason per branch** — Each `elif` handles one specific problem
3. **Happy path is obvious** — The `else` block is always the success case
4. **Easy to add new checks** — Just add another `elif` before the `else`

## Task

Write a defensive validation sequence for a transaction. Check these conditions **in order**:

1. If `account_is_blocked` is `True` → status is `"BLOCKED"`
2. If `amount` <= 0 → status is `"INVALID_AMOUNT"`
3. If `amount` > `balance` → status is `"INSUFFICIENT_FUNDS"`
4. If `is_verified` is `False` → status is `"PENDING_VERIFICATION"`
5. Otherwise → status is `"APPROVED"`

Store the result in `transaction_status`.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Transaction context
amount: Decimal = Decimal("500.00")
balance: Decimal = Decimal("1000.00")
account_is_blocked: bool = False
is_verified: bool = True

# Apply guard clauses for defensive validation
transaction_status: str = ""



# Audit
print(f"Amount: ${amount}")
print(f"Balance: ${balance}")
print(f"Blocked: {account_is_blocked}")
print(f"Verified: {is_verified}")
print(f"")
print(f"Status: {transaction_status}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert amount == Decimal("500.00"), "Don't modify amount"
assert balance == Decimal("1000.00"), "Don't modify balance"
assert transaction_status == "APPROVED", "With these values, transaction should be APPROVED (all checks pass)"
