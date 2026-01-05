---
id: "finguard_02_04"
title: "Unique Customer IDs"
type: "coding"
xp: 100
---

# Unique Customer IDs

When processing thousands of transactions, you often need to answer: "How many **unique** customers transacted today?"

A list might have duplicates. You need a collection that **automatically removes duplicates**.

## The Analogy: The Guest List

At a bank's VIP event, each customer should be on the list **once**, no matter how many times they RSVP'd.

## Sets: Unique Collections

A **set** is an unordered collection of unique elements:

```python
# Creating a set
customer_ids: set[str] = {"CUST-001", "CUST-002", "CUST-003"}

# Duplicates are automatically ignored
customer_ids.add("CUST-001")  # No effect — already exists
customer_ids.add("CUST-004")  # Added

# Check membership (very fast!)
if "CUST-001" in customer_ids:
    print("Customer found")

# Get count of unique items
unique_count: int = len(customer_ids)  # 4
```

## Sets from Lists

Convert a list with duplicates to a set to get unique values:

```python
# Transaction list with repeated customer IDs
all_transactions: list[str] = ["CUST-001", "CUST-002", "CUST-001", "CUST-003", "CUST-002"]

# Convert to set — duplicates removed
unique_customers: set[str] = set(all_transactions)
# {"CUST-001", "CUST-002", "CUST-003"}
```

## Set Operations

Sets support mathematical operations:

```python
vip_customers: set[str] = {"CUST-001", "CUST-002"}
active_today: set[str] = {"CUST-002", "CUST-003"}

# Union — customers who are VIP OR active today
vip_customers | active_today  # {"CUST-001", "CUST-002", "CUST-003"}

# Intersection — customers who are VIP AND active today
vip_customers & active_today  # {"CUST-002"}

# Difference — VIP customers who were NOT active today
vip_customers - active_today  # {"CUST-001"}
```

## The "Pro" Tip

> **Use sets when you need uniqueness or fast membership checking.**

```python
# ❌ Slow — checking list membership is O(n)
if customer_id in customer_list:  # Checks every item

# ✅ Fast — checking set membership is O(1)
if customer_id in customer_set:   # Instant lookup
```

## Task

Given a list of transaction customer IDs (with duplicates), find the unique customers.

Then find which customers are both VIP AND made a transaction today.

<!-- SEPARATOR -->

# seed_code
# Today's transactions (with duplicates)
transaction_customers: list[str] = [
    "CUST-1001", "CUST-1002", "CUST-1001", "CUST-1003",
    "CUST-1002", "CUST-1004", "CUST-1001", "CUST-1005"
]

# VIP customer list
vip_customers: set[str] = {"CUST-1001", "CUST-1003", "CUST-1006", "CUST-1007"}

# Get unique customers from today's transactions


# Find VIP customers who transacted today (intersection)


# Print results
print(f"Total transactions: {len(transaction_customers)}")
print(f"Unique customers: {len(unique_customers)}")
print(f"Unique customer IDs: {unique_customers}")
print(f"VIP customers active today: {vip_active_today}")

<!-- SEPARATOR -->

# validation_code
assert unique_customers == {"CUST-1001", "CUST-1002", "CUST-1003", "CUST-1004", "CUST-1005"}, "unique_customers should have 5 unique IDs"
assert vip_active_today == {"CUST-1001", "CUST-1003"}, "vip_active_today should be CUST-1001 and CUST-1003"
assert isinstance(unique_customers, set), "unique_customers must be a set"
assert isinstance(vip_active_today, set), "vip_active_today must be a set"
