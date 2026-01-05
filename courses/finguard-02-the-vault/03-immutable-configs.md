---
id: "finguard_02_03"
title: "Immutable Configs"
type: "coding"
xp: 100
---

# Immutable Configs

Some data should **never change** after it's created. A transaction, once recorded, cannot be modified — that would be fraud.

## The Analogy: The Audit Trail

In banking, every transaction is written in **permanent ink**. You can't erase a wire transfer. If there's an error, you create a **new** reversal transaction.

## Tuples: Immutable Sequences

A **tuple** is like a list, but **frozen** — you can't modify it after creation.

```python
# Creating a tuple (use parentheses, not brackets)
transaction_record: tuple[str, str, float] = ("TXN-001", "2025-01-06", 1500.00)

# Access by index (same as list)
txn_id: str = transaction_record[0]  # "TXN-001"

# ❌ Cannot modify!
transaction_record[0] = "TXN-002"  # TypeError: 'tuple' does not support item assignment
```

## When to Use Tuples

| Use Case | Why Tuple? |
|----------|------------|
| Transaction records | Cannot be altered after creation |
| Database rows | Data came from storage, shouldn't change |
| Function return values | Return multiple values safely |
| Configuration constants | Prevent accidental modification |

## The "Pro" Tip

> **Use tuples for data that should never change. Use lists for data that grows or changes.**

```python
# ✅ Tuple — this exchange rate was fixed at this moment
exchange_rate_snapshot: tuple[str, str, float] = ("EUR", "USD", 1.0856)

# ✅ List — we keep adding transactions
pending_transactions: list[str] = ["TXN-001"]
pending_transactions.append("TXN-002")  # OK
```

## Tuple Unpacking

Python lets you "unpack" tuples into separate variables:

```python
transaction: tuple[str, str, float] = ("TXN-001", "2025-01-06", 1500.00)

# Unpack into separate variables
txn_id, date, amount = transaction

print(txn_id)   # "TXN-001"
print(date)     # "2025-01-06"
print(amount)   # 1500.00
```

## Task

Create an immutable transaction record as a tuple with:
- Transaction ID: `"TXN-9001"`
- Timestamp: `"2025-01-06T14:30:00Z"`
- Amount: `8750.00`
- Currency: `"USD"`

Then unpack it into separate variables.

<!-- SEPARATOR -->

# seed_code
# Create the immutable transaction record (tuple)


# Unpack the tuple into separate variables


# Print the record
print(f"=== Immutable Transaction Record ===")
print(f"ID: {txn_id}")
print(f"Time: {timestamp}")
print(f"Amount: {currency} {amount:,.2f}")
print(f"Type: {type(transaction_record).__name__} (cannot be modified)")

<!-- SEPARATOR -->

# validation_code
assert transaction_record == ("TXN-9001", "2025-01-06T14:30:00Z", 8750.00, "USD"), "transaction_record should match the expected tuple"
assert txn_id == "TXN-9001", "txn_id should be 'TXN-9001'"
assert timestamp == "2025-01-06T14:30:00Z", "timestamp should be '2025-01-06T14:30:00Z'"
assert amount == 8750.00, "amount should be 8750.00"
assert currency == "USD", "currency should be 'USD'"
assert isinstance(transaction_record, tuple), "transaction_record must be a tuple"
