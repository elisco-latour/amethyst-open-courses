---
id: "finguard_10_04"
title: "Interface Segregation"
type: "coding"
xp: 100
---

# Interface Segregation Principle (ISP)

**Clients should not be forced to depend on methods they don't use.**

## The Problem: The Fat Interface

```python
from abc import ABC, abstractmethod

# ❌ BAD: Forces implementers to implement everything
class BankOperations(ABC):
    @abstractmethod
    def deposit(self, amount): pass
    
    @abstractmethod
    def withdraw(self, amount): pass
    
    @abstractmethod
    def transfer(self, to_account, amount): pass
    
    @abstractmethod
    def apply_interest(self): pass
    
    @abstractmethod
    def issue_checkbook(self): pass
    
    @abstractmethod
    def setup_direct_debit(self): pass

# A basic savings account doesn't need checkbooks!
class SavingsAccount(BankOperations):
    def issue_checkbook(self):
        raise NotImplementedError("Savings accounts don't have checkbooks")
```

## The Solution: Small, Focused Interfaces

```python
class Depositable(ABC):
    @abstractmethod
    def deposit(self, amount: Decimal) -> None: pass

class Withdrawable(ABC):
    @abstractmethod
    def withdraw(self, amount: Decimal) -> bool: pass

class InterestBearing(ABC):
    @abstractmethod
    def apply_interest(self) -> None: pass

class CheckbookEnabled(ABC):
    @abstractmethod
    def issue_checkbook(self) -> str: pass
```

## Composing Interfaces

```python
class SavingsAccount(Depositable, Withdrawable, InterestBearing):
    # Only implements what it needs
    pass

class CheckingAccount(Depositable, Withdrawable, CheckbookEnabled):
    # Different capabilities
    pass
```

## The Analogy: The Swiss Army Knife vs Specialized Tools

- **Fat interface** = Swiss Army knife (has scissors you never use)
- **Segregated interfaces** = Separate knife, separate scissors

A chef doesn't want a knife with a bottle opener attached.

## The "Pro" Tip

> **If you find yourself writing `raise NotImplementedError` or returning dummy values, your interface is too big.**

## Task

Refactor a fat `ReportGenerator` interface into segregated interfaces.

<!-- SEPARATOR -->

# seed_code
from abc import ABC, abstractmethod
from decimal import Decimal

# ❌ BAD: Fat interface
class ReportGeneratorBad(ABC):
    @abstractmethod
    def generate_pdf(self, data: dict) -> bytes: pass
    
    @abstractmethod
    def generate_csv(self, data: dict) -> str: pass
    
    @abstractmethod
    def generate_excel(self, data: dict) -> bytes: pass
    
    @abstractmethod
    def send_email(self, recipient: str, report: bytes) -> bool: pass
    
    @abstractmethod
    def upload_to_cloud(self, report: bytes) -> str: pass


# ✅ GOOD: Segregated interfaces
class PDFGenerator(ABC):
    @abstractmethod
    def generate_pdf(self, data: dict) -> str:
        """Generate PDF and return content as string (simulated)."""
        pass


class CSVGenerator(ABC):
    @abstractmethod
    def generate_csv(self, data: dict) -> str:
        """Generate CSV content."""
        pass


class ReportDistributor(ABC):
    @abstractmethod
    def distribute(self, report: str, destination: str) -> bool:
        """Send report to destination."""
        pass


# Implementations that only implement what they need
class TransactionReportCSV(CSVGenerator):
    """Only generates CSV reports."""
    
    pass  # Replace with your implementation


class TransactionReportPDF(PDFGenerator):
    """Only generates PDF reports."""
    
    pass  # Replace with your implementation


class EmailDistributor(ReportDistributor):
    """Sends reports via email."""
    
    pass  # Replace with your implementation


class CloudUploader(ReportDistributor):
    """Uploads reports to cloud storage."""
    
    pass  # Replace with your implementation


# Test segregated design
print("=== Interface Segregation Demo ===")

# Generate reports
data = {
    "title": "Daily Transactions",
    "date": "2025-01-15",
    "transactions": [
        {"id": "TXN-001", "amount": "500.00"},
        {"id": "TXN-002", "amount": "1500.00"},
    ]
}

csv_gen = TransactionReportCSV()
pdf_gen = TransactionReportPDF()

csv_report = csv_gen.generate_csv(data)
pdf_report = pdf_gen.generate_pdf(data)

print(f"CSV Report:\n{csv_report}")
print(f"\nPDF Report:\n{pdf_report}")

# Distribute
email = EmailDistributor()
cloud = CloudUploader()

print(f"\nEmail sent: {email.distribute(csv_report, 'alice@example.com')}")
print(f"Cloud uploaded: {cloud.distribute(pdf_report, 's3://reports/')}")

<!-- SEPARATOR -->

# validation_code
# Test CSV generator
csv_gen = TransactionReportCSV()
data = {"title": "Test", "transactions": [{"id": "T1", "amount": "100"}]}
csv_output = csv_gen.generate_csv(data)
assert isinstance(csv_output, str), "Should return string"
assert len(csv_output) > 0, "Should generate content"

# Test PDF generator
pdf_gen = TransactionReportPDF()
pdf_output = pdf_gen.generate_pdf(data)
assert isinstance(pdf_output, str), "Should return string"
assert len(pdf_output) > 0, "Should generate content"

# Test distributors
email = EmailDistributor()
result = email.distribute("report content", "test@example.com")
assert result == True, "Should return success"

cloud = CloudUploader()
result = cloud.distribute("report content", "s3://bucket/")
assert result == True, "Should return success"

# Verify segregation: CSV generator shouldn't have PDF methods
assert not hasattr(csv_gen, 'generate_pdf'), "CSV generator should not have generate_pdf"
assert not hasattr(pdf_gen, 'generate_csv'), "PDF generator should not have generate_csv"
