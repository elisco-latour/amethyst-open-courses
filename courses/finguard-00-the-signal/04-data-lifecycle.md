---
id: "finguard_00_04"
title: "The Data Lifecycle"
type: "coding"
xp: 100
---

# The Data Lifecycle

Data isn't static. It's born, it lives, it transforms, and eventually, it dies. Understanding this lifecycle is crucial for building reliable systems.

## The Analogy: The Transaction's Journey

Follow a single wire transfer through FinGuard:

```
┌─────────────────────────────────────────────────────────────────┐
│                     THE DATA LIFECYCLE                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. CREATE      Customer initiates transfer                     │
│       ↓         {"from": "A", "to": "B", "amount": 10000}       │
│                                                                 │
│  2. VALIDATE    Is the data correct? Is there enough balance?  │
│       ↓         Check: amount > 0, account exists, funds OK    │
│                                                                 │
│  3. STORE       Save to database (Data at Rest)                │
│       ↓         INSERT INTO transactions VALUES (...)           │
│                                                                 │
│  4. PROCESS     Apply business logic, detect fraud             │
│       ↓         if amount > 10000: flag_for_review()           │
│                                                                 │
│  5. ANALYZE     Generate reports, find patterns                │
│       ↓         "Average transaction: $2,500"                   │
│                                                                 │
│  6. ARCHIVE     Move old data to cold storage                  │
│       ↓         After 7 years, move to archive                 │
│                                                                 │
│  7. DELETE      Permanently remove (GDPR, retention policy)    │
│                 After 10 years, purge completely               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## Why Each Stage Matters

| Stage | If We Skip It... |
|-------|------------------|
| **Validate** | Garbage data corrupts everything downstream |
| **Store** | Data lost on power failure = lawsuits |
| **Process** | Fraud goes undetected = millions lost |
| **Analyze** | Can't answer "how much fraud this quarter?" |
| **Archive** | Disk fills up, system crashes |
| **Delete** | Violate GDPR = €20 million fine |

## The Golden Rule

> **Bad data in = Bad decisions out.**

If you don't validate at step 2, every subsequent step is working with garbage.

FinGuard's #1 job is **data quality**. The fraud detection is useless if the input data is wrong.

## Task

This code simulates the lifecycle of a single transaction. Each stage transforms or validates the data.

Run it to see how data flows through FinGuard.

<!-- SEPARATOR -->

# seed_code
# THE DATA LIFECYCLE IN ACTION

# Stage 1: CREATE - Raw input from customer
raw_input = {
    "from_account": "ACC-1001",
    "to_account": "ACC-2002",
    "amount": 15000.00,
    "currency": "USD"
}
print("1. CREATE:", raw_input)

# Stage 2: VALIDATE - Check data quality
is_valid = (
    raw_input["amount"] > 0 and
    raw_input["from_account"].startswith("ACC-") and
    raw_input["to_account"].startswith("ACC-")
)
print(f"2. VALIDATE: {'✓ PASSED' if is_valid else '✗ FAILED'}")

# Stage 3: STORE - Add metadata for storage
stored_record = {
    **raw_input,
    "transaction_id": "TXN-78432",
    "status": "pending",
    "created_at": "2025-01-06T10:30:00Z"
}
print(f"3. STORE: Added ID {stored_record['transaction_id']}")

# Stage 4: PROCESS - Apply business rules
is_suspicious = stored_record["amount"] > 10000
stored_record["flagged"] = is_suspicious
print(f"4. PROCESS: Flagged = {is_suspicious}")

# Stage 5: ANALYZE - Extract insights
insight = f"Transaction {stored_record['transaction_id']}: ${stored_record['amount']} {'⚠️ REVIEW' if is_suspicious else '✓ OK'}"
print(f"5. ANALYZE: {insight}")

print("\n✓ Lifecycle complete. Data ready for archival after 7 years.")

<!-- SEPARATOR -->

# validation_code
assert raw_input["amount"] == 15000.00, "raw_input amount should be 15000.00"
assert is_valid == True, "is_valid should be True"
assert "transaction_id" in stored_record, "stored_record should have transaction_id"
assert stored_record["flagged"] == True, "Transaction over 10000 should be flagged"
