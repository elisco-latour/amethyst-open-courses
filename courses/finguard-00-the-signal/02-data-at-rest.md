---
id: "finguard_00_02"
title: "Data at Rest: Persistence & Durability"
type: "coding"
xp: 100
---

# Persistence and Durability

Data that is not persisted does not exist. In banking, **Durability** is our primary directive. If a transaction is acknowledged but lost due to a power failure, we have committed fraud.

## The Storage Hierarchy

We do not simply "save files." We choose storage architectures based on the **Trade-offs of Distributed Systems**.

### 1. The Transactional Authority (RDBMS)
*   **Role:** The "Source of Truth" for account balances.
*   **Constraint:** Must be **ACID** (Atomicity, Consistency, Isolation, Durability).
*   **Example:** PostgreSQL.
*   **Cost:** High. Hard to scale horizontally.

### 2. The Data Lake (Object Storage)
*   **Role:** The infinite archive for audit logs and raw events.
*   **Constraint:** Eventual consistency. High latency.
*   **Example:** AWS S3, Azure Blob.
*   **Insight:** As noted in *Fundamentals of Data Engineering*, we use Object Storage to decouple **Compute** from **Storage**, allowing us to scale them independently.

### 3. Ephemeral Storage (Block/Memory)
*   **Role:** Scratch space for processing.
*   **Constraint:** Volatile. If the instance dies, the data dies.
*   **Rule:** Never treat local disk as a system of record.

## Serialization Protocols

To store data, we must serialize in-memory objects into bytes. The format matters.

*   **JSON:** Self-describing, flexible, but heavy (verbose). Good for APIs.
*   **CSV:** Compact, but fragile (no type enforcement). Good for bulk transfer.
*   **Parquet/Avro:** (Advanced) Schema-enforced binary formats for high-performance engineering.

## Task

Analyze the overhead of descriptive formats versus compact formats. Notice that "human readability" often comes at the cost of "performance efficiency."

<!-- SEPARATOR -->

# seed_code
# Comparison of Serialization overhead for a single account record

# 1. Unstructured Log: No schema, high ambiguity. "Weak Structure" (You must guess how to parse it)
unstructured_log = "Account: 1001, Name: Alice Smith, Balance: 5000.00"

# 2. Delimited Record (CSV): Schema implied by position. "Fragile Order" (If columns swap, code breaks)
delimited_record = "account_id,name,balance\n1001,Alice Smith,5000.00"

# 3. Serialized Document (JSON): Schema embedded in the data. Heavy metadata overhead.
serialized_document = '{"account_id": 1001, "name": "Alice Smith", "balance": 5000.00}'

print("=== UNSTRUCTURED LOG ===")
print(f"Payload: {unstructured_log}")
print(f"Storage Cost: {len(unstructured_log)} bytes\n")

print("=== DELIMITED RECORD (CSV) ===")
print(f"Payload: {delimited_record}")
print(f"Storage Cost: {len(delimited_record)} bytes\n")

print("=== SERIALIZED DOCUMENT (JSON) ===")
print(f"Payload: {serialized_document}")
print(f"Storage Cost: {len(serialized_document)} bytes")

<!-- SEPARATOR -->

# validation_code
assert "1001" in unstructured_log, "unstructured_log should contain account 1001"
assert "account_id" in delimited_record, "delimited_record should have a header row"
assert "account_id" in serialized_document, "serialized_document should have account_id key"
