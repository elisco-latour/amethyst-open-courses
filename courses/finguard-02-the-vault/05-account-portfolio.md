---
id: "finguard_02_05"
title: "The Data Model: Nesting"
type: "coding"
xp: 100
---

# The Data Model: Nesting

Real world banking entities are complicated.
A **Customer** has many **Accounts**. An **Account** has many **Transactions**.

We model this Hierarchy by nesting Lists inside Dictionaries.

## Domain Modeling

This is called "Structuring the Domain".

```python
customer_model = {
    "name": "Alice",
    "accounts": [
        {"type": "checking", "balance": 100},
        {"type": "savings", "balance": 5000}
    ]
}
```

To get Alice's savings balance, we drill down:
`customer_model["accounts"][1]["balance"]`

## Complexity Warning

In **Architecture Patterns with Python**, we practice "The Boy Scout Rule".
Do not create "Big Balls of Mud" (deeply nested mess).
Keep your structure clean.

## Task

Create a nested model for Customer "CUST-5001" (Elena).
She has 2 accounts. Iterate through them to sum the total net worth.

<!-- SEPARATOR -->

# seed_code
# The Data Model
portfolio = {
    "customer_id": "CUST-5001",
    "name": "Elena Rodriguez",
    "accounts": [
        {
            "id": "ACC-5001",
            "type": "checking",
            "balance": 3500.00
        },
        {
            "id": "ACC-5002",
            "type": "savings",
            "balance": 15000.00
        }
    ]
}

# Calculate Total Net Worth programmatically
# We iterate through the list inside the dict.
total_net_worth = 0
for acc in portfolio["accounts"]:
    total_net_worth += acc["balance"]

print(f"Customer: {portfolio['name']}")
print(f"Net Worth: ${total_net_worth:,.2f}")

<!-- SEPARATOR -->

# validation_code
assert portfolio["customer_id"] == "CUST-5001", "ID mismatch"
assert total_net_worth == 18500.00, "Math mismatch"
