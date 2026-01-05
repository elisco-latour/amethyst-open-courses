---
id: "finguard_11_02"
title: "Test Organization"
type: "coding"
xp: 100
---

# Test Organization

Real projects have hundreds of tests. Organization matters.

## The Arrange-Act-Assert Pattern

Every test has three parts:

```python
def test_withdrawal_success():
    # ARRANGE: Set up test data
    account = BankAccount("ACC-001", Decimal("1000.00"))
    
    # ACT: Perform the action
    result = account.withdraw(Decimal("500.00"))
    
    # ASSERT: Check the outcome
    assert result == True
    assert account.balance == Decimal("500.00")
```

## Test Fixtures with pytest

Fixtures provide reusable test data:

```python
import pytest

@pytest.fixture
def sample_account():
    """Create a test account with $1000 balance."""
    return BankAccount("ACC-TEST", Decimal("1000.00"))

def test_deposit(sample_account):  # Fixture injected automatically
    sample_account.deposit(Decimal("500.00"))
    assert sample_account.balance == Decimal("1500.00")

def test_withdraw(sample_account):  # Fresh fixture for each test
    sample_account.withdraw(Decimal("200.00"))
    assert sample_account.balance == Decimal("800.00")
```

## Test Classes for Grouping

```python
class TestBankAccountDeposit:
    """Tests for deposit functionality."""
    
    def test_deposit_increases_balance(self):
        account = BankAccount("A1", Decimal("100"))
        account.deposit(Decimal("50"))
        assert account.balance == Decimal("150")
    
    def test_deposit_accepts_decimal(self):
        account = BankAccount("A1", Decimal("0"))
        account.deposit(Decimal("99.99"))
        assert account.balance == Decimal("99.99")


class TestBankAccountWithdrawal:
    """Tests for withdrawal functionality."""
    
    def test_withdrawal_decreases_balance(self):
        ...
```

## The "Pro" Tip

> **Each test should be independent. Never rely on the order tests run or state from another test.**

```python
# ❌ BAD: Tests depend on each other
def test_1_create_account():
    global account  # Shared state!
    account = BankAccount("A1", Decimal("100"))

def test_2_deposit():
    account.deposit(Decimal("50"))  # Depends on test_1!

# ✅ GOOD: Each test is independent
def test_create_and_deposit():
    account = BankAccount("A1", Decimal("100"))
    account.deposit(Decimal("50"))
    assert account.balance == Decimal("150")
```

## Task

Organize tests for a TransactionValidator using proper structure.

<!-- SEPARATOR -->

# seed_code
from decimal import Decimal

class TransactionValidator:
    """Validates transaction data."""
    
    def __init__(self, max_amount: Decimal = Decimal("100000.00")):
        self.max_amount = max_amount
    
    def validate_amount(self, amount: Decimal) -> tuple[bool, str]:
        if amount <= Decimal("0"):
            return (False, "Amount must be positive")
        if amount > self.max_amount:
            return (False, f"Amount exceeds maximum of ${self.max_amount}")
        return (True, "Valid")
    
    def validate_account_id(self, account_id: str) -> tuple[bool, str]:
        if not account_id:
            return (False, "Account ID is required")
        if not account_id.startswith("ACC-"):
            return (False, "Account ID must start with 'ACC-'")
        if len(account_id) < 7:
            return (False, "Account ID too short")
        return (True, "Valid")


# Simulated fixtures (in real pytest, use @pytest.fixture)
def create_validator() -> TransactionValidator:
    """Fixture: Create validator with default settings."""
    return TransactionValidator()


def create_strict_validator() -> TransactionValidator:
    """Fixture: Create validator with low max amount."""
    return TransactionValidator(max_amount=Decimal("1000.00"))


class TestValidateAmount:
    """Tests for amount validation."""
    
    def test_valid_amount_passes(self):
        # ARRANGE
        pass  # Replace with your implementation
        
        # ACT
        
        # ASSERT
    
    def test_zero_amount_fails(self):
        pass  # Replace with your implementation
    
    def test_negative_amount_fails(self):
        pass  # Replace with your implementation
    
    def test_amount_exceeds_max_fails(self):
        pass  # Replace with your implementation


class TestValidateAccountId:
    """Tests for account ID validation."""
    
    def test_valid_account_id_passes(self):
        pass  # Replace with your implementation
    
    def test_empty_account_id_fails(self):
        pass  # Replace with your implementation
    
    def test_invalid_prefix_fails(self):
        pass  # Replace with your implementation
    
    def test_too_short_account_id_fails(self):
        pass  # Replace with your implementation


# Run tests
print("=== Running Organized Tests ===")

test_classes = [TestValidateAmount, TestValidateAccountId]

passed = 0
failed = 0

for test_class in test_classes:
    print(f"\n{test_class.__name__}:")
    instance = test_class()
    
    for method_name in dir(instance):
        if method_name.startswith("test_"):
            try:
                method = getattr(instance, method_name)
                method()
                print(f"  ✓ {method_name}")
                passed += 1
            except Exception as e:
                print(f"  ✗ {method_name}: {e}")
                failed += 1

print(f"\nTotal: {passed} passed, {failed} failed")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
import inspect

# Run all tests
amt_tests = TestValidateAmount()
amt_tests.test_valid_amount_passes()
amt_tests.test_zero_amount_fails()
amt_tests.test_negative_amount_fails()
amt_tests.test_amount_exceeds_max_fails()

acc_tests = TestValidateAccountId()
acc_tests.test_valid_account_id_passes()
acc_tests.test_empty_account_id_fails()
acc_tests.test_invalid_prefix_fails()
acc_tests.test_too_short_account_id_fails()

# Verify tests have assertions
source = inspect.getsource(TestValidateAmount.test_valid_amount_passes)
assert "assert" in source, "Tests should contain assertions"
