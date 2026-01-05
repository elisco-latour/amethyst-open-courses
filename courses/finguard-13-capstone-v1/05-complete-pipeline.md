---
id: "finguard_13_05"
title: "Complete Pipeline"
type: "coding"
xp: 100
---

# Complete Pipeline: Putting It All Together

## The FinGuard Batch Processor

You've built all the pieces. Now combine them into a working system:

```
┌─────────────────────────────────────────────────────────────────┐
│                    BATCH FRAUD PROCESSOR                         │
│                                                                  │
│  ┌─────────┐   ┌─────────────┐   ┌──────────────┐              │
│  │ CSV     │──►│  Transaction│──►│ Fraud        │              │
│  │ Reader  │   │  Repository │   │ Detector     │              │
│  └─────────┘   └─────────────┘   └──────┬───────┘              │
│                                          │                       │
│                                          ▼                       │
│                                   ┌──────────────┐              │
│                                   │    Alert     │              │
│                                   │  Dispatcher  │              │
│                                   └──────┬───────┘              │
│                                          │                       │
│        ┌─────────────────────────────────┼──────────────────┐   │
│        ▼                                 ▼                  ▼   │
│  ┌───────────┐                   ┌───────────┐      ┌─────────┐│
│  │  Audit    │                   │  Metrics  │      │ Report  ││
│  │  Logger   │                   │  Tracker  │      │ Builder ││
│  └───────────┘                   └───────────┘      └─────────┘│
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## The Processor Class

```python
class BatchFraudProcessor:
    def __init__(
        self,
        repository: TransactionRepository,
        detector: FraudDetector,
        dispatcher: AlertDispatcher
    ):
        ...
    
    def process_batch(self, transactions: list[Transaction]) -> ProcessingResult:
        """Process a batch of transactions."""
        ...
```

## Task

Build the complete batch processor and run it on sample data.

<!-- SEPARATOR -->

# seed_code
from abc import ABC, abstractmethod
from decimal import Decimal
from datetime import datetime
from dataclasses import dataclass, field

# ========== DOMAIN ENTITIES ==========

class Transaction:
    def __init__(self, txn_id: str, account_id: str, amount: Decimal,
                 txn_type: str, timestamp: datetime, status: str = "PENDING"):
        self.txn_id = txn_id
        self.account_id = account_id
        self.amount = amount
        self.txn_type = txn_type
        self.timestamp = timestamp
        self.status = status

@dataclass
class FraudAlert:
    alert_id: str
    txn_id: str
    rule_name: str
    severity: str
    description: str
    timestamp: datetime
    account_id: str = ""

@dataclass
class ProcessingResult:
    transactions_processed: int
    alerts_generated: int
    alerts_by_severity: dict
    flagged_accounts: set
    processing_time_ms: float


# ========== REPOSITORY ==========

class TransactionRepository:
    def __init__(self):
        self._storage: dict[str, Transaction] = {}
    
    def save(self, txn: Transaction) -> None:
        self._storage[txn.txn_id] = txn
    
    def find_all(self) -> list[Transaction]:
        return list(self._storage.values())


# ========== FRAUD STRATEGIES ==========

class FraudStrategy(ABC):
    @abstractmethod
    def analyze(self, txn: Transaction, history: list[Transaction]) -> FraudAlert | None:
        pass

class HighValueStrategy(FraudStrategy):
    _counter = 0
    
    def analyze(self, txn: Transaction, history: list[Transaction]) -> FraudAlert | None:
        if txn.amount > Decimal("10000"):
            HighValueStrategy._counter += 1
            severity = "CRITICAL" if txn.amount > Decimal("50000") else "HIGH"
            return FraudAlert(
                f"ALT-HV-{HighValueStrategy._counter}",
                txn.txn_id, "HIGH_VALUE", severity,
                f"Amount ${txn.amount} exceeds threshold",
                datetime.now(), txn.account_id
            )
        return None


# ========== FRAUD DETECTOR ==========

class FraudDetector:
    def __init__(self, strategies: list[FraudStrategy]):
        self.strategies = strategies
        self.history: list[Transaction] = []
    
    def analyze(self, txn: Transaction) -> list[FraudAlert]:
        alerts = []
        for strategy in self.strategies:
            alert = strategy.analyze(txn, self.history)
            if alert:
                alerts.append(alert)
        self.history.append(txn)
        return alerts


# ========== ALERT OBSERVERS ==========

class AlertObserver(ABC):
    @abstractmethod
    def on_alert(self, alert: FraudAlert) -> None:
        pass

class AuditLogger(AlertObserver):
    def __init__(self):
        self.logs: list[str] = []
    
    def on_alert(self, alert: FraudAlert) -> None:
        self.logs.append(f"[{alert.timestamp}] {alert.alert_id}: {alert.description}")

class MetricsTracker(AlertObserver):
    def __init__(self):
        self.by_severity: dict[str, int] = {"LOW": 0, "MEDIUM": 0, "HIGH": 0, "CRITICAL": 0}
        self.flagged_accounts: set[str] = set()
    
    def on_alert(self, alert: FraudAlert) -> None:
        self.by_severity[alert.severity] = self.by_severity.get(alert.severity, 0) + 1
        if alert.severity in ("HIGH", "CRITICAL"):
            self.flagged_accounts.add(alert.account_id)


# ========== ALERT DISPATCHER ==========

class AlertDispatcher:
    def __init__(self, observers: list[AlertObserver]):
        self.observers = observers
    
    def dispatch(self, alert: FraudAlert) -> None:
        for observer in self.observers:
            observer.on_alert(alert)


# ========== THE BATCH PROCESSOR ==========

class BatchFraudProcessor:
    """Main processor that orchestrates the entire pipeline."""
    
    def __init__(
        self,
        repository: TransactionRepository,
        detector: FraudDetector,
        dispatcher: AlertDispatcher,
        metrics: MetricsTracker
    ):
        self.repository = repository
        self.detector = detector
        self.dispatcher = dispatcher
        self.metrics = metrics
    
    def process_batch(self, transactions: list[Transaction]) -> ProcessingResult:
        """Process a batch of transactions through the fraud pipeline.
        
        Steps:
        1. Save each transaction to repository
        2. Run fraud detection
        3. Dispatch any alerts
        4. Compile results
        """
        pass  # Replace with your implementation
        # Track start time
        # For each transaction:
        #   - Save to repository
        #   - Run detector.analyze()
        #   - For each alert, call dispatcher.dispatch()
        # Calculate processing time
        # Return ProcessingResult


# ========== MAIN EXECUTION ==========

print("=" * 60)
print("    FinGuard Batch Fraud Processor - Capstone v1")
print("=" * 60)

# Setup the pipeline
repository = TransactionRepository()
detector = FraudDetector([HighValueStrategy()])
audit_logger = AuditLogger()
metrics = MetricsTracker()
dispatcher = AlertDispatcher([audit_logger, metrics])
processor = BatchFraudProcessor(repository, detector, dispatcher, metrics)

# Sample daily batch
daily_transactions = [
    Transaction("TXN-001", "ACC-100", Decimal("1500.00"), "DEPOSIT", datetime(2024, 1, 15, 9, 0)),
    Transaction("TXN-002", "ACC-100", Decimal("500.00"), "WITHDRAWAL", datetime(2024, 1, 15, 10, 30)),
    Transaction("TXN-003", "ACC-200", Decimal("25000.00"), "DEPOSIT", datetime(2024, 1, 15, 11, 0)),  # HIGH
    Transaction("TXN-004", "ACC-300", Decimal("75000.00"), "TRANSFER", datetime(2024, 1, 15, 13, 45)),  # CRITICAL
    Transaction("TXN-005", "ACC-100", Decimal("200.00"), "WITHDRAWAL", datetime(2024, 1, 15, 14, 0)),
    Transaction("TXN-006", "ACC-400", Decimal("5000.00"), "DEPOSIT", datetime(2024, 1, 15, 15, 30)),
    Transaction("TXN-007", "ACC-200", Decimal("12000.00"), "TRANSFER", datetime(2024, 1, 15, 16, 0)),  # HIGH
]

print(f"\nProcessing {len(daily_transactions)} transactions...")
result = processor.process_batch(daily_transactions)

print("\n" + "=" * 60)
print("                    PROCESSING REPORT")
print("=" * 60)
print(f"Transactions Processed: {result.transactions_processed}")
print(f"Alerts Generated:       {result.alerts_generated}")
print(f"Processing Time:        {result.processing_time_ms:.2f}ms")
print(f"\nAlerts by Severity:")
for severity, count in result.alerts_by_severity.items():
    if count > 0:
        print(f"  {severity}: {count}")
print(f"\nFlagged Accounts: {result.flagged_accounts if result.flagged_accounts else 'None'}")
print("=" * 60)

# Show stored transactions
print(f"\n📁 Repository now contains {len(repository.find_all())} transactions")

# Show audit trail
print(f"\n📋 Audit Log ({len(audit_logger.logs)} entries):")
for log in audit_logger.logs[:5]:  # First 5 entries
    print(f"  {log}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
from datetime import datetime

# Setup
repository = TransactionRepository()
detector = FraudDetector([HighValueStrategy()])
audit_logger = AuditLogger()
metrics = MetricsTracker()
dispatcher = AlertDispatcher([audit_logger, metrics])
processor = BatchFraudProcessor(repository, detector, dispatcher, metrics)

# Test batch
transactions = [
    Transaction("T1", "A1", Decimal("1000"), "DEPOSIT", datetime.now()),
    Transaction("T2", "A2", Decimal("15000"), "DEPOSIT", datetime.now()),  # HIGH
    Transaction("T3", "A3", Decimal("60000"), "TRANSFER", datetime.now()),  # CRITICAL
]

result = processor.process_batch(transactions)

# Verify result
assert result.transactions_processed == 3, f"Should process 3, got {result.transactions_processed}"
assert result.alerts_generated == 2, f"Should generate 2 alerts, got {result.alerts_generated}"
assert result.alerts_by_severity["HIGH"] >= 1, "Should have HIGH alerts"
assert result.alerts_by_severity["CRITICAL"] >= 1, "Should have CRITICAL alerts"
assert "A2" in result.flagged_accounts or "A3" in result.flagged_accounts, "Should flag accounts"
assert result.processing_time_ms >= 0, "Should track processing time"

# Verify repository
assert len(repository.find_all()) == 3, "Repository should have 3 transactions"

# Verify audit trail
assert len(audit_logger.logs) == 2, "Audit should have 2 entries"

print("\n✅ All validations passed! Capstone v1 complete!")
