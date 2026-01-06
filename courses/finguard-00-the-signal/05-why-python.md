---
id: "finguard_00_05"
title: "Why Python: Your First Tool"
type: "coding"
xp: 100
---

# Why Python?

You're training to build financial systems. Why are we using Python instead of another language?

## The Modern Bank is a City

A real bank isn't one program — it's many systems working together:

- **Trading systems** use C++ (they need extreme speed)
- **Mobile apps** use JavaScript (they run in browsers)
- **Data analysis and automation** use Python

Python is the **connector**. It can talk to databases, read files, send alerts, run calculations, and glue all the other systems together.

This is why Python dominates data engineering: **not because it's the fastest, but because it works with everything.**

## Your First Tool: The Variable

A **variable** is a labeled container for storing data. Think of it as a safety deposit box:
- The **name** is the label on the box
- The **value** is what's inside

```python
daily_limit = 5000
```

Here, `daily_limit` is the name, and `5000` is the value stored inside.

## Why Naming Matters

Look at these two lines of code:

```python
# Amateur code
x = 5000

# Professional code
daily_transfer_limit_usd = 5000
```

Both store the same number. But six months from now, when you (or a teammate) read this code:
- `x` tells you **nothing**. What is x? Why 5000?
- `daily_transfer_limit_usd` tells you **everything**. It's a limit, for transfers, per day, in US dollars.

<div class="key-concept">
<h4>🔑 Key Concept: Code is Communication</h4>

You write code once, but it gets read dozens of times — by you, by teammates, by future maintainers. A good variable name **documents your intent** without needing comments.

In FinGuard, we reject "magic numbers" like `x = 5000`. Every value gets a meaningful name.
</div>

## Task

You're setting up the configuration for a new FinGuard account. Define the following variables with **clear, professional names**:

1. A variable storing the maximum amount (in USD) a customer can withdraw per day: `10000`
2. A variable storing the minimum balance required to avoid fees: `500`  
3. A variable storing the customer's account number: `"ACC-78234"`

Use descriptive names that explain what each value represents. Follow Python naming conventions (lowercase with underscores, like `my_variable_name`).

<!-- SEPARATOR -->

# seed_code
# FinGuard Account Configuration
# Define variables with clear, professional names

# Maximum daily withdrawal (in USD): 10000


# Minimum balance to avoid fees (in USD): 500


# Customer account number: "ACC-78234"


# Display the configuration
print("=== Account Configuration ===")
# Uncomment and update these lines after defining your variables:
# print(f"Max Daily Withdrawal: ${your_variable_name}")
# print(f"Minimum Balance: ${your_variable_name}")
# print(f"Account Number: {your_variable_name}")

<!-- SEPARATOR -->

# validation_code
# Check that variables exist and have correct values
import re

# Get all variable names in the local scope that match our expected values
user_vars = {k: v for k, v in locals().items() if not k.startswith('_')}

# Check for the withdrawal limit variable
withdrawal_vars = [k for k, v in user_vars.items() if v == 10000]
assert len(withdrawal_vars) > 0, "Create a variable with value 10000 for the daily withdrawal limit"
assert any('withdraw' in k.lower() or 'limit' in k.lower() or 'max' in k.lower() for k in withdrawal_vars), "Name your withdrawal limit variable descriptively (include words like 'withdraw', 'limit', or 'max')"

# Check for the minimum balance variable  
balance_vars = [k for k, v in user_vars.items() if v == 500]
assert len(balance_vars) > 0, "Create a variable with value 500 for the minimum balance"
assert any('balance' in k.lower() or 'min' in k.lower() or 'fee' in k.lower() for k in balance_vars), "Name your minimum balance variable descriptively (include words like 'balance', 'min', or 'fee')"

# Check for the account number variable
account_vars = [k for k, v in user_vars.items() if v == "ACC-78234"]
assert len(account_vars) > 0, "Create a variable with value 'ACC-78234' for the account number"
assert any('account' in k.lower() or 'acc' in k.lower() or 'number' in k.lower() for k in account_vars), "Name your account number variable descriptively (include 'account' or similar)"

