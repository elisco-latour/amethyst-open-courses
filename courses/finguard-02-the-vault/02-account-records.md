---
id: "finguard_02_02"
title: "The Record: Dictionaries"
type: "coding"
xp: 100
---

# The Record: Dictionaries

A list is great for sequences, but terrible for **details**.

Look at this list: `["Alice Chen", "ACC-101", 5000, True]`

What does `5000` mean? A balance? A credit limit? An account number? You have to guess.

## Self-Describing Data

In FinGuard, we want data that **explains itself**. We use **dictionaries** — collections of labeled values:

```python
account = {
    "holder_name": "Alice Chen",
    "account_id": "ACC-101",
    "balance": 5000,
    "is_active": True
}
```

Now there's no guessing. `account["balance"]` is obviously the balance.

## Key-Value Pairs

A dictionary stores **key-value pairs**:
- **Key:** The label (like `"balance"`)
- **Value:** The data (like `5000`)

```python
account["balance"]      # Get the value for key "balance" → 5000
account["holder_name"]  # Get the value for key "holder_name" → "Alice Chen"
```

## Safe Access with `.get()`

What happens if you ask for a key that doesn't exist?

```python
account["email"]  # KeyError! Program crashes.
```

In real banking systems, data is sometimes incomplete. We use `.get()` for safe access:

```python
account.get("email")               # Returns None (no crash)
account.get("email", "N/A")        # Returns "N/A" as default
```

<div class="key-concept">
<h4>🔑 Key Concept: Defaults Over Crashes</h4>

Professional systems handle missing data gracefully. Use `.get()` with a default value rather than risking crashes from missing keys.
</div>

## Task

Create a complete account record dictionary with these details:
- Account ID: `"ACC-2002"`
- Holder name: `"Bob Johnson"`
- Balance: `12500.50`
- Account type: `"checking"`
- Is active: `True`

Then safely retrieve the balance and attempt to get a missing "email" field.

<!-- SEPARATOR -->

# seed_code
# Create the account record dictionary
account: dict = {
    # Add the 5 key-value pairs here:
    # account_id, holder_name, balance, account_type, is_active
    
}

# Safely retrieve the balance using .get()
retrieved_balance = 

# Try to get the email (doesn't exist) - use a default value of "Not Provided"
email = 

# Display the record
print(f"Account: {account['account_id']}")
print(f"Holder: {account['holder_name']}")
print(f"Balance: ${retrieved_balance:,.2f}")
print(f"Type: {account['account_type']}")
print(f"Active: {account['is_active']}")
print(f"Email: {email}")

<!-- SEPARATOR -->

# validation_code
assert account["account_id"] == "ACC-2002", "account_id should be 'ACC-2002'"
assert account["holder_name"] == "Bob Johnson", "holder_name should be 'Bob Johnson'"
assert account["balance"] == 12500.50, "balance should be 12500.50"
assert account["account_type"] == "checking", "account_type should be 'checking'"
assert account["is_active"] == True, "is_active should be True"
assert retrieved_balance == 12500.50, "Use .get('balance') to retrieve the balance"
assert email == "Not Provided", "Use .get('email', 'Not Provided') for missing keys"
assert isinstance(account, dict), "account must be a dictionary"
