---
id: "finguard_00_04"
title: "The Data Lifecycle"
type: "coding"
xp: 100
---

# The Lifecycle of Liability

Data is not just "stuff". It is living information that creates risk. The moment we generate data, we are responsible for it.

## The Engineering Lifecycle

In FinGuard, we follow a strict **Lifecycle Model**. We are not just "saving files". We are shepherding data through distinct phases.

```
1. GENERATION   (The Source)   -> A customer swipes a card.
      │
2. INGESTION    (The Gateway)  -> We accept the signal.
      │
3. STORAGE      (The Vault)    -> We write it to disk (Durability).
      │
4. PROCESSING   (The Logic)    -> We detect fraud and update balances.
      │
5. SERVING      (The Output)   -> The mobile app shows the new balance.
```

## The Critical Phase: Generation

Most aspiring engineers focus on step 4 (Processing/AI).
**Senior Engineers focus on Step 1 (Generation).**

> **Garbage In, Liability Out.**

If the data is generated incorrectly (e.g., wrong currency, missing timestamp), no amount of AI can fix it later. We must practice **Defensive Architecture**.

## Task

Run this simulation of the lifecycle. Watch how the data object "gains weight" (metadata) as it passes through the stages. A raw signal becomes a trusted record.

<!-- SEPARATOR -->

# seed_code
# THE LIFECYCLE: From Signal to Record

# 1. GENERATION: The raw signal from the card reader.
# Note: It usually lacks context (time, location).
raw_signal = {"card_id": "CARD-99", "amount": 1500}
print(f"[1] Signal Generated: {raw_signal}")

# 2. INGESTION & ENRICHMENT: We add context.
# We stamp it with time and origin.
ingested_record = {
    **raw_signal,
    "timestamp": "2024-03-20T10:00:00Z",
    "gateway": "POS-TERMINAL-1"
}
print(f"[2] Signal Ingested:  {ingested_record}")

# 3. PROCESSING: We apply business rules.
processed_record = {
    **ingested_record,
    "status": "APPROVED",
    "fraud_score": 0.01
}
print(f"[3] Rules Applied:    {processed_record}")

# 4. SERVING: We present it to the user.
print(f"[4] User View:        You spent ${processed_record['amount']}")

<!-- SEPARATOR -->

# validation_code
assert raw_signal['amount'] == 1500, "Raw signal must be preserved"
assert 'timestamp' in ingested_record, "Ingestion must add timestamp"
assert processed_record['status'] == "APPROVED", "Processing must approve valid txn"


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
