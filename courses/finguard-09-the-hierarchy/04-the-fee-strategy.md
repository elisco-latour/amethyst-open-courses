---
id: "finguard_09_04"
title: "The Fee Strategy"
type: "coding"
xp: 100
---

# The Fee Strategy

Different accounts have different fee structures. Let's model this with polymorphism.

## Fee Calculation: A Polymorphic Example

```python
from abc import ABC, abstractmethod
from decimal import Decimal

class FeeCalculator(ABC):
    @abstractmethod
    def calculate_monthly_fee(self, balance: Decimal) -> Decimal:
        pass

class BasicFeeCalculator(FeeCalculator):
    def calculate_monthly_fee(self, balance: Decimal) -> Decimal:
        return Decimal("5.00")  # Flat $5

class PremiumFeeCalculator(FeeCalculator):
    def calculate_monthly_fee(self, balance: Decimal) -> Decimal:
        if balance > Decimal("10000"):
            return Decimal("0.00")  # No fee for high balances
        return Decimal("15.00")

class PercentageFeeCalculator(FeeCalculator):
    def calculate_monthly_fee(self, balance: Decimal) -> Decimal:
        return balance * Decimal("0.001")  # 0.1% of balance
```

## Using the Strategy Pattern

```python
class Account:
    def __init__(self, balance: Decimal, fee_calculator: FeeCalculator):
        self.balance = balance
        self.fee_calculator = fee_calculator
    
    def apply_monthly_fee(self) -> Decimal:
        fee = self.fee_calculator.calculate_monthly_fee(self.balance)
        self.balance -= fee
        return fee
```

## The Analogy: The Restaurant Menu

- **Interface** = "Dish" (has price, description)
- **Implementations** = Burger, Salad, Pasta (each computes price differently)

The waiter doesn't care how the price is computed — just that every dish has one.

## The "Pro" Tip

> **Prefer composition over inheritance. Instead of `PremiumAccount(Account)`, use `Account(fee_calculator=PremiumFeeCalculator())`.**

## Task

Create fee calculators for different account types:
- `StandardFeeCalculator`: $12/month, waived if balance > $1,500
- `StudentFeeCalculator`: $0/month always
- `BusinessFeeCalculator`: $25/month + 0.05% of balance

<!-- SEPARATOR -->

# seed_code
from abc import ABC, abstractmethod
from decimal import Decimal

class FeeCalculator(ABC):
    @abstractmethod
    def calculate_monthly_fee(self, balance: Decimal) -> Decimal:
        """Calculate monthly account fee based on balance."""
        pass
    
    @abstractmethod
    def get_fee_description(self) -> str:
        """Return human-readable fee description."""
        pass


class StandardFeeCalculator(FeeCalculator):
    """$12/month, waived if balance > $1,500."""
    
    pass  # Replace with your implementation


class StudentFeeCalculator(FeeCalculator):
    """No fees for student accounts."""
    
    pass  # Replace with your implementation


class BusinessFeeCalculator(FeeCalculator):
    """$25/month + 0.05% of balance."""
    
    pass  # Replace with your implementation


# Test the calculators
print("=== Fee Calculator Test ===")

calculators: list[tuple[str, FeeCalculator, Decimal]] = [
    ("Low balance standard", StandardFeeCalculator(), Decimal("500.00")),
    ("High balance standard", StandardFeeCalculator(), Decimal("2000.00")),
    ("Student", StudentFeeCalculator(), Decimal("100.00")),
    ("Business", BusinessFeeCalculator(), Decimal("50000.00")),
]

for name, calc, balance in calculators:
    fee = calc.calculate_monthly_fee(balance)
    print(f"{name} (${balance}): ${fee} - {calc.get_fee_description()}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Test StandardFeeCalculator
std = StandardFeeCalculator()
assert std.calculate_monthly_fee(Decimal("500.00")) == Decimal("12.00"), "Standard fee is $12"
assert std.calculate_monthly_fee(Decimal("2000.00")) == Decimal("0.00"), "Fee waived for balance > $1500"
assert len(std.get_fee_description()) > 0, "Should have description"

# Test StudentFeeCalculator
student = StudentFeeCalculator()
assert student.calculate_monthly_fee(Decimal("100.00")) == Decimal("0.00"), "Student fee is $0"
assert student.calculate_monthly_fee(Decimal("10000.00")) == Decimal("0.00"), "Student fee always $0"

# Test BusinessFeeCalculator  
biz = BusinessFeeCalculator()
# $25 + 0.05% of 50000 = $25 + $25 = $50
fee = biz.calculate_monthly_fee(Decimal("50000.00"))
assert fee == Decimal("50.00"), f"Business fee should be $50, got ${fee}"
