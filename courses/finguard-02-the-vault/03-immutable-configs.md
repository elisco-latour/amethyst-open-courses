---
id: "finguard_02_03"
title: "The Seal: Immutability"
type: "coding"
xp: 100
---

# The Seal: Immutability

In a bank, you can change your "Current Balance". You can never change your "Date of Birth" or "Transaction History".

Data that cannot change is called **Immutable**.

## The Tuple

In Python, a List `[]` is mutable (editable).
A **Tuple** `()` is immutable (frozen).

```python
# List (Mutable)
rates = [1.2, 1.3]
rates[0] = 9.9   # Allowed!

# Tuple (Immutable)
coordinates = (1.2, 1.3)
coordinates[0] = 9.9  # ERROR! Python protects you.
```

## Why We Use Tuples

We use Tuples for **Facts**.
*   A Transaction happened at a specific time. That is a Fact. Even if we reverse the money later, the original event is a Fact.

## Task

Create a "Fact Record" for a transaction.
Observe that it cannot be edited.

<!-- SEPARATOR -->

# seed_code
# A Fact: Customer A sent money to Customer B on Jan 1st.
# Format: (Sender, Receiver, Date, Amount)
txn_fact: tuple = ("ACC-101", "ACC-202", "2025-01-01", 100.00)

print(f"Recorded Fact: {txn_fact}")

# Try to change the date (Simulator Logic)
try:
    # Attempting to tamper with the records...
    txn_fact[2] = "2025-01-02"
except TypeError:
    print("SECURITY ALERT: Attempt to modify immutable record block!")

# Unpack the fact into variables
sender, receiver, date, amount = txn_fact
print(f"Verified Sender: {sender}")

<!-- SEPARATOR -->

# validation_code
assert isinstance(txn_fact, tuple), "txn_fact must be a tuple"
assert sender == "ACC-101", "Unpacking failed"
assert currency == "USD", "currency should be 'USD'"
assert isinstance(transaction_record, tuple), "transaction_record must be a tuple"
