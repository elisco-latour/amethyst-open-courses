---
id: "finguard_03_05"
title: "Early Alert"
type: "coding"
xp: 100
---

# Early Alert

Sometimes, you want to **stop processing immediately** when you find something wrong. Why check 10 rules if the first one already disqualifies the transaction?

## The Analogy: The Bouncer

A nightclub bouncer checks:
1. Are you on the banned list? → **STOP. You're not coming in.**
2. Do you have valid ID? → If no, stop.
3. Is the club at capacity? → If yes, stop.
4. Welcome in!

If check #1 fails, there's no point checking #2, #3, or #4.

## Early Return Pattern

In functions (which we'll cover soon), this pattern is called "early return." For now, we'll simulate it with nested conditions.

```python
# ❌ Nested mess
if not is_blocked:
    if amount <= account_balance:
        if is_verified:
            process_transaction()

# ✅ Early exit pattern (cleaner)
if is_blocked:
    result = "BLOCKED: Account is blocked"
elif amount > account_balance:
    result = "REJECTED: Insufficient funds"
elif not is_verified:
    result = "PENDING: Awaiting verification"
else:
    result = "APPROVED: Processing transaction"
```

## The Guard Clause Pattern

"Guard clauses" check for invalid conditions first and exit early:

```python
# Guard clauses — handle errors first
if amount <= 0:
    status = "REJECTED: Invalid amount"
elif amount > 1000000:
    status = "REJECTED: Exceeds daily limit"
elif sender == receiver:
    status = "REJECTED: Cannot transfer to self"
else:
    # Happy path — all validations passed
    status = "APPROVED"
```

## The "Pro" Tip

> **Check for invalid conditions first. The "happy path" should be the last case.**

This makes code easier to read: all the "what could go wrong" checks are at the top.

## Task

Build a transaction validator with early exit. Check these conditions **in order**:

1. If `account_is_blocked` is `True` → `"BLOCKED"`
2. If `amount` is less than or equal to 0 → `"INVALID_AMOUNT"`
3. If `amount` is greater than `available_balance` → `"INSUFFICIENT_FUNDS"`
4. If `is_verified` is `False` → `"PENDING_VERIFICATION"`
5. Otherwise → `"APPROVED"`

Store the result in `transaction_status`.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Transaction data
amount: Decimal = Decimal("500.00")
available_balance: Decimal = Decimal("1000.00")
account_is_blocked: bool = False
is_verified: bool = True

# Validate transaction with early exit pattern



# Print the result
print(f"=== Transaction Validation ===")
print(f"Amount: ${amount}")
print(f"Available Balance: ${available_balance}")
print(f"Account Blocked: {account_is_blocked}")
print(f"Verified: {is_verified}")
print(f"")
print(f"Status: {transaction_status}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert amount == Decimal("500.00"), "amount should be Decimal('500.00')"
assert available_balance == Decimal("1000.00"), "available_balance should be Decimal('1000.00')"
assert transaction_status == "APPROVED", "transaction_status should be 'APPROVED' for valid transaction"
