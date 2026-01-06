---
id: "finguard_04_02"
title: "The Watchdog"
type: "coding"
xp: 100
---

# The Watchdog

In distributed systems, we often wait for other systems to finish. We wait for a bank transfer to clear, a report to generate, or a server to boot up.

We don't know *how long* it will take. We just know we must wait **while** the task is incomplete.

## The `while` Loop

The `while` loop is a **State Checker**. It keeps running generally until a condition becomes `False`.

```python
status: str = "PENDING"
checks: int = 0

# The Watchdog Pattern
while status == "PENDING":
    print("Checking external API...")
    # ... logic to check API ...
    checks = checks + 1
    
    if checks > 5:
        print("Timeout: Giving up.")
        break
```

## Danger: The Infinite Loop

If the condition *never* becomes False, the program hangs forever. This is a CPU freeze.

> **Engineering Rule:** Every `while` loop must have a guaranteed **exit strategy** (a timeout or a state change).

## Task

Simulate a **Transaction Polling Service**.
1.  Running a loop **while** `status` is `"PENDING"`.
2.  Inside the loop, increment `poll_count`.
3.  Simulate a response: If `poll_count` reaches 5, change `status` to `"APPROVED"`.
4.  Once the loop finishes, log the final result.

<!-- SEPARATOR -->

# seed_code
# Context
transaction_id: str = "TXN-HIGH-001"
status: str = "PENDING"
poll_count: int = 0

# Watchdog Loop
print(f"Monitoring {transaction_id}...")



# Final State
print(f"Transaction {status} after {poll_count} polls")

<!-- SEPARATOR -->

# validation_code
assert transaction_id == "TXN-HIGH-001", "transaction_id should be 'TXN-HIGH-001'"
assert status == "APPROVED", "status should be 'APPROVED' after polling"
assert poll_count == 5, "poll_count should be 5"
