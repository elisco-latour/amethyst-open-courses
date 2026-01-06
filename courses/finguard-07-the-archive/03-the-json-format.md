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

# Test serialization
t = {"id": 99, "val": Decimal("123.45")}
res = serialize_transaction(t)
parsed = json.loads(res)

assert parsed["val"] == "123.45", "Decimal should be converted to string"
assert parsed["id"] == 99, "Non-Decimal values should be preserved"

# Test with nested data
t2 = {"txn": "A", "amount": Decimal("50.00"), "meta": {"source": "web"}}
res2 = serialize_transaction(t2)
parsed2 = json.loads(res2)
assert parsed2["amount"] == "50.00", "Should handle Decimal in nested dict"
