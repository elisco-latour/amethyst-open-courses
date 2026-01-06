---
id: "finguard_07_03"
title: "Structured Data Exchange"
type: "coding"
xp: 100
---

# Structured Data Exchange

CSV is flat. Reality is nested.
A Customer has Accounts. Accounts have Transactions. This hierarchy requires a **Structured Format**.

**JSON (JavaScript Object Notation)** is the standard for APIs (Modern Banking).

## The Concept: Serialization

**Serialization** is converting a live Python object (RAM) into a string (Text) that can be sent over a wire or saved to disk.
**Deserialization** is the reverse.

Memory (Dict) ➡️ `json.dumps()` ➡️ String (JSON)
String (JSON) ➡️ `json.loads()` ➡️ Memory (Dict)

## Handling Types

JSON supports: Strings, Numbers, Booleans, Lists, Null (`None`).
It does **NOT** support `Decimal` or `datetime`. You must convert them manually.

```python
import json
from decimal import Decimal

data = {
    "amount": float(Decimal("10.50")),  # Convert to float (RISKY but required)
    "is_active": True
}
payload = json.dumps(data)
```

## Task

Create a `serialize_transaction` function.
1.  Accept a dict which `amount` as a `Decimal`.
2.  Convert `Decimal` to `str` (Safe) or `float` (Standard). Let's use `str` to preserve precision for the string payload.
3.  Return the JSON string.

<!-- SEPARATOR -->

# seed_code
import json
from decimal import Decimal

def serialize_transaction(txn: dict) -> str:
    """
    Converts txn dict to JSON string.
    Converts Decimal values to string to preserve precision.
    """
    # 1. Prepare copy to avoid mutating input
    # 2. Convert Decimal to str
    # 3. json.dumps
    pass

# Integration
t = {"id": 1, "amt": Decimal("100.00")}
print(serialize_transaction(t))

<!-- SEPARATOR -->

# validation_code
import json
from decimal import Decimal

t = {"id": 99, "val": Decimal("123.45")}
res = serialize_transaction(t)
parsed = json.loads(res)

assert parsed["val"] == "123.45"
assert parsed["id"] == 99
print("Validation passed!")
    data = json.load(file)  # Note: load() not loads()

# Writing JSON file
with open("output.json", "w") as file:
    json.dump(data, file, indent=2)  # indent for readability
```

## The Gotcha: `loads` vs `load`

- `json.loads(string)` — Load from **S**tring
- `json.load(file)` — Load from file object

## The Analogy: The Document vs The Spreadsheet

- **CSV** = A spreadsheet (rows and columns)
- **JSON** = A document with sections and subsections

## The "Pro" Tip

> **Use `indent=2` when writing JSON for human reading. Omit it for machine-to-machine communication (smaller files).**

```python
# For humans (debugging, configs)
json.dump(data, file, indent=2)

# For machines (APIs, data transfer)
json.dump(data, file)  # No indent = compact
```

## Task

Parse the JSON transaction batch and extract summary statistics.

<!-- SEPARATOR -->

# seed_code
import json
from decimal import Decimal

# Simulated JSON API response
json_content: str = """
{
    "batch_id": "BATCH-2025-01-15",
    "processed_at": "2025-01-15T18:00:00Z",
    "transactions": [
        {"id": "TXN-001", "amount": 500.00, "status": "completed"},
        {"id": "TXN-002", "amount": 1500.00, "status": "pending"},
        {"id": "TXN-003", "amount": 750.00, "status": "completed"},
        {"id": "TXN-004", "amount": 3000.00, "status": "failed"},
        {"id": "TXN-005", "amount": 250.00, "status": "completed"}
    ]
}
"""

# Parse the JSON
data: dict = json.loads(json_content)

batch_id: str = ""
total_amount: Decimal = Decimal("0.00")
completed_count: int = 0
pending_count: int = 0
failed_count: int = 0

# Extract and analyze the data



print("=== JSON Batch Analysis ===")
print(f"Batch ID: {batch_id}")
print(f"Total Amount: ${total_amount:,.2f}")
print(f"Completed: {completed_count}")
print(f"Pending: {pending_count}")
print(f"Failed: {failed_count}")

<!-- SEPARATOR -->

# validation_code
from decimal import Decimal
assert batch_id == "BATCH-2025-01-15", "Should extract batch_id"
assert total_amount == Decimal("6000.00"), "Total: 500+1500+750+3000+250 = 6000"
assert completed_count == 3, "Should have 3 completed"
assert pending_count == 1, "Should have 1 pending"
assert failed_count == 1, "Should have 1 failed"
