---
id: "finguard_08_05"
title: "System Architecture"
type: "coding"
xp: 100
---

# System Architecture

We have built the bricks:
1. `Transaction` (Unit of work)
2. `Account` (Container of value)
3. `Customer` (Owner of accounts)

Now we build the **System**.

## The Hierarchy

```
Bank (System)
 └── Customer (Entity)
      └── Account (Entity)
           └── Transaction (Entity)
```

## The Single Source of Truth

The `Bank` object holds the registry of all customers. This is the root of our object graph.

```python
class Bank:
    def __init__(self):
        self.customers: dict[str, Customer] = {}

    def onboard_customer(self, name: str) -> Customer:
        cust = Customer(name)
        self.customers[name] = cust
        return cust
```

## Task

Assemble the system.
1.  Initialize `Bank`.
2.  Onboard customer "Alice".
3.  Alice opens an account with $1000.
4.  Print Alice's total balance.

This is the Capstone of Level 1 (The Foundation).

<!-- SEPARATOR -->

# seed_code
# Building blocks from previous lessons
class Account:
    def __init__(self, amount: int):
        self.balance = amount

class Customer:
    def __init__(self, name: str):
        self.name = name
        self.accounts: list[Account] = []
    
    def open_account(self, amount: int):
        self.accounts.append(Account(amount))

class Bank:
    def __init__(self):
        self.customers: dict[str, Customer] = {}

    def onboard(self, name: str) -> Customer:
        # Create customer, store in registry, return
        pass

# Integration
bank = Bank()
alice = bank.onboard("Alice")
alice.open_account(1000)
print(f"Alice Balance: {alice.accounts[0].balance}")

<!-- SEPARATOR -->

# validation_code
bank = Bank()

# Test onboarding
bob = bank.onboard("Bob")
assert isinstance(bob, Customer), "onboard should return a Customer"
assert "Bob" in bank.customers, "Customer should be in bank's registry"
assert bob.name == "Bob", "Customer should have correct name"

# Test account operations
bob.open_account(500)
assert len(bob.accounts) == 1, "Customer should have one account"
assert bob.accounts[0].balance == 500, "Account should have correct balance"

# Test multiple customers
carol = bank.onboard("Carol")
assert len(bank.customers) == 2, "Bank should have two customers"
