---
id: "finguard_06_05"
title: "The Guard Pattern"
type: "coding"
xp: 100
---

# The Guard Pattern

Beginners write code that looks like an arrow (`>`). Experts write code that looks like a flat list.

Deeply nested `if` statements are hard to read and test. We solve this with **Guard Clauses**.

## The Anti-Pattern: Arrow Code

```python
def process_loan(application):
    if application is not None:
        if application.is_complete:
            if application.score > 700:
                # Actual logic is buried 3 levels deep
                approve_loan()
            else:
                reject_low_score()
        else:
            reject_incomplete()
    else:
        raise ValueError("No application")
```

## The Solution: Fail Fast

Handle the invalid cases **first** and return/raise immediately. The "Happy Path" stays at the bottom, indentation-free.

```python
def process_loan(application):
    # Guard 1: Existence
    if application is None:
        raise ValueError("No application")

    # Guard 2: Completeness
    if not application.is_complete:
        reject_incomplete()
        return

    # Guard 3: Score
    if application.score <= 700:
        reject_low_score()
        return

    # The Happy Path (No indentation!)
    approve_loan()
```

## Benefits
1.  **Readability:** You can read the checks top-to-bottom.
2.  **Cognitive Load:** You don't have to remember "I am inside 3 `if` blocks".
3.  **Diffs:** Changing logic doesn't require re-indenting the whole function.

## Task

Refactor `verify_transaction` to use Guard Clauses.
1. Return `False` immediately if `transaction` is empty.
2. Return `False` immediately if `amount` is negative.
3. Return `False` immediately if `currency` is not "USD".
4. Return `True` at the end (Happy Path).

<!-- SEPARATOR -->

# seed_code
def verify_transaction(transaction: dict) -> bool:
    """
    Refactor this to use Guard Clauses.
    """
    if transaction:
        if transaction.get("amount", 0) > 0:
            if transaction.get("currency") == "USD":
                return True
            else:
                return False
        else:
            return False
    else:
        return False

# Integration
t1 = {"amount": 100, "currency": "USD"}
t2 = {"amount": -50, "currency": "USD"} 
print(f"Valid: {verify_transaction(t1)}")   # True
print(f"Invalid: {verify_transaction(t2)}") # False

<!-- SEPARATOR -->

# validation_code
# Test happy path
assert verify_transaction({"amount": 100, "currency": "USD"}) is True, "Valid transaction should pass"

# Test empty transaction
assert verify_transaction({}) is False, "Empty dict should fail"

# Test negative amount
assert verify_transaction({"amount": -10, "currency": "USD"}) is False, "Negative amount should fail"

# Test wrong currency
assert verify_transaction({"amount": 100, "currency": "EUR"}) is False, "Non-USD currency should fail"

# Test missing currency
assert verify_transaction({"amount": 100}) is False, "Missing currency should fail"
