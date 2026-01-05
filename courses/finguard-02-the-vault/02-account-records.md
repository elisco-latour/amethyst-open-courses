---
id: "finguard_02_02"
title: "Account Records"
type: "coding"
xp: 100
---

# Account Records

Lists are great for ordered sequences. But what if you want to look something up by **name**, not by position?

## The Analogy: The Account Database

When a teller looks up your account, they don't say "show me the 47,382nd account." They say "show me account **ACC-1001**."

This is a **key-value lookup** — the foundation of every database.

## Dictionaries: Key-Value Storage

A **dictionary** (`dict`) stores data as key-value pairs:

```python
# Creating a dictionary
account: dict[str, any] = {
    "account_id": "ACC-1001",
    "holder_name": "Alice Smith",
    "balance": 5000.00,
    "is_active": True
}

# Access by key
name: str = account["holder_name"]  # "Alice Smith"

# Add or update a key
account["email"] = "alice@bank.com"

# Check if key exists
if "balance" in account:
    print("Balance found")
```

## The "Pro" Tip

> **Use `.get()` for safe access — avoids crashes on missing keys.**

```python
# ❌ Dangerous — crashes if key doesn't exist
email = account["email"]  # KeyError if no email!

# ✅ Safe — returns None (or a default) if missing
email = account.get("email")  # None if no email
email = account.get("email", "no-email@bank.com")  # Default value
```

## Why Not Just Use Lists?

```python
# With a list — what does index 2 mean?
account_list = ["ACC-1001", "Alice Smith", 5000.00, True]
balance = account_list[2]  # Unclear!

# With a dict — crystal clear
account_dict = {"account_id": "ACC-1001", "holder_name": "Alice Smith", "balance": 5000.00}
balance = account_dict["balance"]  # Obvious!
```

Dictionaries are **self-documenting**.

## Task

Create an account record dictionary for a new customer with these fields:
- `account_id`: `"ACC-2002"`
- `holder_name`: `"Bob Johnson"`
- `balance`: `12500.50` (use a float for now)
- `account_type`: `"checking"`
- `is_active`: `True`

Then retrieve the balance using `.get()`.

<!-- SEPARATOR -->

# seed_code
# Create the account record dictionary


# Retrieve the balance safely using .get()


# Print the account summary
print(f"=== Account Record ===")
print(f"ID: {account['account_id']}")
print(f"Holder: {account['holder_name']}")
print(f"Balance: ${retrieved_balance:,.2f}")
print(f"Type: {account['account_type']}")
print(f"Active: {account['is_active']}")

<!-- SEPARATOR -->

# validation_code
assert account["account_id"] == "ACC-2002", "account_id should be 'ACC-2002'"
assert account["holder_name"] == "Bob Johnson", "holder_name should be 'Bob Johnson'"
assert account["balance"] == 12500.50, "balance should be 12500.50"
assert account["account_type"] == "checking", "account_type should be 'checking'"
assert account["is_active"] == True, "is_active should be True"
assert retrieved_balance == 12500.50, "retrieved_balance should be 12500.50"
assert isinstance(account, dict), "account must be a dictionary"
