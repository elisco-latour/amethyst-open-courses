---
id: "finguard_04_05"
title: "The Circuit Breaker"
type: "coding"
xp: 100
---

# The Circuit Breaker

Processing millions of transactions takes time. We need ways to optimize the flow.
*   **Filtering:** Ignoring garbage data (cancelled transactions).
*   **Emergency Stop:** Halting the system if we find a security breach.

## Control Flow Tools

### 1. `continue` (The Filter)
"Skip the rest of this iteration and move to the next item."
*   *Use Case:* Skipping invalid or cancelled records without nesting huge `if` blocks.

### 2. `break` (The Eject Button)
"Stop the entire loop immediately."
*   *Use Case:* We found the record we were searching for, OR we found a critical error.

## The Pattern

```python
for txn in transactions:
    # 1. Filter: Skip testing transactions
    if txn['is_test']:
        continue 
    
    # 2. Circuit Breaker: Stop if we hit a known malicious ID
    if txn['id'] in blacklisted_ids:
        print("🚨 SECURITY THREAT FOUND. HALTING.")
        break
        
    # 3. Process normal transaction
    process(txn)
```

## Task

Process the batch with Safety Controls:

1.  Loop through transactions.
2.  If `status` is `"cancelled"`, **continue** (skip counting).
3.  If `amount` > `100,000.00`, set `critical_alert` to `True`, capture the ID in `critical_transaction`, and **break** (stop everything).
4.  Otherwise, increment `processed_count`.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

# Transaction batch
transactions: list[dict] = [
    {"id": "TXN-001", "amount": Decimal("500.00"), "status": "completed"},
    {"id": "TXN-002", "amount": Decimal("1500.00"), "status": "cancelled"},
    {"id": "TXN-003", "amount": Decimal("8000.00"), "status": "completed"},
    {"id": "TXN-004", "amount": Decimal("150000.00"), "status": "completed"},
    {"id": "TXN-005", "amount": Decimal("200.00"), "status": "completed"},
]

processed_count: int = 0
critical_alert: bool = False
critical_transaction: str = ""

# Batch Processing



# Report
print(f"=== Batch Processing Results ===")
print(f"Processed: {processed_count} transactions")
print(f"Critical Alert: {critical_alert}")
if critical_alert:
    print(f"Alert Transaction: {critical_transaction}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert processed_count == 2, "processed_count should be 2 (TXN-001 and TXN-003)"
assert critical_alert == True, "critical_alert should be True (TXN-004 exceeded $100,000)"
assert critical_transaction == "TXN-004", "critical_transaction should be 'TXN-004'"
