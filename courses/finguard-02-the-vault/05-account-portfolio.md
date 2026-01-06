---
id: "finguard_02_05"
title: "The Portfolio: Nested Data"
type: "coding"
xp: 100
---

# The Portfolio: Nested Data

Real banking data is hierarchical. A **Customer** has multiple **Accounts**. Each **Account** has a balance, type, and ID.

How do we represent this structure? By **nesting** collections inside each other.

## Lists Inside Dictionaries

We can put a list inside a dictionary:

```python
customer = {
    "name": "Alice Chen",
    "account_ids": ["ACC-101", "ACC-102", "ACC-103"]
}
```

To get Alice's second account:
```python
customer["account_ids"][1]  # "ACC-102"
```

## Dictionaries Inside Lists

We can also have a list of dictionaries:

```python
accounts = [
    {"id": "ACC-101", "balance": 1000},
    {"id": "ACC-102", "balance": 5000}
]
```

To get the balance of the first account:
```python
accounts[0]["balance"]  # 1000
```

## The Full Portfolio

Combining both patterns, we can model a complete customer portfolio:

```python
portfolio = {
    "customer_id": "CUST-001",
    "name": "Alice Chen",
    "accounts": [
        {"id": "ACC-101", "type": "checking", "balance": 1500.00},
        {"id": "ACC-102", "type": "savings", "balance": 8000.00}
    ]
}
```

To navigate this structure:
- `portfolio["name"]` → `"Alice Chen"`
- `portfolio["accounts"]` → the list of account dictionaries
- `portfolio["accounts"][0]` → the first account dictionary
- `portfolio["accounts"][0]["balance"]` → `1500.00`

<div class="key-concept">
<h4>🔑 Key Concept: Drilling Down</h4>

Reading nested data is like following a path: start at the outer container, then step into each inner layer using `["key"]` for dicts or `[index]` for lists.
</div>

## Task

Given the portfolio structure below, extract specific values by drilling down through the nested data:

1. `customer_name` — the customer's name
2. `checking_balance` — the balance of the checking account (first account)
3. `savings_balance` — the balance of the savings account (second account)
4. `total_balance` — the sum of both balances

<!-- SEPARATOR -->

# seed_code
# A customer's complete portfolio
portfolio: dict = {
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

# Extract the customer's name
customer_name = 

# Extract the checking account balance (first account, index 0)
checking_balance = 

# Extract the savings account balance (second account, index 1)
savings_balance = 

# Calculate total balance
total_balance = 

# Display the portfolio summary
print(f"Customer: {customer_name}")
print(f"Checking: ${checking_balance:,.2f}")
print(f"Savings: ${savings_balance:,.2f}")
print(f"Total Net Worth: ${total_balance:,.2f}")

<!-- SEPARATOR -->

# validation_code
assert customer_name == "Elena Rodriguez", "customer_name should be 'Elena Rodriguez' — use portfolio['name']"
assert checking_balance == 3500.00, "checking_balance should be 3500.00 — use portfolio['accounts'][0]['balance']"
assert savings_balance == 15000.00, "savings_balance should be 15000.00 — use portfolio['accounts'][1]['balance']"
assert total_balance == 18500.00, "total_balance should be checking_balance + savings_balance"
assert portfolio["customer_id"] == "CUST-5001", "Don't modify the portfolio structure"
