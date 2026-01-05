---
id: "finguard_04_02"
title: "Polling for Updates"
type: "coding"
xp: 100
---

# Polling for Updates

Sometimes you don't know **how many** iterations you need. You keep checking until a condition is met.

## The Analogy: Waiting for Approval

When you submit a large transfer, you wait for compliance to approve it. You keep checking:
- "Is it approved yet?" → No → Wait → Check again
- "Is it approved yet?" → No → Wait → Check again
- "Is it approved yet?" → **Yes!** → Stop waiting

## The `while` Loop

A `while` loop repeats **as long as a condition is true**:

```python
approval_status: str = "PENDING"
checks: int = 0

while approval_status == "PENDING":
    checks += 1  # Same as: checks = checks + 1
    print(f"Check #{checks}: Still pending...")
    
    # Simulate eventual approval
    if checks >= 3:
        approval_status = "APPROVED"

print(f"✓ Status: {approval_status}")
```

## Danger: Infinite Loops!

```python
# ❌ NEVER DO THIS — runs forever!
while True:
    print("This never stops")

# ✅ Always have an exit condition
attempts: int = 0
while attempts < 10:
    attempts += 1
    if found_result:
        break  # Exit the loop early
```

## The "Pro" Tip

> **Use `for` when you know how many iterations. Use `while` when you don't know, but have a stop condition.**

```python
# ✅ for — we know there are 5 transactions
for txn in transactions:
    process(txn)

# ✅ while — we don't know when approval comes
while status == "PENDING":
    check_status()
```

## Task

Simulate polling for transaction approval:
1. Start with `status = "PENDING"`
2. Keep polling (incrementing `poll_count`)
3. After 5 polls, change status to `"APPROVED"`
4. Exit the loop
5. Store the final poll count

<!-- SEPARATOR -->

# seed_code
# Transaction awaiting approval
transaction_id: str = "TXN-HIGH-001"
status: str = "PENDING"
poll_count: int = 0

# Poll until approved
print(f"Polling for approval of {transaction_id}...")



print(f"")
print(f"✓ Transaction {status} after {poll_count} polls")

<!-- SEPARATOR -->

# validation_code
assert transaction_id == "TXN-HIGH-001", "transaction_id should be 'TXN-HIGH-001'"
assert status == "APPROVED", "status should be 'APPROVED' after polling"
assert poll_count == 5, "poll_count should be 5"
