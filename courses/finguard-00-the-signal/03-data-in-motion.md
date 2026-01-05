---
id: "finguard_00_03"
title: "Data in Motion"
type: "coding"
xp: 100
---

# Data in Motion

Data at rest is like money in a vault — safe, but not doing anything. The real action happens when data **moves**.

## The Analogy: The Wire Transfer

When you send money from New York to London:

1. **Your bank** creates a message: "Send $10,000 from Account A to Account B"
2. **The SWIFT network** carries that message across the ocean
3. **The London bank** receives, validates, and processes it
4. **Confirmation** travels back: "Transfer complete"

This is **Data in Motion** — information traveling between systems.

## Three Ways Data Moves

| Pattern | Description | FinGuard Example |
|---------|-------------|------------------|
| **Request/Response** | Ask a question, get an answer | "What's my balance?" → "$5,000" |
| **Batch** | Collect data, process later | Nightly regulatory report |
| **Stream** | Continuous flow of events | Real-time fraud detection |

### Request/Response (Synchronous)
You wait for the answer. Like a phone call.
- ✅ Simple to understand
- ❌ Blocks everything until response arrives

### Batch (Scheduled)
Collect all day, process at night. Like mail delivery.
- ✅ Efficient for large volumes
- ❌ Delays — fraud detected 12 hours later is useless

### Stream (Real-time)
Every event processed immediately. Like a security camera.
- ✅ Instant reaction to fraud
- ❌ Complex to build and operate

## Why FinGuard Needs Both

FinGuard uses **Streaming** for fraud detection (milliseconds matter) and **Batch** for regulatory reports (accuracy matters).

## Task

This code simulates the three patterns. Notice how the timing differs.

In a real system, streaming would detect the fraud BEFORE the batch report even starts.

<!-- SEPARATOR -->

# seed_code
# Simulating three data movement patterns

# PATTERN 1: Request/Response
print("=== REQUEST/RESPONSE ===")
account_id = "ACC-1001"
balance_request = f"Requesting balance for {account_id}..."
balance_response = 5000.00
print(balance_request)
print(f"Response: ${balance_response}\n")

# PATTERN 2: Batch (collected transactions)
print("=== BATCH PROCESSING ===")
daily_transactions = [
    {"id": "TXN-001", "amount": 150.00},
    {"id": "TXN-002", "amount": 75000.00},  # Suspicious!
    {"id": "TXN-003", "amount": 200.00},
]
print(f"Processing {len(daily_transactions)} transactions from today...")
print("Batch report generated at midnight.\n")

# PATTERN 3: Stream (real-time)
print("=== STREAM PROCESSING ===")
incoming_event = {"id": "TXN-002", "amount": 75000.00}
print(f"Event received: {incoming_event}")
if incoming_event["amount"] > 10000:
    print("⚠️  ALERT: Large transaction detected IMMEDIATELY!")

<!-- SEPARATOR -->

# validation_code
assert account_id == "ACC-1001", "account_id should be ACC-1001"
assert len(daily_transactions) == 3, "daily_transactions should have 3 items"
assert incoming_event["amount"] == 75000.00, "incoming_event amount should be 75000.00"
