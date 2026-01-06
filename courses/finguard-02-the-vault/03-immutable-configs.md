---
id: "finguard_02_03"
title: "The Seal: Tuples & Immutability"
type: "coding"
xp: 100
---

# The Seal: Immutability

In banking, some data can change and some data **must never change**.

- **Can change:** Your current balance, your address, your phone number
- **Cannot change:** The fact that a transaction occurred, the date it happened, your date of birth

Data that cannot be modified is called **immutable**.

## Lists vs Tuples

Python has two similar-looking collections:

```python
# List: Uses square brackets, CAN be changed
rates_list = [1.2, 1.3, 1.4]
rates_list[0] = 9.9  # ✓ This works

# Tuple: Uses parentheses, CANNOT be changed
rates_tuple = (1.2, 1.3, 1.4)
rates_tuple[0] = 9.9  # ✗ TypeError! Python protects you.
```

## Why Immutability Matters

When a customer sends $500 to another account, that **fact** happened. Even if we reverse the transaction later, the original event is a historical fact that cannot be altered.

We use tuples to represent facts that should never be edited:

```python
# A transaction fact: (sender, receiver, date, amount)
transaction_fact = ("ACC-101", "ACC-202", "2025-01-15", 500.00)
```

If anyone tries to modify this record, Python raises an error. This is a **safety feature**.

## Tuple Unpacking

You can extract values from a tuple into separate variables:

```python
transaction = ("ACC-101", "ACC-202", "2025-01-15", 500.00)

# Unpack into named variables
sender, receiver, date, amount = transaction

print(sender)   # "ACC-101"
print(amount)   # 500.00
```

<div class="key-concept">
<h4>🔑 Key Concept: Use Tuples for Facts</h4>

In FinGuard, we use tuples for data that represents historical facts — things that happened and cannot be changed. Use lists for data that needs to grow or change over time.
</div>

## Task

Create an immutable transaction record as a tuple with these values (in order):
1. Sender account: `"ACC-101"`
2. Receiver account: `"ACC-202"`
3. Transaction date: `"2025-01-15"`
4. Amount: `750.00`
5. Currency: `"USD"`

Then unpack the tuple into individual variables.

<!-- SEPARATOR -->

# seed_code
# Create an immutable transaction record (tuple)
# Order: (sender, receiver, date, amount, currency)
transaction_record: tuple = 

# Unpack the tuple into named variables
sender, receiver, date, amount, currency = transaction_record

# Display the verified record
print("=== Immutable Transaction Record ===")
print(f"From: {sender}")
print(f"To: {receiver}")
print(f"Date: {date}")
print(f"Amount: {currency} {amount}")
print(f"Record Type: {type(transaction_record).__name__}")

<!-- SEPARATOR -->

# validation_code
assert isinstance(transaction_record, tuple), "transaction_record must be a tuple (use parentheses)"
assert len(transaction_record) == 5, "transaction_record should have 5 elements"
assert sender == "ACC-101", "First element (sender) should be 'ACC-101'"
assert receiver == "ACC-202", "Second element (receiver) should be 'ACC-202'"
assert date == "2025-01-15", "Third element (date) should be '2025-01-15'"
assert amount == 750.00, "Fourth element (amount) should be 750.00"
assert currency == "USD", "Fifth element (currency) should be 'USD'"
