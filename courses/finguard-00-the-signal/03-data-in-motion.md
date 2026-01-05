---
id: "finguard_00_03"
title: "Data in Motion: Flows & Streams"
type: "coding"
xp: 100
---

# Data in Motion

Data at rest is safe, but it generates no value. To settle a trade or detect fraud, data must move. We call this **Data in Motion**.

## The Transport Mechanisms

In banking, we move data using three primary patterns. Think of them like physical logistics:

### 1. Request/Response (The Bank Teller)
*   **What it is:** You ask for data, and you **wait** until you get it.
*   **Engineering Term:** Synchronous.
*   **Use Case:** "What is my current balance?"
*   **Risk:** If the system is slow, the user is frozen.

### 2. Batch Processing (The Armored Truck)
*   **What it is:** We collect transactions all day, pile them up, and move them all at once at midnight.
*   **Engineering Term:** High Latency, High Throughput.
*   **Use Case:** "Generate monthly account statements."
*   **Risk:** Data is always slightly old (stale).

### 3. Stream Processing (The Ticker Tape)
*   **What it is:** Every single event travels instantly, one by one.
*   **Engineering Term:** Low Latency, Real-Time.
*   **Use Case:** "Block this credit card transaction NOW."
*   **Risk:** Highly complex to build and maintain.

## The FinGuard Strategy

Startups try to stream everything. Banks use the right tool for the job.
*   We use **Streams** for security (Speed is safety).
*   We use **Batch** for reporting (Accuracy is safety).

## Task

Run this simulation. Observe the "Time to Insight" for each pattern.
*   **Stream:** Insight is instant.
*   **Batch:** Insight waits for the "End of Day" signal.

<!-- SEPARATOR -->

# seed_code
# SIMULATION: Detecting a High-Value Transaction

# 1. Request/Response: We have to ask for the data manually.
print("--- [1] Request/Response ---")
def check_balance_sync(account_id):
    print(f"   > Connecting to database for {account_id}...")
    return 5000.00 # Returns immediately (blocking)

balance = check_balance_sync("ACC-101")
print(f"   > Balance received: ${balance}\n")


# 2. Batch Processing: We wait until the 'truck' is full.
print("--- [2] Batch Processing ---")
batch_queue = []
# Accumulate data
batch_queue.append({"id": "TXN-01", "amount": 100})
batch_queue.append({"id": "TXN-02", "amount": 90000}) # suspicious!
print(f"   > Accumulating {len(batch_queue)} transactions...")
print("   > Waiting for midnight...")
# Process all at once
for txn in batch_queue:
    if txn['amount'] > 10000:
        print(f"   > ALERT (Found in nightly batch): Transaction {txn['id']} is suspicious.")
print("")


# 3. Stream Processing: We react the moment it happens.
print("--- [3] Stream Processing ---")
def on_transaction_event(txn):
    print(f"   > Event Received: {txn['id']} for ${txn['amount']}")
    if txn['amount'] > 10000:
        print(f"   > 🚨 BLOCKING SCRIPT: High Value Alert on {txn['id']}!")

# The event happens...
on_transaction_event({"id": "TXN-02", "amount": 90000})

<!-- SEPARATOR -->

# validation_code
assert balance == 5000.00, "Balance check failed"
assert len(batch_queue) == 2, "Batch queue check failed"

