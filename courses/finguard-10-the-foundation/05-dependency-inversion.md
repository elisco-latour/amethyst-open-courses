---
id: "finguard_10_05"
title: "Dependency Inversion"
type: "coding"
xp: 100
---

# Dependency Inversion Principle (DIP)

**High-level modules should not depend on low-level modules. Both should depend on abstractions.**

## The Problem: Tight Coupling

```python
# ❌ BAD: FraudDetector depends directly on PostgresDatabase
class PostgresDatabase:
    def save_alert(self, alert: dict) -> None:
        # Postgres-specific code
        pass

class FraudDetector:
    def __init__(self):
        self.db = PostgresDatabase()  # Hard dependency!
    
    def detect(self, txn: dict) -> None:
        if txn["amount"] > 10000:
            self.db.save_alert({"txn": txn})
```

Problems:
- Can't test without a real database
- Can't switch to MySQL without rewriting FraudDetector
- FraudDetector knows about Postgres details

## The Solution: Depend on Abstractions

```python
from abc import ABC, abstractmethod

# Abstraction (interface)
class AlertRepository(ABC):
    @abstractmethod
    def save_alert(self, alert: dict) -> None:
        pass

# Low-level module implements the abstraction
class PostgresAlertRepository(AlertRepository):
    def save_alert(self, alert: dict) -> None:
        # Postgres-specific implementation
        pass

# High-level module depends on abstraction
class FraudDetector:
    def __init__(self, alert_repo: AlertRepository):  # Inject dependency
        self.alert_repo = alert_repo
    
    def detect(self, txn: dict) -> None:
        if txn["amount"] > 10000:
            self.alert_repo.save_alert({"txn": txn})
```

## The Analogy: The Power Outlet

Your laptop doesn't care if electricity comes from coal, solar, or nuclear.
It just needs a standard outlet (the abstraction).

The power plant (low-level) and your laptop (high-level) both depend on the outlet standard.

## Benefits of DIP

```python
# Easy testing with mock
class MockAlertRepository(AlertRepository):
    def __init__(self):
        self.alerts: list[dict] = []
    
    def save_alert(self, alert: dict) -> None:
        self.alerts.append(alert)

# Test without real database
def test_fraud_detection():
    mock_repo = MockAlertRepository()
    detector = FraudDetector(mock_repo)
    detector.detect({"amount": 50000})
    assert len(mock_repo.alerts) == 1
```

## The "Pro" Tip

> **Constructor injection is the most common way to apply DIP. Pass dependencies in, don't create them inside.**

```python
# ❌ BAD: Creates its own dependency
class Service:
    def __init__(self):
        self.repo = PostgresRepository()

# ✅ GOOD: Receives dependency
class Service:
    def __init__(self, repo: Repository):
        self.repo = repo
```

## Task

Refactor `TransactionProcessor` to depend on abstractions.

<!-- SEPARATOR -->

# seed_code
from abc import ABC, abstractmethod
from decimal import Decimal
from datetime import datetime

# Define abstractions
class TransactionValidator(ABC):
    @abstractmethod
    def validate(self, amount: Decimal) -> tuple[bool, str]:
        pass


class TransactionStore(ABC):
    @abstractmethod
    def save(self, txn: dict) -> None:
        pass
    
    @abstractmethod
    def get_all(self) -> list[dict]:
        pass


class NotificationService(ABC):
    @abstractmethod
    def send(self, message: str) -> None:
        pass


# Concrete implementations
class BasicValidator(TransactionValidator):
    def validate(self, amount: Decimal) -> tuple[bool, str]:
        pass  # Replace with your implementation


class InMemoryStore(TransactionStore):
    def __init__(self):
        self.transactions: list[dict] = []
    
    def save(self, txn: dict) -> None:
        pass  # Replace with your implementation
    
    def get_all(self) -> list[dict]:
        pass  # Replace with your implementation


class ConsoleNotifier(NotificationService):
    def __init__(self):
        self.messages: list[str] = []
    
    def send(self, message: str) -> None:
        pass  # Replace with your implementation


# High-level module depends on abstractions
class TransactionProcessor:
    """Processes transactions using injected dependencies."""
    
    def __init__(
        self,
        validator: TransactionValidator,
        store: TransactionStore,
        notifier: NotificationService
    ):
        pass  # Replace with your implementation
    
    def process(self, amount: Decimal, account_id: str) -> dict | None:
        """Process a transaction. Returns transaction or None if invalid."""
        pass  # Replace with your implementation


# Test DIP design
print("=== Dependency Inversion Demo ===")

# Create concrete implementations
validator = BasicValidator()
store = InMemoryStore()
notifier = ConsoleNotifier()

# Inject dependencies
processor = TransactionProcessor(validator, store, notifier)

# Process transactions
processor.process(Decimal("500.00"), "ACC-001")
processor.process(Decimal("-100.00"), "ACC-002")  # Should fail
processor.process(Decimal("1500.00"), "ACC-003")

print(f"Stored: {len(store.get_all())} transactions")
print(f"Notifications: {len(notifier.messages)}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Test validator
validator = BasicValidator()
valid, msg = validator.validate(Decimal("100.00"))
assert valid == True, "Valid amount should pass"
valid, msg = validator.validate(Decimal("-50.00"))
assert valid == False, "Negative should fail"

# Test store
store = InMemoryStore()
store.save({"id": "T1", "amount": Decimal("100")})
assert len(store.get_all()) == 1, "Should store transaction"

# Test notifier
notifier = ConsoleNotifier()
notifier.send("Test")
assert len(notifier.messages) == 1, "Should record message"

# Test processor with injected dependencies
processor = TransactionProcessor(
    BasicValidator(),
    InMemoryStore(),
    ConsoleNotifier()
)
result = processor.process(Decimal("500.00"), "ACC-001")
assert result is not None, "Valid transaction should return result"
assert "id" in result, "Result should have transaction id"

# Test rejection
result = processor.process(Decimal("-100.00"), "ACC-002")
assert result is None, "Invalid transaction should return None"
