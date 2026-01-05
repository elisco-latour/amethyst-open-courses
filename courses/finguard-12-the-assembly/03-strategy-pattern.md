---
id: "finguard_12_03"
title: "Strategy Pattern"
type: "coding"
xp: 100
---

# Strategy Pattern

Swap **algorithms at runtime** without changing the code that uses them.

## The Problem

```python
# ❌ Growing if/else for every algorithm variation
def calculate_fraud_score(txn: Transaction, method: str) -> float:
    if method == "simple":
        return _simple_score(txn)
    elif method == "weighted":
        return _weighted_score(txn)
    elif method == "ml":
        return _ml_score(txn)
    # Adding new methods requires modifying this function!
```

## The Solution: Strategy Pattern

```python
# ✅ Each algorithm is a separate class
class FraudStrategy(ABC):
    @abstractmethod
    def calculate_score(self, txn: Transaction) -> float:
        pass

class SimpleFraudStrategy(FraudStrategy):
    def calculate_score(self, txn: Transaction) -> float:
        return _simple_score(txn)

class WeightedFraudStrategy(FraudStrategy):
    def calculate_score(self, txn: Transaction) -> float:
        return _weighted_score(txn)

# Context uses any strategy
class FraudDetector:
    def __init__(self, strategy: FraudStrategy):
        self.strategy = strategy
    
    def detect(self, txn: Transaction) -> bool:
        score = self.strategy.calculate_score(txn)
        return score > 0.7

# Swap strategies at runtime
detector = FraudDetector(SimpleFraudStrategy())
detector = FraudDetector(WeightedFraudStrategy())  # Changed!
```

## Why Strategy?

| Benefit | Explanation |
|---------|-------------|
| **Open/Closed** | Add strategies without modifying existing code |
| **Runtime flexibility** | Switch algorithms based on conditions |
| **Testability** | Test each strategy in isolation |
| **No conditionals** | Eliminate if/else chains |

## The Analogy: The GPS Navigator

Your GPS has multiple routing strategies:
- **Fastest** route
- **Shortest** distance
- **Avoid highways**
- **Scenic route**

Same destination, different algorithms. You swap strategies without changing the GPS app.

## The "Pro" Tip

> **Strategy is about interchangeable algorithms. If two classes compute the same type of result differently, they're strategy candidates.**

## Task

Build fee calculation strategies for different customer tiers.

<!-- SEPARATOR -->

# seed_code
from abc import ABC, abstractmethod
from decimal import Decimal

# Domain entity
class Transaction:
    def __init__(self, txn_id: str, amount: Decimal, txn_type: str):
        self.txn_id = txn_id
        self.amount = amount
        self.txn_type = txn_type


# Strategy interface
class FeeStrategy(ABC):
    @abstractmethod
    def calculate_fee(self, txn: Transaction) -> Decimal:
        """Calculate fee for transaction."""
        pass
    
    @abstractmethod
    def get_name(self) -> str:
        """Return strategy name for display."""
        pass


# Standard tier: 1% fee, minimum $1
class StandardFeeStrategy(FeeStrategy):
    def calculate_fee(self, txn: Transaction) -> Decimal:
        pass  # Replace with your implementation
    
    def get_name(self) -> str:
        return "Standard"


# Premium tier: 0.5% fee, minimum $0.50, capped at $50
class PremiumFeeStrategy(FeeStrategy):
    def calculate_fee(self, txn: Transaction) -> Decimal:
        pass  # Replace with your implementation
    
    def get_name(self) -> str:
        return "Premium"


# VIP tier: Flat $0.25 per transaction
class VIPFeeStrategy(FeeStrategy):
    def calculate_fee(self, txn: Transaction) -> Decimal:
        pass  # Replace with your implementation
    
    def get_name(self) -> str:
        return "VIP"


# Context class that uses strategy
class FeeCalculator:
    def __init__(self, strategy: FeeStrategy):
        self.strategy = strategy
    
    def set_strategy(self, strategy: FeeStrategy) -> None:
        """Change strategy at runtime."""
        self.strategy = strategy
    
    def calculate(self, txn: Transaction) -> Decimal:
        """Calculate fee using current strategy."""
        return self.strategy.calculate_fee(txn)
    
    def get_tier_name(self) -> str:
        return self.strategy.get_name()


# Test the strategies
print("=== Fee Strategy Pattern ===")

# Create a transaction
txn = Transaction("TXN-001", Decimal("1000.00"), "TRANSFER")
print(f"Transaction: ${txn.amount}")

# Calculate with different strategies
calculator = FeeCalculator(StandardFeeStrategy())
print(f"\n{calculator.get_tier_name()} fee: ${calculator.calculate(txn)}")

calculator.set_strategy(PremiumFeeStrategy())
print(f"{calculator.get_tier_name()} fee: ${calculator.calculate(txn)}")

calculator.set_strategy(VIPFeeStrategy())
print(f"{calculator.get_tier_name()} fee: ${calculator.calculate(txn)}")

# Test with large amount
large_txn = Transaction("TXN-002", Decimal("50000.00"), "WIRE")
print(f"\nLarge transaction: ${large_txn.amount}")

calculator.set_strategy(PremiumFeeStrategy())
print(f"{calculator.get_tier_name()} fee (capped): ${calculator.calculate(large_txn)}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

txn = Transaction("TXN-001", Decimal("1000.00"), "TRANSFER")

# Test Standard: 1% = $10, but min $1
standard = StandardFeeStrategy()
fee = standard.calculate_fee(txn)
assert fee == Decimal("10.00"), f"Standard should be $10, got ${fee}"

# Test small amount (min fee)
small_txn = Transaction("TXN-002", Decimal("50.00"), "TRANSFER")
small_fee = standard.calculate_fee(small_txn)
assert small_fee >= Decimal("1.00"), f"Standard min should be $1, got ${small_fee}"

# Test Premium: 0.5% = $5
premium = PremiumFeeStrategy()
fee = premium.calculate_fee(txn)
assert fee == Decimal("5.00"), f"Premium should be $5, got ${fee}"

# Test Premium cap
large_txn = Transaction("TXN-003", Decimal("50000.00"), "WIRE")
capped_fee = premium.calculate_fee(large_txn)
assert capped_fee == Decimal("50.00"), f"Premium capped at $50, got ${capped_fee}"

# Test VIP: flat $0.25
vip = VIPFeeStrategy()
fee = vip.calculate_fee(txn)
assert fee == Decimal("0.25"), f"VIP should be $0.25, got ${fee}"

# Test runtime swap
calc = FeeCalculator(StandardFeeStrategy())
assert calc.get_tier_name() == "Standard"
calc.set_strategy(VIPFeeStrategy())
assert calc.get_tier_name() == "VIP"
