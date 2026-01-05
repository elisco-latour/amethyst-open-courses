---
id: "finguard_09_05"
title: "The Plugin System"
type: "coding"
xp: 100
---

# The Plugin System

FinGuard's fraud detection needs to be extensible. New rules should be easy to add without modifying existing code.

## The Bridge: Extensibility

In data engineering, requirements change constantly. New fraud patterns emerge. Your system should be **open for extension** but **closed for modification** (the Open/Closed Principle).

## The Plugin Architecture

```python
from abc import ABC, abstractmethod
from decimal import Decimal

class FraudRule(ABC):
    @abstractmethod
    def check(self, transaction: dict) -> tuple[bool, str]:
        """Check if transaction violates this rule.
        
        Returns:
            (is_suspicious, reason)
        """
        pass

class HighAmountRule(FraudRule):
    def check(self, transaction: dict) -> tuple[bool, str]:
        if transaction["amount"] > Decimal("10000"):
            return (True, "Amount exceeds $10,000")
        return (False, "")

class OddHoursRule(FraudRule):
    def check(self, transaction: dict) -> tuple[bool, str]:
        hour = transaction["timestamp"].hour
        if hour < 6 or hour > 22:
            return (True, "Transaction outside business hours")
        return (False, "")
```

## The Rule Engine

```python
class FraudEngine:
    def __init__(self):
        self.rules: list[FraudRule] = []
    
    def add_rule(self, rule: FraudRule) -> None:
        self.rules.append(rule)
    
    def check_transaction(self, transaction: dict) -> list[str]:
        alerts = []
        for rule in self.rules:
            is_suspicious, reason = rule.check(transaction)
            if is_suspicious:
                alerts.append(reason)
        return alerts
```

## Adding New Rules Without Modification

```python
# Easy to add new rules — just create a new class!
class NewCountryRule(FraudRule):
    def check(self, transaction: dict) -> tuple[bool, str]:
        # New fraud pattern detection
        ...

engine.add_rule(NewCountryRule())  # Plug it in!
```

## The "Pro" Tip

> **Design for extensibility. When you see "if type == X, elif type == Y, ...", consider polymorphism instead.**

## Task

Build a pluggable fraud detection system:
1. Create `FraudRule` abstract base class
2. Implement: `AmountThresholdRule`, `RapidTransactionRule`, `SuspiciousCountryRule`
3. Create `FraudDetector` that runs all rules and collects alerts

<!-- SEPARATOR -->

# seed_code
from abc import ABC, abstractmethod
from decimal import Decimal
from datetime import datetime

SUSPICIOUS_COUNTRIES: list[str] = ["XX", "YY", "ZZ"]

class FraudRule(ABC):
    """Abstract base class for fraud detection rules."""
    
    @abstractmethod
    def check(self, transaction: dict) -> tuple[bool, str]:
        """Check transaction against this rule.
        
        Returns:
            (is_suspicious, alert_message)
        """
        pass
    
    @abstractmethod
    def get_rule_name(self) -> str:
        """Return the rule name."""
        pass


class AmountThresholdRule(FraudRule):
    """Flag transactions over $10,000."""
    
    pass  # Replace with your implementation


class RapidTransactionRule(FraudRule):
    """Flag if more than 5 transactions in history."""
    
    pass  # Replace with your implementation


class SuspiciousCountryRule(FraudRule):
    """Flag transactions to suspicious countries."""
    
    pass  # Replace with your implementation


class FraudDetector:
    """Fraud detection engine with pluggable rules."""
    
    def __init__(self):
        self.rules: list[FraudRule] = []
    
    def add_rule(self, rule: FraudRule) -> None:
        pass  # Replace with your implementation
    
    def scan(self, transaction: dict) -> list[str]:
        """Run all rules and return list of alerts."""
        pass  # Replace with your implementation


# Test the fraud detector
print("=== Fraud Detection System ===")

detector = FraudDetector()
detector.add_rule(AmountThresholdRule())
detector.add_rule(RapidTransactionRule())
detector.add_rule(SuspiciousCountryRule())

test_transactions = [
    {"id": "T1", "amount": Decimal("500.00"), "country": "US", "recent_count": 2},
    {"id": "T2", "amount": Decimal("15000.00"), "country": "US", "recent_count": 2},
    {"id": "T3", "amount": Decimal("100.00"), "country": "XX", "recent_count": 8},
]

for txn in test_transactions:
    alerts = detector.scan(txn)
    if alerts:
        print(f"⚠️ {txn['id']}: {', '.join(alerts)}")
    else:
        print(f"✓ {txn['id']}: Clear")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Test individual rules
amt_rule = AmountThresholdRule()
suspicious, msg = amt_rule.check({"amount": Decimal("15000.00"), "country": "US", "recent_count": 1})
assert suspicious == True, "Should flag amount > $10,000"

suspicious, msg = amt_rule.check({"amount": Decimal("500.00"), "country": "US", "recent_count": 1})
assert suspicious == False, "Should not flag normal amount"

rapid_rule = RapidTransactionRule()
suspicious, msg = rapid_rule.check({"amount": Decimal("100.00"), "country": "US", "recent_count": 8})
assert suspicious == True, "Should flag rapid transactions (>5)"

country_rule = SuspiciousCountryRule()
suspicious, msg = country_rule.check({"amount": Decimal("100.00"), "country": "XX", "recent_count": 1})
assert suspicious == True, "Should flag suspicious country"

# Test detector
detector = FraudDetector()
detector.add_rule(AmountThresholdRule())
detector.add_rule(SuspiciousCountryRule())

alerts = detector.scan({"amount": Decimal("15000.00"), "country": "XX", "recent_count": 1})
assert len(alerts) == 2, "Should have 2 alerts (amount + country)"

alerts = detector.scan({"amount": Decimal("100.00"), "country": "US", "recent_count": 1})
assert len(alerts) == 0, "Clean transaction should have no alerts"
