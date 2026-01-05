---
id: "finguard_13_02"
title: "Data Layer"
type: "coding"
xp: 100
---

# Data Layer: Repository Implementation

## The Repository Pattern in Action

The repository abstracts data storage. Your business logic doesn't care if data comes from a file, database, or API.

```
┌─────────────────┐     ┌──────────────────────┐
│  Business Logic │────►│ TransactionRepository│ (interface)
└─────────────────┘     └──────────────────────┘
                                   │
                    ┌──────────────┼──────────────┐
                    ▼              ▼              ▼
            ┌────────────┐ ┌────────────┐ ┌────────────┐
            │  InMemory  │ │   JSON     │ │  SQLite    │
            │    Repo    │ │   Repo     │ │   Repo     │
            └────────────┘ └────────────┘ └────────────┘
```

## Query Methods

```python
class TransactionRepository(ABC):
    # CRUD operations
    def save(self, txn: Transaction) -> None: ...
    def find_by_id(self, txn_id: str) -> Transaction | None: ...
    def find_all(self) -> list[Transaction]: ...
    def delete(self, txn_id: str) -> bool: ...
    
    # Query methods
    def find_by_account(self, account_id: str) -> list[Transaction]: ...
    def find_by_date_range(self, start: datetime, end: datetime) -> list[Transaction]: ...
    def find_by_status(self, status: str) -> list[Transaction]: ...
    def find_flagged(self) -> list[Transaction]: ...
```

## Why Separate Repositories?

| Concern | Repository Responsibility |
|---------|---------------------------|
| Testing | Use `InMemoryRepository` |
| Development | Use `JsonFileRepository` |
| Production | Use `PostgreSQLRepository` |

Same interface, different implementations. Your business logic never changes.

## Task

Implement a full `TransactionRepository` with query capabilities.

<!-- SEPARATOR -->

# seed_code
from abc import ABC, abstractmethod
from decimal import Decimal
from datetime import datetime

# Simplified Transaction for this exercise
class Transaction:
    def __init__(self, txn_id: str, account_id: str, amount: Decimal,
                 txn_type: str, timestamp: datetime, status: str = "PENDING"):
        self.txn_id = txn_id
        self.account_id = account_id
        self.amount = amount
        self.txn_type = txn_type
        self.timestamp = timestamp
        self.status = status
        self.flag_reason: str | None = None


# Repository interface
class TransactionRepository(ABC):
    @abstractmethod
    def save(self, txn: Transaction) -> None:
        """Save or update a transaction."""
        pass
    
    @abstractmethod
    def find_by_id(self, txn_id: str) -> Transaction | None:
        """Find by ID, return None if not found."""
        pass
    
    @abstractmethod
    def find_all(self) -> list[Transaction]:
        """Return all transactions."""
        pass
    
    @abstractmethod
    def find_by_account(self, account_id: str) -> list[Transaction]:
        """Find all transactions for an account."""
        pass
    
    @abstractmethod
    def find_by_status(self, status: str) -> list[Transaction]:
        """Find all transactions with given status."""
        pass
    
    @abstractmethod
    def find_by_amount_range(
        self, 
        min_amount: Decimal, 
        max_amount: Decimal
    ) -> list[Transaction]:
        """Find transactions within amount range."""
        pass
    
    @abstractmethod
    def count(self) -> int:
        """Return total count of transactions."""
        pass


# In-memory implementation
class InMemoryTransactionRepository(TransactionRepository):
    def __init__(self):
        self._storage: dict[str, Transaction] = {}
    
    def save(self, txn: Transaction) -> None:
        pass  # Replace with your implementation
    
    def find_by_id(self, txn_id: str) -> Transaction | None:
        pass  # Replace with your implementation
    
    def find_all(self) -> list[Transaction]:
        pass  # Replace with your implementation
    
    def find_by_account(self, account_id: str) -> list[Transaction]:
        pass  # Replace with your implementation
    
    def find_by_status(self, status: str) -> list[Transaction]:
        pass  # Replace with your implementation
    
    def find_by_amount_range(
        self, 
        min_amount: Decimal, 
        max_amount: Decimal
    ) -> list[Transaction]:
        pass  # Replace with your implementation
    
    def count(self) -> int:
        pass  # Replace with your implementation


# Test the repository
print("=== Transaction Repository ===")

repo = InMemoryTransactionRepository()

# Create test data
transactions = [
    Transaction("TXN-001", "ACC-100", Decimal("500.00"), "DEPOSIT", 
                datetime(2024, 1, 15, 10, 30), "COMPLETED"),
    Transaction("TXN-002", "ACC-100", Decimal("200.00"), "WITHDRAWAL",
                datetime(2024, 1, 15, 14, 0), "COMPLETED"),
    Transaction("TXN-003", "ACC-200", Decimal("15000.00"), "DEPOSIT",
                datetime(2024, 1, 16, 9, 0), "FLAGGED"),
    Transaction("TXN-004", "ACC-100", Decimal("1000.00"), "TRANSFER",
                datetime(2024, 1, 16, 11, 30), "PENDING"),
    Transaction("TXN-005", "ACC-300", Decimal("75.50"), "WITHDRAWAL",
                datetime(2024, 1, 16, 16, 45), "COMPLETED"),
]

# Save all
for txn in transactions:
    repo.save(txn)
print(f"Saved {repo.count()} transactions")

# Query tests
print(f"\nACC-100 transactions: {len(repo.find_by_account('ACC-100'))}")
print(f"FLAGGED transactions: {len(repo.find_by_status('FLAGGED'))}")
print(f"High value ($1000-$20000): {len(repo.find_by_amount_range(Decimal('1000'), Decimal('20000')))}")

# Find specific transaction
found = repo.find_by_id("TXN-003")
if found:
    print(f"\nFound TXN-003: ${found.amount} ({found.status})")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
from datetime import datetime

repo = InMemoryTransactionRepository()

# Create test data
t1 = Transaction("TXN-001", "ACC-100", Decimal("500.00"), "DEPOSIT", datetime.now(), "COMPLETED")
t2 = Transaction("TXN-002", "ACC-100", Decimal("200.00"), "WITHDRAWAL", datetime.now(), "COMPLETED")
t3 = Transaction("TXN-003", "ACC-200", Decimal("15000.00"), "DEPOSIT", datetime.now(), "FLAGGED")
t4 = Transaction("TXN-004", "ACC-100", Decimal("1000.00"), "TRANSFER", datetime.now(), "PENDING")

repo.save(t1)
repo.save(t2)
repo.save(t3)
repo.save(t4)

# Test count
assert repo.count() == 4, f"Count should be 4, got {repo.count()}"

# Test find_by_id
found = repo.find_by_id("TXN-001")
assert found is not None, "Should find TXN-001"
assert found.amount == Decimal("500.00"), "Amount should match"

not_found = repo.find_by_id("TXN-999")
assert not_found is None, "Should return None for missing"

# Test find_by_account
acc_txns = repo.find_by_account("ACC-100")
assert len(acc_txns) == 3, f"ACC-100 should have 3 transactions, got {len(acc_txns)}"

# Test find_by_status
flagged = repo.find_by_status("FLAGGED")
assert len(flagged) == 1, "Should have 1 flagged transaction"
assert flagged[0].txn_id == "TXN-003", "Flagged should be TXN-003"

# Test find_by_amount_range
high_value = repo.find_by_amount_range(Decimal("1000"), Decimal("20000"))
assert len(high_value) == 2, f"Should have 2 high-value transactions, got {len(high_value)}"
