---
id: "finguard_02_04"
title: "The Identify: Sets"
type: "coding"
xp: 100
---

# The Identity: Sets

If Customer A visits the bank 5 times, how many customers visited?
Answer: **1 Unique Customer**.

In Data Engineering, identifying "Uniques" is critical for reporting.

## The Set

A **Set** (`set`) is a collection where duplicates are impossible.
If you try to add "Customer A" twice, Python just ignores you.

```python
# A list of visitors (with duplicates)
visitors = ["Alice", "Bob", "Alice"]
print(len(visitors)) # 3

# A set of identities (Unique only)
identities = set(visitors)
print(len(identities)) # 2 (Alice is counted once)
```

## Task

De-duplicate the transaction log to find the distinct customers.

<!-- SEPARATOR -->

# seed_code
# A raw list of transaction initiator IDs (with duplicates)
transaction_customers: list[str] = [
    "CUST-1001", "CUST-1002", "CUST-1001", "CUST-1003",
    "CUST-1002", "CUST-1004", "CUST-1001", "CUST-1005"
]

# Convert to a list of unique identities using set()
unique_customers = set(transaction_customers)

print(f"Total Transactions: {len(transaction_customers)}")
print(f"Unique Customers: {len(unique_customers)}")
print(f"Customer Registry: {unique_customers}")

<!-- SEPARATOR -->

# validation_code
assert len(unique_customers) == 5, "Should be 5 unique users"
assert "CUST-1001" in unique_customers, "User CUST-1001 should be in the set"
assert isinstance(unique_customers, set), "Must use a set"