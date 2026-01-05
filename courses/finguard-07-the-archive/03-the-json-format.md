---
id: "finguard_07_03"
title: "The JSON Format"
type: "coding"
xp: 100
---

# The JSON Format

**JSON (JavaScript Object Notation)** is the format of APIs. When systems talk to each other, they usually speak JSON.

## JSON vs CSV

| Feature | CSV | JSON |
|---------|-----|------|
| Structure | Flat tables | Nested hierarchies |
| Types | Everything is string | Strings, numbers, booleans, null |
| Readability | Spreadsheet-like | Document-like |
| Use case | Data export/import | APIs, configs |

## JSON Structure

```json
{
    "transaction_id": "TXN-001",
    "amount": 500.00,
    "is_verified": true,
    "tags": ["urgent", "international"],
    "metadata": {
        "source": "mobile_app",
        "device_id": "iPhone-123"
    }
}
```

## Python's `json` Module

```python
import json

# Parse JSON string → Python dict
data = json.loads('{"name": "Alice", "balance": 1000}')
print(data["name"])  # "Alice"

# Python dict → JSON string
output = json.dumps({"name": "Bob", "balance": 2000})
print(output)  # '{"name": "Bob", "balance": 2000}'

# Reading JSON file
with open("data.json", "r") as file:
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
