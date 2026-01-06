---
id: "finguard_02_04"
title: "The Registry: Sets"
type: "coding"
xp: 100
---

# The Registry: Sets

Here's a common banking question: "How many unique customers made transactions today?"

If Customer A made 5 purchases, they should still count as **1 unique customer**, not 5.

## The Problem with Lists

Lists keep duplicates:

```python
visitors = ["Alice", "Bob", "Alice", "Alice", "Bob"]
print(len(visitors))  # 5 (counts duplicates)
```

## The Set Solution

A **set** is a collection where duplicates are automatically removed:

```python
visitors = ["Alice", "Bob", "Alice", "Alice", "Bob"]
unique_visitors = set(visitors)
print(unique_visitors)      # {"Alice", "Bob"}
print(len(unique_visitors)) # 2 (only unique values)
```

When you add something that already exists, the set simply ignores it:

```python
customers = {"Alice", "Bob"}
customers.add("Alice")  # Ignored — Alice is already there
print(customers)        # {"Alice", "Bob"}
```

## Sets for Reporting

Sets are essential for data engineering reports:
- How many **unique** customers this month?
- Which **distinct** products were purchased?
- How many **different** ATMs were used?

<div class="key-concept">
<h4>🔑 Key Concept: Sets for Uniqueness</h4>

Use a set whenever you need to track unique items or count distinct values. This is one of the most common operations in data analysis.
</div>

## Task

The fraud detection system logged every transaction initiator today. Your job:
1. Create `unique_customers` — a set of unique customer IDs from the raw list
2. Calculate `total_transactions` — how many transactions occurred (length of raw list)
3. Calculate `unique_count` — how many unique customers there are

<!-- SEPARATOR -->

# seed_code
# Raw transaction log - customer IDs with duplicates
transaction_log: list[str] = [
    "CUST-1001", "CUST-1002", "CUST-1001", "CUST-1003",
    "CUST-1002", "CUST-1004", "CUST-1001", "CUST-1005",
    "CUST-1003", "CUST-1002", "CUST-1001", "CUST-1004"
]

# Convert to a set of unique customer IDs
unique_customers = 

# Calculate metrics
total_transactions = 
unique_count = 

# Generate the report
print("=== Daily Activity Report ===")
print(f"Total Transactions: {total_transactions}")
print(f"Unique Customers: {unique_count}")
print(f"Customer Registry: {unique_customers}")

<!-- SEPARATOR -->

# validation_code
assert isinstance(unique_customers, set), "unique_customers must be a set"
assert len(unique_customers) == 5, "There should be 5 unique customers"
assert "CUST-1001" in unique_customers, "CUST-1001 should be in the set"
assert "CUST-1005" in unique_customers, "CUST-1005 should be in the set"
assert total_transactions == 12, "total_transactions should be 12 (length of original list)"
assert unique_count == 5, "unique_count should be 5"