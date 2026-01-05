---
id: "finguard_02_02"
title: "The Record: Dictionaries"
type: "coding"
xp: 100
---

# The Record: Dictionaries

A List is good for sequences. But a List is bad for **Details**.
If I give you `["Alice", "ACC-101", 500]`, you have to guess what "500" means. Is it a balance? A limit? A credit score?

## Explicit Data Modeling

In FinGuard, we want our data to explain itself.
We use **Dictionaries** (`dict`).

```python
# Self-Describing Data
account = {
    "name": "Alice", 
    "id": "ACC-101", 
    "balance_usd": 500
}
```

Now, `account["balance_usd"]` is unambiguous. We call this a **Key-Value Pair**.

## The Safety of `.get()`

In a real bank, data is sometimes messy. A field might be missing.
If you try `account["email"]` and it's not there, Python crashes (KeyError).
Engineers usage `.get("email")` which returns `None` instead of crashing.

> **Engineering Principle:** Defaults over Crashes.

## Task

Model a Customer Record. Use explicit Keys.

<!-- SEPARATOR -->

# seed_code
# A complete Account Record
account: dict = {
    "account_id": "ACC-2002",
    "holder_name": "Bob Johnson",
    "balance": 12500.50,
    "status": "Active"
}

# 1. Retrieve the Balance safely
balance = account.get("balance")

# 2. Try to retrieve a missing field (email) without crashing
email = account.get("email", "Not Provided")

print(f"User: {account['holder_name']}")
print(f"Balance: ${balance}")
print(f"Email: {email}")

<!-- SEPARATOR -->

# validation_code
assert account["account_id"] == "ACC-2002", "account_id should be 'ACC-2002'"
assert balance == 12500.50, "balance retrieval failed"
assert email == "Not Provided", "default value for email failed"assert account["holder_name"] == "Bob Johnson", "holder_name should be 'Bob Johnson'"
assert account["balance"] == 12500.50, "balance should be 12500.50"
assert account["account_type"] == "checking", "account_type should be 'checking'"
assert account["is_active"] == True, "is_active should be True"
assert retrieved_balance == 12500.50, "retrieved_balance should be 12500.50"
assert isinstance(account, dict), "account must be a dictionary"
