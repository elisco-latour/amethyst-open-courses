---
id: "finguard_10_02"
title: "Open/Closed Principle"
type: "coding"
xp: 100
---

# Open/Closed Principle (OCP)

**Classes should be open for extension, but closed for modification.**

## The Problem: The Switch Statement

```python
# ❌ Every new transaction type requires modifying this function
def calculate_fee(transaction_type: str, amount: Decimal) -> Decimal:
    if transaction_type == "DEPOSIT":
        return Decimal("0")
    elif transaction_type == "WITHDRAWAL":
        return amount * Decimal("0.01")
    elif transaction_type == "TRANSFER":
        return Decimal("5.00")
    elif transaction_type == "WIRE":  # Added later
        return amount * Decimal("0.005")
    # ... keeps growing
```

Every new type = **modify existing code** = **risk of breaking things**.

## The Solution: Polymorphism

```python
from abc import ABC, abstractmethod
from decimal import Decimal

class FeeCalculator(ABC):
    @abstractmethod
    def calculate(self, amount: Decimal) -> Decimal:
        pass

class DepositFeeCalculator(FeeCalculator):
    def calculate(self, amount: Decimal) -> Decimal:
        return Decimal("0")

class WithdrawalFeeCalculator(FeeCalculator):
    def calculate(self, amount: Decimal) -> Decimal:
        return amount * Decimal("0.01")

# Add new types WITHOUT modifying existing code
class WireFeeCalculator(FeeCalculator):
    def calculate(self, amount: Decimal) -> Decimal:
        return amount * Decimal("0.005")
```

## The Analogy: The USB Port

A USB port is **closed** — you can't modify its internal circuitry.
But it's **open** — you can plug in keyboards, mice, cameras, phones.

New devices work without changing the port.

## The Registration Pattern

```python
class FeeEngine:
    def __init__(self):
        self.calculators: dict[str, FeeCalculator] = {}
    
    def register(self, txn_type: str, calculator: FeeCalculator):
        self.calculators[txn_type] = calculator
    
    def calculate_fee(self, txn_type: str, amount: Decimal) -> Decimal:
        calculator = self.calculators.get(txn_type)
        if not calculator:
            raise ValueError(f"Unknown transaction type: {txn_type}")
        return calculator.calculate(amount)

# Usage: extend without modifying FeeEngine
engine = FeeEngine()
engine.register("DEPOSIT", DepositFeeCalculator())
engine.register("WIRE", WireFeeCalculator())  # New type, no modification needed
```

## The "Pro" Tip

> **When you see `if/elif/elif...` based on type, consider replacing with polymorphism.**

## Task

Create an extensible alert severity calculator following OCP.

<!-- SEPARATOR -->

# seed_code
from abc import ABC, abstractmethod
from decimal import Decimal

class SeverityCalculator(ABC):
    """Base class for alert severity calculations."""
    
    @abstractmethod
    def calculate(self, transaction: dict) -> str:
        """Calculate severity: LOW, MEDIUM, HIGH, CRITICAL."""
        pass
    
    @abstractmethod
    def get_rule_name(self) -> str:
        pass


class AmountBasedSeverity(SeverityCalculator):
    """Severity based on transaction amount."""
    
    pass  # Replace with your implementation


class CountryBasedSeverity(SeverityCalculator):
    """Severity based on destination country."""
    
    pass  # Replace with your implementation


class TimeBasedSeverity(SeverityCalculator):
    """Severity based on transaction time (hour)."""
    
    pass  # Replace with your implementation


class SeverityEngine:
    """Engine that runs multiple severity calculators and returns highest."""
    
    def __init__(self):
        self.calculators: list[SeverityCalculator] = []
    
    def register(self, calculator: SeverityCalculator) -> None:
        pass  # Replace with your implementation
    
    def calculate_severity(self, transaction: dict) -> str:
        """Run all calculators and return the highest severity."""
        pass  # Replace with your implementation


# Test OCP design
print("=== Open/Closed Principle Demo ===")

engine = SeverityEngine()
engine.register(AmountBasedSeverity())
engine.register(CountryBasedSeverity())
engine.register(TimeBasedSeverity())

test_transactions = [
    {"amount": Decimal("500.00"), "country": "US", "hour": 10},
    {"amount": Decimal("50000.00"), "country": "US", "hour": 10},
    {"amount": Decimal("1000.00"), "country": "NK", "hour": 10},
    {"amount": Decimal("100.00"), "country": "US", "hour": 3},
]

for txn in test_transactions:
    severity = engine.calculate_severity(txn)
    print(f"${txn['amount']} to {txn['country']} at {txn['hour']}:00 → {severity}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

SEVERITY_ORDER = {"LOW": 0, "MEDIUM": 1, "HIGH": 2, "CRITICAL": 3}

# Test individual calculators
amt_calc = AmountBasedSeverity()
assert amt_calc.calculate({"amount": Decimal("500"), "country": "US", "hour": 10}) == "LOW"
assert amt_calc.calculate({"amount": Decimal("50000"), "country": "US", "hour": 10}) in ["HIGH", "CRITICAL"]

country_calc = CountryBasedSeverity()
assert country_calc.calculate({"amount": Decimal("100"), "country": "NK", "hour": 10}) in ["HIGH", "CRITICAL"]

time_calc = TimeBasedSeverity()
assert time_calc.calculate({"amount": Decimal("100"), "country": "US", "hour": 3}) in ["MEDIUM", "HIGH"]

# Test engine combines correctly
engine = SeverityEngine()
engine.register(AmountBasedSeverity())
engine.register(CountryBasedSeverity())

# High amount + suspicious country = should return the highest
result = engine.calculate_severity({"amount": Decimal("50000"), "country": "NK", "hour": 10})
assert result in ["HIGH", "CRITICAL"], "Should return highest severity"

# Normal transaction
result = engine.calculate_severity({"amount": Decimal("100"), "country": "US", "hour": 10})
assert result == "LOW", "Normal transaction should be LOW"
