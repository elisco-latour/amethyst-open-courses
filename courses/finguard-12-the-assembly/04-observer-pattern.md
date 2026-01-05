---
id: "finguard_12_04"
title: "Observer Pattern"
type: "coding"
xp: 100
---

# Observer Pattern

**Notify** multiple objects when something changes.

## The Problem

```python
# ❌ Transaction processor knows about every system to notify
class TransactionProcessor:
    def process(self, txn: Transaction):
        # Process transaction
        save_transaction(txn)
        
        # Now notify everyone (tightly coupled!)
        send_email_notification(txn)
        update_dashboard(txn)
        log_to_audit_system(txn)
        check_fraud_rules(txn)
        # Adding a new notification = modify this class
```

## The Solution: Observer Pattern

```python
# ✅ Observers subscribe to events
class TransactionObserver(ABC):
    @abstractmethod
    def on_transaction(self, txn: Transaction) -> None:
        pass

class TransactionProcessor:
    def __init__(self):
        self._observers: list[TransactionObserver] = []
    
    def subscribe(self, observer: TransactionObserver) -> None:
        self._observers.append(observer)
    
    def process(self, txn: Transaction):
        save_transaction(txn)
        # Notify all observers
        for observer in self._observers:
            observer.on_transaction(txn)

# Add observers without changing processor
processor.subscribe(EmailNotifier())
processor.subscribe(AuditLogger())
processor.subscribe(FraudDetector())
```

## The Flow

```
                    ┌─────────────────┐
                    │   Processor     │
                    │  (Subject)      │
                    └────────┬────────┘
                             │ notify all
        ┌────────────────────┼────────────────────┐
        ▼                    ▼                    ▼
┌───────────────┐  ┌───────────────┐  ┌───────────────┐
│ EmailNotifier │  │  AuditLogger  │  │ FraudDetector │
│  (Observer)   │  │  (Observer)   │  │  (Observer)   │
└───────────────┘  └───────────────┘  └───────────────┘
```

## Why Observer?

| Benefit | Explanation |
|---------|-------------|
| **Loose coupling** | Subject doesn't know observer details |
| **Extensible** | Add observers without changing subject |
| **Event-driven** | Natural for reactive systems |
| **Maintainable** | Each observer handles one concern |

## The Analogy: The Newsletter

You subscribe to a newsletter. The publisher doesn't know you personally. When they publish, all subscribers get notified automatically. You can unsubscribe anytime.

## The "Pro" Tip

> **Observer is the foundation of event-driven architecture. Kafka, RabbitMQ, and webhooks are all observer pattern at scale.**

## Task

Build an observer system for transaction events.

<!-- SEPARATOR -->

# seed_code
from abc import ABC, abstractmethod
from decimal import Decimal
from datetime import datetime

# Domain entity
class Transaction:
    def __init__(self, txn_id: str, account_id: str, amount: Decimal, txn_type: str):
        self.txn_id = txn_id
        self.account_id = account_id
        self.amount = amount
        self.txn_type = txn_type
        self.timestamp = datetime.now()


# Observer interface
class TransactionObserver(ABC):
    @abstractmethod
    def on_transaction(self, txn: Transaction) -> None:
        """Called when a transaction is processed."""
        pass


# Subject (the thing being observed)
class TransactionProcessor:
    def __init__(self):
        self._observers: list[TransactionObserver] = []
        self._processed: list[Transaction] = []
    
    def subscribe(self, observer: TransactionObserver) -> None:
        """Add an observer."""
        pass  # Replace with your implementation
    
    def unsubscribe(self, observer: TransactionObserver) -> None:
        """Remove an observer."""
        pass  # Replace with your implementation
    
    def process(self, txn: Transaction) -> None:
        """Process transaction and notify observers."""
        pass  # Replace with your implementation


# Concrete observers
class AuditLogger(TransactionObserver):
    def __init__(self):
        self.logs: list[str] = []
    
    def on_transaction(self, txn: Transaction) -> None:
        """Log transaction for audit trail."""
        pass  # Replace with your implementation


class FraudDetector(TransactionObserver):
    def __init__(self, threshold: Decimal = Decimal("10000.00")):
        self.threshold = threshold
        self.alerts: list[str] = []
    
    def on_transaction(self, txn: Transaction) -> None:
        """Check for suspicious transactions."""
        pass  # Replace with your implementation


class NotificationService(TransactionObserver):
    def __init__(self):
        self.notifications: list[str] = []
    
    def on_transaction(self, txn: Transaction) -> None:
        """Send notification for transaction."""
        pass  # Replace with your implementation


class BalanceTracker(TransactionObserver):
    def __init__(self):
        self.balances: dict[str, Decimal] = {}
    
    def on_transaction(self, txn: Transaction) -> None:
        """Track running balance per account."""
        pass  # Replace with your implementation


# Test the observer pattern
print("=== Observer Pattern ===")

# Create processor and observers
processor = TransactionProcessor()
audit = AuditLogger()
fraud = FraudDetector(threshold=Decimal("5000.00"))
notifier = NotificationService()
tracker = BalanceTracker()

# Subscribe observers
processor.subscribe(audit)
processor.subscribe(fraud)
processor.subscribe(notifier)
processor.subscribe(tracker)

# Process transactions
transactions = [
    Transaction("TXN-001", "ACC-100", Decimal("1000.00"), "DEPOSIT"),
    Transaction("TXN-002", "ACC-100", Decimal("500.00"), "WITHDRAWAL"),
    Transaction("TXN-003", "ACC-200", Decimal("15000.00"), "DEPOSIT"),  # High value!
]

for txn in transactions:
    processor.process(txn)
    print(f"Processed: {txn.txn_id} - ${txn.amount}")

# Check results
print(f"\nAudit logs: {len(audit.logs)}")
print(f"Fraud alerts: {len(fraud.alerts)}")
print(f"Notifications sent: {len(notifier.notifications)}")
print(f"Accounts tracked: {len(tracker.balances)}")

if fraud.alerts:
    print(f"\n⚠️ Alert: {fraud.alerts[0]}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Create fresh test
processor = TransactionProcessor()
audit = AuditLogger()
fraud = FraudDetector(threshold=Decimal("5000.00"))
tracker = BalanceTracker()

processor.subscribe(audit)
processor.subscribe(fraud)
processor.subscribe(tracker)

# Process transactions
t1 = Transaction("TXN-001", "ACC-100", Decimal("1000.00"), "DEPOSIT")
t2 = Transaction("TXN-002", "ACC-100", Decimal("500.00"), "WITHDRAWAL")
t3 = Transaction("TXN-003", "ACC-200", Decimal("15000.00"), "DEPOSIT")

processor.process(t1)
processor.process(t2)
processor.process(t3)

# Verify audit logs all transactions
assert len(audit.logs) == 3, f"Should have 3 logs, got {len(audit.logs)}"

# Verify fraud detects high-value transaction
assert len(fraud.alerts) >= 1, "Should have at least 1 fraud alert"
assert "15000" in fraud.alerts[0] or "TXN-003" in fraud.alerts[0], "Alert should reference high-value transaction"

# Verify balance tracking
assert "ACC-100" in tracker.balances, "Should track ACC-100"
assert "ACC-200" in tracker.balances, "Should track ACC-200"

# Test unsubscribe
processor.unsubscribe(audit)
processor.process(Transaction("TXN-004", "ACC-100", Decimal("100.00"), "DEPOSIT"))
assert len(audit.logs) == 3, "Unsubscribed observer should not receive events"
