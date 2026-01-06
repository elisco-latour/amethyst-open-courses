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
# Assume previous classes exist or mock them
class Account:
    def __init__(self, amoun: int):
        self.balance = amoun

class Customer:
    def __init__(self, name: str):
        self.name = name
        self.accounts = []
    
    def open_account(self, amount: int):
        self.accounts.append(Account(amount))

class Bank:
    def __init__(self):
        self.customers = {}

    def onboard(self, name: str) -> Customer:
        # Create, store, return
        pass

# Integration
bank = Bank()
alice = bank.onboard("Alice")
alice.open_account(1000)
print(f"Alice Balance: {alice.accounts[0].balance}")

<!-- SEPARATOR -->

# validation_code
bank = Bank()
bob = bank.onboard("Bob")
assert isinstance(bob, Customer)
assert "Bob" in bank.customers
bob.open_account(500)
assert bob.accounts[0].balance == 500
print("Validation passed!")
                "HIGH"
            )
            self.alerts.append(alert)
            return alert
        return None
```

## The Bridge: Data Modeling

In data engineering, you'll design **data models** — the structure that represents your domain. Classes are one way to express these models.

## The "Pro" Tip

> **Start with the domain. What are the real-world concepts? Model those first. The code structure follows the domain structure.**

## Task

Build a mini FinGuard system with:
- `Customer` class (has accounts)
- `Account` class (has transactions, balance)
- `TransactionProcessor` that processes transactions and returns alerts for amounts > $10,000

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal
from datetime import datetime

class Account:
    def __init__(self, account_id: str, owner_name: str):
        self.account_id: str = account_id
        self.owner_name: str = owner_name
        self.balance: Decimal = Decimal("0.00")
        self.transactions: list[dict] = []
    
    def deposit(self, amount: Decimal, txn_id: str) -> dict:
        self.balance += amount
        txn = {"id": txn_id, "type": "DEPOSIT", "amount": amount, "timestamp": datetime.now()}
        self.transactions.append(txn)
        return txn
    
    def withdraw(self, amount: Decimal, txn_id: str) -> dict | None:
        if amount > self.balance:
            return None
        self.balance -= amount
        txn = {"id": txn_id, "type": "WITHDRAWAL", "amount": amount, "timestamp": datetime.now()}
        self.transactions.append(txn)
        return txn


class Customer:
    def __init__(self, customer_id: str, name: str):
        pass  # Replace with your implementation
    
    def open_account(self, account_id: str) -> Account:
        pass  # Replace with your implementation
    
    def get_total_balance(self) -> Decimal:
        pass  # Replace with your implementation


class TransactionProcessor:
    def __init__(self):
        self.processed: list[dict] = []
        self.alerts: list[dict] = []
    
    def process(self, account: Account, txn_type: str, amount: Decimal) -> dict:
        """Process a transaction. Returns alert dict if flagged."""
        pass  # Replace with your implementation


# Test the system
print("=== FinGuard Mini System ===")

# Create customer and accounts
customer = Customer("CUST-001", "Alice")
checking = customer.open_account("CHK-001")
savings = customer.open_account("SAV-001")

# Process transactions
processor = TransactionProcessor()

processor.process(checking, "DEPOSIT", Decimal("5000.00"))
processor.process(savings, "DEPOSIT", Decimal("15000.00"))
processor.process(checking, "WITHDRAWAL", Decimal("1000.00"))

print(f"Customer: {customer.name}")
print(f"Total Balance: ${customer.get_total_balance():,.2f}")
print(f"Transactions Processed: {len(processor.processed)}")
print(f"Fraud Alerts: {len(processor.alerts)}")

for alert in processor.alerts:
    print(f"  ⚠️ Alert: {alert['reason']}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Test Customer
c = Customer("C1", "Test")
assert c.customer_id == "C1", "Should set customer_id"
assert c.name == "Test", "Should set name"
assert len(c.accounts) == 0, "Should start with empty accounts"

# Test opening accounts
acc = c.open_account("A1")
assert len(c.accounts) == 1, "Should add account"
assert acc.owner_name == "Test", "Account owner should be customer name"

# Test balance calculation
acc.deposit(Decimal("1000.00"), "T1")
assert c.get_total_balance() == Decimal("1000.00"), "Should calculate total balance"

# Test processor alerts
processor = TransactionProcessor()
processor.process(acc, "DEPOSIT", Decimal("15000.00"))
assert len(processor.alerts) == 1, "Should create alert for >$10,000"
assert len(processor.processed) == 1, "Should track processed transactions"
