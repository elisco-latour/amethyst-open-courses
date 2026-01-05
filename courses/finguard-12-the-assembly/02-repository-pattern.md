---
id: "finguard_12_02"
title: "Repository Pattern"
type: "coding"
xp: 100
---

# Repository Pattern

Separate **data access** from **business logic**.

## The Problem

```python
# ❌ Business logic mixed with database access
class TransactionService:
    def process(self, txn: Transaction):
        # Business logic
        if txn.amount > Decimal("10000"):
            flag_for_review(txn)
        
        # Database access (what if we switch databases?)
        conn = sqlite3.connect("bank.db")
        cursor = conn.cursor()
        cursor.execute("INSERT INTO transactions VALUES (?, ?)", ...)
        conn.commit()
```

## The Solution: Repository Pattern

```python
# ✅ Repository handles all data access
class TransactionRepository(ABC):
    @abstractmethod
    def save(self, txn: Transaction) -> None:
        pass
    
    @abstractmethod
    def find_by_id(self, txn_id: str) -> Transaction | None:
        pass

# Service only knows about business logic
class TransactionService:
    def __init__(self, repo: TransactionRepository):
        self.repo = repo
    
    def process(self, txn: Transaction):
        if txn.amount > Decimal("10000"):
            flag_for_review(txn)
        self.repo.save(txn)  # Doesn't care HOW it's saved
```

## Why Repository?

| Benefit | Explanation |
|---------|-------------|
| **Testability** | Replace with in-memory repo for tests |
| **Flexibility** | Switch from SQLite to PostgreSQL easily |
| **Single Responsibility** | Services focus on logic, repos on storage |
| **Domain isolation** | Business objects don't know about databases |

## The Analogy: The Library System

You (the reader) don't organize shelves. You ask the librarian (repository):
- "Find me the book with ISBN 123"
- "Add this new book"
- "Find all books by author X"

The librarian knows the filing system. You don't have to.

## The "Pro" Tip

> **Repositories return domain objects, not database rows. They translate between storage and your business model.**

## Task

Build a `TransactionRepository` with in-memory and file-based implementations.

<!-- SEPARATOR -->

# seed_code
from abc import ABC, abstractmethod
from decimal import Decimal
from datetime import datetime
import json

# Domain entity
class Transaction:
    def __init__(self, txn_id: str, account_id: str, amount: Decimal, 
                 txn_type: str, timestamp: datetime):
        self.txn_id = txn_id
        self.account_id = account_id
        self.amount = amount
        self.txn_type = txn_type
        self.timestamp = timestamp
    
    def to_dict(self) -> dict:
        return {
            "txn_id": self.txn_id,
            "account_id": self.account_id,
            "amount": str(self.amount),
            "txn_type": self.txn_type,
            "timestamp": self.timestamp.isoformat()
        }
    
    @classmethod
    def from_dict(cls, data: dict) -> "Transaction":
        return cls(
            txn_id=data["txn_id"],
            account_id=data["account_id"],
            amount=Decimal(data["amount"]),
            txn_type=data["txn_type"],
            timestamp=datetime.fromisoformat(data["timestamp"])
        )


# Repository interface
class TransactionRepository(ABC):
    @abstractmethod
    def save(self, txn: Transaction) -> None:
        """Save or update a transaction."""
        pass
    
    @abstractmethod
    def find_by_id(self, txn_id: str) -> Transaction | None:
        """Find transaction by ID. Returns None if not found."""
        pass
    
    @abstractmethod
    def find_by_account(self, account_id: str) -> list[Transaction]:
        """Find all transactions for an account."""
        pass
    
    @abstractmethod
    def find_all(self) -> list[Transaction]:
        """Return all transactions."""
        pass


# In-memory implementation (for testing)
class InMemoryTransactionRepository(TransactionRepository):
    def __init__(self):
        self._storage: dict[str, Transaction] = {}
    
    def save(self, txn: Transaction) -> None:
        pass  # Replace with your implementation
    
    def find_by_id(self, txn_id: str) -> Transaction | None:
        pass  # Replace with your implementation
    
    def find_by_account(self, account_id: str) -> list[Transaction]:
        pass  # Replace with your implementation
    
    def find_all(self) -> list[Transaction]:
        pass  # Replace with your implementation


# JSON file implementation (for persistence)
class JsonFileTransactionRepository(TransactionRepository):
    def __init__(self, filepath: str):
        self.filepath = filepath
        self._load()
    
    def _load(self) -> None:
        """Load transactions from file."""
        # For this exercise, we'll use in-memory but simulate file ops
        self._storage: dict[str, Transaction] = {}
    
    def _save_to_file(self) -> None:
        """Save all transactions to file (simulated)."""
        # In real code: json.dump to file
        pass
    
    def save(self, txn: Transaction) -> None:
        pass  # Replace with your implementation
    
    def find_by_id(self, txn_id: str) -> Transaction | None:
        pass  # Replace with your implementation
    
    def find_by_account(self, account_id: str) -> list[Transaction]:
        pass  # Replace with your implementation
    
    def find_all(self) -> list[Transaction]:
        pass  # Replace with your implementation


# Test the repositories
print("=== Repository Pattern ===")

# Test with in-memory repo
repo = InMemoryTransactionRepository()

# Create transactions
t1 = Transaction("TXN-001", "ACC-100", Decimal("500.00"), "DEPOSIT", datetime.now())
t2 = Transaction("TXN-002", "ACC-100", Decimal("200.00"), "WITHDRAWAL", datetime.now())
t3 = Transaction("TXN-003", "ACC-200", Decimal("1000.00"), "DEPOSIT", datetime.now())

# Save
repo.save(t1)
repo.save(t2)
repo.save(t3)
print(f"Saved {len(repo.find_all())} transactions")

# Find by ID
found = repo.find_by_id("TXN-001")
if found:
    print(f"Found: {found.txn_id} - ${found.amount}")

# Find by account
acc_txns = repo.find_by_account("ACC-100")
print(f"Account ACC-100 has {len(acc_txns)} transactions")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
from datetime import datetime

# Test InMemoryTransactionRepository
repo = InMemoryTransactionRepository()

t1 = Transaction("TXN-001", "ACC-100", Decimal("500.00"), "DEPOSIT", datetime.now())
t2 = Transaction("TXN-002", "ACC-100", Decimal("200.00"), "WITHDRAWAL", datetime.now())
t3 = Transaction("TXN-003", "ACC-200", Decimal("1000.00"), "DEPOSIT", datetime.now())

repo.save(t1)
repo.save(t2)
repo.save(t3)

# Test find_all
all_txns = repo.find_all()
assert len(all_txns) == 3, "Should have 3 transactions"

# Test find_by_id
found = repo.find_by_id("TXN-001")
assert found is not None, "Should find TXN-001"
assert found.amount == Decimal("500.00"), "Amount should match"

# Test find_by_account
acc_txns = repo.find_by_account("ACC-100")
assert len(acc_txns) == 2, "ACC-100 should have 2 transactions"

# Test not found
not_found = repo.find_by_id("TXN-999")
assert not_found is None, "Should return None for missing"
