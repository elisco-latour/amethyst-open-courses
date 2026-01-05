---
id: "finguard_08_04"
title: "Object Relationships"
type: "coding"
xp: 100
---

# Object Relationships

A Customer **has** Accounts. An Account **has** Transactions. Objects relate to each other.

## Composition: "Has-A" Relationship

```python
from decimal import Decimal

class Customer:
    def __init__(self, customer_id: str, name: str):
        self.customer_id: str = customer_id
        self.name: str = name
        self.accounts: list[BankAccount] = []  # Customer HAS accounts
    
    def open_account(self, account_id: str, initial_deposit: Decimal) -> BankAccount:
        """Open a new account for this customer."""
        account = BankAccount(account_id, initial_deposit)
        self.accounts.append(account)
        return account
    
    def get_total_balance(self) -> Decimal:
        """Sum balances across all accounts."""
        return sum(acc.balance for acc in self.accounts)
```

## Using Relationships

```python
# Create customer
alice = Customer("CUST-001", "Alice")

# Open accounts (composition)
checking = alice.open_account("CHK-001", Decimal("1000.00"))
savings = alice.open_account("SAV-001", Decimal("5000.00"))

# Access through the relationship
print(f"Total balance: ${alice.get_total_balance()}")  # $6000.00

# Direct operations on accounts
checking.deposit(Decimal("500.00"))
```

## The Analogy: The Organization Chart

- **Company** has **Departments**
- **Department** has **Employees**

Each level contains the next. This is composition.

## Reference vs Containment

```python
# Containment: Customer owns accounts
class Customer:
    def __init__(self):
        self.accounts: list[BankAccount] = []  # Accounts belong to customer

# Reference: Transaction references an account
class Transaction:
    def __init__(self, account: BankAccount):
        self.account: BankAccount = account  # Transaction points to account
```

## The "Pro" Tip

> **Think about ownership. Who creates it? Who destroys it? That's the owner.**

## Task

Create a `Portfolio` class that holds multiple `Investment` objects and can calculate total value.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

class Investment:
    """Represents a single investment holding."""
    
    def __init__(self, symbol: str, shares: int, price_per_share: Decimal):
        self.symbol: str = symbol
        self.shares: int = shares
        self.price_per_share: Decimal = price_per_share
    
    def get_value(self) -> Decimal:
        return self.price_per_share * Decimal(str(self.shares))


class Portfolio:
    """Represents an investment portfolio."""
    
    def __init__(self, portfolio_id: str, owner_name: str):
        pass  # Replace with your implementation
    
    def add_investment(self, symbol: str, shares: int, price: Decimal) -> Investment:
        """Add a new investment to the portfolio."""
        pass  # Replace with your implementation
    
    def get_total_value(self) -> Decimal:
        """Calculate total portfolio value."""
        pass  # Replace with your implementation
    
    def get_holdings_report(self) -> str:
        """Generate a report of all holdings."""
        pass  # Replace with your implementation


# Test the portfolio
portfolio = Portfolio("PORT-001", "Alice Smith")

portfolio.add_investment("AAPL", 10, Decimal("175.00"))
portfolio.add_investment("GOOGL", 5, Decimal("140.00"))
portfolio.add_investment("MSFT", 15, Decimal("380.00"))

print("=== Portfolio Report ===")
print(portfolio.get_holdings_report())
print(f"\nTotal Value: ${portfolio.get_total_value():,.2f}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Test portfolio creation
p = Portfolio("P1", "Test")
assert p.portfolio_id == "P1", "Should set portfolio_id"
assert p.owner_name == "Test", "Should set owner_name"

# Test adding investments
inv = p.add_investment("TEST", 10, Decimal("100.00"))
assert inv.symbol == "TEST", "Should return the created investment"
assert len(p.investments) == 1, "Should add to investments list"

# Test total value calculation
p.add_investment("ABC", 5, Decimal("50.00"))
# 10 * 100 + 5 * 50 = 1000 + 250 = 1250
total = p.get_total_value()
assert total == Decimal("1250.00"), f"Total should be 1250.00, got {total}"

# Test report
report = p.get_holdings_report()
assert "TEST" in report, "Report should include stock symbols"
