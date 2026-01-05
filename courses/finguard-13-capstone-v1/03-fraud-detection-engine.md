---
id: "finguard_13_03"
title: "Fraud Detection Engine"
type: "coding"
xp: 100
---

# Fraud Detection Engine

## The Strategy Pattern for Rules

Different fraud rules for different scenarios:

```
┌─────────────────────────────────────────┐
│           FraudDetector                 │
│  ┌─────────────────────────────────┐    │
│  │ strategies: list[FraudStrategy] │    │
│  └─────────────────────────────────┘    │
│                                         │
│  analyze(txn) → list[FraudAlert]        │
└─────────────────────────────────────────┘
                   │
     ┌─────────────┼─────────────┐
     ▼             ▼             ▼
┌─────────┐  ┌─────────┐  ┌─────────┐
│HighValue│  │ Velocity│  │ Pattern │
│ Strategy│  │ Strategy│  │ Strategy│
└─────────┘  └─────────┘  └─────────┘
```

## Fraud Detection Rules

| Rule | Description | Threshold |
|------|-------------|-----------|
| **High Value** | Single transaction exceeds limit | > $10,000 |
| **Velocity** | Too many transactions in time window | > 5 in 1 hour |
| **Round Amount** | Suspiciously round amounts | Exact thousands |
| **New Account** | High activity on new accounts | First week patterns |

## The Alert Entity

```python
class FraudAlert:
    alert_id: str
    txn_id: str
    rule_name: str
    severity: str       # LOW, MEDIUM, HIGH, CRITICAL
    description: str
    timestamp: datetime
```

## Task

Build a fraud detection engine with multiple strategies.

<!-- SEPARATOR -->

# seed_code
from abc import ABC, abstractmethod
from decimal import Decimal
from datetime import datetime
from dataclasses import dataclass

# Simplified Transaction
class Transaction:
    def __init__(self, txn_id: str, account_id: str, amount: Decimal,
                 txn_type: str, timestamp: datetime):
        self.txn_id = txn_id
        self.account_id = account_id
        self.amount = amount
        self.txn_type = txn_type
        self.timestamp = timestamp


@dataclass
class FraudAlert:
    alert_id: str
    txn_id: str
    rule_name: str
    severity: str  # LOW, MEDIUM, HIGH, CRITICAL
    description: str
    timestamp: datetime


# Strategy interface
class FraudStrategy(ABC):
    @abstractmethod
    def analyze(self, txn: Transaction, history: list[Transaction]) -> FraudAlert | None:
        """Analyze transaction. Return alert if suspicious, None otherwise."""
        pass
    
    @abstractmethod
    def get_name(self) -> str:
        pass


# Strategy 1: High Value Detection
class HighValueStrategy(FraudStrategy):
    def __init__(self, threshold: Decimal = Decimal("10000.00")):
        self.threshold = threshold
        self._alert_counter = 0
    
    def analyze(self, txn: Transaction, history: list[Transaction]) -> FraudAlert | None:
        pass  # Replace with your implementation
        # Check if amount > threshold
        # Return FraudAlert with severity based on amount
        # >$10k = MEDIUM, >$50k = HIGH, >$100k = CRITICAL
    
    def get_name(self) -> str:
        return "HIGH_VALUE"


# Strategy 2: Round Amount Detection
class RoundAmountStrategy(FraudStrategy):
    def __init__(self):
        self._alert_counter = 0
    
    def analyze(self, txn: Transaction, history: list[Transaction]) -> FraudAlert | None:
        pass  # Replace with your implementation
        # Check if amount is exactly a multiple of 1000
        # These are often structuring attempts
        # Return LOW severity alert
    
    def get_name(self) -> str:
        return "ROUND_AMOUNT"


# Strategy 3: Velocity Detection  
class VelocityStrategy(FraudStrategy):
    def __init__(self, max_per_hour: int = 5):
        self.max_per_hour = max_per_hour
        self._alert_counter = 0
    
    def analyze(self, txn: Transaction, history: list[Transaction]) -> FraudAlert | None:
        pass  # Replace with your implementation
        # Count transactions for same account in last hour
        # Filter history for same account_id
        # Check if timestamp within 1 hour of txn.timestamp
        # If count > max_per_hour, return HIGH severity alert
    
    def get_name(self) -> str:
        return "VELOCITY"


# The Fraud Detector (uses multiple strategies)
class FraudDetector:
    def __init__(self, strategies: list[FraudStrategy] | None = None):
        self.strategies = strategies or []
        self.history: list[Transaction] = []
    
    def add_strategy(self, strategy: FraudStrategy) -> None:
        """Add a detection strategy."""
        self.strategies.append(strategy)
    
    def analyze(self, txn: Transaction) -> list[FraudAlert]:
        """Run all strategies against transaction."""
        pass  # Replace with your implementation
        # Run each strategy's analyze() method
        # Collect non-None alerts
        # Add txn to history after analysis
        # Return list of alerts


# Test the fraud detector
print("=== Fraud Detection Engine ===")

# Create detector with strategies
detector = FraudDetector()
detector.add_strategy(HighValueStrategy(Decimal("5000.00")))
detector.add_strategy(RoundAmountStrategy())
detector.add_strategy(VelocityStrategy(max_per_hour=3))

# Test transactions
test_transactions = [
    Transaction("TXN-001", "ACC-100", Decimal("1500.00"), "DEPOSIT", datetime(2024, 1, 15, 10, 0)),
    Transaction("TXN-002", "ACC-100", Decimal("2000.00"), "DEPOSIT", datetime(2024, 1, 15, 10, 15)),
    Transaction("TXN-003", "ACC-100", Decimal("10000.00"), "DEPOSIT", datetime(2024, 1, 15, 10, 30)),  # Round + High
    Transaction("TXN-004", "ACC-100", Decimal("500.00"), "WITHDRAWAL", datetime(2024, 1, 15, 10, 45)),
    Transaction("TXN-005", "ACC-100", Decimal("750.00"), "TRANSFER", datetime(2024, 1, 15, 10, 50)),  # Velocity!
]

print("Analyzing transactions...")
for txn in test_transactions:
    alerts = detector.analyze(txn)
    if alerts:
        print(f"\n{txn.txn_id} (${txn.amount}):")
        for alert in alerts:
            print(f"  ⚠️ [{alert.severity}] {alert.rule_name}: {alert.description}")
    else:
        print(f"{txn.txn_id}: OK")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
from datetime import datetime, timedelta

# Test HighValueStrategy
high_value = HighValueStrategy(Decimal("5000.00"))

small_txn = Transaction("T1", "A1", Decimal("1000"), "DEPOSIT", datetime.now())
alert = high_value.analyze(small_txn, [])
assert alert is None, "Small transaction should not trigger"

large_txn = Transaction("T2", "A1", Decimal("15000"), "DEPOSIT", datetime.now())
alert = high_value.analyze(large_txn, [])
assert alert is not None, "Large transaction should trigger"
assert alert.rule_name == "HIGH_VALUE", "Rule name should be HIGH_VALUE"

# Test RoundAmountStrategy
round_amount = RoundAmountStrategy()

non_round = Transaction("T3", "A1", Decimal("1234.56"), "DEPOSIT", datetime.now())
alert = round_amount.analyze(non_round, [])
assert alert is None, "Non-round amount should not trigger"

round_txn = Transaction("T4", "A1", Decimal("5000.00"), "DEPOSIT", datetime.now())
alert = round_amount.analyze(round_txn, [])
assert alert is not None, "Round amount should trigger"

# Test VelocityStrategy
velocity = VelocityStrategy(max_per_hour=2)
now = datetime.now()
history = [
    Transaction("T5", "A1", Decimal("100"), "DEPOSIT", now - timedelta(minutes=30)),
    Transaction("T6", "A1", Decimal("100"), "DEPOSIT", now - timedelta(minutes=15)),
]
new_txn = Transaction("T7", "A1", Decimal("100"), "DEPOSIT", now)
alert = velocity.analyze(new_txn, history)
assert alert is not None, "Velocity exceeded should trigger"
assert alert.severity == "HIGH", "Velocity should be HIGH severity"

# Test FraudDetector combines strategies
detector = FraudDetector()
detector.add_strategy(HighValueStrategy(Decimal("1000")))
detector.add_strategy(RoundAmountStrategy())

combo_txn = Transaction("T8", "A1", Decimal("5000.00"), "DEPOSIT", datetime.now())
alerts = detector.analyze(combo_txn)
assert len(alerts) == 2, f"Should trigger 2 alerts, got {len(alerts)}"
