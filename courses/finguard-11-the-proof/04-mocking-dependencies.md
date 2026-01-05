---
id: "finguard_11_04"
title: "Mocking Dependencies"
type: "coding"
xp: 100
---

# Mocking Dependencies

How do you test code that sends emails? You don't actually send emails — you **mock** the email service.

## The Problem: External Dependencies

```python
class AlertService:
    def __init__(self, email_client: EmailClient):
        self.email_client = email_client
    
    def send_fraud_alert(self, txn_id: str, amount: Decimal) -> bool:
        message = f"Fraud alert: {txn_id} - ${amount}"
        return self.email_client.send("security@bank.com", message)
```

Testing this with a real email client would:
- Send actual emails (bad!)
- Require network (slow)
- Fail if email server is down (flaky)

## The Solution: Mock Objects

```python
class MockEmailClient:
    def __init__(self):
        self.sent_emails: list[tuple] = []
    
    def send(self, to: str, message: str) -> bool:
        self.sent_emails.append((to, message))
        return True  # Always succeeds

def test_send_fraud_alert():
    mock_email = MockEmailClient()
    service = AlertService(mock_email)
    
    result = service.send_fraud_alert("TXN-001", Decimal("50000"))
    
    assert result == True
    assert len(mock_email.sent_emails) == 1
    assert "TXN-001" in mock_email.sent_emails[0][1]
```

## What to Mock

| Mock | Don't Mock |
|------|-----------|
| External APIs | Your own code |
| Databases | Pure functions |
| Email services | Data structures |
| File systems | Business logic |

## The Analogy: The Flight Simulator

Pilots train on simulators, not real planes.
- **Real plane**: Expensive, dangerous, can't test crashes
- **Simulator**: Cheap, safe, can test any scenario

Mocks are your simulators.

## The "Pro" Tip

> **A mock should have the same interface as the real object. If it doesn't, your tests might pass but real code fails.**

## Task

Create mock objects to test a fraud detection service.

<!-- SEPARATOR -->

# seed_code
from abc import ABC, abstractmethod
from decimal import Decimal
from datetime import datetime

# Interfaces
class AlertRepository(ABC):
    @abstractmethod
    def save(self, alert: dict) -> str:
        """Save alert and return ID."""
        pass

class NotificationService(ABC):
    @abstractmethod
    def send_urgent(self, message: str) -> bool:
        pass

# The class to test
class FraudDetectionService:
    def __init__(self, alert_repo: AlertRepository, notifier: NotificationService):
        self.alert_repo = alert_repo
        self.notifier = notifier
        self.threshold = Decimal("10000.00")
    
    def check_transaction(self, txn: dict) -> dict | None:
        """Check transaction for fraud. Returns alert if suspicious."""
        if txn["amount"] <= self.threshold:
            return None
        
        alert = {
            "transaction_id": txn["id"],
            "amount": txn["amount"],
            "reason": "Amount exceeds threshold",
            "timestamp": datetime.now().isoformat()
        }
        
        alert_id = self.alert_repo.save(alert)
        self.notifier.send_urgent(f"Fraud alert: {alert_id}")
        
        alert["id"] = alert_id
        return alert


# Mock implementations
class MockAlertRepository(AlertRepository):
    def __init__(self):
        self.saved_alerts: list[dict] = []
        self.next_id: int = 1
    
    def save(self, alert: dict) -> str:
        pass  # Replace with your implementation


class MockNotificationService(NotificationService):
    def __init__(self):
        self.messages: list[str] = []
        self.should_succeed: bool = True  # Control mock behavior
    
    def send_urgent(self, message: str) -> bool:
        pass  # Replace with your implementation


# Tests using mocks
def test_check_transaction_below_threshold_no_alert():
    """Transaction below threshold should not create alert."""
    pass  # Replace with your implementation


def test_check_transaction_above_threshold_creates_alert():
    """Transaction above threshold should create alert."""
    pass  # Replace with your implementation


def test_check_transaction_sends_notification():
    """Fraud alert should trigger notification."""
    pass  # Replace with your implementation


def test_alert_contains_transaction_details():
    """Alert should contain transaction ID and amount."""
    pass  # Replace with your implementation


# Run tests
print("=== Mock Tests ===")

mock_tests = [
    test_check_transaction_below_threshold_no_alert,
    test_check_transaction_above_threshold_creates_alert,
    test_check_transaction_sends_notification,
    test_alert_contains_transaction_details,
]

passed = 0
for test_fn in mock_tests:
    try:
        test_fn()
        print(f"✓ {test_fn.__name__}")
        passed += 1
    except Exception as e:
        print(f"✗ {test_fn.__name__}: {e}")

print(f"\nResults: {passed}/{len(mock_tests)} passed")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Test mock implementations work
mock_repo = MockAlertRepository()
alert_id = mock_repo.save({"test": "data"})
assert alert_id is not None, "Mock should return ID"
assert len(mock_repo.saved_alerts) == 1, "Mock should track saved alerts"

mock_notifier = MockNotificationService()
result = mock_notifier.send_urgent("Test message")
assert len(mock_notifier.messages) == 1, "Mock should track messages"

# Run actual tests
test_check_transaction_below_threshold_no_alert()
test_check_transaction_above_threshold_creates_alert()
test_check_transaction_sends_notification()
test_alert_contains_transaction_details()
