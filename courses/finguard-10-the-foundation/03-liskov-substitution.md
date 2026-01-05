---
id: "finguard_10_03"
title: "Liskov Substitution"
type: "coding"
xp: 100
---

# Liskov Substitution Principle (LSP)

**Subtypes must be substitutable for their base types.**

## The Problem: The Square/Rectangle Trap

```python
class Rectangle:
    def __init__(self, width: int, height: int):
        self.width = width
        self.height = height
    
    def set_width(self, w: int):
        self.width = w
    
    def set_height(self, h: int):
        self.height = h
    
    def area(self) -> int:
        return self.width * self.height

class Square(Rectangle):  # Is a Square really a Rectangle?
    def set_width(self, w: int):
        self.width = w
        self.height = w  # 🚩 Surprise behavior!
    
    def set_height(self, h: int):
        self.width = h
        self.height = h  # 🚩 Surprise behavior!
```

Code that uses `Rectangle` might break with `Square`:

```python
def test_rectangle(rect: Rectangle):
    rect.set_width(5)
    rect.set_height(4)
    assert rect.area() == 20  # Fails for Square! (16)
```

## The Rule: Subtypes Can't Surprise

If `B` extends `A`, then anywhere you use `A`, you should be able to use `B` **without changing behavior**.

## Banking Example: Account Types

```python
from decimal import Decimal

class Account:
    def withdraw(self, amount: Decimal) -> bool:
        """Returns True if withdrawal succeeded."""
        ...

class SavingsAccount(Account):
    def withdraw(self, amount: Decimal) -> bool:
        # ✅ OK: Same behavior, with added constraint
        if self.withdrawals_this_month >= 6:
            return False
        return super().withdraw(amount)

class FrozenAccount(Account):
    def withdraw(self, amount: Decimal) -> bool:
        # ❌ VIOLATES LSP: Never works!
        return False  # Callers expect this might succeed
```

## The Analogy: The Job Replacement

If you hire a "Python Developer" replacement, you expect them to:
- Write Python code ✅
- Debug Python issues ✅
- NOT refuse to code entirely ❌

A subtype that refuses to do what the parent does violates expectations.

## The "Pro" Tip

> **LSP violation signs: methods that throw "not supported" exceptions, methods that always return failure, or methods with surprise side effects.**

## Task

Fix the LSP violation in a payment processing hierarchy.

<!-- SEPARATOR -->

# seed_code
from abc import ABC, abstractmethod
from decimal import Decimal

# ❌ BAD DESIGN: Violates LSP
class PaymentMethodBad:
    def process_payment(self, amount: Decimal) -> bool:
        """Process a payment. Returns True if successful."""
        raise NotImplementedError()
    
    def refund(self, transaction_id: str) -> bool:
        """Refund a payment. Returns True if successful."""
        raise NotImplementedError()

class CreditCardBad(PaymentMethodBad):
    def process_payment(self, amount: Decimal) -> bool:
        return True  # OK
    
    def refund(self, transaction_id: str) -> bool:
        return True  # OK

class CashPaymentBad(PaymentMethodBad):
    def process_payment(self, amount: Decimal) -> bool:
        return True  # OK
    
    def refund(self, transaction_id: str) -> bool:
        # ❌ Cash can't be refunded like this!
        raise Exception("Cash refunds must be handled manually")


# ✅ GOOD DESIGN: Fix LSP violation with proper abstraction
class PaymentProcessor(ABC):
    """Can process payments."""
    
    @abstractmethod
    def process_payment(self, amount: Decimal) -> bool:
        pass


class RefundablePayment(PaymentProcessor):
    """Payment method that supports refunds."""
    
    @abstractmethod
    def refund(self, transaction_id: str) -> bool:
        pass


class CreditCard(RefundablePayment):
    """Credit card - supports both payment and refund."""
    
    pass  # Replace with your implementation


class CashPayment(PaymentProcessor):
    """Cash payment - only supports payment, not refund."""
    
    pass  # Replace with your implementation


# Test LSP compliance
print("=== LSP Compliant Design ===")

def process_order(processor: PaymentProcessor, amount: Decimal) -> bool:
    """Works with ANY PaymentProcessor."""
    return processor.process_payment(amount)

def refund_order(processor: RefundablePayment, txn_id: str) -> bool:
    """Only works with RefundablePayment."""
    return processor.refund(txn_id)

# Credit card supports both
cc = CreditCard()
print(f"CC Payment: {process_order(cc, Decimal('100.00'))}")
print(f"CC Refund: {refund_order(cc, 'TXN-001')}")

# Cash only supports payment
cash = CashPayment()
print(f"Cash Payment: {process_order(cash, Decimal('50.00'))}")
# refund_order(cash, 'TXN-002')  # Would be a type error!

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal

# Test CreditCard
cc = CreditCard()
assert cc.process_payment(Decimal("100.00")) == True, "CC should process"
assert cc.refund("TXN-001") == True, "CC should refund"

# Test CashPayment
cash = CashPayment()
assert cash.process_payment(Decimal("50.00")) == True, "Cash should process"
assert not hasattr(cash, 'refund') or not callable(getattr(cash, 'refund', None)), "Cash should not have refund method"

# Test inheritance
assert isinstance(cc, PaymentProcessor), "CC should be PaymentProcessor"
assert isinstance(cc, RefundablePayment), "CC should be RefundablePayment"
assert isinstance(cash, PaymentProcessor), "Cash should be PaymentProcessor"
assert not isinstance(cash, RefundablePayment), "Cash should NOT be RefundablePayment"
