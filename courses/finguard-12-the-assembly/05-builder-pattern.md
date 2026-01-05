---
id: "finguard_12_05"
title: "Builder Pattern"
type: "coding"
xp: 100
---

# Builder Pattern

Construct **complex objects step by step**.

## The Problem

```python
# ❌ Constructor with too many parameters
report = FinancialReport(
    title="Q3 Transactions",
    start_date=date(2024, 7, 1),
    end_date=date(2024, 9, 30),
    include_deposits=True,
    include_withdrawals=True,
    include_transfers=False,
    include_fees=True,
    group_by="account",
    sort_by="date",
    format="pdf",
    page_size="A4",
    include_charts=True,
    ...  # 20 more parameters
)
```

## The Solution: Builder Pattern

```python
# ✅ Build step by step with clear method names
report = (
    ReportBuilder()
    .title("Q3 Transactions")
    .date_range(date(2024, 7, 1), date(2024, 9, 30))
    .include_deposits()
    .include_withdrawals()
    .include_fees()
    .group_by("account")
    .sort_by("date")
    .as_pdf()
    .build()
)
```

## The Flow

```
Builder()
    │
    ├──► .title("...")────────────────► returns Builder
    │
    ├──► .date_range(...)─────────────► returns Builder
    │
    ├──► .include_deposits()──────────► returns Builder
    │
    └──► .build()─────────────────────► returns Final Object
```

## Why Builder?

| Benefit | Explanation |
|---------|-------------|
| **Readable** | Method names describe what's being set |
| **Flexible** | Set only what you need |
| **Immutable result** | Build once, can't modify |
| **Validation** | Check constraints in `build()` |

## The Analogy: The Sandwich Shop

"I'll have wheat bread, turkey, no mayo, extra pickles, toasted."

Each instruction is a step. The sandwich maker (builder) assembles everything. You get a complete sandwich at the end.

## The "Pro" Tip

> **Return `self` from each method to enable method chaining. Call `build()` to get the final immutable object.**

## Task

Build a `TransactionReportBuilder` for generating fraud reports.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal
from datetime import date

# The complex object to build
class FraudReport:
    def __init__(
        self,
        title: str,
        start_date: date,
        end_date: date,
        accounts: list[str],
        min_amount: Decimal,
        max_amount: Decimal | None,
        transaction_types: list[str],
        include_summary: bool,
        include_details: bool,
        output_format: str
    ):
        self.title = title
        self.start_date = start_date
        self.end_date = end_date
        self.accounts = accounts
        self.min_amount = min_amount
        self.max_amount = max_amount
        self.transaction_types = transaction_types
        self.include_summary = include_summary
        self.include_details = include_details
        self.output_format = output_format
    
    def __str__(self) -> str:
        return f"FraudReport('{self.title}', {len(self.accounts)} accounts, {self.output_format})"


class FraudReportBuilder:
    """Builder for creating FraudReport objects."""
    
    def __init__(self):
        # Initialize with defaults
        self._title: str = "Fraud Report"
        self._start_date: date = date.today()
        self._end_date: date = date.today()
        self._accounts: list[str] = []
        self._min_amount: Decimal = Decimal("0")
        self._max_amount: Decimal | None = None
        self._transaction_types: list[str] = []
        self._include_summary: bool = True
        self._include_details: bool = False
        self._output_format: str = "text"
    
    def title(self, title: str) -> "FraudReportBuilder":
        """Set report title."""
        pass  # Replace: set _title and return self
    
    def date_range(self, start: date, end: date) -> "FraudReportBuilder":
        """Set date range for the report."""
        pass  # Replace: set _start_date, _end_date and return self
    
    def for_accounts(self, *account_ids: str) -> "FraudReportBuilder":
        """Filter to specific accounts."""
        pass  # Replace: set _accounts and return self
    
    def amount_range(
        self, 
        min_amount: Decimal, 
        max_amount: Decimal | None = None
    ) -> "FraudReportBuilder":
        """Filter by amount range."""
        pass  # Replace: set _min_amount, _max_amount and return self
    
    def include_deposits(self) -> "FraudReportBuilder":
        """Include deposit transactions."""
        pass  # Replace: append to _transaction_types and return self
    
    def include_withdrawals(self) -> "FraudReportBuilder":
        """Include withdrawal transactions."""
        pass  # Replace: append to _transaction_types and return self
    
    def include_transfers(self) -> "FraudReportBuilder":
        """Include transfer transactions."""
        pass  # Replace: append to _transaction_types and return self
    
    def with_summary(self) -> "FraudReportBuilder":
        """Include summary section."""
        pass  # Replace: set _include_summary and return self
    
    def with_details(self) -> "FraudReportBuilder":
        """Include detailed transaction list."""
        pass  # Replace: set _include_details and return self
    
    def as_pdf(self) -> "FraudReportBuilder":
        """Output as PDF format."""
        pass  # Replace: set _output_format and return self
    
    def as_csv(self) -> "FraudReportBuilder":
        """Output as CSV format."""
        pass  # Replace: set _output_format and return self
    
    def build(self) -> FraudReport:
        """Build and validate the final report."""
        # Validation
        if self._end_date < self._start_date:
            raise ValueError("End date must be after start date")
        
        if not self._transaction_types:
            # Default to all types
            self._transaction_types = ["DEPOSIT", "WITHDRAWAL", "TRANSFER"]
        
        return FraudReport(
            title=self._title,
            start_date=self._start_date,
            end_date=self._end_date,
            accounts=self._accounts,
            min_amount=self._min_amount,
            max_amount=self._max_amount,
            transaction_types=self._transaction_types,
            include_summary=self._include_summary,
            include_details=self._include_details,
            output_format=self._output_format
        )


# Test the builder
print("=== Builder Pattern ===")

# Build a simple report
simple_report = (
    FraudReportBuilder()
    .title("Weekly Review")
    .date_range(date(2024, 1, 1), date(2024, 1, 7))
    .build()
)
print(f"Simple: {simple_report}")

# Build a complex report
complex_report = (
    FraudReportBuilder()
    .title("Q4 High-Value Fraud Analysis")
    .date_range(date(2024, 10, 1), date(2024, 12, 31))
    .for_accounts("ACC-100", "ACC-200", "ACC-300")
    .amount_range(Decimal("10000.00"), Decimal("100000.00"))
    .include_withdrawals()
    .include_transfers()
    .with_summary()
    .with_details()
    .as_pdf()
    .build()
)
print(f"Complex: {complex_report}")
print(f"  - Accounts: {complex_report.accounts}")
print(f"  - Types: {complex_report.transaction_types}")
print(f"  - Format: {complex_report.output_format}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
from datetime import date

# Test builder returns self for chaining
builder = FraudReportBuilder()
result = builder.title("Test")
assert result is builder, "title() should return self"

# Test building with all options
report = (
    FraudReportBuilder()
    .title("Full Test Report")
    .date_range(date(2024, 1, 1), date(2024, 3, 31))
    .for_accounts("ACC-001", "ACC-002")
    .amount_range(Decimal("1000.00"), Decimal("50000.00"))
    .include_deposits()
    .include_withdrawals()
    .with_summary()
    .with_details()
    .as_csv()
    .build()
)

assert report.title == "Full Test Report", "Title not set"
assert report.start_date == date(2024, 1, 1), "Start date not set"
assert len(report.accounts) == 2, "Accounts not set"
assert report.min_amount == Decimal("1000.00"), "Min amount not set"
assert "DEPOSIT" in report.transaction_types, "Deposit type not included"
assert "WITHDRAWAL" in report.transaction_types, "Withdrawal type not included"
assert report.output_format == "csv", "Format not set"

# Test validation
try:
    FraudReportBuilder().date_range(date(2024, 12, 31), date(2024, 1, 1)).build()
    assert False, "Should raise ValueError for invalid dates"
except ValueError:
    pass
