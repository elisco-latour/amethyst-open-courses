---
id: "finguard_13_04"
title: "Alert System"
type: "coding"
xp: 100
---

# Alert System: Observer Pattern

## Event-Driven Alerts

When fraud is detected, multiple systems need to know:
- **Audit Log**: Record for compliance
- **Dashboard**: Real-time display
- **Email**: Notify fraud team
- **Block Service**: Freeze suspicious accounts

## The Observer Pattern

```
                    ┌─────────────────┐
                    │ AlertDispatcher │
                    │    (Subject)    │
                    └────────┬────────┘
                             │ dispatch()
        ┌────────────────────┼────────────────────┐
        ▼                    ▼                    ▼
┌───────────────┐  ┌───────────────┐  ┌───────────────┐
│  AuditLogger  │  │ EmailNotifier │  │ MetricsTracker│
│  (Observer)   │  │  (Observer)   │  │  (Observer)   │
└───────────────┘  └───────────────┘  └───────────────┘
```

## Alert Severity Levels

| Severity | Action | Response Time |
|----------|--------|---------------|
| **LOW** | Log only | 24 hours |
| **MEDIUM** | Log + Dashboard | 4 hours |
| **HIGH** | Log + Email + Dashboard | 1 hour |
| **CRITICAL** | All + Account Block | Immediate |

## Task

Build an alert dispatcher with multiple observers.

<!-- SEPARATOR -->

# seed_code
from abc import ABC, abstractmethod
from decimal import Decimal
from datetime import datetime
from dataclasses import dataclass

@dataclass
class FraudAlert:
    alert_id: str
    txn_id: str
    rule_name: str
    severity: str
    description: str
    timestamp: datetime
    account_id: str = ""


# Observer interface
class AlertObserver(ABC):
    @abstractmethod
    def on_alert(self, alert: FraudAlert) -> None:
        """Handle a fraud alert."""
        pass
    
    @abstractmethod
    def get_name(self) -> str:
        """Return observer name for logging."""
        pass


# Observer 1: Audit Logger
class AuditLogger(AlertObserver):
    def __init__(self):
        self.logs: list[str] = []
    
    def on_alert(self, alert: FraudAlert) -> None:
        pass  # Replace with your implementation
        # Log ALL alerts regardless of severity
        # Format: "[timestamp] ALERT alert_id: description"
    
    def get_name(self) -> str:
        return "AuditLogger"


# Observer 2: Email Notifier (HIGH and CRITICAL only)
class EmailNotifier(AlertObserver):
    def __init__(self, recipients: list[str]):
        self.recipients = recipients
        self.sent_emails: list[dict] = []
    
    def on_alert(self, alert: FraudAlert) -> None:
        pass  # Replace with your implementation
        # Only send for HIGH or CRITICAL severity
        # Record email in sent_emails list
        # Include: to, subject, body
    
    def get_name(self) -> str:
        return "EmailNotifier"


# Observer 3: Metrics Tracker
class MetricsTracker(AlertObserver):
    def __init__(self):
        self.counts_by_severity: dict[str, int] = {
            "LOW": 0, "MEDIUM": 0, "HIGH": 0, "CRITICAL": 0
        }
        self.counts_by_rule: dict[str, int] = {}
    
    def on_alert(self, alert: FraudAlert) -> None:
        pass  # Replace with your implementation
        # Increment count for severity
        # Increment count for rule_name
    
    def get_name(self) -> str:
        return "MetricsTracker"
    
    def get_summary(self) -> dict:
        return {
            "by_severity": self.counts_by_severity,
            "by_rule": self.counts_by_rule,
            "total": sum(self.counts_by_severity.values())
        }


# Observer 4: Account Blocker (CRITICAL only)
class AccountBlocker(AlertObserver):
    def __init__(self):
        self.blocked_accounts: set[str] = set()
    
    def on_alert(self, alert: FraudAlert) -> None:
        pass  # Replace with your implementation
        # Only block for CRITICAL severity
        # Add account_id to blocked_accounts set
    
    def get_name(self) -> str:
        return "AccountBlocker"


# The Alert Dispatcher (Subject)
class AlertDispatcher:
    def __init__(self):
        self._observers: list[AlertObserver] = []
    
    def subscribe(self, observer: AlertObserver) -> None:
        """Add an observer."""
        self._observers.append(observer)
    
    def unsubscribe(self, observer: AlertObserver) -> None:
        """Remove an observer."""
        self._observers.remove(observer)
    
    def dispatch(self, alert: FraudAlert) -> None:
        """Send alert to all observers."""
        pass  # Replace with your implementation
        # Call on_alert() for each observer
        # Print which observers were notified


# Test the alert system
print("=== Alert Dispatch System ===")

# Create dispatcher and observers
dispatcher = AlertDispatcher()
audit = AuditLogger()
email = EmailNotifier(["fraud@finguard.com", "security@finguard.com"])
metrics = MetricsTracker()
blocker = AccountBlocker()

dispatcher.subscribe(audit)
dispatcher.subscribe(email)
dispatcher.subscribe(metrics)
dispatcher.subscribe(blocker)

# Test alerts
test_alerts = [
    FraudAlert("ALT-001", "TXN-001", "ROUND_AMOUNT", "LOW", "Round amount detected", datetime.now(), "ACC-100"),
    FraudAlert("ALT-002", "TXN-002", "HIGH_VALUE", "MEDIUM", "Amount exceeds $10,000", datetime.now(), "ACC-200"),
    FraudAlert("ALT-003", "TXN-003", "VELOCITY", "HIGH", "5 transactions in 1 hour", datetime.now(), "ACC-100"),
    FraudAlert("ALT-004", "TXN-004", "HIGH_VALUE", "CRITICAL", "Amount exceeds $100,000", datetime.now(), "ACC-300"),
]

print("Dispatching alerts...")
for alert in test_alerts:
    print(f"\n--- {alert.alert_id} [{alert.severity}] ---")
    dispatcher.dispatch(alert)

# Results
print("\n=== Results ===")
print(f"Audit logs: {len(audit.logs)}")
print(f"Emails sent: {len(email.sent_emails)}")
print(f"Accounts blocked: {blocker.blocked_accounts}")
print(f"Metrics: {metrics.get_summary()}")

<!-- SEPARATOR -->

# validation_code
from datetime import datetime

dispatcher = AlertDispatcher()
audit = AuditLogger()
email = EmailNotifier(["test@test.com"])
metrics = MetricsTracker()
blocker = AccountBlocker()

dispatcher.subscribe(audit)
dispatcher.subscribe(email)
dispatcher.subscribe(metrics)
dispatcher.subscribe(blocker)

# Test LOW alert
low_alert = FraudAlert("A1", "T1", "ROUND", "LOW", "Test", datetime.now(), "ACC-1")
dispatcher.dispatch(low_alert)

assert len(audit.logs) == 1, "Audit should log LOW"
assert len(email.sent_emails) == 0, "Email should NOT send for LOW"
assert "ACC-1" not in blocker.blocked_accounts, "Should NOT block for LOW"

# Test HIGH alert
high_alert = FraudAlert("A2", "T2", "VELOCITY", "HIGH", "Test", datetime.now(), "ACC-2")
dispatcher.dispatch(high_alert)

assert len(audit.logs) == 2, "Audit should log HIGH"
assert len(email.sent_emails) == 1, "Email should send for HIGH"
assert "ACC-2" not in blocker.blocked_accounts, "Should NOT block for HIGH"

# Test CRITICAL alert
critical_alert = FraudAlert("A3", "T3", "HIGH_VALUE", "CRITICAL", "Test", datetime.now(), "ACC-3")
dispatcher.dispatch(critical_alert)

assert len(audit.logs) == 3, "Audit should log CRITICAL"
assert len(email.sent_emails) == 2, "Email should send for CRITICAL"
assert "ACC-3" in blocker.blocked_accounts, "Should block for CRITICAL"

# Test metrics
summary = metrics.get_summary()
assert summary["by_severity"]["LOW"] == 1, "Should count 1 LOW"
assert summary["by_severity"]["HIGH"] == 1, "Should count 1 HIGH"
assert summary["by_severity"]["CRITICAL"] == 1, "Should count 1 CRITICAL"
assert summary["total"] == 3, "Total should be 3"
