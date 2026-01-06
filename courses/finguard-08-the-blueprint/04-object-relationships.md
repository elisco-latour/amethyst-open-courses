---
id: "finguard_08_04"
title: "Composition"
type: "coding"
xp: 100
---

# Composition

Rarely does an object stand alone.
A `Car` **has an** `Engine`.
A `Bank` **has** `Customers`.
A `Customer` **has** `Accounts`.

This "Has-A" relationship is called **Composition**.

## Embedding Objects

We store instances of other classes inside attributes (often Lists/Dicts).

```python
class Customer:
    def __init__(self, name: str):
        self.name = name
        # The Customer "Has-Many" Accounts
        self.accounts: list[BankAccount] = []

    def open_account(self, initial_deposit: int):
        # We create the dependent object HERE
        new_acc = BankAccount(initial_deposit)
        self.accounts.append(new_acc)
```

## Traversing the Graph

We can access deep data by chaining dots.

```python
alice = Customer("Alice")
alice.open_account(500)

# Access: Customer -> List -> Account -> Balance
total = alice.accounts[0].balance
```

## Task

Create a `Portfolio` class.
1. `__init__`: Initializes empty `holdings` (list).
2. `add_holding(symbol: str, shares: int)`: Adds a dictionary (or object) to list.
3. `total_shares()`: Sums up all shares.

<!-- SEPARATOR -->

# seed_code
class Portfolio:
    def __init__(self):
        self.holdings = []

    def add_holding(self, symbol: str, shares: int):
        pass

    def total_shares(self) -> int:
        pass

# Integration
p = Portfolio()
p.add_holding("AAPL", 10)
p.add_holding("GOOG", 5)
print(f"Total: {p.total_shares()}")

<!-- SEPARATOR -->

# validation_code
# Test empty portfolio
p = Portfolio()
assert p.total_shares() == 0, "Empty portfolio should have 0 shares"

# Test adding holdings
p.add_holding("A", 10)
p.add_holding("B", 20)
assert p.total_shares() == 30, "Should sum all shares: 10 + 20 = 30"

# Test additional holdings
p.add_holding("C", 5)
assert p.total_shares() == 35, "Should include new holding: 10 + 20 + 5 = 35"

# Verify holdings are stored
assert len(p.holdings) == 3, "Should have 3 holdings"
