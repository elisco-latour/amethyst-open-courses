---
id: "finguard_02_05"
title: "Account Portfolio"
type: "coding"
xp: 100
---

# Account Portfolio

Real data is **nested**. A customer has multiple accounts. Each account has multiple transactions. Each transaction has multiple fields.

## The Analogy: The Bank's Complete Records

A bank's customer record contains:
- Customer info (name, address, ID)
- List of accounts (checking, savings, investment)
- Each account has a list of transactions
- Each transaction has details (amount, date, type)

This is **hierarchical data** — data within data within data.

## Nesting Collections

Python collections can contain other collections:

```python
# A customer with multiple accounts
customer: dict[str, any] = {
    "customer_id": "CUST-1001",
    "name": "Alice Smith",
    "accounts": [
        {
            "account_id": "ACC-1001",
            "type": "checking",
            "balance": 5000.00
        },
        {
            "account_id": "ACC-1002",
            "type": "savings",
            "balance": 25000.00
        }
    ]
}

# Access nested data
first_account = customer["accounts"][0]
first_balance = customer["accounts"][0]["balance"]  # 5000.00
```

## Navigating Nested Data

```python
# Get all account balances
for account in customer["accounts"]:
    print(f"{account['type']}: ${account['balance']}")

# Total portfolio value
total: float = sum(acc["balance"] for acc in customer["accounts"])
```

## The "Pro" Tip

> **Deep nesting gets messy. If you're going 3+ levels deep, consider restructuring your data.**

```python
# ❌ Too deep — hard to read and error-prone
value = data["customers"][0]["accounts"][1]["transactions"][3]["amount"]

# ✅ Better — extract intermediate values
customer = data["customers"][0]
account = customer["accounts"][1]
transaction = account["transactions"][3]
value = transaction["amount"]
```

## Task

Build a complete customer portfolio with two accounts. Each account should have a list of recent transactions.

Structure:
- Customer: `"CUST-5001"`, `"Elena Rodriguez"`
- Account 1: `"ACC-5001"`, `"checking"`, balance `3500.00`, transactions: two entries
- Account 2: `"ACC-5002"`, `"savings"`, balance `15000.00`, transactions: one entry

Then calculate the total portfolio balance.

<!-- SEPARATOR -->

# seed_code
# Build the customer portfolio
customer_portfolio: dict = {
    "customer_id": "CUST-5001",
    "name": "Elena Rodriguez",
    "accounts": [
        {
            "account_id": "ACC-5001",
            "type": "checking",
            "balance": 3500.00,
            "transactions": [
                {"txn_id": "TXN-001", "amount": -150.00, "description": "ATM Withdrawal"},
                {"txn_id": "TXN-002", "amount": 2000.00, "description": "Salary Deposit"}
            ]
        },
        {
            "account_id": "ACC-5002",
            "type": "savings",
            "balance": 15000.00,
            "transactions": [
                {"txn_id": "TXN-003", "amount": 500.00, "description": "Monthly Transfer"}
            ]
        }
    ]
}

# Calculate total portfolio balance


# Count total transactions across all accounts


# Print portfolio summary
print(f"=== Customer Portfolio ===")
print(f"Customer: {customer_portfolio['name']} ({customer_portfolio['customer_id']})")
print(f"")
for account in customer_portfolio["accounts"]:
    print(f"  {account['type'].upper()} ({account['account_id']}): ${account['balance']:,.2f}")
    print(f"    Recent transactions: {len(account['transactions'])}")
print(f"")
print(f"Total Portfolio Value: ${total_balance:,.2f}")
print(f"Total Transactions: {total_transactions}")

<!-- SEPARATOR -->

# validation_code
assert customer_portfolio["customer_id"] == "CUST-5001", "customer_id should be 'CUST-5001'"
assert customer_portfolio["name"] == "Elena Rodriguez", "name should be 'Elena Rodriguez'"
assert len(customer_portfolio["accounts"]) == 2, "Should have 2 accounts"
assert customer_portfolio["accounts"][0]["balance"] == 3500.00, "Checking balance should be 3500.00"
assert customer_portfolio["accounts"][1]["balance"] == 15000.00, "Savings balance should be 15000.00"
assert total_balance == 18500.00, "total_balance should be 18500.00"
assert total_transactions == 3, "total_transactions should be 3"
